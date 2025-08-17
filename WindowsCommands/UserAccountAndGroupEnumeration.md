# User Account Commands

##### Powershell

powershell -c (get-wmiobject win32_useraccount)


##### CMD

net users --> displays all users

net user administrator --> displays information regarding that specific user account.

net localgroup --> displays all groups

query user --> to check which users are logged on to the system.

cmdkey /list --> to show if user got stored credentials.

runas /user:Administrator /noprofile /savecred "cmd.exe /c whoami > C:\users\security\desktop\output.txt"
