# Licensing Snippets

Notes and helpers for Microsoft 365 and Entra ID licensing.

---

## List Subscribed SKUs

**When:** License inventory  
**Why:** Identify available SKUs and usage

```powershell
Get-MgSubscribedSku |
  Select SkuPartNumber, ConsumedUnits
```

---

## Users Without Licenses

**When:** Cleanup / audits  
**Why:** Identify unlicensed accounts

```powershell
Get-MgUser -All |
  Where-Object { $_.AssignedLicenses.Count -eq 0 } |
  Select DisplayName, UserPrincipalName
```

---

## Group-Based Licensing Reminder

**When:** Troubleshooting license issues  
**Why:** Direct assignment may be overridden

> Always check group membership before assigning licenses directly.

---

# Troubleshooting

## License Assignment Errors

**When:** Assignment failures  
**Why:** SKU conflicts, disabled plans, or group precedence

**Checks:**
- Conflicting SKUs
- Disabled service plans
- Group-based licensing precedence
- Group membership replication delay
