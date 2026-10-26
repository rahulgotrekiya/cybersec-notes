---
type: lab
subject: csf-ns
tags: [cybersecurity, lab, csf-ns, nmap, scanning]
status: learning
created: 2026-07-02
---

# Lab: Nmap Network Scanning and Service Enumeration

> 🔬 **Lab Practical** — CSF & NS Practical 2 | Est. time: 4 hrs

---

## 🎯 Objective
Scan Metasploitable 2 using multiple Nmap techniques, build a complete service inventory.

## 📋 Prerequisites
- [ ] Kali Linux or lab workstation
- [ ] Metasploitable 2 on isolated host-only network

## 📝 Procedure
```bash
# 1. Host discovery
nmap -sn 192.168.x.0/24

# 2. TCP SYN scan
sudo nmap -sS 192.168.x.target

# 3. Version detection
nmap -sV 192.168.x.target

# 4. OS fingerprinting
sudo nmap -O 192.168.x.target

# 5. NSE default scripts
nmap --script=default 192.168.x.target

# 6. Save output
nmap -sV -oN scan_results.txt 192.168.x.target
```

## ✅ Expected Output
- [ ] Service inventory table (min 8 rows)
- [ ] nmap -oN output file screenshot
- [ ] OS detection result

## 💭 Observation Questions
1. FTP port 21 is open with vsftpd 2.3.4. How do you check for known vulnerabilities without exploiting?
2. What is the difference between SYN scan (-sS) and connect scan (-sT)? Which leaves more evidence?

## 🔗 Related
- [[Nmap]] | [[Nmap Commands Cheatsheet|Cheatsheet]] ⚡
