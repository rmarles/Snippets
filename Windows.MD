# Windows Workstation \ Server related snippets

# Serial Number
get wmix bios get serialnumber has been deprecated.  Instead, use PowerShell:
```
(Get-CimInstance Win32_BIOS).SerialNumber
```
