# SNMP (Simple Network Management Protocol)

responsible for managing and monitoring devices on networks, like routers, switches, servers, and printers.

It acts as a messenger system.

--> A manager (monitoring pc) asks for information. 
--> An agent (software of the network device) answers. 
usually in pre-defined stats like "Printer is out of paper" or "Network port is down"

A tool which we can utilize to enumerate SNMP-enabled devices is onesixtyone and give us the community string of the SNMP Server (password).

```
onesixtyone -c /usr/share/seclists/Discovery/SNMP/snmp-onesixtyone.txt <target_ip>
```

The best tool to enumerate information out of an running SNMP Server is snmp-check.

```
snmp-check -c openview <target_ip>
```

Since often times snmp-check doesn't work.
Another tool which displays us actual information about queries and responses is snmpwalk.

```
snmpwalk -v2c -c openview 10.10.38.124 1.3.6.1.4.1.77.1.2.25
```

- snmpwalk utility for queuering multiple OIDs sequentially 
- -v 2c uses SNMP version 2c 
- -c openview stands for the Community string (password) is "openview"
- 1.3.6.1.4.1.77.1.2.25 is the OID for Windows user account enumeration

In order to enumerate more potential community strings, we can utilize an tool called "snmpbrute.py".

#### List Processes

```
snmpwalk -c public -v1 192.168.145.151 1.3.6.1.2.1.25.4.2.1.2
```


```
python3 snmpbrute.py -t 10.129.228.102 -f /usr/share/wordlists/SecLists/Discovery/SNMP/common-snmp-community-strings.txt
```
