# Pivoting

---

##### Metasploit

once we get a meterpreter session, we can utilize commands.

run autoroute -s <target_ip><target_prefix> 10.0.29.0/20 ---> creates a route
which gives us access to other ip adresses within the network.

use auxiliary/scanner/portscan/tcp
set RHOSTS demo2.ine.local
set PORTS 1-100
exploit

sessions -i 1

portfwd add -l 1234 -p 80 -r <IP Address of demo2.ine.local>
portfwd list

nmap -sCV -p 1234 localhost
