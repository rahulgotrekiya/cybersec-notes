---
type: reference
subject: shared
tags: [cybersecurity, reference, cheatsheet, windows, event-ids, soc, forensics, quickref]
status: learning
created: 2026-07-02
---

# Windows Event IDs Reference ⚡

> ⚡ **Quick Reference** — Critical Windows Event IDs for SOC analysts and forensics

---

## Authentication Events

| Event ID | Description | Significance |
|----------|-------------|-------------|
| **4624** | Successful logon | Track valid access |
| **4625** | Failed logon | Brute force detection |
| **4634** | Logoff | Session tracking |
| **4647** | User-initiated logoff | Normal logoff |
| **4648** | Explicit credentials logon | RunAs, lateral movement |
| **4672** | Special privileges assigned | Admin/elevated logon |
| **4740** | Account lockout | Brute force indicator |

## Logon Types (used with 4624/4625)

| Type | Name | Meaning |
|------|------|---------|
| 2 | Interactive | Local keyboard login |
| 3 | Network | SMB share, remote service |
| 4 | Batch | Scheduled task |
| 5 | Service | Service startup |
| 7 | Unlock | Workstation unlock |
| 8 | NetworkCleartext | IIS basic auth (cleartext!) |
| 10 | RemoteInteractive | RDP session |
| 11 | CachedInteractive | Cached domain credentials |

## Account Management

| Event ID | Description | Significance |
|----------|-------------|-------------|
| **4720** | User account created | Persistence check |
| **4722** | User account enabled | Account re-activation |
| **4724** | Password reset attempted | Takeover attempt |
| **4725** | User account disabled | Defensive action |
| **4726** | User account deleted | Evidence destruction |
| **4732** | Member added to security group | Privilege escalation |
| **4733** | Member removed from security group | Access change |
| **4738** | User account changed | Configuration change |

## System Events

| Event ID | Description | Significance |
|----------|-------------|-------------|
| **7045** | New service installed | Malware persistence |
| **7040** | Service start type changed | Config change |
| **1102** | Audit log cleared | Anti-forensics! 🚨 |
| **4688** | New process created | Track execution |
| **4689** | Process terminated | Process lifecycle |
| **4697** | Service installation attempt | Persistence |

## Quick SPL Templates
```spl
# Brute force detection
index=main EventCode=4625 | stats count by src_ip | where count > 20

# Successful login after failures
index=main (EventCode=4625 OR EventCode=4624) | transaction src_ip maxspan=10m | search EventCode=4625 EventCode=4624

# New admin accounts
index=main EventCode=4732 TargetUserName="Administrators"

# Service installation
index=main EventCode=7045 | table _time, ServiceName, ImagePath
```
