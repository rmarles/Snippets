# General Tips + Tricks

Small quality-of-life PowerShell patterns and references that don't fit elsewhere.

---

## Tail a File (PowerShell `tail -f`)

**When:** Watching logs in real time  
**Why:** PowerShell equivalent of Linux `tail -f`

```powershell
Get-Content -Path "C:\scripts\test.txt" -Wait
```

---

## Start Transcript with Script Name + Date

**When:** Logging script execution  
**Why:** Automatic, self-naming logs

```powershell
Start-Transcript -Path (
  $PSScriptRoot + "\" +
  ([io.fileinfo]$MyInvocation.MyCommand.Definition).BaseName + "_" +
  (Get-Date -Format yyyy-MM-dd) + ".log"
)
```

---

## Poor-Man's `watch`

**When:** Periodic command execution  
**Why:** Observe changing state

```powershell
while ($true) {
  Get-Process | Sort CPU -Descending | Select -First 5
  Start-Sleep 5
  Clear-Host
}
```

---

## aka.ms Shortcuts

**When:** Quick access to Microsoft portals

- https://aka.ms/nohello
- https://aka.ms/GraphExplorer
- https://aka.ms/entra
- https://aka.ms/M365Maps
