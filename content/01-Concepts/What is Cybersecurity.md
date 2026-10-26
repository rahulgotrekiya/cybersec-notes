---
type: concept
subject: csf-ns
tags:
  - cybersecurity
  - concept
  - csf-ns
  - concepts
  - fundamentals
  - cia-triad
  - attacks
  - threat-actors
status: done
created: 2026-07-02
---

# What is Cybersecurity

> 📝 **Concept Note** — CSF & NS Unit I | Source: MCA Sem III Syllabus

---

## The Problem It Solves

In 2025, every aspect of modern life depends on technology — banking, healthcare, government, communication, transportation. With this dependence comes risk:

- **Data breaches** exposing millions of personal records
- **Ransomware attacks** shutting down hospitals and pipelines
- **Nation-state attacks** targeting critical infrastructure
- **Identity theft** ruining people's financial lives
- **Corporate espionage** stealing trade secrets

Without cybersecurity, the digital world would be chaos. Every system, every network, every piece of data would be vulnerable to anyone with malicious intent.

---

## What is Cybersecurity?

**Definition:** Cybersecurity is the practice of protecting systems, networks, programs, and data from digital attacks, unauthorized access, damage, or disruption.

**Scope:** It encompasses everything from protecting a single laptop to defending an entire nation's critical infrastructure.

### Information Security vs Cybersecurity

| Aspect | Information Security | Cybersecurity |
|--------|---------------------|---------------|
| **Scope** | Protects ALL information (digital + physical) | Protects only DIGITAL assets |
| **Includes** | Paper documents, verbal info, digital data | Networks, systems, applications, data |
| **Focus** | Confidentiality, integrity, availability of all info | Defending against cyber threats specifically |
| **Example** | Locking a filing cabinet | Configuring a firewall |

> 💡 Cybersecurity is a **subset** of information security. All cybersecurity is infosec, but not all infosec is cybersecurity.

---

## The CIA Triad

The three fundamental pillars of security. Every security control aims to protect one or more of these:

### Confidentiality
**Only authorised people can access the data.**

- **Encryption** — Scramble data so only key-holders can read it
- **Access controls** — Login credentials, role-based permissions
- **Data classification** — Label data by sensitivity level

**Example:** Your bank account balance is visible only to you and the bank, not to anyone on the internet.

**Attack:** A hacker intercepts unencrypted emails → **breach of confidentiality**

### Integrity
**Data has not been tampered with or altered.**

- **Hashing** — Generate a unique fingerprint of data (MD5, SHA-256)
- **Digital signatures** — Prove data came from a trusted source
- **Version control** — Track every change

**Example:** When you download software, the website shows a SHA-256 hash. You verify the downloaded file's hash matches → confirms no one tampered with it.

**Attack:** A man-in-the-middle alters a bank transfer amount from ₹1,000 to ₹1,00,000 → **breach of integrity**

### Availability
**Systems and data are accessible when needed.**

- **Redundancy** — Backup servers, multiple data centres
- **DDoS protection** — Absorb massive traffic floods
- **Disaster recovery** — Plans to restore after failure

**Example:** Netflix must be available 24/7 worldwide. Even if one data centre fails, others take over.

**Attack:** DDoS attack floods a hospital's network → doctors can't access patient records → **breach of availability**

> 🔗 Deep dive: [[CIA Triad]]

---

## Core Attack Types

### Malware (Malicious Software)

| Type | What It Does | Example |
|------|-------------|---------|
| **Virus** | Attaches to legitimate programs, spreads when executed | Infects .exe files |
| **Worm** | Self-replicates across networks without user action | WannaCry (2017) |
| **Ransomware** | Encrypts files, demands payment for decryption key | LockBit, REvil |
| **Trojan** | Disguises as legitimate software, opens backdoor | Fake antivirus software |
| **Spyware** | Secretly monitors user activity, steals data | Keyloggers, screen capture |

### Phishing
Fraudulent communications (usually email) that appear to come from a trusted source, designed to steal credentials or install malware.

- **Spear phishing** — Targeted at specific individuals
- **Whaling** — Targeted at executives (CEO, CFO)
- **Smishing** — Phishing via SMS
- **Vishing** — Phishing via voice calls

### Social Engineering
Manipulating people into breaking security procedures. Exploits human psychology, not technical vulnerabilities.

- **Pretexting** — Creating a fabricated scenario to extract information
- **Baiting** — Leaving infected USB drives in public places
- **Tailgating** — Following an authorised person through a secured door
- **Quid pro quo** — Offering something in exchange for information

### SQL Injection (concept)
Inserting malicious SQL code into input fields to manipulate databases.

```
Login form: Username = admin' OR '1'='1' --
Result: Bypasses authentication entirely
```

### DoS / DDoS
- **DoS (Denial of Service)** — Single source overwhelms a target
- **DDoS (Distributed DoS)** — Thousands of compromised machines (botnet) attack simultaneously

### Man-in-the-Middle (MitM)
Attacker secretly intercepts and potentially alters communication between two parties who believe they're communicating directly.

```
Alice → [Attacker] → Bob
Alice thinks she's talking to Bob directly
The attacker can read, modify, or inject messages
```

---

## Threat Actors

| Actor Type | Motivation | Skill Level | Example |
|-----------|-----------|------------|---------|
| **Nation-State** | Political, espionage, sabotage | Very high (APT groups) | Stuxnet, APT28, Lazarus Group |
| **Hacktivist** | Political/social cause | Medium | Anonymous, LulzSec |
| **Insider** | Revenge, financial gain, accident | Varies | Disgruntled employee |
| **Cybercriminal** | Financial gain | Medium to high | Ransomware gangs |
| **Script Kiddie** | Fun, bragging rights, chaos | Low (uses others' tools) | Using Metasploit without understanding |

---

## Attack Surface

**Definition:** The total number of points where an unauthorised user can try to enter or extract data from a system.

**Components:**
- Open ports and services
- Web applications and APIs
- Employee email accounts
- Physical access points
- Cloud services and third-party integrations
- IoT devices
- Mobile applications

> 💡 The larger the attack surface, the harder it is to defend. Security aims to **minimise the attack surface**.

> 🔎 **See your own attack surface:** tools like [[Shodan]] index every Internet-exposed service, letting you (and attackers) view what your organisation exposes — passively, without touching the target.

---

## Information Security Principles

### Defence in Depth
**Multiple layers of security** — if one layer fails, others still protect.

```
Internet → Firewall → IDS/IPS → Network Segmentation → Host Firewall → Antivirus → Encryption
```

Like a castle: moat → outer wall → inner wall → guards → locked doors → safe

### Least Privilege
Users and programs should have **only the minimum permissions** needed to do their job.

- A marketing employee doesn't need access to source code
- A web server process shouldn't run as root
- Temporary admin access should be revoked after use

### Separation of Duties
**No single person should control all aspects** of a critical process.

- The person who writes code shouldn't deploy it to production alone
- The DBA who manages the database shouldn't also manage backups
- Requires multiple people to complete sensitive operations

---

## Common Misconceptions

❌ **"I'm not important enough to be hacked"**
- Reality: Automated attacks scan the entire internet. Everyone is a target.

❌ **"Antivirus is enough"**
- Reality: Modern attacks bypass antivirus easily. Defence in depth is required.

❌ **"Strong passwords make me safe"**
- Reality: Passwords are just one layer. Phishing, social engineering, and password reuse are bigger threats.

❌ **"Cybersecurity is only an IT problem"**
- Reality: It's everyone's responsibility. Most breaches start with human error.

---

## Real-World Examples

### WannaCry (2017)
- **Type:** Ransomware worm
- **Impact:** 200,000+ computers in 150 countries, including NHS hospitals
- **How:** Exploited a Windows vulnerability (EternalBlue) leaked from NSA
- **Lesson:** Patch management is critical

### SolarWinds (2020)
- **Type:** Supply chain attack (nation-state)
- **Impact:** Compromised 18,000 organisations including US government agencies
- **How:** Malware injected into legitimate software update
- **Lesson:** Even trusted software can be compromised

### Colonial Pipeline (2021)
- **Type:** Ransomware
- **Impact:** Largest fuel pipeline in US shut down for 6 days
- **How:** Single compromised password (no MFA)
- **Lesson:** One weak credential can shut down critical infrastructure

---

## Related Concepts

- [[CIA Triad]] — Deep dive into the three pillars
- [[OSI Model & TCP-IP Security]] — Understanding network layers for security
- [[Ethical Hacking Fundamentals]] — The offensive side of cybersecurity
- [[CSF & NS MOC]] — Subject navigation

---

## My Learning Notes

**Key Insights:**
- Cybersecurity is fundamentally about **risk management**, not about being 100% secure
- The human element (social engineering) is often the weakest link
- Understanding attack types helps you think like an attacker to defend better

**Questions I Still Have:**
- How do organisations prioritise which threats to defend against first?
- What does a typical cybersecurity career path look like in India?

**Practical Next Steps:**
- Set up Wireshark and capture live traffic
- Explore TryHackMe Pre-Security path
- Understand the CIA Triad through real-world case studies

---

## Tags

#cybersecurity #csf-ns #concepts #fundamentals #cia-triad #attacks #threat-actors

---

## Resources

- Course: MCA Sem III — Cybersecurity Fundamentals & Network Security, Unit I
- TryHackMe Pre-Security: https://tryhackme.com/path/outline/presecurity
- Related: [[00-Cyber Security MOC|Cyber Security MOC]] for navigation
