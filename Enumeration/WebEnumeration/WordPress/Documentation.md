# WordPress

---

This command targets a WordPress site, -u enumerates all the users, -ap enumerates all the plugins and -api-token is optional but if you provide one you’ll get access to the vulnerability database lookups and without it WPScan still runs but skips vuln checks. You can sign up and get a few free daily tokens at https://wpscan.com/ I regenerated mine specifically for this lab.


## Syntax

```
wpscan --url http://192.168.199.229 --enumerate u,ap --api-token n0AA2fsKcUHcutaqKzkKi4WP0Ot4DQ5ugRMIuOWMmqg
```

Default Scan, if plugins are missed.

```
wpscan --url http://192.168.199.229 
```
