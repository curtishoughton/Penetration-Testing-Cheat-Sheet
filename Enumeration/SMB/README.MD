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

If a valid username, password and workgroup is known this can be specified as so:

`smbmap -u username -p password1 -d workgroup -H 192.168.X.X`

To use smbmap with a password has that has been obtained, both the LM and NT hashes must be specified.  Modern operating systems no longer use the LM hash, therefore it can be replaced with the 'blank' LM hash `aad3b435b51404eeaad3b435b51404ee`:

`smbmap -u username -p 'aad3b435b51404eeaad3b435b51404ee:NTHASH' -H 192.168.X.X`
