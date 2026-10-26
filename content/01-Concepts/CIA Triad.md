---
type: concept
subject: csf-ns
tags:
  - cybersecurity
  - concept
  - csf-ns
  - concepts
  - cia-triad
  - confidentiality
  - integrity
  - availability
  - fundamentals
status: done
created: 2026-07-02
---

# CIA Triad

> 📝 **Concept Note** — CSF & NS Unit I Deep Dive | Source: MCA Sem III Syllabus

---

## The Problem It Solves

Without a framework, security decisions become ad hoc — "secure this, protect that" without understanding **what** you're trying to achieve. The CIA Triad provides a **universal model** that every security control, policy, and decision maps back to.

Every security breach is a failure of at least one of these three properties.

---

## What is the CIA Triad?

The CIA Triad is the foundational model in information security. Every security measure exists to protect one or more of these three properties:

```
        Confidentiality
             /\
            /  \
           /    \
          / CIA  \
         / Triad  \
        /          \
       /____________\
  Integrity    Availability
```

---

## Confidentiality — "Who can see it?"

### Definition
Ensuring that information is accessible **only to those authorised** to access it.

### How It's Achieved

| Mechanism | How It Works | Example |
|-----------|-------------|---------|
| **Encryption** | Data is scrambled; only key-holders can read | AES-256 for files, TLS for web traffic |
| **Access Control Lists (ACLs)** | Rules defining who can access what | File permissions (rwx), firewall rules |
| **Authentication** | Verifying identity before granting access | Passwords, biometrics, MFA |
| **Data Classification** | Labelling data by sensitivity level | Public, Internal, Confidential, Top Secret |
| **Network Segmentation** | Isolating sensitive systems | HR database on separate VLAN |

### Threats to Confidentiality
- Eavesdropping / sniffing unencrypted traffic
- Unauthorized access (stolen credentials)
- Social engineering (phishing for passwords)
- Shoulder surfing
- Dumpster diving (physical documents)

### Real-World Example
**Scenario:** A hospital stores patient records.
- **Protected:** Records encrypted at rest and in transit, only doctors and authorised staff can access, MFA required
- **Breach:** An intern finds an unlocked workstation → accesses patient records they shouldn't see → confidentiality violated

---

## Integrity — "Has it been changed?"

### Definition
Ensuring that data is **accurate, complete, and unaltered** by unauthorized parties.

### How It's Achieved

| Mechanism | How It Works | Example |
|-----------|-------------|---------|
| **Hashing** | Creates a unique fingerprint of data | SHA-256 hash of a file |
| **Digital Signatures** | Proves data origin and integrity | Signed software updates |
| **Checksums** | Quick verification of data integrity | MD5 checksum for downloads |
| **Version Control** | Tracks every change to data | Git for source code |
| **Input Validation** | Prevents malicious data entry | Sanitising form inputs |
| **Database Constraints** | Enforces data rules | Foreign keys, NOT NULL |

### How Hashing Works

```
Original file → SHA-256 → a1b2c3d4e5f6...  (hash value)

If even ONE bit changes:
Modified file → SHA-256 → 9z8y7x6w5v4u...  (completely different hash)
```

> 💡 Hashing is **one-way** — you can't reverse a hash back to the original data. It's used for **verification**, not encryption.

### Common Hash Algorithms

| Algorithm | Output Length | Status | Use Case |
|-----------|-------------|--------|----------|
| MD5 | 128-bit (32 hex chars) | ⚠️ Broken — collisions found | Legacy, quick checksums |
| SHA-1 | 160-bit (40 hex chars) | ⚠️ Deprecated | Legacy |
| SHA-256 | 256-bit (64 hex chars) | ✅ Secure | File integrity, forensics |
| SHA-512 | 512-bit (128 hex chars) | ✅ Secure | High-security applications |

### Threats to Integrity
- Man-in-the-Middle attacks (modifying data in transit)
- SQL injection (altering database records)
- Malware modifying files
- Insider threats (unauthorized data changes)
- Bit rot (unintentional data corruption)

### Real-World Example
**Scenario:** Online banking transfer.
- **Protected:** TLS encrypts the transaction, digital signatures verify the bank's server, checksums verify transfer details
- **Breach:** MitM attacker intercepts and changes transfer amount from ₹5,000 to ₹5,00,000 → integrity violated

---

## Availability — "Can I access it?"

### Definition
Ensuring that systems, data, and services are **accessible and operational** when needed by authorised users.

### How It's Achieved

| Mechanism | How It Works | Example |
|-----------|-------------|---------|
| **Redundancy** | Duplicate critical systems | Multiple servers, RAID storage |
| **Load Balancing** | Distribute traffic across servers | AWS ELB across availability zones |
| **DDoS Protection** | Absorb/filter malicious traffic | Cloudflare, AWS Shield |
| **Backups** | Regular copies of critical data | Daily incremental, weekly full |
| **Disaster Recovery** | Plans to restore after catastrophe | Failover to DR site |
| **UPS / Generators** | Power backup | Battery + diesel generator |
| **Patch Management** | Prevent crashes from vulnerabilities | Regular OS/software updates |

### Threats to Availability
- DDoS attacks (flooding with traffic)
- Hardware failures (disk crash, power failure)
- Natural disasters (earthquake, flood)
- Ransomware (encrypts files, makes them unavailable)
- Human error (accidental deletion, misconfiguration)
- Software bugs causing crashes

### Real-World Example
**Scenario:** E-commerce website during Diwali sale.
- **Protected:** Auto-scaling servers, CDN for static content, DDoS protection, multiple availability zones
- **Breach:** DDoS attack floods the website → customers can't place orders → availability violated → company loses crores in revenue

---

## CIA Triad in Action — Mapping Controls

| Security Control | C | I | A | Explanation |
|-----------------|---|---|---|-------------|
| Encryption | ✅ | | | Prevents unauthorized reading |
| Hashing | | ✅ | | Detects data modification |
| Backups | | | ✅ | Enables recovery after failure |
| Firewall | ✅ | | ✅ | Blocks unauthorized access, prevents DDoS |
| Access Control | ✅ | ✅ | | Controls who can read/modify |
| RAID Storage | | | ✅ | Disk redundancy |
| Digital Signatures | | ✅ | | Proves data authenticity |
| MFA | ✅ | | | Stronger authentication |
| IDS/IPS | ✅ | ✅ | ✅ | Detects attacks on all three |
| Load Balancer | | | ✅ | Distributes traffic |

---

## Beyond CIA — Extended Models

### CIA + AAA

| Property | Meaning |
|----------|---------|
| **Authentication** | Proving who you are |
| **Authorization** | What you're allowed to do |
| **Accounting** | Logging what you did |

### Parkerian Hexad (Extended CIA)
Adds three more properties:
- **Possession/Control** — Physical possession of data medium
- **Authenticity** — Data genuinely comes from claimed source
- **Utility** — Data is in a usable format

---

## CIA Triad — Quick Decision Framework

When evaluating any security measure, ask:

1. **Does it protect confidentiality?** → Who can see this data?
2. **Does it protect integrity?** → Can this data be trusted as accurate?
3. **Does it protect availability?** → Can authorised users access this when needed?

If a control doesn't clearly protect at least one of these, question whether it's actually a security control.

---

## Common Misconceptions

❌ **"All three are equally important for every system"**
- Reality: Priority depends on context. A military system prioritises **confidentiality**. A hospital's emergency system prioritises **availability**. A financial ledger prioritises **integrity**.

❌ **"CIA Triad is outdated"**
- Reality: It remains the foundational model taught worldwide. Extended models build on it, not replace it.

❌ **"Encryption handles everything"**
- Reality: Encryption primarily addresses confidentiality. You still need hashing (integrity) and redundancy (availability).

---

## Related Concepts

- [[What is Cybersecurity]] — The broader context
- [[OSI Model & TCP-IP Security]] — CIA applied to network layers
- [[Network Defence Mechanisms]] — Controls that protect CIA
- [[CSF & NS MOC]] — Subject navigation

---

## Tags

#cybersecurity #csf-ns #concepts #cia-triad #confidentiality #integrity #availability #fundamentals

---

## Resources

- Course: MCA Sem III — Cybersecurity Fundamentals & Network Security, Unit I
- CompTIA Security+ Study Guide — CIA Triad chapter
- Related: [[00-Cyber Security MOC|Cyber Security MOC]] for navigation
