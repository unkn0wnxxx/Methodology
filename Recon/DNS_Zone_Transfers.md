## DNS Zone Transfers

---

DNS Records

A - resolves a hostname or domain to an IPv4 Address  
AAAA - resolves a hostname or domain to an IPv6 Address  
NS - Referenceto the domains nameserver.  
MX - Resolves a domain to a mail server.  
CNAME - Used for domain aliases... etc...

A tool named dnsenum can enumerate dns records, which aren't getting listed by passive recon tools
like dnsrecon -d <target_ip>. It can also be used to perform a dns zone transfer
automatically and perform an dns brute-force --> to identify dns records/sub-domains.  

Before any DNS Server the computer utilizes the internal hosts file. In Linux it is /etc/hosts.  
You can utilize the hosts file whenever u wanna map a certain domain to an ip address.
e.G you can get the ip of google and save the ip of google to another domain named --> stick.com.  
If you would copy & paste the domain inside the browser it would load up the google website.  

dnsrecon -d 10.129.161.230 -r 10.0.0.0/8

dnsdumpster

wafw00f <target_ip> --> shows u which firewall/proxy is behind an webpage
