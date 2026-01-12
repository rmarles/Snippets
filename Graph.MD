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

## Permissions Gotcha

**Note:** Missing properties often mean missing scopes, not empty data.
