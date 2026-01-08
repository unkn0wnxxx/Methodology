# Methodology when in Windows

---

## Username and hostname

```
C:\Users\dave>whoami
whoami
```

## Group memberships of the current user

```
whoami /groups
```

In PowerShell

```
Get-LocalUser
```

## Existing users and groups


```
net users
```

In PowerShell

```
Get-LocalGroup
```

```
Get-LocalGroupMember adminteam
```

## Operating system, version and architecture
 
```
systeminfo
```

Improved Version

```
systeminfo | findstr /B /C:"Host Name" /C:"OS Name" /C:"OS Version" /C:"System Type" /C:"Network Card(s)" /C:"Hotfix(s)"
```

## Network information


```
ipconfig /all
```

Display routing table for pivoting.

```
route print
```

## Installed applications

32-Bit Enumeration

```
Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
```

64-Bit Enumeration

```
Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
```

```
Check Downloads Directories.
```

## Running Services

```
ss -tulnp
```

```
netstat -ano
```

Checking for running and stopping services.

```
Get-CimInstance -ClassName win32_service | Select Name,State,PathName
```

## Running processes


```
tasklist /v
```

More efficient 

```
wmic process get name,processid,executablepath
```

In PowerShell

```
Get-Process
```
