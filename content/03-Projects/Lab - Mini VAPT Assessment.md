---
type: lab
subject: ehpt
tags: [cybersecurity, lab, ehpt, vapt, penetration-testing, assessment]
status: learning
created: 2026-07-02
---

# Lab: Mini VAPT Assessment

> 🔬 **Lab Practical** — EHPT Practical 8 | Est. time: 5 hrs

---

## 🎯 Objective
Conduct a mini end-to-end penetration test against Metasploitable 2 following the 5-phase methodology; document findings for capstone report.

## 📝 Procedure

### Phase 1: Reconnaissance
```bash
# Network discovery
nmap -sn 192.168.x.0/24

# Identify target services
nmap -sV -O 192.168.x.target
```

### Phase 2: Scanning & Enumeration
```bash
# Full port scan with scripts
sudo nmap -sS -sV -O -p- --script=vuln -T4 -oA mini_vapt 192.168.x.target

# SMB enumeration
enum4linux -a 192.168.x.target
```

### Phase 3: Exploitation
- Use findings from Practicals 3-6
- Exploit at least 3 different vulnerabilities
- Document each with screenshots

### Phase 4: Post-Exploitation
- Demonstrate privilege escalation
- Extract credentials
- Show lateral movement potential

### Phase 5: Documentation

**Finding Template:**

| Field | Content |
|-------|---------|
| Title | |
| Severity | CVSS score + rating |
| Affected System | IP/Port/Service |
| Description | What the vulnerability is |
| PoC / Evidence | Screenshot |
| Business Impact | What could happen |
| Remediation | How to fix |
| CVE Reference | If applicable |

## ✅ Expected Output
- [ ] Completed finding sheet with 5 documented vulnerabilities
- [ ] All screenshots labelled
- [ ] CVSS scores calculated
- [ ] Ready for capstone expansion

## 💭 Observation Questions
1. You found 12 vulnerabilities. Client can fix only 3 this month. How do you prioritise?
2. What is the difference between grey-box and black-box testing? When would you choose each?

## 🔗 Related Notes
- [[VAPT Reporting]]
- [[CVSS Scoring Reference|CVSS Scoring]] ⚡
