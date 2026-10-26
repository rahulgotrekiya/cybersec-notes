---
type: moc
subject: shared
tags: [cybersecurity, moc, csf-ns, network-security, soc, foundation]
status: learning
created: 2026-07-02
---

# Cybersecurity Fundamentals & Network Security MOC

> 🗺️ **Navigation Hub** — Subject 1: Foundation of Cyber Security (MCA Sem III)

---

## 🎯 About This Subject

The foundational subject that covers cybersecurity principles, networking from a security perspective, network defence mechanisms, traffic analysis tools, and Security Operations Center (SOC) basics. **No prior cybersecurity knowledge required.**

**Pre-requisites:** Basic computer literacy, internet familiarity, MCA Sem I & II completed

---

## 📚 Core Concepts

### Unit I — Cybersecurity Foundations
- [[What is Cybersecurity]] — Definition, scope, CIA Triad, attack types
- [[CIA Triad]] — Confidentiality, Integrity, Availability deep dive

### Unit II — Networking for Security
- [[Networking Fundamentals]] — Devices, topologies, communication types, OSI & TCP/IP models
- [[OSI Model & TCP-IP Security]] — Security attack mapping per OSI layer, IPv4/IPv6, DNS, ports

### Unit III — Network Defence
- [[Network Defence Mechanisms]] — **Unit III hub** (overview + links to all deep dives)

**Deep dives — how each mechanism actually works:**
- [[Firewalls]] — Stateful connection table, DPI/NGFW, rule order, egress filtering
- [[IDS & IPS]] — Detection pipeline, SPAN vs inline, signature vs anomaly, tuning
- [[DMZ & Network Segmentation]] — DMZ designs, VLANs/802.1Q, lateral movement, micro-segmentation
- [[VPN]] — IPsec IKE phases, ESP vs AH, tunnel vs transport, split tunnelling
- [[Proxy Servers]] — Forward vs reverse, transparent vs explicit, TLS interception
- [[Zero Trust Architecture]] — NIST PE/PA/PEP model, continuous verification, ZTNA vs VPN
- [[Wireless Security]] — WPA2 4-way handshake, WPA3 SAE, evil twin, deauth, KRACK

### Unit IV — Traffic Analysis Tools
- [[Network Traffic Analysis]] — Wireshark, Nmap, Splunk SIEM basics

### Unit V — SOC Operations
- [[SOC Operations & Incident Lifecycle]] — SOC tiers, log analysis, NIST IR phases

---

## ⚡ Quick References

- [[Wireshark Commands Cheatsheet]] ⚡
- [[Nmap Commands Cheatsheet]] ⚡
- [[Splunk SPL Cheatsheet]] ⚡
- [[Common Ports & Protocols Reference]] ⚡
- [[Windows Event IDs Reference]] ⚡
- [[OWASP Top 10 Quick Reference|OWASP Top 10 : 2025]] ⚡

---

## 🔬 Lab Practicals

- [[Lab - Wireshark Live Traffic Capture|Practical 1 – Wireshark: Capturing and Filtering Live Traffic]]
- [[Lab - Nmap Network Scanning|Practical 2 – Nmap: Network Scanning and Service Enumeration]]
- [[Lab - Malicious PCAP Analysis|Practical 3 – Analysing a Malicious PCAP File]]
- [[Lab - Firewall Rule Analysis|Practical 4 – Firewall Rule Analysis and Network Topology Design]]
- [[Lab - Splunk SOC Alert Triage|Practical 5 – Splunk: Log Ingestion and SOC Alert Triage]]
- [[Lab - Suspicious Traffic Patterns|Practical 6 – Identifying Suspicious Network Traffic Patterns]]
- [[Capstone - SOC Monitoring Dashboard|Capstone – SOC Monitoring Mini-Project]]

---

## 🔧 Tools Used

- [[Wireshark]] — Packet capture and analysis
- [[Nmap]] — Network scanning and enumeration
- [[Splunk]] — SIEM platform for log analysis
- [[Snort]] — Signature-based IDS/IPS (Unit III)
- [[Suricata]] — Multi-threaded IDS/IPS/NSM (Unit III)
- [[Shodan]] — Internet-wide device search / passive recon (OSINT)
- [[netstat & ss]] — Local socket/connection inspection (Practical 6)
- [[VirusTotal]] — Multi-engine IoC reputation lookup (Practical 6)

---

## 📖 Reference Books

1. "CompTIA Security+ Study Guide" — Mike Chapple & David Seidl, Sybex
2. "Security Operations Center" — Joseph Muniz et al.
3. "Network Security Essentials" — William Stallings, Pearson
4. "The Practice of Network Security Monitoring" — Richard Bejtlich, No Starch Press
5. Splunk Documentation — https://docs.splunk.com
6. Wireshark User Guide — https://www.wireshark.org/docs/

## 🌐 Web Resources

- TryHackMe Pre-Security Path — https://tryhackme.com/path/outline/presecurity
- Cisco Networking Academy — https://skillsforall.com
- Google Cybersecurity Certificate — Coursera
- Cybrary SOC Analyst course — https://www.cybrary.it

---

## ✅ Coverage Check

> [!tip] Completeness audit
> Lists any CSF & NS note **not yet linked** in the curated lists above. Empty = fully covered.

<!-- dataview:start -->
<!-- query
LIST WITHOUT ID file.link
FROM "Study/Cyber Security"
WHERE subject = "csf-ns" AND type != "moc" AND !contains(this.file.outlinks, file.link)
SORT type ASC, file.name ASC
-->
_No notes match yet._
<!-- dataview:end -->

---

## 🔗 Related MOCs

- [[00-Cyber Security MOC]] — Main Cyber Security hub
- [[EHPT MOC]] — Ethical Hacking & Penetration Testing
- [[DFIR MOC]] — Digital Forensics & Incident Response
