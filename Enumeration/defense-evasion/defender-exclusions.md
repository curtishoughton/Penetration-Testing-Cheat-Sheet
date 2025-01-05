# Windows Defender

## Checking for Exclusions

### If admin 

```powershell
 Get-MpPreference | Select-Object -Property ExclusionPath -ExpandProperty ExclusionPath
```

### Standard (Low Privilege) User

A standard user can check for the event log 5007 and query exclusions with the following command:


```powershell
 Get-WinEvent -LogName "Microsoft-Windows-Windows Defender/Operational" -FilterXPath "*[System[(EventID=5007)]]" | Where-Object {  $_.Message -like "*exclusions\Path*" }  | Select-Object Message | FL
```
