# SNMP Subnet Enum

---

## PoC

Mapping community strings to variable

```
echo public > community
echo private >> community
echo manager >> community
```

Mapping network scan to ips variable

```
for ip in $(seq 1 254); do echo 192.168.145.$ip; done > ips
```

Enumerating snmp services on the entire subnet.

```
onesixtyone -c community -i ips
```
