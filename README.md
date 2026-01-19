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



