# Privilege Escalation using Unquoted Service Paths

## Description

A service with an unquoted path can be abused to escalate privileges on a Windows host.  If the services binpath does not have quotes and contains spaces, a malicious binary could be placed within the path.

### Example

If a service had the following binpath without `"` surrounding it, it would look like:

`C:\Program Files\programname\someProgram.exe`

If, as an attacker, it was possible to write to the "Program Files" folder, a malicious executable named `Program
.exe` could be placed.

Once the service was reloaded, it would look at the path and now load the malicious executable:

`C:\Program.exe`

This is why any services that contain spaces within the binpath should contain `"` around them.

## Locating Unquoted Service Paths

### CMD

`wmic service get name,displayname,pathname,startmode 2>nul |findstr /i "Auto" 2>nul |findstr /i /v "C:\Windows\\" 2>nul |findstr /i /v """`

### Powershell

`gwmi -class Win32_Service -Property Name, DisplayName, PathName, StartMode | Where {$_.StartMode -eq "Auto" -and $_.PathName -notlike "C:\Windows*" -and $_.PathName -notlike '"*'} | select PathName,DisplayName,Name`
