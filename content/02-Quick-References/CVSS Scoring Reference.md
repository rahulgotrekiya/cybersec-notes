---
type: reference
subject: shared
tags: [cybersecurity, reference, cheatsheet, cvss, scoring, vulnerability, quickref]
status: learning
created: 2026-07-02
---

# CVSS Scoring Reference ⚡

> ⚡ **Quick Reference** — CVSS v3.1 Base Score calculation

---

## Score Ranges

| Score | Severity | Color |
|-------|----------|-------|
| 0.0 | None | ⬜ |
| 0.1 – 3.9 | Low | 🟢 |
| 4.0 – 6.9 | Medium | 🟡 |
| 7.0 – 8.9 | High | 🟠 |
| 9.0 – 10.0 | Critical | 🔴 |

## Base Metrics

### Exploitability Metrics

| Metric | Values | Higher Score |
|--------|--------|-------------|
| **Attack Vector (AV)** | Network (N) / Adjacent (A) / Local (L) / Physical (P) | Network = highest |
| **Attack Complexity (AC)** | Low (L) / High (H) | Low = higher |
| **Privileges Required (PR)** | None (N) / Low (L) / High (H) | None = highest |
| **User Interaction (UI)** | None (N) / Required (R) | None = higher |
| **Scope (S)** | Unchanged (U) / Changed (C) | Changed = higher |

### Impact Metrics

| Metric | Values | Higher Score |
|--------|--------|-------------|
| **Confidentiality (C)** | None (N) / Low (L) / High (H) | High = highest |
| **Integrity (I)** | None (N) / Low (L) / High (H) | High = highest |
| **Availability (A)** | None (N) / Low (L) / High (H) | High = highest |

## Example Scores

| Vulnerability | AV | AC | PR | UI | S | C | I | A | Score |
|--------------|----|----|----|----|---|---|---|---|-------|
| Remote Code Execution (no auth) | N | L | N | N | U | H | H | H | **9.8 Critical** |
| SQL Injection | N | L | N | N | U | H | H | N | **9.1 Critical** |
| Stored XSS | N | L | L | R | C | L | L | N | **5.4 Medium** |
| Local privilege escalation | L | L | L | N | U | H | H | H | **7.8 High** |
| CSRF | N | L | N | R | U | N | L | N | **4.3 Medium** |
| Info disclosure (minor) | N | L | N | N | U | L | N | N | **5.3 Medium** |

## Calculator
Online: https://www.first.org/cvss/calculator/3.1
