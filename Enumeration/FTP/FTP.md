# File Transfer Protocol (FTP) Enumeration

The File Transfer Protocol (FTP) allows files to be transferred between a client and a server over a cleartext channel.

Default Port: **21**

## FTP Anonymous Login

When checking an FTP server, a common misconfiguration is having FTP Anonymous login enabled. This allows any user to login with the username "Anonymous" and any password to gain access to the files on the server. To check for anonymous login:

```
ftp <IP address or hostname> <port>
Username: Anonymous
Password: AnythingHere
```

From here the anonymous user would be able to download files using `get <filename>.txt` or potentially upload files using `put <filename>.txt`


## Enumeration using NMAP

NMAP can be used to automatilly scan for FTP misconfigurations and well-known vulnerabilities that exist within certain FTP server implementations.  The following command will check for a number of common misconfigurations and vulnerabilities:

`nmap --script=ftp-anon,ftp-bounce,ftp-libopie,ftp-proftpd-backdoor,ftp-vsftpd-backdoor,ftp-vuln-cve2010-4221,tftp-enum -p <21> <IP or Hostname>`

## Metasploit Framework Enumeration

The Metasploit Framework can also be used to enumerate FTP.  The following section shows some uses:

### Anonymous Login

Check for FTP Anonymous Login:

```
use auxiliary/scanner/ftp/anonymous
set RHOSTS <IP Address>
set RPORT <Port>
exploit
```

### FTP Version

Enumerate FTP Version:

```
use auxiliary/scanner/ftp/ftp_version
set RHOSTS <IP Address>
set RPORT <Port>
exploit
```

### FTP Login

Bruteforce FTP Login:

```
use auxiliary/scanner/ftp/ftp_login
set RHOSTS <IP Address>
set RPORT <Port>
set user_file /path/to/usernames.txt
set pass_file /path/to/passwords.txt
exploit

```


## Python3 Pyftpdlib Temporary FTP Server

Python 3 can be used to bring up a temporary FTP server when needed in a pentest. The following command will install the pyftpdlib library:

`pip3 install pyftpdlib`

Once installed the following command can be used to stand up a temporary FTP server within the directory it's started from:

`python3 -m pyftpdlib -t <IP Address> -p <port>`

If the "-t" and "-p" options aren't given, it will, by default, listen on all intefaces (0.0.0.0) on port 2121.
