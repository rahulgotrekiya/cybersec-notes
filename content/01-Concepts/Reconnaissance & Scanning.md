---
type: concept
subject: ehpt
tags: [cybersecurity, concept, ehpt, concepts, reconnaissance, osint, nmap, nessus, scanning, cvss]
status: learning
created: 2026-07-02
---

# Reconnaissance & Scanning

> 📝 **Concept Note** — EHPT Unit II | Source: MCA Sem III Syllabus

---

## The Problem It Solves

Before attacking a target, an ethical hacker needs to understand it — what systems exist, what services run, what vulnerabilities are present. Reconnaissance and scanning provide this intelligence.

---

## Passive Reconnaissance (OSINT)

Information gathered **without directly touching the target** — the target has no idea you're researching them.

### Google Dorking

| Operator | Purpose | Example |
|----------|---------|---------|
| `site:` | Restrict to domain | `site:example.com login` |
| `filetype:` | Find specific files | `filetype:pdf site:example.com` |
| `intitle:` | Search page titles | `intitle:"index of" passwords` |
| `inurl:` | Search URLs | `inurl:admin login` |
| `cache:` | View Google's cached version | `cache:example.com` |
| `-` | Exclude terms | `site:example.com -www` |

**Dangerous dorks (for authorised testing):**
```
filetype:sql "INSERT INTO" password
intitle:"index of" "wp-config.php"
inurl:"/phpinfo.php"
filetype:env "DB_PASSWORD"
```

### WHOIS Lookups
Reveals domain registration info — registrant name, email, registrar, creation/expiry dates, name servers.

```bash
whois example.com
```

### DNS Reconnaissance

```bash
# Query A records
dig example.com A

# Query mail servers
dig example.com MX

# Query name servers
dig example.com NS

# Query all TXT records (SPF, DKIM)
dig example.com TXT

# Attempt zone transfer (reveals ALL records if misconfigured)
dig @ns1.example.com example.com AXFR
```

> ⚠️ A successful zone transfer is a **critical security finding** — it exposes the entire DNS structure.

### Shodan — Search Engine for IoT
Searches for **internet-connected devices** — webcams, servers, routers, industrial control systems.

```
# Find Apache servers in India
apache country:IN

# Find open MongoDB instances
port:27017

# Find exposed RDP
port:3389 country:IN
```

### theHarvester
Automated email and subdomain harvesting:
```bash
theHarvester -d example.com -b google,bing,linkedin -l 500
```

### Maltego Community Edition
Visual link analysis tool — maps relationships between domains, IPs, emails, people, social profiles.

### LinkedIn OSINT
- Employee names → usernames (often firstname.lastname)
- Technologies mentioned in job postings → tech stack
- Organisational structure → target for social engineering

### Recon-NG Framework
Modular reconnaissance framework (like Metasploit for recon):
```
recon-ng
[recon-ng] > marketplace search
[recon-ng] > modules load recon/domains-hosts/google_site_web
[recon-ng] > options set SOURCE example.com
[recon-ng] > run
```

---

## Active Scanning

### Nmap Scan Types

| Scan | Flag | How It Works | Stealth | Use |
|------|------|-------------|---------|-----|
| **TCP SYN** | `-sS` | Sends SYN, reads SYN-ACK or RST, never completes | High | Default, stealthy |
| **TCP Connect** | `-sT` | Full TCP 3-way handshake | Low (logged) | When no root |
| **UDP** | `-sU` | Sends UDP packets, waits for ICMP unreachable | Medium | DNS, SNMP |
| **Stealth FIN** | `-sF` | Sends FIN flag only | High | Bypass simple firewalls |
| **Xmas** | `-sX` | Sends FIN+PSH+URG flags | High | Bypass simple firewalls |
| **Null** | `-sN` | Sends no flags | High | Bypass simple firewalls |
| **Aggressive** | `-A` | OS + version + scripts + traceroute | None | Full enumeration |

### Version & OS Detection
```bash
# Service version detection
nmap -sV 192.168.1.10

# OS fingerprinting
sudo nmap -O 192.168.1.10

# Both + scripts
sudo nmap -A 192.168.1.10
```

### NSE Scripts (Nmap Scripting Engine)
```bash
# Default scripts
nmap --script=default 192.168.1.10

# Vulnerability scripts
nmap --script=vuln 192.168.1.10

# Specific script
nmap --script=http-enum 192.168.1.10
```

### Timing Templates

| Template | Name | Use Case |
|----------|------|----------|
| T0 | Paranoid | IDS evasion (very slow) |
| T1 | Sneaky | IDS evasion |
| T2 | Polite | Reduced bandwidth |
| T3 | Normal | Default |
| T4 | Aggressive | Fast, reliable network |
| T5 | Insane | Very fast, may miss results |

### Banner Grabbing with Netcat
```bash
# Connect to service and read banner
nc -v 192.168.1.10 22
# Output: SSH-2.0-OpenSSH_7.6p1

nc -v 192.168.1.10 80
# Then type: HEAD / HTTP/1.0
# Output: Server: Apache/2.4.29
```

---

## Network Enumeration

### SMB Enumeration
```bash
# Comprehensive SMB enumeration
enum4linux -a 192.168.1.10

# Discovers: users, shares, groups, OS info, password policy
```

### SNMP Walking
```bash
# Walk SNMP tree (if community string is known/default)
snmpwalk -v2c -c public 192.168.1.10
```

### SMTP User Enumeration
```bash
# Verify if user exists via VRFY command
telnet 192.168.1.10 25
VRFY admin
# 250 = user exists, 550 = doesn't exist
```

---

## Vulnerability Scanning — Nessus

### What is Nessus?
Automated vulnerability scanner that checks systems against a database of known vulnerabilities.

> 🔗 Deep dive: [[Nessus]]

### Nessus vs Nmap
- **Nmap:** "What ports and services are running?" (discovery)
- **Nessus:** "What known vulnerabilities exist?" (assessment)

### CVSS v3.1 Score Components

| Metric | Values | Meaning |
|--------|--------|---------|
| **Attack Vector (AV)** | Network / Adjacent / Local / Physical | How attacker reaches target |
| **Attack Complexity (AC)** | Low / High | Difficulty of exploitation |
| **Privileges Required (PR)** | None / Low / High | Auth needed before exploit |
| **User Interaction (UI)** | None / Required | Does victim need to do something? |
| **Scope (S)** | Unchanged / Changed | Does exploit affect other components? |
| **Confidentiality (C)** | None / Low / High | Impact on data secrecy |
| **Integrity (I)** | None / Low / High | Impact on data accuracy |
| **Availability (A)** | None / Low / High | Impact on system uptime |

### CVSS Score Ranges

| Score | Rating | Example |
|-------|--------|---------|
| 0.0 | None | Informational finding |
| 0.1–3.9 | Low | Minor information disclosure |
| 4.0–6.9 | Medium | Stored XSS |
| 7.0–8.9 | High | SQL injection |
| 9.0–10.0 | Critical | Remote code execution, no auth |

### CVE Lookup
- **CVE** = Common Vulnerabilities and Exposures
- Format: CVE-YYYY-NNNNN (e.g., CVE-2021-44228 = Log4Shell)
- Lookup: https://nvd.nist.gov/

### False Positive Concept
- Scanner reports a vulnerability that **doesn't actually exist**
- Must be verified manually before reporting to client
- Common with version-based detection (patched but version unchanged)

---

## Related Concepts

- [[Ethical Hacking Fundamentals]] — Context and legal framework
- [[Exploitation & Post-Exploitation]] — What comes next
- [[Network Traffic Analysis]] — Wireshark for traffic analysis
- [[Nmap Commands Cheatsheet|Nmap Cheatsheet]] ⚡
- [[CVSS Scoring Reference]] ⚡
- [[EHPT MOC]] — Subject navigation
