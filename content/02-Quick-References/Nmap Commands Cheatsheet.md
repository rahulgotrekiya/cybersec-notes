---
type: reference
subject: shared
tags: [cybersecurity, reference, cheatsheet, nmap, scanning, network-security, quickref]
status: learning
created: 2026-07-02
---

# Nmap Commands Cheatsheet ⚡

> ⚡ **Quick Reference** — Nmap scan types, flags, and common usage patterns

---

## Host Discovery
```bash
nmap -sn 192.168.1.0/24          # Ping sweep (no port scan)
nmap -sn -PS22,80 192.168.1.0/24 # TCP SYN discovery on ports 22,80
nmap -sn -PE 192.168.1.0/24      # ICMP echo discovery
```

## Port Scanning
```bash
# TCP SYN scan (default, stealthy, needs root)
sudo nmap -sS 192.168.1.10

# TCP Connect scan (no root needed, logged on target)
nmap -sT 192.168.1.10

# UDP scan (slow)
sudo nmap -sU 192.168.1.10

# Stealth scans
sudo nmap -sF 192.168.1.10      # FIN scan
sudo nmap -sX 192.168.1.10      # Xmas scan
sudo nmap -sN 192.168.1.10      # Null scan

# Specific ports
nmap -p 22,80,443 192.168.1.10  # Scan specific ports
nmap -p- 192.168.1.10           # Scan ALL 65535 ports
nmap -p 1-1000 192.168.1.10     # Port range
nmap -F 192.168.1.10            # Fast scan (top 100 ports)
```

## Service & OS Detection
```bash
nmap -sV 192.168.1.10           # Service version detection
sudo nmap -O 192.168.1.10       # OS fingerprinting
sudo nmap -A 192.168.1.10       # Aggressive (OS + version + scripts + traceroute)
```

## NSE Scripts
```bash
nmap --script=default 192.168.1.10           # Default scripts
nmap --script=vuln 192.168.1.10              # Vulnerability scripts
nmap --script=http-enum 192.168.1.10         # HTTP enumeration
nmap --script=smb-enum-shares 192.168.1.10   # SMB shares
nmap --script=banner 192.168.1.10            # Service banners
```

## Timing Templates
```bash
nmap -T0 192.168.1.10     # Paranoid (IDS evasion)
nmap -T1 192.168.1.10     # Sneaky
nmap -T2 192.168.1.10     # Polite
nmap -T3 192.168.1.10     # Normal (default)
nmap -T4 192.168.1.10     # Aggressive
nmap -T5 192.168.1.10     # Insane
```

## Output Formats
```bash
nmap -oN results.txt 192.168.1.10    # Normal text output
nmap -oX results.xml 192.168.1.10    # XML output
nmap -oG results.grep 192.168.1.10   # Grepable output
nmap -oA results 192.168.1.10        # All formats at once
```

## Common Combinations
```bash
# Full enumeration
sudo nmap -sS -sV -O -p- -T4 192.168.1.10

# Quick vulnerability scan
nmap -sV --script=vuln -T4 192.168.1.10

# Stealthy scan
sudo nmap -sS -T2 -f 192.168.1.10    # -f fragments packets

# Complete audit
sudo nmap -A -p- -T4 -oA full_scan 192.168.1.10
```

## Port States

| State | Meaning |
|-------|---------|
| **open** | Service accepting connections |
| **closed** | Reachable but no service |
| **filtered** | Firewall blocking — can't determine |
| **unfiltered** | Reachable, state unknown |
| **open\|filtered** | Can't determine which |
