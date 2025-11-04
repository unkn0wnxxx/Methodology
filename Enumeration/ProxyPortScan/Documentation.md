# Proxy Port Scan / Service Enumeration

We can scan open ports/services running behind an proxy with an tool called "spose.py".

---

## PoC

```
python spose.py --proxy http://192.168.108.189:3128 --target 192.168.108.189 
Scanning default common ports
Using proxy address http://192.168.108.189:3128
192.168.108.189:3306 seems OPEN
192.168.108.189:8080 seems OPEN
```

Let's configure an proxy using the firefox extension tool called "foxyproxy" it's important to specify the port 3128 because only then we are able to connect to mysql & 8080.

```
Selected HTTP > Target IP of Box 192.168.187.189 & proxy port 3128
```

Now we are able to see the ports behind the proxy 3306 (mysql) & 8080 (webserver)
