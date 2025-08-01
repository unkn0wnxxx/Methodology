# Hydra Brute-Forcing

hydra -l <user> -P /usr/share/wordlists/rockyou.txt <target_ip> -s <target_port> http-get

hydra -l <user> -P /usr/share/wordlists/rockyou.txt <target_ip> -s <target_port> http-post-form
