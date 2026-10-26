---
type: reference
subject: shared
tags:
  - cybersecurity
  - reference
  - cheatsheet
  - owasp
  - owasp-2025
  - web-security
  - ehpt
  - quickref
status: learning
created: 2026-07-02
---

# OWASP Top 10 Quick Reference ⚡

> ⚡ **Quick Reference** — OWASP Top 10 : 2025 (8th edition)
> Based on 175,000+ CVE records and 589 CWEs | Official: https://owasp.org/Top10/2025/

---

## The Top 10 at a Glance

| # | Category | Status vs 2021 |
|---|----------|---------------|
| **A01** | Broken Access Control | ↔ Stayed #1 |
| **A02** | Security Misconfiguration | ⬆️ Up from #5 |
| **A03** | Software Supply Chain Failures | 🆕 New (replaces Vulnerable Components) |
| **A04** | Cryptographic Failures | ↔ Moved from #2 |
| **A05** | Injection | ↔ Moved from #3 |
| **A06** | Insecure Design | ↔ Same |
| **A07** | Authentication Failures | ↔ Same |
| **A08** | Software or Data Integrity Failures | ↔ Same |
| **A09** | Security Logging & Alerting Failures | ↔ Same |
| **A10** | Mishandling of Exceptional Conditions | 🆕 New |

> **Removed from 2021:** SSRF — absorbed into A01 Broken Access Control

---

## Detailed Breakdown

### A01 — Broken Access Control 🔴 Critical

**What is it?** Users can act outside their intended permissions — accessing data or functions they shouldn't be allowed to.

**SSRF is now included here** (moved from standalone in 2021)

**Common vulnerabilities:**
- IDOR (Insecure Direct Object Reference): `?id=123` → change to `?id=124` → access another user's data
- Missing function-level access control: non-admin accessing `/admin/deleteUser`
- CORS misconfiguration allowing unauthorised origins
- Force browsing to authenticated pages without login
- SSRF: Server fetches `http://169.254.169.254/` (AWS metadata service)

**Attack example:**
```
GET /api/orders/1001          → Your order ✅
GET /api/orders/1002          → Someone else's order ❌ → IDOR vulnerability
```

**Defence:**
- Deny access by default — whitelist, don't blacklist
- Enforce access control server-side (never client-side only)
- Rate limit API calls
- Validate all server-side requests (prevent SSRF)

---

### A02 — Security Misconfiguration 🔴 (⬆️ Up from #5)

**What is it?** Insecure default configurations, incomplete setups, open cloud storage, unnecessary features enabled, or verbose error messages.

**Why it jumped to #2:** Cloud adoption, DevOps pipelines, and containerisation have massively increased attack surface from misconfigured infrastructure.

**Common examples:**
- Default credentials left unchanged (`admin:admin`, `admin:password`)
- Directory listing enabled on web server
- Unnecessary ports/services open
- Detailed error messages exposing stack traces, DB queries
- Cloud S3 bucket set to public read
- Missing security headers (no CSP, HSTS, X-Frame-Options)
- XML External Entity (XXE) processing enabled

**Attack example:**
```
Target runs Apache with default config:
→ Directory listing enabled → attacker browses /backup/ → finds database dump
→ phpinfo() page accessible → reveals PHP version, server paths, loaded modules
```

**Defence:**
- Automated configuration scanning (checkov, tfsec for IaC)
- Disable all unused features, ports, services
- CIS Benchmark hardening guides
- Regular security audits

---

### A03 — Software Supply Chain Failures 🆕 New

**What is it?** Weaknesses in the software supply chain — compromised dependencies, build systems, update mechanisms, or CI/CD pipelines. This expands 2021's "Vulnerable and Outdated Components" to the full supply chain.

**Key incidents that drove this category:**
- **SolarWinds (2020):** Build pipeline compromised → malicious code in signed software updates → 18,000 organisations infected
- **Log4Shell (2021):** Vulnerability in widely-used Log4j library → millions of apps vulnerable
- **XZ Utils backdoor (2024):** Malicious actor gained trust over 2 years → inserted backdoor in compression library

**Attack vectors:**
- Compromised open-source packages (typosquatting: `lodash` vs `l0dash`)
- Vulnerable third-party libraries with known CVEs
- Compromised build server → malicious code injected during compilation
- Unsigned or unverified updates pushed to end users

**Attack example:**
```
Attacker publishes npm package "color5" (typosquat of "colors")
→ Developer runs npm install → installs malicious package
→ Package exfiltrates environment variables (API keys) during build
```

**Defence:**
- **SBOM** (Software Bill of Materials) — inventory all dependencies
- Dependency scanning tools: Snyk, Dependabot, OWASP Dependency-Check
- Use package integrity verification (npm lockfiles, pip hashes)
- Sign and verify all build artifacts
- Principle of least privilege for CI/CD pipelines

---

### A04 — Cryptographic Failures (Moved from #2)

**What is it?** Failures related to cryptography — storing sensitive data unencrypted, using weak/broken algorithms, or poor key management.

**Common failures:**
- Passwords stored in plaintext or with MD5/SHA-1 (no salt)
- Sensitive data transmitted over HTTP (not HTTPS)
- Weak cipher suites (DES, RC4, SSL 2.0/3.0)
- Hard-coded cryptographic keys in source code
- Insufficient randomness (predictable tokens)

**Attack example:**
```
Database dump reveals:
username | password
admin    | 5f4dcc3b5aa765d61d8327deb882cf99  ← MD5 of "password"
→ hashcat -m 0 hash.txt rockyou.txt → cracked in seconds
```

**Defence:**
- Use bcrypt, Argon2, or scrypt for passwords (not MD5/SHA-256 directly)
- TLS 1.2+ for all data in transit
- AES-256 for data at rest
- Never hard-code keys; use a secrets manager (Vault, AWS Secrets Manager)

---

### A05 — Injection (Moved from #3)

**What is it?** Untrusted data sent to an interpreter causes unintended commands to execute.

**Types:**
- SQL Injection
- Command Injection (`; rm -rf /`)
- LDAP Injection
- XPath Injection
- NoSQL Injection (MongoDB: `{"$gt": ""}`)

**Classic SQL Injection:**
```sql
-- Login form: username = ' OR '1'='1' --
SELECT * FROM users WHERE username='' OR '1'='1' --' AND password='...'
→ Returns all rows → bypasses login ✅ for attacker
```

**UNION-based data extraction:**
```sql
1' UNION SELECT null, version() -- -        → DB version
1' UNION SELECT user, password FROM users-- → dump credentials
```

**Defence:**
- Parameterised queries / prepared statements ← The #1 fix
- ORM frameworks (SQLAlchemy, Hibernate)
- Input validation and whitelisting
- Least privilege on database accounts

---

### A06 — Insecure Design

**What is it?** Security flaws baked into the design — no security threat modelling done, no rate limiting designed, no secure defaults.

**Design failures (not implementation bugs):**
- No account lockout → brute force possible by design
- Password reset via security questions (knowledge that can be researched)
- "Remember me" stores plaintext credentials in cookie
- Business logic flaws (buy item for negative price)

**Defence:**
- Threat modelling (STRIDE) during design phase
- Security design review before development begins
- Secure design patterns and reference architectures

---

### A07 — Authentication Failures

**What is it?** Weak authentication mechanisms or session management failures that allow attackers to compromise passwords, keys, or session tokens.

**Common weaknesses:**
- No account lockout (brute force/credential stuffing)
- Weak passwords allowed (less than 8 chars, no complexity)
- Credentials in URL (`?sessionid=abc123`)
- Session ID not rotated after login (session fixation)
- Session doesn't expire after logout

**Credential stuffing:** Using username:password pairs leaked from one breach on other sites (most people reuse passwords).

**Defence:**
- MFA (Multi-Factor Authentication) — highest impact single control
- Block commonly used passwords (HaveIBeenPwned API)
- Rate limit and lockout after failed attempts
- Regenerate session ID after login
- Set secure + httpOnly flags on session cookies

---

### A08 — Software or Data Integrity Failures

**What is it?** Code and infrastructure that does not verify the integrity of software updates, plugins, or serialised data before using it.

**Examples:**
- Auto-update without signature verification → attacker serves malicious update
- Insecure deserialisation — deserialising untrusted data leads to RCE
- CI/CD pipeline using unverified plugins from public repos

**Attack example (deserialisation):**
```
Attacker sends crafted serialised Java object via HTTP parameter
→ Server deserialises without validation
→ Object executes attacker's payload on deserialisation → RCE
```

**Defence:**
- Verify digital signatures on all updates and packages
- Never deserialise data from untrusted sources
- Use JSON (text-based) instead of binary serialisation where possible

---

### A09 — Security Logging & Alerting Failures

**What is it?** Insufficient logging, monitoring, and alerting — attackers operate undetected for extended periods (average breach dwell time: 200+ days).

**Signs of failure:**
- Login failures are not logged
- No alerts for suspicious activity (brute force, privilege escalation)
- Logs stored locally and overwritten (not forwarded to SIEM)
- No monitoring of API abuse
- Audit trail is incomplete → forensics fails

**Attacker's perspective:** If there's no logging, there's no detection. If there's no detection, the attacker has unlimited time.

**Defence:**
- Log all authentication events (4624, 4625), access control failures, admin actions
- Forward logs to centralised SIEM (Splunk, ELK)
- Set up alerts: >10 failed logins in 5 min, new admin accounts, off-hours access
- Protect logs from tampering (write-once storage)

---

### A10 — Mishandling of Exceptional Conditions 🆕 New

**What is it?** Applications that don't handle errors, exceptions, or edge cases properly — leading to security failures, crashes, or unintended "fail open" states.

**Examples:**
- Authentication error is caught and silently ignored → user authenticated anyway ("fail open")
- Division by zero / null pointer reveals stack trace with internal paths
- Race condition: two requests processed simultaneously bypass a check
- Business logic: if payment fails, order is still placed due to poor exception handling

**Fail open vs Fail closed:**
```
Fail open  (insecure): Error occurs → grant access anyway → ❌
Fail closed (secure):  Error occurs → deny access → ✅
```

**Defence:**
- Always fail closed (deny by default on error)
- Never expose stack traces or internal error details to users
- Test exception paths explicitly
- Use structured error handling — catch specific exceptions, don't catch-all and ignore

---

## 2025 vs 2021 Comparison

| 2021 | 2025 | Change |
|------|------|--------|
| A01: Broken Access Control | A01: Broken Access Control | ↔ Same |
| A02: Cryptographic Failures | **A02: Security Misconfiguration** | ⬆️ Jumped from #5 |
| A03: Injection | **A03: Software Supply Chain Failures** | 🆕 New |
| A04: Insecure Design | A04: Cryptographic Failures | Moved down |
| A05: Security Misconfiguration | A05: Injection | Moved down |
| A06: Vulnerable Components | A06: Insecure Design | Same position |
| A07: Auth Failures | A07: Auth Failures | ↔ Same |
| A08: Data Integrity Failures | A08: Data Integrity Failures | ↔ Same |
| A09: Logging Failures | A09: Logging Failures | ↔ Same |
| A10: SSRF | **A10: Mishandling of Exceptional Conditions** | 🆕 New |
| — | SSRF absorbed into A01 | Merged |

---

## Attack Payload Quick Reference

### SQL Injection
```sql
' OR '1'='1' --                           # Auth bypass
1' UNION SELECT null, version() --        # DB version
1' UNION SELECT table_name,null FROM information_schema.tables --
```

### XSS (A05 Injection)
```html
<script>alert('XSS')</script>
<img src=x onerror=alert(document.cookie)>
<svg onload=alert('XSS')>
<Script>alert('XSS')</Script>             <!-- case bypass -->
```

### Command Injection (A05)
```bash
; whoami          # Linux
| dir             # Windows
`id`              # Backtick
$(whoami)         # $() syntax
```

### SSRF (now A01)
```
http://169.254.169.254/latest/meta-data/  # AWS metadata
http://localhost:8080/admin               # Internal service
file:///etc/passwd                        # Local file
```
