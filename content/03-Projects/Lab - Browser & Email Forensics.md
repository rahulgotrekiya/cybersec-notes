---
type: lab
subject: dfir
tags: [cybersecurity, lab, dfir, browser-forensics, email-forensics, phishing]
status: learning
created: 2026-07-02
---

# Lab: Browser & Email Forensics

> 🔬 **Lab Practical** — DFIR Practical 7 | Est. time: 3 hrs

---

## 🎯 Objective
Extract browser forensic artefacts from the forensic image and trace a phishing email to its origin.

## 📝 Procedure

### Part A: Browser Forensics
1. Open forensic image in Autopsy
2. Navigate to Web History module
3. Extract:
   - URLs visited with timestamps
   - Search queries
   - Downloaded files
   - Cached pages

### Part B: Email Forensics
1. Open provided .eml phishing email in text editor
2. Read headers **bottom to top**
3. Trace the hop path:
   - Identify originating IP from bottom Received: header
   - Map each hop
4. Check SPF/DKIM results in headers
5. Determine: Is the From address spoofed?

### Email Hop Path Diagram
```
Originating Server (IP: ???)
    ↓
Relay Server 1
    ↓
Recipient Mail Server
    ↓
Recipient Inbox
```

## ✅ Expected Output
- [ ] Browser history table (min 10 entries)
- [ ] Downloads list
- [ ] Email hop-path diagram
- [ ] Originating IP details (WHOIS)
- [ ] SPF/DKIM analysis
- [ ] Verdict: spoofed or legitimate?

## 💭 Observation Questions
1. Chrome stores timestamps as WebKit format (microseconds since 1 Jan 1601). Convert 13350000000000000 to human-readable date.
2. SPF check result is "softfail". What does this mean? Is it sufficient to block the email?

## 🔗 Related Notes
- [[Network & Email Forensics]]
- [[Autopsy]]
