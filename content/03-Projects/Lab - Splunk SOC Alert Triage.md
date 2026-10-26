---
type: lab
subject: csf-ns
tags: [cybersecurity, lab, csf-ns, splunk, soc, alert-triage]
status: learning
created: 2026-07-02
---

# Lab: Splunk — Log Ingestion and SOC Alert Triage

> 🔬 **Lab Practical** — CSF & NS Practical 5 | Est. time: 4 hrs

---

## 🎯 Objective
Ingest Windows Security event logs into Splunk Free, write SPL queries to detect brute force and account creation, triage a simulated alert.

## 📝 SPL Queries to Write
```spl
# 1. Failed logins count
index=main EventCode=4625 | stats count

# 2. Failed logins by source IP
index=main EventCode=4625 | stats count by src_ip | sort -count

# 3. Timeline of failed logins
index=main EventCode=4625 | timechart count

# 4. New accounts created
index=main EventCode=4720 | table _time, TargetUserName

# 5. Successful login after failures
index=main EventCode=4624 src_ip="<suspicious_ip>"
```

## 💭 Observation Questions
1. Event ID 4625 with Logon Type 3 vs Type 10 — what's the difference in attack method?
2. If confirmed brute-force true positive, what immediate containment steps should Tier 1 take?

## 🔗 Related
- [[Splunk]] | [[Splunk SPL Cheatsheet|SPL Cheatsheet]] ⚡
