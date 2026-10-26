---
type: tool
subject: shared
tags: [cybersecurity, tool, john-the-ripper, hashcat, password-cracking]
status: learning
created: 2026-07-02
---

# John the Ripper & Hashcat

> 🔧 **Tool Note** — Category: Password Cracking | Open Source

---

## 📌 Quick Overview
Two complementary password cracking tools:
- **John the Ripper:** CPU-based, auto-detects hash types, great for quick attacks
- **Hashcat:** GPU-accelerated, fastest cracker available, requires specifying hash mode

**Used in:** EHPT

## John the Ripper
```bash
# Dictionary attack (auto-detect hash)
john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt

# Specify format
john --format=raw-md5 --wordlist=rockyou.txt hashes.txt

# Show cracked passwords
john --show hashes.txt
```

## Hashcat
```bash
# MD5 (mode 0)
hashcat -m 0 hashes.txt rockyou.txt

# NTLM (mode 1000)
hashcat -m 1000 hashes.txt rockyou.txt

# SHA-256 (mode 1400)
hashcat -m 1400 hashes.txt rockyou.txt

# Show results
hashcat -m 0 hashes.txt --show
```

## ⚡ Quick Reference
→ [[Hash & Encryption Reference]]

## 🔗 Related Notes
- [[Exploitation & Post-Exploitation|Exploitation]]
- [[Hydra]] — Online brute force (complements offline cracking)
