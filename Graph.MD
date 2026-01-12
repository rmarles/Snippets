# Microsoft Graph Snippets

Common Microsoft Graph PowerShell patterns and reminders.

---

## Connect to Graph

**When:** Starting Graph work  
**Why:** Establish context

```powershell
Connect-MgGraph -Scopes "User.Read.All","Group.Read.All","Directory.Read.All"
Get-MgContext
```

---

## Paging Reminder

**When:** Large tenants  
**Why:** Avoid partial results

```powershell
Get-MgUser -All
```

---

## Get Object by ID

**When:** ID-only data  
**Why:** Resolve display name

```powershell
Get-MgDirectoryObject -DirectoryObjectId <ObjectId>
```

---

# Troubleshooting

## Graph: Insufficient Privileges (403)

**When:** 403 / Insufficient privileges  
**Why:** Missing scopes or missing admin consent

```powershell
Get-MgContext
```

**Checks:**
- Verify scopes listed include what you need
- Confirm admin consent for app permissions
- Confirm you're using the right auth mode (delegated vs app-only)

## Empty Results / Missing Properties

**When:** Returned objects miss expected fields  
**Why:** Many properties are not returned unless requested

Use `-Property` and select explicitly, e.g.:

```powershell
Get-MgUser -UserId user@domain.com -Property Id,DisplayName,OnPremisesImmutableId
```
