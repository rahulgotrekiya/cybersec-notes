---
type: moc
subject: shared
tags: [cybersecurity, moc, dfir, digital-forensics, incident-response, investigation]
status: learning
created: 2026-07-02
---

# Digital Forensics & Incident Response MOC

> 🗺️ **Navigation Hub** — Subject 3: Investigation & Response (MCA Sem III)

---

## 🎯 About This Subject

The investigative subject that teaches you to collect, preserve, analyse, and present digital evidence in a legally defensible manner — plus how to respond to security incidents using the NIST framework.

**Pre-requisites:** CSF & NS completed or concurrent, basic computer hardware and file system knowledge, FTK Imager + Autopsy + Volatility 3 + Wireshark + Splunk Free lab setup

---

## 📚 Core Concepts

### Unit I — Digital Forensics Foundations
- [[Digital Forensics Fundamentals]] — Types of forensics, Locard's Principle, legal framework, investigation process
- [[Cyber Law & Legal Framework]] — IPC, CrPC S.91/93, IEA S.65B, IT Act S.65/66/79, DPDP Act 2023

### Unit II — Evidence Acquisition
- [[Evidence Acquisition & Imaging]] — Volatile vs non-volatile, imaging, hashing, deleted file recovery, RAM capture

### Unit III — OS Forensics
- [[Windows & Linux Forensics]] — NTFS, MFT, registry hives, event logs, browser artifacts, Linux logs

### Unit IV — Network & Mobile Forensics
- [[Network & Email Forensics]] — Packet capture, NetFlow, email headers, cloud forensics, mobile forensics

### Unit V — Incident Response
- [[Incident Response & Malware Analysis]] — NIST IR lifecycle, malware analysis, forensic reporting, emerging trends

---

## ⚡ Quick References

- [[Forensic Artifacts Reference]] ⚡
- [[Windows Event IDs Reference]] ⚡
- [[Volatility Commands Cheatsheet]] ⚡
- [[Splunk SPL Cheatsheet]] ⚡
- [[Wireshark Commands Cheatsheet]] ⚡
- [[Hash & Encryption Reference]] ⚡

---

## 🔬 Lab Practicals

- [[Lab - Evidence Acquisition with FTK Imager|Practical 1 – Evidence Acquisition with FTK Imager]]
- [[Lab - Autopsy Deleted File Recovery|Practical 2 – Autopsy: Deleted File Recovery & Activity Timeline]]
- [[Lab - Windows Registry Forensics|Practical 3 – Windows Registry Forensic Analysis]]
- [[Lab - Windows Event Log Analysis|Practical 4 – Windows Event Log Analysis with Splunk]]
- [[Lab - Network Forensics with Wireshark|Practical 5 – Network Forensics with Wireshark]]
- [[Lab - Memory Forensics with Volatility|Practical 6 – Memory Forensics with Volatility 3]]
- [[Lab - Browser & Email Forensics|Practical 7 – Browser & Email Forensics]]
- [[Lab - Splunk Multi-Source Correlation|Practical 8 – Splunk Multi-Source Correlation & IR Dashboard]]
- [[Capstone - Full Forensic Investigation|Capstone – Full Forensic Investigation Report]]

---

## 🔧 Tools Used

- [[FTK Imager]] — Evidence acquisition and imaging
- [[Autopsy]] — Disk forensic analysis
- [[Volatility]] — Memory forensics
- [[Wireshark]] — Network forensics
- [[Splunk]] — Log correlation and SIEM

---

## 📖 Reference Books

1. "Digital Forensics and Incident Response" — Gerard Johansen, Packt Publishing
2. "The Art of Memory Forensics" — Hale Ligh et al., Wiley
3. "Windows Forensic Analysis Toolkit" — Harlan Carvey, Syngress
4. "Incident Response & Computer Forensics" — Luttgens, Pepe, Mandia, McGraw-Hill
5. "Practical Malware Analysis" — Sikorski & Honig, No Starch Press

---

## ✅ Coverage Check

> [!tip] Completeness audit
> Lists any DFIR note **not yet linked** in the curated lists above. Empty = fully covered.

<!-- dataview:start -->
<!-- query
LIST WITHOUT ID file.link
FROM "Study/Cyber Security"
WHERE subject = "dfir" AND type != "moc" AND !contains(this.file.outlinks, file.link)
SORT type ASC, file.name ASC
-->
_No notes match yet._
<!-- dataview:end -->

---

## 🔗 Related MOCs

- [[00-Cyber Security MOC]] — Main Cyber Security hub
- [[CSF & NS MOC]] — Cybersecurity Fundamentals & Network Security
- [[EHPT MOC]] — Ethical Hacking & Penetration Testing
