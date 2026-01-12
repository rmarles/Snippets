# Identity Related

```
$initialPW = $null
$initialPW += -join ('abcdefghkmnrstuvwxyzABCDEFGHKLMNPRSTUVWXYZ23456789$%&*#'.ToCharArray() | Get-Random -Count 45)
```

```
$UserObject = Get-ADUser "CN=User1,OU=Corp.Users,DC=itsrob,DC=local" -server dc1.itsrob.local
$UserObjectGuid = $UserObject.ObjectGUID
$UserObjectGuidBase64 = [System.Convert]::ToBase64String($UserObjectGuid.ToByteArray())
write-host
write-host $UserObjectGuidBase64
```

```
$DN = $ADObject.distinguishedName
$Domain = $DN -Split "," | ? {$_ -like "DC=*"}
$Domain = $Domain -join "." -replace ("DC=", ""
```
