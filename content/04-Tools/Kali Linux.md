---
type: tool
subject: shared
tags: [cybersecurity, tool, kali-linux, penetration-testing, offensive-security]
status: learning
created: 2026-07-02
---

# Kali Linux

> 🔧 **Tool Note** — Category: Penetration Testing OS | Open Source

---

## 📌 Quick Overview
**What is it?** Debian-based Linux distribution designed for penetration testing and digital forensics, with 600+ security tools pre-installed.

**Official Website:** https://www.kali.org/
**License:** GNU GPL (Free)
**Used in:** EHPT, DFIR

## 🎯 What Problem Does It Solve?
- All security tools in one place — no installation needed
- Pre-configured for offensive security workflows
- Regular updates with latest tool versions
- Available as VM, ISO, Docker, WSL, cloud images

## 🚀 Pre-installed Tools (Key Ones)

| Category | Tools |
|----------|-------|
| **Scanning** | Nmap, Nikto, enum4linux |
| **Exploitation** | Metasploit, Searchsploit |
| **Web Testing** | Burp Suite, SQLmap, Dirb |
| **Password** | John, Hashcat, Hydra |
| **Wireless** | Aircrack-ng, Reaver |
| **Forensics** | Autopsy, Volatility, Binwalk |
| **OSINT** | theHarvester, Maltego, Recon-NG |
| **Sniffing** | Wireshark, tcpdump |

## Lab Setup
```
VirtualBox / VMware
├── Kali Linux (Attacker)        ← Host-only adapter
├── Metasploitable 2 (Target)    ← Host-only adapter
└── DVWA (Web Target)            ← Host-only adapter
```

> ⚠️ **Always** use isolated host-only networks for lab work.

## 🔗 Related Notes
- [[Ethical Hacking Fundamentals]]
- [[Metasploit]], [[Nmap]], [[Burp Suite]], [[Wireshark]]
