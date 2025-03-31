Check user rights and of a user on an endpoint:
```powershell
whoami /priv
```

Get group details:
```powershell
Get-ADGroup -Identity "Server Operators" -Properties *
```

Domain admins group membership:
```powershell
Get-ADGroup -Identity "Domain Admins" -Properties * | select DistinguishedName,GroupCategory,GroupScope,Name,Members
```
