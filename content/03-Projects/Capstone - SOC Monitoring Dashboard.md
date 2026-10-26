---
type: capstone
subject: csf-ns
tags: [cybersecurity, capstone, csf-ns, splunk, soc, dashboard, incident-report]
status: learning
created: 2026-07-02
---

# Capstone: SOC Monitoring Mini-Project

> 🚀 **Capstone Project** — CSF & NS Weeks 11-13 | Est. time: 8 hrs | 30 marks

---

## 🎯 Project Goal
Build a functional Splunk dashboard that a Tier 1 SOC analyst could use to monitor a Windows environment, using faculty-provided log files simulating one week of activity from a company that experienced a security incident.

---

## 📋 Prerequisites
- [ ] Splunk Free installed (localhost:8000)
- [ ] Faculty-provided log files ingested
- [ ] Completed [[Lab - Splunk SOC Alert Triage|Practical 5 – Splunk Alert Triage]]
- [ ] Understanding of [[SOC Operations & Incident Lifecycle|SOC Operations]]

---

## 🏗️ Dashboard Requirements

### Panel 1: Failed Login Attempts Over Time (Timechart)
```spl
index=main EventCode=4625 | timechart span=1h count
```
- Visualisation: Line chart or area chart
- Purpose: Spot brute-force attack windows

### Panel 2: Top 10 Source IPs in Failed Logins (Bar Chart)
```spl
index=main EventCode=4625 | top limit=10 src_ip
```
- Visualisation: Horizontal bar chart
- Purpose: Identify attacker IPs

### Panel 3: New Accounts Created This Week (Table)
```spl
index=main EventCode=4720 | table _time, TargetUserName, SubjectUserName
| sort _time
```
- Visualisation: Table
- Purpose: Detect unauthorised account creation

### Panel 4: Services Installed This Week (Table)
```spl
index=main EventCode=7045 | table _time, ServiceName, ImagePath, ServiceType
| sort _time
```
- Visualisation: Table
- Purpose: Detect malware persistence via service installation

---

## 📝 Incident Report (1 page)

Your report must answer:

| Question | Your Finding |
|----------|-------------|
| **What happened?** | Describe the attack scenario |
| **When?** | Timeline with key timestamps |
| **Which account was compromised?** | Username and evidence |
| **What evidence supports your conclusion?** | SPL queries, event IDs, timestamps |
| **What remediation do you recommend?** | Specific, actionable steps |

### Report Structure
1. **Incident Summary** (2-3 sentences)
2. **Timeline of Events** (chronological with timestamps)
3. **Evidence** (reference dashboard panels and SPL queries)
4. **Root Cause** (how the attacker got in)
5. **Impact Assessment** (what was affected)
6. **Remediation Recommendations** (specific steps)

---

## 📊 Assessment Criteria (30 marks)

| Criteria | Marks | What's Evaluated |
|----------|-------|-----------------|
| Dashboard accuracy & SPL query quality | 15 | Correct queries, meaningful visualisations |
| Incident report | 10 | Completeness, accuracy, evidence-backed conclusions |
| Presentation to faculty | 5 | Clear explanation, ability to answer questions |

---

## ✅ Deliverables Checklist
- [ ] Splunk dashboard with minimum 4 panels
- [ ] All SPL queries documented
- [ ] Dashboard screenshot(s)
- [ ] 1-page Incident Report
- [ ] Presentation prepared

---

## 💡 Tips
- Start by **exploring the data** — run basic searches to understand what's in the logs
- Look for **anomalies** — spikes in failed logins, activity outside business hours
- **Correlate** — connect failed logins → successful login → account creation → service installed
- The incident tells a **story** — your dashboard and report should narrate it

---

## 🔗 Related Notes
- [[Splunk]] | [[Splunk SPL Cheatsheet|SPL Cheatsheet]] ⚡
- [[Windows Event IDs Reference|Windows Event IDs]] ⚡
- [[SOC Operations & Incident Lifecycle|SOC Operations]]
- [[CSF & NS MOC]]
