# Runas.exe

Once we have administrator credentials, we can utilize runas.exe to run our payload e.G shell.exe as Administrator

---

## PoC

```
runas.exe /u:Administrator shell.exe
*prompt password*
```

Gained new cmd session.

```
runas.exe /user:backupadmin cmd.exe
```
