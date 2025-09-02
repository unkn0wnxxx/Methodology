# SNMP (Simple Network Management Protocol)

responsible for managing and monitoring devices on networks, like routers, switches, servers, and printers.

It acts as a messenger system.

--> A manager (monitoring pc) asks for information. 
--> An agent (software of the network device) answers. 
usually in pre-defined stats like "Printer is out of paper" or "Network port is down"

A tool which we can utilize to enumerate SNMP-enabled devices is onesixtyone.

```
onesixtyone -c /usr/share/seclists/Discovery/SNMP/snmp-onesixtyone.txt <target_ip>
```

Another tool which displays us actual information about queries and responses is snmpwalk.

```
snmpwalk -v 2c -c openview 10.10.38.124 1.3.6.1.4.1.77.1.2.25
```

snmpwalk utility for queuering multiple OIDs sequentially
-v 2c uses SNMP version 2c
-c openview stands for the Community string (password) is "openview"
1.3.6.1.4.1.77.1.2.25 is the OID for Windows user account enumeration
