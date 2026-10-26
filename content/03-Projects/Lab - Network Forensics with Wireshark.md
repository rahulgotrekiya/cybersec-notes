---
type: lab
subject: dfir
tags: [cybersecurity, lab, dfir, wireshark, network-forensics, c2]
status: learning
created: 2026-07-02
---

# Lab: Network Forensics with Wireshark

> 🔬 **Lab Practical** — DFIR Practical 5 | Est. time: 4 hrs

---

## 🎯 Objective
Analyse a faculty-provided malicious traffic capture to identify attack type, extract credentials, detect beaconing, and build an attack timeline.

## 📝 Procedure

### Step 1: Extract FTP Credentials
```
Filter: ftp
Look for USER and PASS commands in cleartext
```

### Step 2: Find SQLi Payload
```
Filter: http.request.method == "POST"
Follow HTTP Stream → Look for SQL keywords in POST body
```

### Step 3: Detect DNS Anomalies
```
Filter: dns
Look for: unusually long subdomain names (DNS tunnelling), high query volume to single domain
```

### Step 4: Identify C2 Beaconing
```
Filter: ip.dst == <suspicious_ip>
Statistics → IO Graphs → Look for regular interval pattern
Regular 60-second intervals = beaconing
```

### Step 5: Build Attack Timeline

| # | Timestamp | Source IP | Dest IP | Protocol | Event |
|---|-----------|----------|---------|----------|-------|
| 1 | | | | FTP | Credentials stolen |
| 2 | | | | HTTP | SQLi attempt |
| 3 | | | | DNS | Tunnelling detected |
| 4 | | | | TCP | C2 beaconing starts |
| 5 | | | | TCP | Data exfiltration |
| 6 | | | | | |

## ✅ Expected Output
- [ ] FTP credentials screenshot
- [ ] SQLi payload in POST body
- [ ] DNS anomaly screenshot
- [ ] Beaconing pattern screenshot (regular intervals)
- [ ] Attack timeline with 6 events

## 💭 Observation Questions
1. C2 beaconing interval is exactly 60 seconds. How could an attacker make this harder to detect with jitter?
2. DNS tunnelling used subdomains like `dGhpcyBpcyBhIHRlc3Q=.evil.com`. What encoding is this and what does it contain?

## 🔗 Related Notes
- [[Wireshark]]
- [[Network & Email Forensics]]
