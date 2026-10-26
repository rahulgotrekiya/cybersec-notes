---
type: capstone
subject: ehpt
tags: [cybersecurity, capstone, ehpt, vapt, report, penetration-testing]
status: learning
created: 2026-07-02
---

# Capstone: Full VAPT Report

> 🚀 **Capstone Project** — EHPT Weeks 11-13 | Est. time: 12 hrs | 30 marks

---

## 🎯 Project Goal
Expand the mini VAPT assessment (Practical 8) into a complete, professionally structured penetration test report for a simulated client engagement.

---

## 📋 Prerequisites
- [ ] Completed all EHPT Practicals 1-8
- [ ] Minimum 6 documented findings ready
- [ ] Understanding of [[VAPT Reporting]]
- [ ] CVSS calculator accessible

---

## 📝 Report Structure

### 1. Cover Page
| Field | Content |
|-------|---------|
| Title | Vulnerability Assessment & Penetration Test Report |
| Client | [Simulated Company Name] |
| Tester | [Your Name] |
| Date | [Testing Date Range] |
| Classification | CONFIDENTIAL |
| Scope | Metasploitable 2 + DVWA |

### 2. Executive Summary (1 page, non-technical)
- What was tested and why
- Overall risk posture: **Critical / High / Medium / Low**
- Number of findings by severity
- Top 3 most critical findings (plain language)
- Key recommendations (3-5 bullet points)

> 💡 Written for management — NO technical jargon. A CEO should understand this page.

### 3. Scope & Methodology
- Systems in scope (IPs, URLs)
- Testing approach (black-box / grey-box)
- Tools used (Nmap, Metasploit, Burp Suite, Nessus, etc.)
- Timeframe
- Limitations and exclusions
- Standards followed (PTES, OWASP)

### 4. Technical Findings (minimum 6 findings)

**Each finding must include ALL of these fields:**

| Field | Content |
|-------|---------|
| **Finding Title** | Clear, descriptive name |
| **Severity** | CVSS v3.1 score + rating (Critical/High/Medium/Low/Info) |
| **Affected System** | IP address, port, service, or URL |
| **Vulnerability Description** | Technical explanation of the vulnerability |
| **Proof of Concept** | Step-by-step reproduction with screenshots |
| **Business Impact** | What could an attacker do? Data loss? Downtime? |
| **Remediation** | Specific fix with implementation guidance |
| **Reference** | CVE number or CWE where applicable |

### 5. Risk Heat Map

```
Impact
  High   │  ●Medium    ●High      ●Critical
         │
  Medium │  ●Low       ●Medium    ●High
         │
  Low    │  ●Info      ●Low       ●Medium
         │
         └────────────────────────────────
              Low       Medium     High
                    Likelihood
```

Plot each finding on the heat map.

### 6. Remediation Priority Table

| Priority | Finding | CVSS | Effort | Recommendation |
|----------|---------|------|--------|---------------|
| 1 | | | | |
| 2 | | | | |
| 3 | | | | |
| 4 | | | | |
| 5 | | | | |
| 6 | | | | |

### 7. Conclusion
- Summary of overall security posture
- Most critical risks
- Recommended next steps
- Offer for re-test after remediation

---

## 📊 Assessment Criteria (30 marks)

| Criteria | Marks | What's Evaluated |
|----------|-------|-----------------|
| Finding quality and evidence | 15 | Accurate descriptions, clear PoC, proper screenshots |
| Report structure and writing | 10 | Professional format, clear language, completeness |
| CVSS accuracy | 5 | Correct CVSS calculations with justification |

---

## ✅ Deliverables Checklist
- [ ] Cover page with all required fields
- [ ] Executive Summary (1 page, non-technical)
- [ ] Scope & Methodology section
- [ ] Minimum 6 technical findings (all fields complete)
- [ ] Risk Heat Map
- [ ] Remediation Priority Table
- [ ] Conclusion
- [ ] All screenshots labelled and referenced

---

## 🔗 Related Notes
- [[VAPT Reporting]] — Report structure guide
- [[CVSS Scoring Reference|CVSS Scoring]] ⚡ — Score calculator
- [[Lab - Mini VAPT Assessment|Practical 8 – Mini VAPT]] — Source findings
- [[EHPT MOC]]
