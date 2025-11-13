# HashCat

---

## PoC

```
hashcat -m 2600 password.txt -r salt.txt /usr/share/wordlists/rockyou.txt 
```


```
hashcat -m 2600 password.txt -r <salt> <wordlist>
```

