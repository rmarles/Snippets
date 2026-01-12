# PowerShell Best Practices

A lightweight checklist and patterns for writing maintainable, safe PowerShell.
Optimized for scripts you’ll run in automation and revisit months later.

---

## Use strict mode (when you can)

**When:** Scripts should fail fast  
**Why:** Catch uninitialized variables and subtle bugs

```powershell
Set-StrictMode -Version Latest
```

---

## Prefer approved verbs and consistent naming

**When:** Writing functions/modules  
**Why:** Discoverability and consistency

```powershell
Get-Verb
# Example: Get-Thing, Set-Thing, Test-Thing, New-Thing
```

---

## Add comment-based help

**When:** Any reusable function  
**Why:** Self-documenting, supports Get-Help

```powershell
function Get-Widget {
<#
.SYNOPSIS
  Returns widgets.
.DESCRIPTION
  Queries widgets from source X and returns objects.
.PARAMETER Name
  Widget name filter.
.EXAMPLE
  Get-Widget -Name "blue"
#>
  param([string]$Name)
}
```

---

## Make scripts safe by default

**When:** Anything that changes state  
**Why:** Reduce blast radius

**Pattern:**
- Support `-WhatIf` and `-Confirm` for destructive actions
- Implement `SupportsShouldProcess`

```powershell
function Remove-Widget {
  [CmdletBinding(SupportsShouldProcess)]
  param([Parameter(Mandatory)][string]$Id)

  if ($PSCmdlet.ShouldProcess($Id, "Remove widget")) {
    # remove logic
  }
}
```

---

## Use `$ErrorActionPreference` intentionally

**When:** Automation needs deterministic failure  
**Why:** Avoid silent partial success

```powershell
$ErrorActionPreference = "Stop"
try {
  # work
} catch {
  Write-Error $_
  throw
}
```

---

## Prefer `Write-Verbose` / `Write-Information` over `Write-Host`

**When:** Logging  
**Why:** Lets callers control verbosity and transcript capture

```powershell
[CmdletBinding()]
param()

Write-Verbose "Starting..."
Write-Information "User created"
```

---

## Return objects, not strings

**When:** Anything that might be reused  
**Why:** Easier piping, exporting, testing

```powershell
[pscustomobject]@{
  User  = $upn
  State = "Created"
}
```

---

## Validate inputs

**When:** Functions called with user input  
**Why:** Fail early and clearly

```powershell
param(
  [Parameter(Mandatory)]
  [ValidateNotNullOrEmpty()]
  [string]$UserPrincipalName
)
```

---

## Avoid array += in loops for large data

**When:** Large tenants/datasets  
**Why:** Performance

```powershell
$list = [System.Collections.Generic.List[object]]::new()
$list.Add($obj)
```

---

## Use modules and pin versions in automation

**When:** CI/CD or scheduled jobs  
**Why:** Predictable behavior

```powershell
# Example: install exact version
Install-Module Microsoft.Graph -RequiredVersion 2.18.0 -Scope AllUsers
```

---

## Prefer `CimInstance` over legacy WMI cmdlets

**When:** System inventory  
**Why:** More modern and robust

```powershell
Get-CimInstance Win32_OperatingSystem
```

---

## Use `Export-Csv` with `-NoTypeInformation` and a stable schema

**When:** Reports  
**Why:** Clean output and predictable columns

```powershell
$rows | Select Col1,Col2 | Export-Csv .\report.csv -NoTypeInformation
```
