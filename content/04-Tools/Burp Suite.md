---
type: tool
subject: shared
tags: [cybersecurity, tool, burp-suite, web-security, proxy, penetration-testing]
status: learning
created: 2026-07-02
---

# Burp Suite

> 🔧 **Tool Note** — Category: Web Application Testing | Free Community Edition

---

## 📌 Quick Overview
**What is it?** An integrated platform for web application security testing — intercepts, modifies, and analyses HTTP/S traffic between browser and server.

**Official Website:** https://portswigger.net/burp
**License:** Community (Free) / Professional / Enterprise
**Used in:** EHPT

## 🎯 What Problem Does It Solve?
- Intercept and modify web requests in real-time
- Test for SQL injection, XSS, CSRF, and other web vulnerabilities
- Automate credential stuffing and fuzzing attacks
- Analyse application behaviour and responses

## 🚀 Key Components
| Component | Purpose |
|-----------|---------|
| **Proxy** | Intercept browser↔server traffic |
| **Repeater** | Manually modify and resend requests |
| **Intruder** | Automated attack (brute force, fuzzing) |
| **Decoder** | Encode/decode data (Base64, URL, HTML) |
| **Target** | Map application structure |

## 💻 Setup
1. Launch Burp Suite → Temporary Project
2. Configure browser: `127.0.0.1:8080` proxy (FoxyProxy recommended)
3. Import Burp CA certificate for HTTPS interception

## ⚡ Quick Reference
→ [[Burp Suite Cheatsheet]]

## 🔗 Related Notes
- [[Web Application Security]]
- [[Exploitation & Post-Exploitation|Exploitation]]
- [[Metasploit]] — System-level exploitation counterpart
