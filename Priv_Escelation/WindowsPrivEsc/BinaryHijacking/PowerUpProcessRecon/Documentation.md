# PowerUp

---

Is an tool which automatically detects priv esc vectors for binary hijacking.

## Syntax

```
powershell -ep bypass
```

Execute it

```
. .\PowerUp.ps1
```

Now we can use "Get-ModifiableServiceFile".

This function displays services the current user can modify, such as the service binary or configuration files.

```
Get-ModifiableServiceFile
```
