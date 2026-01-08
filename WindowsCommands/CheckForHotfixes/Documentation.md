# Check if Patches are done

---

When an version is vulnerable to an kernel exploit, it can still have the same version but Hotfixes in place.

## Syntax

```
wmic qfe list | findstr "KB2503665 KB2592799 KB2645640 KB2875783 KB2975684"
```

```
wmic qfe list | findstr "<hotfix_num>"
```
