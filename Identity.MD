# Identity Related Snippets

Snippets related to Active Directory, identity objects, and identifier manipulation.

---

## Random Password Generator (Long)

**When:** Initial or break-glass passwords  
**Why:** High-entropy generation

```powershell
$initialPW = -join (
 'abcdefghkmnrstuvwxyzABCDEFGHKLMNPRSTUVWXYZ23456789$%&*#'.ToCharArray() |
 Get-Random -Count 45
)
```

---

## AD ObjectGUID → Base64 (ImmutableID)

**When:** Sync / federation troubleshooting  
**Why:** Required for Entra ID correlation

```powershell
$user = Get-ADUser user1
[Convert]::ToBase64String($user.ObjectGUID.ToByteArray())
```

---

## Base64 ImmutableID → GUID

**When:** Reverse lookup  
**Why:** Identify on-prem AD object

```powershell
[guid]([Convert]::FromBase64String($ImmutableId))
```

---

## Extract Domain from Distinguished Name

**When:** Parsing DNs  
**Why:** Build domain strings dynamically

```powershell
$DN = $ADObject.DistinguishedName
($DN -split "," | Where-Object {$_ -like "DC=*"} ) -replace "DC=" -join "."
```

---

# Troubleshooting

## "Empty" AD Results

**When:** Expected attributes missing  
**Why:** AD cmdlets don’t return non-default properties unless requested

```powershell
Get-ADUser user1 -Properties *
```

Prefer requesting only what you need:
```powershell
Get-ADUser user1 -Properties ObjectGUID, mail, proxyAddresses
```
