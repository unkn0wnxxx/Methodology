## WordsPress Enumeration

---


#### PoC

```
sudo nmap -T4 -Pn -sC --script http-wordpress-enum --script-args http-wordpress-enum.root="/webservices/wp/",http-wordpress-enum.search-limit="all",http-wordpress-enum.check-latest="true" -p80 <domain>
```
