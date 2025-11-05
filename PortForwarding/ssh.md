# Port Forwarding in SSH

---


#### PoC

```
ssh amay@sea -L 8080:127.0.0.1:8080
```

#### Mapping internal port 80 to target ip address.

```
ssh -N -L 0.0.0.0:80:192.168.242.99:80 ariah@192.168.242.99
```



```

```
