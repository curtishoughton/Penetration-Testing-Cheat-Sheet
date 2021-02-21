# Server Message Block (SMB)

SMB is a protocol which allows for the sharing and discovery of Files, Printers, Serial Ports and Named Pipes accross a network.  A number of techniques can be used to enumerate SMB and have been listed below for reference:

## SMB Null Session
To test for an anonumous user SMB null session the following command can be used:

`smbclient -N -L //192.168.X.X`

The command will use smbclient to test whether the machine accepts SMB null sessions with an anonymous user.  If successful, this will open up an unauthenticated netbios session between two computers.  SMB null sesssions are a common misconfiguration seen when SMBv1 is enabled on operating systems such as Windows 2000, XP and Server 2003.

If an NTLM hash has been obtained during the penetration testing process smbclient can be used to pass-the-hash and obtain access to a resource, such as a file share:

`smbclient -L \\\\192.168.X.X --pw-nt-hash -U username%NTHASH`

Note for the command above `username` will have to be changed to a valid user and `NTHASH` will have to be replaced with a valid NT password hash for that user.

A tool named **smbmap** can also be used to enumerate file shares on a system.  To check for file shares without providing a username and password the following command can be used:

`smbmap -H 192.168.X.X`

### Relative Identifier (RID) Enumeration
Once a null session has been obtained it is possible to enumerate users on a Windows domain using RID cycling.  A tool named **RidEnum** can be used to enumerate valid users on the system.  Note that a RID of `500` would be the default built-in Windows administrator account.  Even if renamed, the account would still retain the same RID.  Newly created user accounts start at a RID of `1000`.  

The following command can be used to enumerate RID's from 500 - 20,000:

`ridenum.py 192.168.X.X 500 20000`

Optionally a password list can be specified, which will attempt to brute force passwords of any valid users found:


`ridenum.py 192.168.X.X 500 20000 /usr/share/wordlists/password.lst`

## Enumerating SMB File Shares

### SMBMap

If a valid username, password and workgroup is known this can be specified as so:

`smbmap -u username -p password1 -d workgroup -H 192.168.X.X`

To use smbmap with a password has that has been obtained, both the LM and NT hashes must be specified.  Modern operating systems no longer use the LM hash, therefore it can be replaced with the 'blank' LM hash `aad3b435b51404eeaad3b435b51404ee`:

`smbmap -u username -p 'aad3b435b51404eeaad3b435b51404ee:NTHASH' -H 192.168.X.X`

The `-x` allows a command to execute on the target host, providing the credentials of a local administrator are known.  The following command will run `ipconfig` on the target:

`smbmap -u local_admin -p password1 -d workgroup -H 192.168.X.X -x 'ipconfig'`

If you want to interact with a particular share the `-r` option can be specified with a share name:

`smbmap -u local_admin -p password1 -d workgroup -H 192.168.X.X -r 'C$'`

To download a file from a share the `--download` option can be used:

`smbmap -u local_admin -p password1 -d workgroup -H 192.168.X.X --download 'C$\filename.txt'`

To upload a file to a file share the `--upload` command can be used.  The first argument is the file to be uploaded and the second argument is the destination on the "victim" machine:

`smbmap -u local_admin -p password1 -d workgroup -H 192.168.X.X --upload '/local/file/path/file_to_upload.txt' 'C$\destination\path\uploaded.txt'`

### Nmap

#### Enumerate Shares

Nmap contains NSE scripts that can be used for enumerating SMB shares:

`nmap -p 445 -sT -vv --script=smb-enum-shares.nse,smb-enum-users.nse 192.168.X.X`

#### Check Available Protocols

Check what SMB protocols the host supports:

`nmap -p 445 -sT -vv --script=smb-protocols 192.168.X.X`

#### Check SMB Security Mode

Check what SMB security protocols are enabled (E.G if message signing is required etc)

`nmap -p 445 -sT -vv --script=smb-security-mode 192.168.X.X`

#### Enumerate Logged in Users

Check what users are logged into the system.  If no credentials are specified, it will default to trying "guest" login:

`nmap -p 445 -sT -vv --script=smb-enum-sessions 192.168.X.X`

To specify credentials the following can be appended to the command, where `<username>` and `<password>` should be changed to the user/pass credentials to try: 

`nmap -p 445 -sT -vv --script=smb-enum-sessions --script-args smbusername=<username>,smbpassword=<password> 192.168.X.X`

#### Enumerate Available Domains

Return a list of domain specific information, such as users and groups (Use Local Admin Account for best results):

`nmap -p 445 -sT -vv --script=smb-enum-sessions --script-args smbusername=<username>,smbpassword=<password> 192.168.X.X`



#### NSE SMB Script List

```
smb2-capabilities.nse
smb2-security-mode.nse
smb2-time.nse
smb2-vuln-uptime.nse
smb-brute.nse
smb-double-pulsar-backdoor.nse
smb-enum-domains.nse
smb-enum-groups.nse
smb-enum-processes.nse
smb-enum-services.nse
smb-enum-sessions.nse
smb-enum-shares.nse
smb-enum-users.nse
smb-flood.nse
smb-ls.nse
smb-mbenum.nse
smb-os-discovery.nse
smb-print-text.nse
smb-protocols.nse
smb-psexec.nse
smb-security-mode.nse
smb-server-stats.nse
smb-system-info.nse
smb-vuln-conficker.nse
smb-vuln-cve2009-3103.nse
smb-vuln-cve-2017-7494.nse
smb-vuln-ms06-025.nse
smb-vuln-ms07-029.nse
smb-vuln-ms08-067.nse
smb-vuln-ms10-054.nse
smb-vuln-ms10-061.nse
smb-vuln-ms17-010.nse
smb-vuln-regsvc-dos.nse
smb-vuln-webexec.nse
smb-webexec-exploit.nse

```

## SMB File Retrieval

## SMBGet

Smbget can be used to recursively download files from an SMB file share:

`smbget -R smb://<ip_address>/<sharenamegoeshere>`

