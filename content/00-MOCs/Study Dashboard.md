---
type: moc
subject: shared
tags: [cybersecurity, dashboard]
status: done
created: 2026-08-17
---

# 📊 Study Dashboard

> 🗺️ Overview of the Cyber Security vault. Every table below is generated from note frontmatter (`status`, `subject`, `created`) by `tools/expand-dataview.mjs` — re-run it to refresh.
> Related: [[00-Cyber Security MOC]] · [[CSF & NS MOC]] · [[EHPT MOC]] · [[DFIR MOC]]

---

## 🎯 Progress by Subject

<!-- dataview:start -->
<!-- query
TABLE WITHOUT ID
  key AS "Subject",
  length(filter(rows, (r) => r.status = "learning")) AS "📖 Learning",
  length(filter(rows, (r) => r.status = "practicing")) AS "🛠️ Practicing",
  length(filter(rows, (r) => r.status = "review")) AS "🔁 Review",
  length(filter(rows, (r) => r.status = "done")) AS "✅ Done",
  length(rows) AS "Total"
FROM "Study/Cyber Security"
WHERE type != "moc"
GROUP BY subject
-->
| Subject | 📖 Learning | 🛠️ Practicing | 🔁 Review | ✅ Done | Total |
| --- | --- | --- | --- | --- | --- |
| Cybersecurity Fundamentals & Network Security | 17 | 0 | 0 | 4 | 21 |
| Digital Forensics & Incident Response | 15 | 0 | 0 | 0 | 15 |
| Ethical Hacking & Penetration Testing | 15 | 0 | 2 | 0 | 17 |
| Shared / Cross-cutting | 29 | 0 | 0 | 0 | 29 |
<!-- dataview:end -->

## 🗂️ Notes by Type

<!-- dataview:start -->
<!-- query
TABLE WITHOUT ID key AS "Type", length(rows) AS "Count"
FROM "Study/Cyber Security"
WHERE type != "moc"
GROUP BY type
-->
| Type | Count |
| --- | --- |
| Capstones | 3 |
| Concepts | 28 |
| Labs | 22 |
| Quick References | 12 |
| Tools | 17 |
<!-- dataview:end -->

---

## 📖 Currently Learning

<!-- dataview:start -->
<!-- query
LIST WITHOUT ID link(file.link, file.name)
FROM "Study/Cyber Security"
WHERE status = "learning" AND type != "moc"
SORT file.name ASC
GROUP BY subject
-->
**Cybersecurity Fundamentals & Network Security**

- [[Capstone - SOC Monitoring Dashboard]]
- [[DMZ & Network Segmentation]]
- [[Firewalls]]
- [[IDS & IPS]]
- [[Lab - Firewall Rule Analysis]]
- [[Lab - Malicious PCAP Analysis]]
- [[Lab - Nmap Network Scanning]]
- [[Lab - Splunk SOC Alert Triage]]
- [[Lab - Suspicious Traffic Patterns]]
- [[Lab - Wireshark Live Traffic Capture]]
- [[Network Defence Mechanisms]]
- [[Network Traffic Analysis]]
- [[Proxy Servers]]
- [[SOC Operations & Incident Lifecycle]]
- [[VPN]]
- [[Wireless Security]]
- [[Zero Trust Architecture]]

**Digital Forensics & Incident Response**

- [[Capstone - Full Forensic Investigation]]
- [[Cyber Law & Legal Framework]]
- [[Digital Forensics Fundamentals]]
- [[Evidence Acquisition & Imaging]]
- [[Incident Response & Malware Analysis]]
- [[Lab - Autopsy Deleted File Recovery]]
- [[Lab - Browser & Email Forensics]]
- [[Lab - Evidence Acquisition with FTK Imager]]
- [[Lab - Memory Forensics with Volatility]]
- [[Lab - Network Forensics with Wireshark]]
- [[Lab - Splunk Multi-Source Correlation]]
- [[Lab - Windows Event Log Analysis]]
- [[Lab - Windows Registry Forensics]]
- [[Network & Email Forensics]]
- [[Windows & Linux Forensics]]

**Ethical Hacking & Penetration Testing**

- [[Capstone - Full VAPT Report]]
- [[Ethical Hacking Fundamentals]]
- [[Exploitation & Post-Exploitation]]
- [[Lab - Exploitation with Metasploit]]
- [[Lab - Linux Privilege Escalation]]
- [[Lab - Mini VAPT Assessment]]
- [[Lab - OSINT Reconnaissance]]
- [[Lab - Password Cracking]]
- [[Lab - SQL Injection on DVWA]]
- [[Lab - Vulnerability Assessment with Nessus]]
- [[Reconnaissance & Scanning]]
- [[SQL Essentials for SQL Injection]]
- [[SQL Injection]]
- [[VAPT Reporting]]
- [[Web & Database Basics for SQL Injection]]

**Shared / Cross-cutting**

- [[Autopsy]]
- [[Burp Suite]]
- [[Burp Suite Cheatsheet]]
- [[Common Ports & Protocols Reference]]
- [[CVSS Scoring Reference]]
- [[Forensic Artifacts Reference]]
- [[FTK Imager]]
- [[Hash & Encryption Reference]]
- [[Hydra]]
- [[John the Ripper & Hashcat]]
- [[Kali Linux]]
- [[Metasploit]]
- [[Metasploit Commands Cheatsheet]]
- [[Nessus]]
- [[netstat & ss]]
- [[Nmap]]
- [[Nmap Commands Cheatsheet]]
- [[OWASP Top 10 Quick Reference]]
- [[Shodan]]
- [[Snort]]
- [[Splunk]]
- [[Splunk SPL Cheatsheet]]
- [[Suricata]]
- [[VirusTotal]]
- [[Volatility]]
- [[Volatility Commands Cheatsheet]]
- [[Windows Event IDs Reference]]
- [[Wireshark]]
- [[Wireshark Commands Cheatsheet]]
<!-- dataview:end -->

## 🔁 Needs Review

<!-- dataview:start -->
<!-- query
LIST WITHOUT ID link(file.link, file.name)
FROM "Study/Cyber Security"
WHERE status = "review" AND type != "moc"
SORT file.name ASC
-->
- [[Lab - XSS and File Upload on DVWA]]
- [[Web Application Security]]
<!-- dataview:end -->

## ✅ Done

<!-- dataview:start -->
<!-- query
LIST WITHOUT ID link(file.link, file.name)
FROM "Study/Cyber Security"
WHERE status = "done" AND type != "moc"
SORT file.name ASC
-->
- [[CIA Triad]]
- [[Networking Fundamentals]]
- [[OSI Model & TCP-IP Security]]
- [[What is Cybersecurity]]
<!-- dataview:end -->

---

## 🆕 Recently Created

<!-- dataview:start -->
<!-- query
TABLE WITHOUT ID file.link AS "Note", subject AS "Subject", type AS "Type", created AS "Created"
FROM "Study/Cyber Security"
WHERE type != "moc"
SORT created DESC
LIMIT 8
-->
| Note | Subject | Type | Created |
| --- | --- | --- | --- |
| [[SQL Essentials for SQL Injection]] | Ethical Hacking & Penetration Testing | Concept | 2026-09-10 |
| [[SQL Injection]] | Ethical Hacking & Penetration Testing | Concept | 2026-09-10 |
| [[Web & Database Basics for SQL Injection]] | Ethical Hacking & Penetration Testing | Concept | 2026-09-10 |
| [[DMZ & Network Segmentation]] | Cybersecurity Fundamentals & Network Security | Concept | 2026-09-04 |
| [[Firewalls]] | Cybersecurity Fundamentals & Network Security | Concept | 2026-09-04 |
| [[IDS & IPS]] | Cybersecurity Fundamentals & Network Security | Concept | 2026-09-04 |
| [[Proxy Servers]] | Cybersecurity Fundamentals & Network Security | Concept | 2026-09-04 |
| [[VPN]] | Cybersecurity Fundamentals & Network Security | Concept | 2026-09-04 |
<!-- dataview:end -->

## 🚧 Stubs / To Write

> Notes tagged `#stub` — placeholders to fill in the syllabus pass. Remove the `stub` tag once written.

<!-- dataview:start -->
<!-- query
LIST WITHOUT ID file.link
FROM #stub
SORT file.name ASC
-->
- [[Lab - XSS and File Upload on DVWA]]
- [[Web Application Security]]
<!-- dataview:end -->
