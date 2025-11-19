# Smb Enumeration with Nmap

---

##### Shares

```
nmap -p445 --script=smb-enum-shares.nse <target_ip>
```

```
nmap -p445 --script=smb-enum-shares.nse --script-args smbusername="",smbpassword="" <target_ip>
```


##### Users

```
nmap -p445 --script=smb-enum-users.nse <target_ip>
```

```
nmap -p445 --script=smb-enum-users.nse --script-args smbusername="",smbpassword="" <target_ip>
```



##### Sessions

```
nmap -p445 --script=smb-enum-sessions.nse --script-args smbusername="",smbpassword="" <target_ip>
```

#### OS

```
nmap -v -p 139,445 --script smb-os-discovery 192.168.50.152
```
