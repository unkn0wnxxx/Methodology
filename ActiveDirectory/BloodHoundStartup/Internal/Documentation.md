# SharpHound

---

## PoC

1. Downloading it onto target system.

2. Bypassing policys.

```
powershell -ep bypass
```

3. Importing Script to activate functionalities.

```
Import-Module .\SharpHound.ps1
```

3. Download all local policies.

```
Invoke-BloodHound -CollectionMethod All -OutputDirectory C:\Users\stephanie\Desktop\ -OutputPrefix "corp audit"
```

Startup docker since it runs the backend of bloodhound

```
systemctl start docker
docker ps
```

Next step is to start up neo4j & bloodhound with credentials neo4j:HonorShard302!
Startup neo4j

```
neo4j console
```

Startup bloodhound

```
bloodhound
```

In order for bloodhound to work I had to reinstall it manually in an docker environment.
Logged into http://127.0.0.1:8080 (I also had to terminate Burp Proxy since it listens on port 8080).
Logged in with admin:HonorShard302!

Navigated to the left tab > Quick Upload > Selected all the .json files we received earlier.
