# Security Snippets

Common security-related checks for Entra ID and Microsoft 365.

---

## Risky Sign-ins

**When:** Investigating potential compromise  
**Why:** Identify high-risk authentication events

```powershell
Get-MgIdentityProtectionRiskySignIn -Top 50 |
  Select UserDisplayName, RiskLevel, RiskState, IpAddress
```

---

## Risky Users

**When:** Security incident review  
**Why:** Find accounts flagged by Identity Protection

```powershell
Get-MgIdentityProtectionRiskyUser |
  Select UserDisplayName, RiskLevel, RiskState
```

---

## Conditional Access Policies

**When:** Auditing access controls  
**Why:** Validate policy presence and state

```powershell
Get-MgIdentityConditionalAccessPolicy |
  Select DisplayName, State
```

---

## Audit Logs (Directory)

**When:** Change tracking  
**Why:** Identify who changed what

```powershell
Get-MgAuditLogDirectoryAudit -Top 50 |
  Select ActivityDisplayName, InitiatedBy, TargetResources
```
