---
type: tool
subject: shared
tags: [cybersecurity, tool, metasploit, exploitation, penetration-testing]
status: learning
created: 2026-07-02
---

# Metasploit

> 🔧 **Tool Note** — Category: Exploitation Framework | Open Source

---

## 📌 Quick Overview
**What is it?** The world's most used penetration testing framework — provides exploit code, payloads, and post-exploitation tools in a unified platform.

**Official Website:** https://www.metasploit.com/
**License:** Metasploit Framework (BSD, free) / Pro (commercial)
**Used in:** EHPT

## 🎯 What Problem Does It Solve?
- Automates exploitation of known vulnerabilities
- Provides ready-to-use exploit modules for thousands of CVEs
- Generates payloads (reverse shells, Meterpreter)
- Post-exploitation: privilege escalation, persistence, pivoting
- Standardised penetration testing workflow

## 🏗️ Architecture
```
Metasploit Framework
├── Auxiliary     → Scanning, fuzzing, sniffing
├── Exploit       → Attack code for vulnerabilities
├── Payload       → Code that runs after exploitation
├── Encoder       → Obfuscation to evade antivirus
├── NOP           → Padding for exploits
└── Post          → Post-exploitation modules
```

## 💻 Installation
```bash
# Kali Linux — pre-installed
msfconsole    # Launch

# Other Linux — use installer
curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall
chmod 755 msfinstall && ./msfinstall
```

## ⚡ Quick Reference
→ [[Metasploit Commands Cheatsheet]]

## 🔗 Related Notes
- [[Exploitation & Post-Exploitation]]
- [[Ethical Hacking Fundamentals]]
- [[Burp Suite]] — Web application counterpart
