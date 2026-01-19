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
sudo nmap -sS <ubuntu ip>
```
<img width="1846" height="1197" alt="screen shot 5" src="https://github.com/user-attachments/assets/a3e3ab3f-7737-4f5f-8c11-69bee9948543" />
