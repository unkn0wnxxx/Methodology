# Subdomain Enumeration Methodology

---

```
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/bitquark-subdomains-top100000.txt -u http://mentorquotes.htb/ -H "Host: FUZZ.mentorquotes.htb" -fs 290-360
```



```
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -u http://10.129.228.102 -H 'Host: FUZZ.mentorquotes.htb' -mc 200,404 
```



```

```
