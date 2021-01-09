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
