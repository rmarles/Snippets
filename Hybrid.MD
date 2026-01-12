# Hybrid Identity Snippets

Notes for Entra Connect, hybrid identity, and sync-related troubleshooting.

---

## ImmutableID Matching

**When:** Sync / sign-in issues  
**Why:** Confirm cloud ↔ on-prem object match

```powershell
[Convert]::ToBase64String(
  (Get-ADUser user1).ObjectGUID.ToByteArray()
)
```

---

## Entra Connect Sync Status

**When:** Changes not syncing  
**Why:** Confirm scheduler state

```powershell
Get-ADSyncScheduler
```

---

## Force Delta Sync

**When:** Urgent identity updates  
**Why:** Bypass scheduler delay

```powershell
Start-ADSyncSyncCycle -PolicyType Delta
```

---

# Troubleshooting

## Attribute Not Flowing to Entra

**When:** Expected attribute missing in Entra  
**Why:** Sync rules, filtering, or permissions exclude it

**Checks:**
- Sync rules editor (inbound/outbound)
- Attribute permissions
- Filtering (OU / domain)
- Connector space vs metaverse in Sync Service Manager
