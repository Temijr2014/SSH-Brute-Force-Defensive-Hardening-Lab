# SSH-Brute-Force-Defensive-Hardening-Lab
This project demonstrates a real-world cybersecurity scenario: a Brute-Force Attack on an SSH service and the subsequent Defensive Hardening of the server to mitigate such threats.
The lab utilizes a Red Team/Blue Team approach to explore how attackers bypass basic security and how defenders can implement intelligent, behavior-based blocking.
# Technologies Used
- Attacker Machine: Kali Linux
- Victim Machine: Ubuntu (Running in VirtualBox)
- Attack Tools: Hydra (Network Logon Cracker)
- Defensive Tools: UFW (Uncomplicated Firewall), Fail2Ban
- Monitoring: Linux Auth Logs (/var/log/auth.log), Fail2Ban Logs
# Part 1: The Attack (Red Team)
In this phase, I simulated an SSH brute-force attack from a Kali Linux machine targeting the Ubuntu server.
- Initial Reconnaissance
Verified the SSH service was active on the target:
```
sudo nmap -sS <TARGET_IP>
```
<img width="1846" height="1197" alt="screen shot 5" src="https://github.com/user-attachments/assets/a3e3ab3f-7737-4f5f-8c11-69bee9948543" />

# Performing the Attack
Used Hydra to attempt a brute-force attack against the root and temiloluwa users.

# Slow and stealthy attack to bypass basic rate limiting
```
hydra -l temiloluwa -P /usr/share/wordlists/rockyou.txt -t 1 -W 10 ssh://<TARGET_IP>
```
# Key Finding 
By using a "Low-and-Slow" strategy (waiting 10 seconds between attempts), I was able to bypass the default ufw limit rule which only triggers if more than 6 connections occur within 30 seconds.

<img width="1844" height="1196" alt="screen shot 1" src="https://github.com/user-attachments/assets/47889f72-b0a5-4ece-ae81-cd4a7e764191" />

<img width="2560" height="1440" alt="screen shot 2" src="https://github.com/user-attachments/assets/2db68aab-c7f2-4459-a2a8-a9db104e0897" />

# Part 2: The Defense (Blue Team)
To secure the server against the "Low-and-Slow" attack, I implemented a multi-layered defense.

- Advanced Hardening (Fail2Ban)
Configured Fail2Ban to monitor authentication logs and automatically ban IPs exhibiting malicious behavior.

Configuration (/etc/fail2ban/jail.local):
```
[sshd]
enabled = true
maxretry = 3
findtime = 10m
bantime = 1h
```
By putting those lines in that file, you told Fail2Ban:
Watch SSH: Specifically look for people trying to get in via port 22.
- 3 Strikes: If someone fails 3 times (maxretry).
- Long Memory: If those 3 failures happen within 10 minutes(findtime).
- The Penalty: Block them for 1 hour (bantime).

How to verify it worked
After you restart the service (sudo systemctl restart fail2ban), run this:
```
sudo fail2ban-client status
```
If you see Number of jail: 1 and Jail list: sshd, then it was a success!

# Results & Analysis
The implementation of Fail2Ban successfully mitigated the attack. Unlike the basic UFW limit, Fail2Ban:
Increased Memory: The findtime of 10 minutes ensured that even slow attacks were recorded and aggregated.
Automated Response: After the 3rd failed attempt, the attacker's IP was automatically added to the firewall drop-list.
<img width="2560" height="1440" alt="screen shot 3" src="https://github.com/user-attachments/assets/11972344-7246-4b77-ba07-9313c24c1cc6" />

# Attacker fails to exploit the system 
<img width="1840" height="1203" alt="screen shot 4" src="https://github.com/user-attachments/assets/63aeb092-b99a-4cf4-a201-4150dbe59484" />

# Monitored Logs
It identified the specific "Failed password"
```
sudo tail -f /var/log/auth.log
```

<img width="2560" height="1440" alt="screen shot 6" src="https://github.com/user-attachments/assets/9d1ea9d3-590e-402c-b3f3-ca1d391dddcc" />

# Lessons Learned
- Static vs. Dynamic Security: Static rules (like UFW limit) are easily bypassed by changing the timing of an attack. Dynamic security (Fail2Ban) is necessary to catch persistent threats.

- Log Analysis: Understanding the structure of /var/log/auth.log is critical for identifying unauthorized access attempts.

- The Power of Whitelisting: In a production environment, it is vital to whitelist administrative IPs to avoid self-lockout during testing.

# 🛑Disclaimer!
This project was conducted in a controlled lab environment for educational purposes only!.
