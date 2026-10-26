---
type: lab
subject: dfir
tags: [cybersecurity, lab, dfir, splunk, correlation, ir-dashboard]
status: learning
created: 2026-07-02
---

# Lab: Splunk Multi-Source Correlation & IR Dashboard

> 🔬 **Lab Practical** — DFIR Practical 8 | Est. time: 4 hrs

---

## 🎯 Objective
Ingest multiple log sources into Splunk and build an IR dashboard that tells the story of a complete attack scenario.

## 📝 Procedure

### Step 1: Ingest Log Sources
- Security.evtx (Windows event logs)
- firewall.csv (firewall allow/deny logs)
- web_access.log (web server access logs)

### Step 2: Correlate Across Sources

**Find the attacker's IP across all sources:**
```spl
# In Windows logs
index=main EventCode=4625 | top src_ip

# In firewall logs
index=firewall action=blocked | top src_ip

# In web logs
index=web status>=400 | top clientip
```

### Step 3: Build IR Dashboard

**Panel 1: Failed Login Timeline**
```spl
index=main EventCode=4625 | timechart span=5m count by src_ip
```

**Panel 2: Firewall Blocks by IP**
```spl
index=firewall action=blocked | stats count by src_ip | sort -count
```

**Panel 3: Web Attack Attempts**
```spl
index=web (uri="*UNION*" OR uri="*SELECT*" OR uri="*script*") 
| table _time, clientip, uri, status
```

### Step 4: Write Incident Summary
1-page summary with:
- Attack timeline (when did each phase occur?)
- Attacker IP(s) identified
- Attack methods used
- Impact assessment
- Recommended response

## ✅ Expected Output
- [ ] Splunk dashboard with 3 panels
- [ ] SPL queries for each saved report
- [ ] 1-page incident summary with timeline

## 💭 Observation Questions
1. How does correlating across three sources give a more complete picture than any single source?
2. Web log shows SQLi from 10.5.5.20 but firewall blocked that IP 5 minutes earlier. What does this contradiction tell you?

## 🔗 Related Notes
- [[Splunk]] | [[Splunk SPL Cheatsheet|SPL Cheatsheet]] ⚡
- [[SOC Operations & Incident Lifecycle|SOC Operations]]
