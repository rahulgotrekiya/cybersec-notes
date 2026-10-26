---
type: lab
subject: ehpt
tags: [cybersecurity, lab, ehpt, osint, reconnaissance]
status: learning
created: 2026-07-02
---

# Lab: Lab Setup & OSINT Reconnaissance

> 🔬 **Lab Practical** — EHPT Practical 1 | Est. time: 3 hrs

---

## 🎯 Objective
Verify lab environment isolation, confirm all tools are functional, and perform passive OSINT reconnaissance on a simulated target.

## 📋 Prerequisites
- [ ] Kali Linux VM running
- [ ] Metasploitable 2 on host-only network
- [ ] DVWA accessible

## 📝 Procedure

### Step 1: Verify Lab Isolation
```bash
# Confirm Kali IP
ip addr show

# Confirm Metasploitable is reachable
ping 192.168.x.target

# Confirm DVWA is accessible
curl http://192.168.x.target/dvwa/

# Verify NO internet access (host-only)
ping 8.8.8.8    # Should fail
```

### Step 2: OSINT with theHarvester
```bash
# Harvest emails and subdomains (simulated target)
theHarvester -d example.com -b google,bing -l 200
```

### Step 3: DNS Reconnaissance
```bash
# Query DNS records
dig example.com A
dig example.com MX
dig example.com NS
dig example.com TXT

# Attempt zone transfer
dig @ns1.example.com example.com AXFR
```

### Step 4: WHOIS Lookup
```bash
whois example.com
```

### Step 5: Document Findings
- Both VM IPs documented
- DVWA working screenshot
- theHarvester output saved
- DNS records table created
- Zone transfer result (success or failure with reason)

## ✅ Expected Output
- [ ] Both VM IPs documented
- [ ] DVWA working screenshot
- [ ] theHarvester output
- [ ] DNS records table
- [ ] Zone transfer result

## 💭 Observation Questions
1. Why does a successful DNS zone transfer represent a security risk?
2. From LinkedIn OSINT results, what information could an attacker use for spear-phishing?

## 🔗 Related Notes
- [[Reconnaissance & Scanning]]
- [[Ethical Hacking Fundamentals]]
- [[Kali Linux]]
