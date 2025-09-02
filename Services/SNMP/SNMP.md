# SNMP (Simple Network Management Protocol)

responsible for managing and monitoring devices on networks, like routers, switches, servers, and printers.

It acts as a messenger system.

--> A manager (monitoring pc) asks for information. 
--> An agent (software of the network device) answers. 
usually in pre-defined stats like "Printer is out of paper" or "Network port is down"

A tool which we can utilize to enumerate SNMP-enabled devices or queries is onesixtyone.

```
onesixtyone -c /usr/share/seclists/Discovery/SNMP/snmp-onesixtyone.txt <target_ip>
```
