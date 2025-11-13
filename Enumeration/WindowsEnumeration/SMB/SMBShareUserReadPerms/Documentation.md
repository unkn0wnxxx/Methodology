# SMB Share Perms

When u have creds for an user and want to check his perms for shares use the following.

---

## PoC

```
smbmap -H 10.129.161.230 -d active.htb -u svc_tgs -p GPPstillStandingStrong2k18
```

#### Syntax

```
smbmap -H <target_ip> -d <domain> -u <user> -p <password>
```
