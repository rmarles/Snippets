# Windows Workstation \ Server Snippets

Lightweight PowerShell snippets for Windows client and server systems.
Focused on fast local diagnostics and inventory checks.

---

## Serial Number (WMIC Deprecated)

**When:** Need hardware serial number  
**Why:** `wmic` is deprecated

```powershell
(Get-CimInstance Win32_BIOS).SerialNumber
```

---

## Computer Make / Model

**When:** Asset identification  
**Why:** Useful for support and warranty lookups

```powershell
Get-CimInstance Win32_ComputerSystem |
  Select Manufacturer, Model
```

---

## Logged-on User

**When:** Shared systems / RDP  
**Why:** Confirm active session identity

```powershell
(Get-CimInstance Win32_ComputerSystem).UserName
```

---

## Last Boot Time

**When:** Uptime validation  
**Why:** Troubleshooting stability / patching

```powershell
(Get-CimInstance Win32_OperatingSystem).LastBootUpTime
```

---

## BitLocker Status

**When:** Device security checks  
**Why:** Confirm encryption state

```powershell
Get-BitLockerVolume |
  Select MountPoint, VolumeStatus, ProtectionStatus
```

---

# Troubleshooting

## CIM/WMI Queries Failing

**When:** CIM commands error or return nothing  
**Why:** Namespace/provider issues, WinRM, permissions

**Checks:**
- Run PowerShell elevated
- Confirm WMI service is running (`Winmgmt`)
- Try a basic query:

```powershell
Get-CimInstance Win32_OperatingSystem
```

If remote, confirm WinRM and firewall rules are enabled.
