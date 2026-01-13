# AD Enum

---

## PoC

Enum users inside domain.

```
C:\Users\stephanie>net user /domain
The request will be processed at a domain controller for domain corp.com.

User accounts for \\DC1.corp.com

-------------------------------------------------------------------------------
Administrator            dave                     Guest
iis_service              jeff                     jeffadmin
jen                      krbtgt                   pete
stephanie
```

Inspecting user information.

```
net user jeffadmin /domain
```

Enumerating groups on domain.

```
net group /domain
```

Enumerating devices and there os + dns names.

```
Get-NetComputer | select operatingsystem,dnshostname
```

Enumerating DC

```
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
```
