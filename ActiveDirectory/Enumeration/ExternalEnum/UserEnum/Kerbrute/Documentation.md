# Kerbrute

---

We can enumerate users once we got the domain of the active directory and the target ip.

## PoC

```
./kerbrute userenum -d hutch.offsec --dc 192.168.155.122 /usr/share/wordlists/SecLists/Usernames/xato-net-10-million-usernames.txt -t 100

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: v1.0.3 (9dad6e1) - 01/24/26 - Ronnie Flathers @ropnop

2026/01/24 08:19:22 >  Using KDC(s):
2026/01/24 08:19:22 >   192.168.155.122:88

2026/01/24 08:19:22 >  [+] VALID USERNAME:       admin@hutch.offsec
2026/01/24 08:19:24 >  [+] VALID USERNAME:       administrator@hutch.offsec
2026/01/24 08:19:24 >  [+] VALID USERNAME:       Admin@hutch.offsec
2026/01/24 08:19:27 >  [+] VALID USERNAME:       Administrator@hutch.offsec
2026/01/24 08:19:47 >  [+] VALID USERNAME:       ADMIN@hutch.offsec
```

