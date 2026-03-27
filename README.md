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

# ⚙️ TASK 1: Configure ModSecurity

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
