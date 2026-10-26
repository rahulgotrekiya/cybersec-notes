---
type: concept
subject: ehpt
tags: [cybersecurity, concept, ehpt, concepts, ethical-hacking, kill-chain, penetration-testing, legal-framework]
status: learning
created: 2026-07-02
---

# Ethical Hacking Fundamentals

> 📝 **Concept Note** — EHPT Unit I | Source: MCA Sem III Syllabus

---

## What is Hacking?

**Hacking** is the practice of finding vulnerabilities in systems and exploiting them. The intent behind it determines whether it's ethical or criminal.

---

## Types of Hackers

| Type | Hat Colour | Motivation | Legal Status |
|------|-----------|-----------|-------------|
| **White Hat** | ⬜ | Improve security, authorised testing | Legal (with permission) |
| **Black Hat** | ⬛ | Financial gain, destruction, espionage | Illegal |
| **Grey Hat** | 🔲 | Finds vulns without permission, reports them | Legally questionable |
| **Hacktivist** | — | Political/social cause | Usually illegal |
| **Script Kiddie** | — | Fun, bragging — uses others' tools blindly | Usually illegal |
| **Nation-State** | — | Espionage, sabotage, warfare | State-sponsored |

### How Ethical Hacking Strengthens Security
- Find vulnerabilities **before** criminals do
- Test defences under realistic conditions
- Validate security controls actually work
- Comply with regulatory requirements
- Build confidence in security posture

### Why Companies Pay for Penetration Tests
- Regulatory compliance (PCI-DSS, HIPAA, ISO 27001)
- Insurance requirements
- Customer trust and contractual obligations
- Real-world validation that automated scans can't provide

---

## Legal Framework — IT Act 2000 (India)

| Section | Offence | Relevance |
|---------|---------|-----------|
| **43** | Unauthorised access to computer system | Testing without permission = violation |
| **65** | Tampering with computer source code | Modifying systems without authorisation |
| **66B** | Identity theft | Using stolen credentials |
| **66C** | Cheating by personation using computer | Impersonation attacks |
| **66F** | Cyber terrorism | Attacks on critical infrastructure |

### Rules of Engagement (RoE)
A legal document signed before any penetration test that defines:
- **Scope** — What systems can be tested
- **Out of scope** — What must NOT be touched
- **Testing window** — When testing can occur
- **Contact information** — Who to call if something breaks
- **Acceptable methods** — What techniques are allowed
- **Data handling** — How discovered data is treated

### Critical Rule
> ⚠️ **Testing without written permission is a CRIME.** Even if you find a vulnerability and report it responsibly, unauthorized testing can lead to prosecution.

### Non-Disclosure Agreements (NDAs)
- Tester must not reveal vulnerabilities to anyone except the client
- Test results are confidential
- Discovered data (passwords, PII) must be handled securely

---

## The 5 Phases of Ethical Hacking

```
Phase 1          Phase 2              Phase 3          Phase 4          Phase 5
Reconnaissance → Scanning &     →  Gaining    →  Maintaining  →  Covering
                 Enumeration        Access         Access          Tracks
```

### Phase 1: Reconnaissance
**Gathering information about the target** without directly interacting with it.
- Google Dorking, WHOIS, DNS lookups
- Social media, LinkedIn, job postings
- Passive — no direct contact with target

### Phase 2: Scanning & Enumeration
**Actively probing the target** to discover open ports, services, and vulnerabilities.
- Nmap port scanning, Nessus vulnerability scanning
- Service enumeration (SMB, SMTP, SNMP)
- Active — directly touches the target

### Phase 3: Gaining Access
**Exploiting vulnerabilities** to gain unauthorized access.
- Metasploit exploits, password attacks
- Web application attacks (SQLi, XSS)
- Social engineering

### Phase 4: Maintaining Access
**Ensuring persistent access** even if the system is rebooted or patched.
- Backdoors, rootkits, trojans
- Creating admin accounts
- Scheduled tasks / cron jobs

### Phase 5: Covering Tracks
**Removing evidence** of the intrusion.
- Clearing log files
- Modifying timestamps
- Deleting tools used during attack

> 💡 Ethical hackers document phases 4 and 5 as **findings** in their report. They demonstrate these techniques to show risk but restore systems to original state.

---

## Cyber Kill Chain (Lockheed Martin)

Maps how attackers operate — defenders use it to **detect and interrupt** at each phase:

| Kill Chain Phase | Attacker Action | Defender Response |
|-----------------|----------------|-------------------|
| 1. **Reconnaissance** | Research target | Monitor for OSINT exposure |
| 2. **Weaponisation** | Create exploit/payload | Threat intelligence |
| 3. **Delivery** | Send phishing email / exploit | Email filtering, web proxy |
| 4. **Exploitation** | Trigger vulnerability | Patch management, IPS |
| 5. **Installation** | Install backdoor | Endpoint detection (EDR) |
| 6. **Command & Control** | Establish C2 channel | Network monitoring, DNS filtering |
| 7. **Actions on Objectives** | Steal data, encrypt files | DLP, network segmentation |

> 💡 The earlier you break the chain, the less damage the attacker can do.

---

## Red Team vs Blue Team vs Purple Team

| Team | Role | Focus |
|------|------|-------|
| **Red Team** | Attackers | Simulate real-world attacks, find vulnerabilities |
| **Blue Team** | Defenders | Detect, respond, and prevent attacks |
| **Purple Team** | Collaboration | Red + Blue work together to improve both |

---

## VAPT vs Vulnerability Assessment vs Penetration Test

| Aspect | Vulnerability Assessment | Penetration Test | VAPT |
|--------|------------------------|------------------|------|
| **Approach** | Automated scanning | Manual exploitation | Both combined |
| **Depth** | Broad, surface-level | Deep, targeted | Comprehensive |
| **Exploitation** | No — finds and lists | Yes — proves exploitability | Yes |
| **Output** | List of vulnerabilities | Proof of compromise | Both |
| **Tools** | Nessus, OpenVAS | Metasploit, Burp Suite | All |

---

## Key Standards & Frameworks

### PTES (Penetration Testing Execution Standard)
Defines 7 phases: Pre-engagement → Intelligence Gathering → Threat Modelling → Vulnerability Analysis → Exploitation → Post-Exploitation → Reporting

### OWASP Testing Guide v4.2
Comprehensive guide for web application security testing — covers testing methodology for all OWASP Top 10 categories.

### Lab Environment Overview

```
┌──────────────┐     Host-Only Network     ┌──────────────────┐
│  Kali Linux  │ ←───────────────────────→ │ Metasploitable 2  │
│  (Attacker)  │                            │  (Vulnerable VM)  │
│              │                            │                   │
│  Tools:      │                            │  Services:        │
│  - Nmap      │                            │  - vsftpd 2.3.4   │
│  - Metasploit│ ←─────────────────────→   │  - SSH, Telnet    │
│  - Burp Suite│     ┌────────────┐        │  - Samba, MySQL   │
│  - Nessus    │     │   DVWA     │        │  - Apache, etc.   │
│  - Hydra     │     │ (Web App)  │        └──────────────────┘
└──────────────┘     └────────────┘
```

> ⚠️ **ALWAYS** use isolated host-only networks for lab work. Never scan networks you don't own.

---

## Related Concepts

- [[What is Cybersecurity]] — The defensive context
- [[Reconnaissance & Scanning]] — Phase 1 & 2 deep dive
- [[Exploitation & Post-Exploitation]] — Phase 3 & 4 deep dive
- [[Web Application Security]] — Web-specific attacks
- [[VAPT Reporting]] — Phase 5 — documenting findings
- [[EHPT MOC]] — Subject navigation
