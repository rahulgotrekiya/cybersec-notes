---
type: lab
subject: dfir
tags: [cybersecurity, lab, dfir, splunk, event-logs, brute-force]
status: learning
created: 2026-07-02
---

# Lab: Windows Event Log Analysis with Splunk

> 🔬 **Lab Practical** — DFIR Practical 4 | Est. time: 4 hrs

---

## 🎯 Objective
Ingest Windows event logs into Splunk, write SPL queries to identify brute-force attack, successful compromise, and new account creation.

## 📝 SPL Queries

### Query 1: Total Failed Logins
```spl
index=main EventCode=4625 | stats count
```

### Query 2: Failed Logins by Source IP
```spl
index=main EventCode=4625 | stats count by src_ip | sort -count
```

### Query 3: Brute Force Timeline
```spl
index=main EventCode=4625 | timechart span=5m count by src_ip
```

### Query 4: Successful Login After Failures
```spl
index=main (EventCode=4624 OR EventCode=4625) src_ip="<suspicious_ip>" 
| table _time, EventCode, TargetUserName, LogonType
| sort _time
```

### Query 5: New Accounts Created
```spl
index=main EventCode=4720 | table _time, TargetUserName, SubjectUserName
```

## ✅ Expected Output
- [ ] All 5 SPL query screenshots with results
- [ ] Brute-force timeline chart
- [ ] 1-paragraph triage note with recommended next step

## 💭 Observation Questions
1. 847 failed logins in 3 minutes from IP 10.0.0.55 targeting Administrator. Write the Splunk alert rule.
2. Event 4624 Logon Type 3 from a workstation IP using Administrator at 03:00 AM. What does this indicate?

## 🔗 Related Notes
- [[Splunk]] | [[Splunk SPL Cheatsheet|SPL Cheatsheet]] ⚡
- [[Windows Event IDs Reference|Event IDs]] ⚡
