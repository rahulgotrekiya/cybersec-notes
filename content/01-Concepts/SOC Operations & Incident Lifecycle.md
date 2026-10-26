---
type: concept
subject: csf-ns
tags: [cybersecurity, concept, csf-ns, concepts, soc, incident-response, siem, log-analysis, threat-intelligence]
status: learning
created: 2026-07-02
---

# SOC Operations & Incident Lifecycle

> 📝 **Concept Note** — CSF & NS Unit V | Source: MCA Sem III Syllabus

---

## The Problem It Solves

Every organisation generates millions of log events daily. Without a structured team and process, attacks go undetected, alerts pile up, and incidents escalate into disasters. The SOC provides 24/7 monitoring, detection, and response.

---

## SOC Structure

### What is a SOC?
A **Security Operations Center** is a centralised team that continuously monitors, detects, analyses, and responds to cybersecurity incidents using technology, people, and processes.

### Tier Structure

```
┌─────────────────────────────────────────┐
│              CISO                         │
│    Chief Information Security Officer     │
└────────────────┬────────────────────────┘
                 │
┌────────────────▼────────────────────────┐
│          SOC Manager                      │
│   Oversees operations, metrics, staffing  │
└────────────────┬────────────────────────┘
                 │
    ┌────────────┼────────────┐
    ▼            ▼            ▼
┌────────┐ ┌────────┐ ┌────────┐
│ Tier 3 │ │ Tier 2 │ │ Tier 1 │
│Threat  │ │Deep    │ │Alert   │
│Hunting │ │Analysis│ │Triage  │
│Advanced│ │Incident│ │First   │
│   IR   │ │Invest. │ │Response│
└────────┘ └────────┘ └────────┘
```

### Tier Responsibilities

| Tier | Role | Key Tasks |
|------|------|-----------|
| **Tier 1** | Alert Triage / First Response | Monitor SIEM, triage alerts (true/false positive), create tickets, escalate to T2, basic containment |
| **Tier 2** | Investigation / Deeper Analysis | Deep-dive investigation, correlate across sources, determine scope, perform containment actions |
| **Tier 3** | Threat Hunting / Advanced IR | Proactive threat hunting, advanced incident response, tuning detection rules, malware analysis |
| **SOC Manager** | Operations Lead | Shift scheduling, metrics, process improvement, reporting to CISO |
| **CISO** | Executive | Strategic security decisions, board reporting, risk management |

### SOC Playbook
Pre-defined procedures for common incident types:
- **Phishing playbook** — Steps to investigate a reported phishing email
- **Brute force playbook** — Steps when SIEM alerts on multiple failed logins
- **Malware playbook** — Steps when endpoint detects malicious file
- Each playbook defines: triggers, investigation steps, containment, escalation criteria, documentation

---

## SIEM in the SOC

### Log Collection Flow

```
Endpoints → ┐
Firewalls → ├──→ Log Collectors → SIEM → Dashboard/Alerts
Servers   → ┤                              ↓
IDS/IPS   → ┘                        SOC Analyst
```

### Key SIEM Functions
1. **Log collection** — Gather from endpoints, firewalls, servers, apps
2. **Normalisation** — Convert different log formats to standard format
3. **Parsing** — Extract fields (IP, username, timestamp, event type)
4. **Correlation** — Connect related events across sources
5. **Alerting** — Trigger alerts when rules match
6. **Dashboard** — Visual overview of security posture

### SIEM Use Cases

| Use Case | Detection Logic | Alert |
|----------|----------------|-------|
| **Brute Force** | >10 failed logins from same IP in 5 min | High |
| **Privilege Escalation** | User added to admin group | Critical |
| **Data Exfiltration** | Large outbound transfer to unknown IP | High |
| **Account Creation** | New account created outside business hours | Medium |
| **Service Installed** | New service installed on server | Medium |

---

## Log Analysis

### Windows Event Logs

| Event ID | Meaning | Security Significance |
|----------|---------|----------------------|
| **4624** | Successful logon | Track who's logging in |
| **4625** | Failed logon | Brute force detection |
| **4720** | User account created | Insider threat / persistence |
| **4732** | User added to security group | Privilege escalation |
| **7045** | New service installed | Malware persistence |
| **4688** | Process creation | Track program execution |
| **1102** | Audit log cleared | Anti-forensics attempt |

#### Logon Types (with Event 4624/4625)

| Logon Type | Meaning | Example |
|------------|---------|---------|
| 2 | Interactive (keyboard) | Local login |
| 3 | Network | SMB share, remote service |
| 7 | Unlock | Workstation unlock |
| 10 | RemoteInteractive | RDP session |

### Linux Logs

| Log File | Contents |
|----------|----------|
| `/var/log/auth.log` | Authentication events (SSH, sudo) |
| `/var/log/syslog` | System messages |
| `/var/log/kern.log` | Kernel messages |
| `/var/log/apache2/access.log` | Web server access |

### What Analysts Look For
- **Patterns** — Multiple failed logins followed by success = compromised account
- **Anomalies** — Login at 3 AM from unusual location
- **Correlation** — Failed login on VPN + successful login on server = lateral movement
- **Volume** — Spike in outbound DNS queries = possible data exfiltration

---

## Alert Handling Process

```
Alert in SIEM
    │
    ▼
1. RECEIVE alert — Tier 1 sees alert in queue
    │
    ▼
2. TRIAGE — Is this a True Positive, False Positive, or needs escalation?
    │
    ├── False Positive → Document, close ticket, tune rule
    │
    ├── True Positive → Investigate + contain
    │
    └── Uncertain → Escalate to Tier 2
    │
    ▼
3. INVESTIGATE — Gather context, check logs, correlate
    │
    ▼
4. CONTAIN — Block IP, disable account, isolate host
    │
    ▼
5. DOCUMENT — Record findings, actions, evidence
    │
    ▼
6. CLOSE — Update ticket, lessons learned
```

### Ticket Lifecycle
1. **New** — Alert generated, ticket created
2. **Assigned** — Analyst takes ownership
3. **In Progress** — Investigation underway
4. **Escalated** — Handed to T2/T3 if needed
5. **Resolved** — Incident contained and remediated
6. **Closed** — Post-incident review complete

---

## Incident Lifecycle — NIST SP 800-61

### The 4 Phases (6 Steps)

```
1. Preparation → 2. Detection & Analysis → 3. Containment, Eradication, Recovery → 4. Post-Incident Activity
```

#### Phase 1: Preparation
- Build the SOC team, deploy tools (SIEM, IDS, EDR)
- Create playbooks and procedures
- Train staff, run tabletop exercises
- Maintain incident response plan

#### Phase 2: Detection & Analysis
- Monitor alerts from SIEM, IDS, endpoints
- Triage and validate alerts
- Determine scope and impact
- Classify incident severity

#### Phase 3: Containment, Eradication, Recovery
- **Containment** — Stop the bleeding (isolate infected host, block attacker IP)
- **Eradication** — Remove the threat (delete malware, patch vulnerability)
- **Recovery** — Restore to normal operations (rebuild system, restore from backup)

#### Phase 4: Post-Incident Activity
- **Lessons learned** meeting — What happened? What worked? What didn't?
- Update playbooks and detection rules
- Document everything for future reference
- Improve defences based on findings

### How Tiers Work Together

```
Tier 1: Detects alert → Triages → Escalates to Tier 2
Tier 2: Investigates → Determines scope → Escalates to Tier 3 if advanced
Tier 3: Advanced analysis → Threat hunting → Malware reverse engineering
```

---

## Threat Intelligence Basics

### Indicators of Compromise (IoCs)

| IoC Type | Example | Use |
|----------|---------|-----|
| **IP Address** | 185.220.101.1 | Block at firewall, search in SIEM |
| **Domain** | evil-c2-server.com | Block in DNS/proxy |
| **File Hash** | d41d8cd98f00b204e9800998ecf8427e | Search on endpoints |
| **User Agent** | Mozilla/5.0 (custom bot) | Detect in web logs |
| **Email Address** | phisher@fake-domain.com | Block in email gateway |

### Threat Feeds
External sources of IoCs that SOC teams subscribe to:
- Open-source: AlienVault OTX, Abuse.ch
- Commercial: Recorded Future, CrowdStrike
- Government: CERT-In (India), US-CERT

### How Tier 1 Uses Threat Intelligence
1. Alert fires on suspicious IP
2. Check IP against threat feeds → Known C2 server
3. Context helps determine: True Positive (high confidence)
4. Escalate with enriched context to Tier 2

---

## Related Concepts

- [[What is Cybersecurity]] — Foundation
- [[Network Traffic Analysis]] — Tools used in SOC
- [[Network Defence Mechanisms]] — What generates the alerts
- [[Incident Response & Malware Analysis]] — DFIR deep dive
- [[Windows Event IDs Reference]] ⚡
- [[Splunk SPL Cheatsheet]] ⚡
- [[CSF & NS MOC]] — Subject navigation
