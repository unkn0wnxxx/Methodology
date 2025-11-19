# Host Discovery

---

Scanning subnet for alive hosts

```
nmap -sn 192.168.50.0/24 -oG alive_hosts.txt 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-11-17 16:30 EST
Nmap scan report for 192.168.50.0
Host is up (0.000088s latency).
Nmap scan report for 192.168.50.1
Host is up (0.00044s latency).
Nmap scan report for 192.168.50.2
Host is up (0.00030s latency).
Nmap scan report for 192.168.50.3
Host is up (0.000066s latency).
Nmap scan report for 192.168.50.4
Host is up (0.00013s latency).
Nmap scan report for 192.168.50.5
Host is up (0.00011s latency).
Nmap scan report for 192.168.50.6
Host is up (0.000084s latency).
Nmap scan report for 192.168.50.7
Host is up (0.00028s latency).
Nmap scan report for 192.168.50.8
Host is up (0.00021s latency).
Nmap scan report for 192.168.50.9
Host is up (0.00016s latency).
Nmap scan report for 192.168.50.10
```

```
grep Up alive_hosts.txt| cut -d " " -f2
```


```
netdiscover -i eth0 -r 192.168.2.0/24
```

## Service Discovery

```
nmap -p 80 192.168.50.0-255 -oG service_discovery.txt
```

```
grep open service_discovery.txt | cut -d " " -f2
```
