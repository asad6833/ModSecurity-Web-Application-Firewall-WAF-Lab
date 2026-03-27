# 🛡️ Applied Live Lab: Testing a Web Application Firewall (WAF)

## 📌 Scenario
This lab focuses on configuring and testing **ModSecurity**, an open-source Web Application Firewall (WAF).

ModSecurity protects web applications by:
- Inspecting HTTP traffic
- Blocking malicious payloads
- Logging attack attempts

---

## 🎯 Objectives

- Configure ModSecurity in blocking mode
- Perform vulnerability scans
- Simulate attacks:
  - SQL Injection (SQLi)
  - Cross-Site Scripting (XSS)
  - Command Injection
  - Directory Traversal
- Analyze Apache logs

---

## 🖥️ Lab Environment

| System | Role |
|------|------|
| LAMP VM | Web Server (Debian + Apache + ModSecurity) |
| Kali Linux | Attacker Machine |

---
![modsecurityinner1](https://github.com/user-attachments/assets/7221b3f2-0891-4738-8961-0d284c14d11f)

<img width="823" height="243" alt="image" src="https://github.com/user-attachments/assets/2e04fb49-a586-49b7-8981-9aed536d22fa" />

<img width="820" height="1173" alt="image" src="https://github.com/user-attachments/assets/6a4f0483-e5b1-4dee-95bd-c4ca9a184ec6" />


<img width="599" height="646" alt="image" src="https://github.com/user-attachments/assets/6ed6e4b5-5573-4751-bf28-4cd8878b7c22" />

<img width="624" height="361" alt="image" src="https://github.com/user-attachments/assets/d729535d-7c5d-47e7-9107-dca5cf1e9eee" />
<img width="836" height="253" alt="image" src="https://github.com/user-attachments/assets/89cf6fa6-b32c-4669-9c58-37fab207b191" />

![gallery-5ce06222d18ec](https://github.com/user-attachments/assets/3c24a0bc-299f-4021-826a-91fc22b6ce84)
<img width="![image-63](https://github.com/user-attachments/assets/a340d2b0-3c6f-4f39-8deb-0e096a29422c)
836" height="253" alt="image" src="https://github.com/user-attachments/assets/190c24ac-bb30-4136-8df7-bd11b33d46e2" />
<img width="669" height="578" alt="image" src="https://github.com/user-attachments/assets/d47f8a3d-4bb1-40b4-9a31-231ccb902f1d" />

SQLi Attack 
<img width="867" height="456" alt="image18" src="https://github.com/user-attachments/assets/795df1b3-12ff-4e18-8f24-250b16a77b86" />
<img width="269" height="187" alt="image" src="https://github.com/user-attachments/assets/d490d1ad-68a2-4fcb-85ac-875aced0444c" />

<img width="299" height="168" alt="image" src="https://github.com/user-attachments/assets/cd4db845-c4d8-4978-a44e-e34e4f7b5752" />




# ⚙️ TASK 1: Conf!
igure ModSecurity

## Step 1: Edit Configuration File

```bash
nano /etc/modsecurity/modsecurity.conf

Change:

SecRuleEngine DetectionOnly

To:

SecRuleEngine On

Step 2: Restart Apache
systemctl restart apache2

🔍 TASK 2: Run Nikto Scan

From Kali:

nikto -h 203.0.113.1 -ask no

📊 TASK 3: Analyze Logs (Pattern Match)
Count matches:
grep "Pattern match" /var/log/apache2/error.log | wc -l

Get last 5 entries:
grep "Pattern match" /var/log/apache2/error.log | tail -5 > /root/last-five.txt
cat /root/last-five.txt

💉 TASK 4: SQL Injection Attack
From Kali:
curl "http://203.0.113.1/?id=1%27%20OR%201=1"

On LAMP:
grep "1=1" /var/log/apache2/error.log > /root/ModS-SQLi.txt
cat /root/ModS-SQLi.txt

⚡ TASK 5: XSS Attack
From Kali:
curl "http://203.0.113.1/?q=<script>alert('RedBlue')</script>"

On LAMP:
grep "RedBlue" /var/log/apache2/error.log > /root/ModS-XSS.txt
cat /root/ModS-XSS.txt

💻 TASK 6: Command Injection
From Kali:
curl "http://203.0.113.1/?cmd=ls%20cat%20/etc/passwd"

On LAMP:
grep "/etc/passwd" /var/log/apache2/error.log | tail -5 > /root/ModS-CI.txt
cat /root/ModS-CI.txt

📂 TASK 7: Directory Traversal
From Kali:
curl "http://203.0.113.1/?file=../../../../etc/passwd"

On LAMP:
grep "../../" /var/log/apache2/error.log | tail -5 > /root/ModS-DT.txt
cat /root/ModS-DT.txt

🧠 Key Findings
ModSecurity successfully:
Detected SQLi attempts
Blocked XSS payloads
Logged command injection patterns
Identified directory traversal attempts
Logs provide:
Severity levels
Rule IDs
Attack signatures
🏁 Conclusion

This lab demonstrated how a WAF:

Protects web applications in real time
Detects malicious payloads
Logs detailed attack information

ModSecurity + OWASP CRS = 🔥 strong defense layer

📚 Skills Gained
WAF configuration
Log analysis
Web attack simulation
Security monitoring
