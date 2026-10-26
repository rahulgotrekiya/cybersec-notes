---
type: concept
subject: dfir
tags: [cybersecurity, concept, dfir, concepts, digital-forensics, chain-of-custody, legal, locard, investigation]
status: learning
created: 2026-07-02
---

# Digital Forensics Fundamentals

> 📝 **Concept Note** — DFIR Unit I | Source: MCA Sem III Syllabus

---

## What is Digital Forensics?

**Definition:** The application of scientific methods to identify, collect, preserve, examine, analyse, and present digital evidence in a way that is legally admissible.

### Types of Digital Forensics

| Type | Focus | Examples |
|------|-------|---------|
| **Disk Forensics** | Hard drives, SSDs, USB | Recovering deleted files, timeline analysis |
| **Memory Forensics** | RAM analysis | Detecting running malware, extracting credentials |
| **Network Forensics** | Traffic captures | Reconstructing attacks, finding C2 channels |
| **Mobile Forensics** | Phones, tablets | Call logs, messages, app data |
| **Cloud Forensics** | Cloud infrastructure | AWS/Azure/GCP logs, virtual disk images |
| **Email Forensics** | Email headers and content | Tracing phishing, proving spoofing |

### Goals of a Forensic Investigation
1. **What** happened?
2. **When** did it happen?
3. **Who** did it (or which account/system)?
4. **How** was it done?
5. **What data** was affected?

---

## Locard's Exchange Principle

> **"Every contact leaves a trace."**

In the physical world: A burglar leaves fingerprints, footprints, fibres.
In the digital world: Every action leaves **artefacts** — log entries, timestamps, registry changes, network connections, file metadata.

**Implication:** An attacker **cannot** interact with a system without leaving evidence. The forensic investigator's job is to find, preserve, and interpret that evidence.

---

## The Digital Forensics Investigation Process

```
1. Identification → 2. Preservation → 3. Collection → 4. Examination → 5. Analysis → 6. Reporting
```

### 1. Identification
- Recognise that an incident has occurred
- Identify potential sources of evidence (which systems, which devices)
- Determine scope of investigation

### 2. Preservation
- **Protect evidence from modification** — this is critical
- Use write blockers, create forensic images (not original)
- Maintain chain of custody
- Document everything

### 3. Collection
- Create forensic images (bit-for-bit copies)
- Capture volatile data (RAM) before it's lost
- Record hashes for integrity verification
- Fill out chain of custody forms

### 4. Examination
- Use forensic tools to extract data from images
- Recover deleted files, extract metadata
- Parse structured data (registry, event logs, browser history)

### 5. Analysis
- Interpret findings in context
- Build timelines of events
- Correlate evidence from multiple sources
- Form conclusions supported by evidence

### 6. Reporting
- Document findings in a legally defensible report
- Write for technical AND non-technical audiences
- Include all evidence with proper references

---

## Chain of Custody

### What is it?
A documented, unbroken trail showing **who had possession of evidence, when, and what they did with it.**

### Why it Matters
- **Legal requirement** — Evidence without chain of custody can be **thrown out of court**
- Proves evidence hasn't been tampered with
- Shows continuous accountability

### Chain of Custody Fields

| Field | Purpose |
|-------|---------|
| Case number | Unique identifier |
| Evidence description | What the item is |
| Date/time collected | When evidence was obtained |
| Collected by | Who obtained it |
| Location found | Where it was |
| Hash values (MD5 + SHA-256) | Integrity verification |
| Transfer log | Each person who handled it, with dates/times |
| Storage location | Where evidence is kept |

### Common Pitfalls
- ❌ Booting suspect's computer before imaging → changes timestamps
- ❌ Not recording hash values → can't prove integrity
- ❌ Gaps in custody log → evidence may be challenged
- ❌ Using original evidence for analysis → risk of modification

---

## Legal & Ethical Framework

### IT Act 2000 (India)

| Section | Relevance |
|---------|-----------|
| Section 65B | Electronic records as evidence (certificate required) |
| Section 43 | Unauthorised access penalties |
| Section 66 | Computer-related offences |

### Indian Evidence Act — Electronic Records
- Section 65B certificate required for electronic evidence
- Printouts/copies must be certified as accurate
- Original storage medium should be preserved when possible

### DPDP Act 2023 (Digital Personal Data Protection)
- Regulates how personal data is collected, processed, stored
- Forensic investigators must handle discovered PII carefully
- Breach notification requirements

### Admissibility Standards
- **Daubert Standard:** Is the forensic method scientifically valid and reliable?
- **Frye Standard:** Is the method generally accepted by the forensic community?

---

## Forensic Operating Systems

| OS | Type | Key Feature |
|----|------|-------------|
| **CAINE** | Linux Live CD | Write-blocking built in, forensic tools preinstalled |
| **SIFT** | Ubuntu-based VM | SANS-maintained, comprehensive forensic toolkit |
| **Kali Linux** | Debian-based | Primarily offensive, but includes forensic tools |

---

## Anti-Forensics Awareness

Techniques attackers use to hinder investigation:
- **Log deletion** — Clearing event logs, bash_history
- **Timestamp manipulation** — Changing file modification times
- **Encryption** — Encrypting evidence
- **Steganography** — Hiding data inside images/files
- **Secure deletion** — Overwriting files to prevent recovery
- **Living off the land** — Using legitimate system tools to avoid detection

> 💡 Investigators must be aware of anti-forensics techniques to look for evidence of evidence destruction itself.

---

## Related Concepts

- [[Evidence Acquisition & Imaging]] — Next step: how to collect evidence
- [[Windows & Linux Forensics]] — OS-specific artefacts
- [[Incident Response & Malware Analysis]] — How forensics supports IR
- [[DFIR MOC]] — Subject navigation
