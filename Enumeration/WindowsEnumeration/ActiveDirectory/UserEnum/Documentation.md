# User Enumeration in Active Directory

---

```
nmap -p 88 --script=krb5-enum-users --script-args krb5-enum-users.realm="thm.corp", userdb=/usr/share/wordlists/seclists/Usernames/top-usernames-shortlist.txt 10.10.173.0
Starting Nmap 7.95 ( https://nmap.org ) at 2025-09-15 07:18 CDT
Unable to split netmask from target expression: "userdb=/usr/share/wordlists/seclists/Usernames/top-usernames-shortlist.txt"
Nmap scan report for thm.corp0 (10.10.173.0)
Host is up (0.034s latency).

PORT   STATE SERVICE
88/tcp open  kerberos-sec
| krb5-enum-users: 
| Discovered Kerberos principals
|     guest@thm.corp
|_    administrator@thm.corp

Nmap done: 1 IP address (1 host up) scanned in 0.49 seconds
```

Utilizing netcatexec we can enumerate users

```
nxc smb dc01.soupedecode.local -u 'guest' -p '' --rid
```


```

```



```

```



```

```
