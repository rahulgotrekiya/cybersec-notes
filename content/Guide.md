---
type: note
subject: shared
tags: [cybersecurity, note]
status: learning
created: 2026-07-02
---

# 📖 Complete Cyber Security Note Structure Guide

## 📁 Directory Structure Summary

```
Cyber Security/
│
├── 📁 00-MOCs/                    ← Your navigation hubs
│   ├── 00-Cyber Security MOC.md
│   ├── CSF & NS MOC.md
│   ├── EHPT MOC.md
│   └── DFIR MOC.md
│
├── 📁 01-Concepts/                ← Deep explanations
│   ├── What is Cybersecurity.md
│   ├── CIA Triad.md
│   ├── Networking Fundamentals.md
│   ├── OSI Model & TCP-IP Security.md
│   ├── Network Defence Mechanisms.md
│   ├── Network Traffic Analysis.md
│   ├── SOC Operations & Incident Lifecycle.md
│   ├── Ethical Hacking Fundamentals.md
│   ├── Reconnaissance & Scanning.md
│   ├── Exploitation & Post-Exploitation.md
│   ├── Web Application Security.md
│   ├── VAPT Reporting.md
│   ├── Digital Forensics Fundamentals.md
│   ├── Cyber Law & Legal Framework.md
│   ├── Evidence Acquisition & Imaging.md
│   ├── Windows & Linux Forensics.md
│   ├── Network & Email Forensics.md
│   └── Incident Response & Malware Analysis.md
│
├── 📁 02-Quick-References/        ← Fast lookups (⚡)
│   ├── Wireshark Commands Cheatsheet.md
│   ├── Nmap Commands Cheatsheet.md
│   ├── Splunk SPL Cheatsheet.md
│   ├── Metasploit Commands Cheatsheet.md
│   ├── Burp Suite Cheatsheet.md
│   ├── Common Ports & Protocols Reference.md
│   ├── Windows Event IDs Reference.md
│   ├── OWASP Top 10 Quick Reference.md
│   ├── CVSS Scoring Reference.md
│   ├── Forensic Artifacts Reference.md
│   ├── Hash & Encryption Reference.md
│   └── Volatility Commands Cheatsheet.md
│
├── 📁 03-Projects/                ← Hands-on practice (Labs)
│   ├── Lab - Wireshark Live Traffic Capture.md
│   ├── Lab - Nmap Network Scanning.md
│   ├── Lab - Malicious PCAP Analysis.md
│   ├── Lab - Firewall Rule Analysis.md
│   ├── Lab - Splunk SOC Alert Triage.md
│   ├── Lab - Suspicious Traffic Patterns.md
│   ├── Lab - OSINT Reconnaissance.md
│   ├── Lab - Vulnerability Assessment with Nessus.md
│   ├── Lab - Exploitation with Metasploit.md
│   ├── Lab - Linux Privilege Escalation.md
│   ├── Lab - SQL Injection on DVWA.md
│   ├── Lab - XSS and File Upload on DVWA.md
│   ├── Lab - Password Cracking.md
│   ├── Lab - Mini VAPT Assessment.md
│   ├── Lab - Evidence Acquisition with FTK Imager.md
│   ├── Lab - Autopsy Deleted File Recovery.md
│   ├── Lab - Windows Registry Forensics.md
│   ├── Lab - Windows Event Log Analysis.md
│   ├── Lab - Network Forensics with Wireshark.md
│   ├── Lab - Memory Forensics with Volatility.md
│   ├── Lab - Browser & Email Forensics.md
│   ├── Lab - Splunk Multi-Source Correlation.md
│   ├── Capstone - SOC Monitoring Dashboard.md
│   ├── Capstone - Full VAPT Report.md
│   └── Capstone - Full Forensic Investigation.md
│
├── 📁 04-Tools/                   ← Specific tool docs
│   ├── Wireshark.md
│   ├── Nmap.md
│   ├── Splunk.md
│   ├── Metasploit.md
│   ├── Burp Suite.md
│   ├── Nessus.md
│   ├── FTK Imager.md
│   ├── Autopsy.md
│   ├── Volatility.md
│   ├── John the Ripper & Hashcat.md
│   ├── Hydra.md
│   └── Kali Linux.md
│
└── Guide.md                       ← This file
```

---

## 🎯 Three Subjects Covered

This Cyber Security section covers **three MCA Semester III subjects**:

### 1. Cybersecurity Fundamentals & Network Security (CSF & NS)
- Foundation subject — no prior cybersecurity knowledge required
- CIA Triad, attack types, OSI/TCP-IP security layers
- Firewalls, IDS/IPS, VPNs, Zero Trust
- Wireshark, Nmap, Splunk basics
- SOC operations and incident lifecycle

### 2. Ethical Hacking & Penetration Testing (EHPT)
- Prerequisite: CSF & NS completed or concurrent
- Reconnaissance, scanning, exploitation
- Metasploit, Burp Suite, DVWA
- OWASP Top 10, web app security
- VAPT report writing

### 3. Digital Forensics & Incident Response (DFIR)
- Prerequisite: CSF & NS completed or concurrent
- Digital evidence, chain of custody, legal framework
- FTK Imager, Autopsy, Volatility
- Windows/Linux/Network forensics
- Incident response lifecycle, forensic reporting

---

## 📚 Suggested Study Order

```
Start Here
    ↓
CSF & NS (Foundation)
    ↓
EHPT (Offensive Security) ←→ DFIR (Defensive/Investigative)
    ↓                              ↓
Capstone VAPT Report     Capstone Forensic Investigation
```

---

## 🔗 Linking Strategy

### Internal Links
```markdown
# Use double brackets for links
[[Note Name]]

# With custom text
[[../directory/Note Name|Custom Display Text]]

# Link to specific heading
[[Note Name#Heading Name]]
```

### Cross-Subject Linking
Many topics overlap between the three subjects:
- **Wireshark** appears in CSF & NS, EHPT, and DFIR
- **Splunk** appears in CSF & NS and DFIR
- **Nmap** appears in CSF & NS and EHPT
- **Incident lifecycle** spans CSF & NS and DFIR

Link freely between subjects — this is the power of Obsidian!

---

## 🏷️ Tagging System

**By Type:**
- `#moc` — Map of Content
- `#concept` — Concept note
- `#cheatsheet` — Quick reference
- `#lab` — Lab practical / hands-on
- `#tool` — Tool-specific note
- `#capstone` — Capstone project

**By Subject:**
- `#cybersecurity` — All subjects
- `#csf-ns` — Cybersecurity Fundamentals & Network Security
- `#ehpt` — Ethical Hacking & Penetration Testing
- `#dfir` — Digital Forensics & Incident Response

**By Domain:**
- `#network-security` — Firewalls, IDS, VPN
- `#soc` — SOC operations
- `#offensive-security` — Pentesting, exploitation
- `#web-security` — OWASP, SQLi, XSS
- `#forensics` — Digital forensics
- `#incident-response` — IR lifecycle

**By Status:**
- `#learning` — Currently learning
- `#practicing` — Actively practicing
- `#review` — Needs review

---

## 🚀 You're Ready!

You now have:
- ✅ Complete folder structure for all 3 Cyber Security subjects
- ✅ MOCs for navigation
- ✅ Concept notes covering every syllabus unit
- ✅ Quick reference cheatsheets for all tools
- ✅ Lab practicals matching your syllabus
- ✅ Tool documentation
- ✅ Cross-linked knowledge graph

**Next step:** Start with the CSF & NS concepts — they're the foundation for everything else!
