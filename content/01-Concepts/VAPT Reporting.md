---
type: concept
subject: ehpt
tags: [cybersecurity, concept, ehpt, concepts, vapt, reporting, cvss, remediation, ethics]
status: learning
created: 2026-07-02
---

# VAPT Reporting

> 📝 **Concept Note** — EHPT Unit V | Source: MCA Sem III Syllabus

---

## Penetration Test Report Structure

A professional VAPT report is the **primary deliverable** — it's what the client pays for.

### Report Sections

#### 1. Cover Page
- Tester name and organisation
- Client name
- Date of testing
- Target systems and scope
- Classification: CONFIDENTIAL

#### 2. Executive Summary (1 page, non-technical)
- What was tested and why
- Overall risk posture (Critical/High/Medium/Low)
- Top 3 most critical findings
- Key recommendations
- Written for **management** — no technical jargon

#### 3. Scope & Methodology
- Systems in scope (IPs, URLs, applications)
- Testing approach (black-box, grey-box, white-box)
- Tools used (Nmap, Metasploit, Burp Suite, etc.)
- Timeframe of testing
- Limitations and exclusions

#### 4. Technical Findings
Each finding includes:

| Field | Content |
|-------|---------|
| **Title** | Clear, descriptive name |
| **Severity** | CVSS v3.1 score + rating |
| **Affected System** | IP, URL, or component |
| **Description** | What the vulnerability is |
| **Proof of Concept** | Step-by-step with screenshots |
| **Business Impact** | What could happen if exploited |
| **Remediation** | How to fix it |
| **Reference** | CVE number or CWE |

#### 5. Risk Heat Map
Plot findings by **likelihood vs impact:**

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

#### 6. Remediation Priority Table

| Priority | Finding | CVSS | Fix |
|----------|---------|------|-----|
| 1 | vsftpd Backdoor (RCE) | 9.8 | Update to latest version |
| 2 | SQL Injection in login | 8.6 | Implement parameterised queries |
| 3 | Stored XSS in comments | 6.1 | Implement output encoding + CSP |

#### 7. Conclusion
Summary of findings and next steps.

---

## CVSS v3.1 Calculation Examples

### Example 1: vsftpd Backdoor (RCE)

| Metric | Value | Justification |
|--------|-------|---------------|
| Attack Vector | Network | Exploitable remotely |
| Attack Complexity | Low | No special conditions needed |
| Privileges Required | None | No authentication needed |
| User Interaction | None | No victim action needed |
| Scope | Unchanged | Same component affected |
| Confidentiality | High | Full file system access |
| Integrity | High | Can modify any file |
| Availability | High | Can shut down service |

**Score: 9.8 (Critical)**

### Example 2: Stored XSS

| Metric | Value | Justification |
|--------|-------|---------------|
| Attack Vector | Network | Via web browser |
| Attack Complexity | Low | Simple payload |
| Privileges Required | Low | Need account to post |
| User Interaction | Required | Victim must view page |
| Scope | Changed | Affects other users' browsers |
| Confidentiality | Low | Cookie theft possible |
| Integrity | Low | Page content modified |
| Availability | None | Service stays up |

**Score: 5.4 (Medium)**

> 🔗 Calculator: [[CVSS Scoring Reference]] ⚡

---

## Remediation Strategies

| Strategy | Application |
|----------|------------|
| **Patching** | Update software to fix known CVEs |
| **Secure Configuration** | Disable defaults, remove unnecessary services |
| **Least Privilege** | Remove excessive permissions |
| **Input Validation** | Sanitise all user input |
| **Output Encoding** | Encode output to prevent XSS |
| **Network Segmentation** | Isolate critical systems |
| **MFA** | Add multi-factor authentication |

---

## Professional Ethics

### Responsible Disclosure
1. Discover vulnerability
2. Report privately to affected organisation
3. Give reasonable time to fix (typically 90 days)
4. Disclose publicly only after fix is available (or deadline passes)

### Bug Bounty Platforms
- **HackerOne** — Largest bug bounty platform
- **Bugcrowd** — Another major platform
- Companies pay researchers for finding vulnerabilities legally

### Absolute Rules
- **Never test without written permission**
- **Protect client data** discovered during testing
- **Don't exfiltrate real data** — demonstrate access, don't steal
- **Report everything** — even findings that seem minor

---

## Certification Pathways

| Certification | Level | Focus |
|--------------|-------|-------|
| **CompTIA Security+** | Entry | Broad security fundamentals |
| **CEH (EC-Council)** | Intermediate | Ethical hacking methodology |
| **eJPT (eLearnSecurity)** | Beginner-Intermediate | Practical pentesting |
| **OSCP (OffSec)** | Advanced | Hands-on exploitation (24hr exam) |

---

## Related Concepts

- [[Ethical Hacking Fundamentals]] — Legal and ethical context
- [[Reconnaissance & Scanning]] — Finding vulnerabilities
- [[Exploitation & Post-Exploitation]] — Proving exploitability
- [[Web Application Security]] — Web vulnerability findings
- [[CVSS Scoring Reference]] ⚡
- [[EHPT MOC]] — Subject navigation
