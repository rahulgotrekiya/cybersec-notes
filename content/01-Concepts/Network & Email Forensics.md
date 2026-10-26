---
type: concept
subject: dfir
tags: [cybersecurity, concept, dfir, concepts, network-forensics, email-forensics, cloud-forensics, mobile-forensics]
status: learning
created: 2026-07-02
---

# Network & Email Forensics

> 📝 **Concept Note** — DFIR Unit IV | Source: MCA Sem III Syllabus

---

## Network Forensics

### What to Capture
- **Full packet capture (PCAP)** — Complete packet data including payloads
- **NetFlow data** — Metadata only (source/dest IP, ports, bytes, duration)
- **Firewall logs** — Allow/deny decisions
- **IDS/IPS logs** — Detected attacks
- **Proxy logs** — Web browsing activity

### Where to Capture

| Location | Method | What You See |
|----------|--------|-------------|
| **Network TAP** | Hardware device that copies all traffic | Complete, unfiltered traffic |
| **SPAN port** | Switch mirrors traffic to monitoring port | Selected switch traffic |
| **Endpoint** | Wireshark on host | Only that host's traffic |
| **Inline device** | IPS, proxy | Filtered/inspected traffic |

### Forensic Analysis with Wireshark

**Identifying C2 Traffic:**
- Regular interval connections (beaconing) — e.g., every 60 seconds
- Connections to unusual ports
- DNS queries with encoded subdomains (DNS tunnelling)
- HTTPS to self-signed certificates

**Identifying Data Exfiltration:**
- Large outbound transfers to unknown IPs
- DNS queries with unusually long subdomains (data encoded in DNS)
- Encrypted traffic to non-standard ports
- Upload volume >> download volume

**Identifying Lateral Movement:**
- SMB connections between workstations (unusual)
- RDP connections from unexpected sources
- PsExec / WMI remote execution patterns

---

## Email Forensics

### Email Header Analysis
Headers reveal the true path an email took:

```
Return-Path: <sender@domain.com>
Received: from mail.domain.com (10.0.0.5) by mx.recipient.com
    with SMTP; Wed, 02 Jul 2025 10:30:00 +0530
Received: from [192.168.1.10] by mail.domain.com
    with SMTP; Wed, 02 Jul 2025 10:29:55 +0530
From: "CEO" <ceo@company.com>
To: finance@company.com
Subject: Urgent Wire Transfer
Date: Wed, 02 Jul 2025 10:29:50 +0530
Message-ID: <unique-id@domain.com>
X-Mailer: Custom Mailer 1.0
```

### Reading Headers (Bottom to Top)
Headers are added at each hop — **read from bottom to top** to trace the path:
1. Bottom `Received:` = originating server
2. Each subsequent `Received:` = next hop
3. Top `Received:` = recipient's mail server

### Identifying Spoofing
- **From:** field says `ceo@company.com`
- But `Received:` headers show originating IP is `185.220.101.1` (not company's mail server)
- **SPF check:** Does the sending IP match company.com's SPF record?
- **DKIM check:** Is the cryptographic signature valid?

### SPF/DKIM/DMARC

| Check | Purpose | Result Meanings |
|-------|---------|----------------|
| **SPF** | Verifies sending IP is authorised | pass / fail / softfail / none |
| **DKIM** | Verifies email content integrity (crypto signature) | pass / fail |
| **DMARC** | Policy combining SPF + DKIM | reject / quarantine / none |

---

## Cloud Forensics Challenges

| Challenge | Description |
|-----------|-------------|
| **Shared responsibility** | Cloud provider owns infrastructure; you own data/config |
| **Jurisdiction** | Data may be stored in different countries |
| **Evidence acquisition** | Can't physically seize cloud servers |
| **Multi-tenancy** | Your data shares infrastructure with others |
| **Volatility** | Auto-scaling may destroy instances with evidence |
| **API-based collection** | Must use cloud APIs (AWS CloudTrail, Azure Activity Logs) |

### Conceptual Evidence Sources

| Cloud | Log Source | What It Shows |
|-------|-----------|--------------|
| **AWS** | CloudTrail | API calls (who did what) |
| **Azure** | Activity Logs | Resource changes |
| **GCP** | Audit Logs | Admin activity |

---

## Mobile Forensics (Introduction)

### iOS vs Android

| Aspect | iOS | Android |
|--------|-----|---------|
| **Encryption** | Full disk, default since iOS 8 | Full disk, varies by manufacturer |
| **Acquisition** | Difficult without passcode | Easier with USB debugging enabled |
| **Backup** | iTunes/iCloud backups | ADB backups |
| **App data** | Sandboxed, encrypted | Varies, some in SQLite |

### Common Mobile Artifacts
- Call logs, SMS/MMS messages
- App data (WhatsApp, Telegram databases)
- GPS/location data
- Photos with EXIF metadata (GPS coordinates, timestamps)
- Browser history
- Wi-Fi connection history

---

## Dark Web & Encryption Challenges

### Dark Web
- Accessed via Tor browser
- .onion domains — not indexed by search engines
- Challenges: anonymity, jurisdiction, encrypted communications
- Investigators may need specialised training and legal authority

### Encryption Challenges
- **Full Disk Encryption (FDE):** If powered off, data is inaccessible without key
- **Solution:** Live acquisition (capture RAM → extract encryption keys)
- **End-to-end encrypted messaging:** Content not available from provider
- **VPN/Tor:** Harder to trace network activity

---

## Related Concepts

- [[Windows & Linux Forensics]] — Disk-level analysis
- [[Evidence Acquisition & Imaging]] — How evidence is collected
- [[Digital Forensics Fundamentals]] — Investigation framework
- [[Network Traffic Analysis]] — Traffic analysis tools
- [[DFIR MOC]] — Subject navigation
