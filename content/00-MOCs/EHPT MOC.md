---
type: moc
subject: shared
tags: [cybersecurity, moc, ehpt, ethical-hacking, penetration-testing, offensive-security]
status: learning
created: 2026-07-02
---

# Ethical Hacking & Penetration Testing MOC

> 🗺️ **Navigation Hub** — Subject 2: Offensive Security (MCA Sem III)

---

## 🎯 About This Subject

The offensive security subject covering the entire penetration testing lifecycle — from reconnaissance through exploitation to professional report writing. Learn to think like an attacker to defend like a professional.

**Pre-requisites:** CSF & NS completed or concurrent, basic Linux CLI, Kali Linux + Metasploitable 2 + DVWA lab setup

---

## 📚 Core Concepts

### Unit I — Ethical Hacking Foundations
- [[Ethical Hacking Fundamentals]] — Hacker types, legal framework, 5 phases, Kill Chain, Red/Blue/Purple teams

### Unit II — Reconnaissance & Scanning
- [[Reconnaissance & Scanning]] — OSINT, Google Dorking, Nmap, Nessus, CVSS scoring

### Unit III — Exploitation & Post-Exploitation
- [[Exploitation & Post-Exploitation]] — Metasploit, Meterpreter, privilege escalation, password attacks

### Unit IV — Web Application Security
- [[Web Application Security]] — OWASP Top 10, Burp Suite, SQLi, XSS, file upload

**SQL Injection deep dives:**
- [[SQL Essentials for SQL Injection]] — the SQL syntax you must know first
- [[Web & Database Basics for SQL Injection]] — how user input reaches the database
- [[SQL Injection]] — the attack in detail: types, impact, prevention

### Unit V — VAPT Reporting
- [[VAPT Reporting]] — Report structure, CVSS calculation, remediation, ethics, certifications

---

## ⚡ Quick References

- [[Nmap Commands Cheatsheet]] ⚡
- [[Metasploit Commands Cheatsheet]] ⚡
- [[Burp Suite Cheatsheet]] ⚡
- [[OWASP Top 10 Quick Reference]] ⚡
- [[CVSS Scoring Reference]] ⚡
- [[Hash & Encryption Reference]] ⚡

---

## 🔬 Lab Practicals

- [[Lab - OSINT Reconnaissance|Practical 1 – Lab Setup & OSINT Reconnaissance]]
- [[Lab - Vulnerability Assessment with Nessus|Practical 2 – Active Scanning and Vulnerability Assessment]]
- [[Lab - Exploitation with Metasploit|Practical 3 – Exploitation with Metasploit Framework]]
- [[Lab - Linux Privilege Escalation|Practical 4 – Linux Privilege Escalation]]
- [[Lab - SQL Injection on DVWA|Practical 5 – Web App Testing: SQL Injection on DVWA]]
- [[Lab - XSS and File Upload on DVWA|Practical 6 – Web App Testing: XSS and File Upload on DVWA]]
- [[Lab - Password Cracking|Practical 7 – Password Cracking and Hydra Brute Force]]
- [[Lab - Mini VAPT Assessment|Practical 8 – Mini VAPT Assessment]]
- [[Capstone - Full VAPT Report|Capstone – Full VAPT Report]]

---

## 🔧 Tools Used

- [[Kali Linux]] — Primary attack platform
- [[Nmap]] — Network scanning and enumeration
- [[Metasploit]] — Exploitation framework
- [[Burp Suite]] — Web application testing proxy
- [[Nessus]] — Vulnerability scanner
- [[John the Ripper & Hashcat]] — Password cracking
- [[Hydra]] — Online brute force

---

## 📖 Reference Books

1. "Penetration Testing: A Hands-On Introduction to Hacking" — Georgia Weidman, No Starch Press
2. "Metasploit: The Penetration Tester's Guide" — David Kennedy et al., No Starch Press
3. "The Web Application Hacker's Handbook" — Stuttard & Pinto, Wiley
4. "Hacking: The Art of Exploitation" — Jon Erickson, No Starch Press
5. OWASP Testing Guide v4.2 — https://owasp.org

---

## ✅ Coverage Check

> [!tip] Completeness audit
> Lists any EHPT note **not yet linked** in the curated lists above. Empty = fully covered.

<!-- dataview:start -->
<!-- query
LIST WITHOUT ID file.link
FROM "Study/Cyber Security"
WHERE subject = "ehpt" AND type != "moc" AND !contains(this.file.outlinks, file.link)
SORT type ASC, file.name ASC
-->
_No notes match yet._
<!-- dataview:end -->

---

## 🔗 Related MOCs

- [[00-Cyber Security MOC]] — Main Cyber Security hub
- [[CSF & NS MOC]] — Cybersecurity Fundamentals & Network Security
- [[DFIR MOC]] — Digital Forensics & Incident Response
