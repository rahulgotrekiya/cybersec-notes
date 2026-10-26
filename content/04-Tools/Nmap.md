---
type: tool
subject: shared
tags: [cybersecurity, tool, nmap, network-scanning, reconnaissance]
status: learning
created: 2026-07-02
---

# Nmap

> 🔧 **Tool Note** — Category: Network Scanning | Open Source

---

## 📌 Quick Overview
**What is it?** Network exploration and security auditing tool that discovers hosts, open ports, services, and operating systems.

**Official Website:** https://nmap.org/
**License:** Custom open-source (Nmap Public Source License)
**Used in:** CSF & NS, EHPT

## 🎯 What Problem Does It Solve?
- Discover what devices and services exist on a network
- Identify open ports and running service versions
- Detect operating systems remotely
- Find vulnerabilities via NSE scripts
- Verify firewall rules and network security

## ⚙️ How It Works
Nmap learns about a target by **sending crafted packets and interpreting the replies** — it never needs to log in. Everything keys off the **TCP 3-way handshake** (`SYN → SYN-ACK → ACK`) and how hosts respond to unusual packets.

**Port scanning — how each scan type probes:**
| Scan | What Nmap sends | Open port | Closed port | Notes |
|---|---|---|---|---|
| `-sS` **SYN** (half-open) | `SYN` | gets `SYN-ACK`, replies `RST` (never completes) | gets `RST` | Default; fast; **stealthier** — no full connection, so often unlogged. Needs root. |
| `-sT` **Connect** | full `SYN/SYN-ACK/ACK` | OS completes connection | `RST` | Uses OS `connect()`; no root needed but **logged** by the app. |
| `-sU` **UDP** | empty UDP datagram | usually no reply / app data | ICMP port-unreachable | Slow (no handshake); relies on timeouts. |
| `-sn` **host discovery** | ICMP echo + ARP/TCP pings | host replies = "up" | — | "Ping scan", no ports. |

**Beyond open/closed:**
- **`-sV` version detection** — connects to the open port and matches the service's banner/responses against the `nmap-service-probes` database → "vsftpd 2.3.4", "Apache 2.4.7".
- **`-O` OS fingerprinting** — sends a series of odd TCP/IP probes and compares quirks (TTL, TCP window size, options ordering) against `nmap-os-db`; every OS's stack behaves slightly differently.
- **NSE (`--script`)** — a Lua engine of 600+ scripts that go further: check for specific CVEs, brute-force logins, pull SMB shares. `--script=default` runs the safe common set.

A port can be **open**, **closed** (reachable, nothing listening), or **filtered** (a firewall dropped the probe — no reply at all). Distinguishing these is the core skill.

## 🚀 Key Features
1. TCP SYN, Connect, UDP, and stealth scan types
2. Service version detection (-sV)
3. OS fingerprinting (-O)
4. Nmap Scripting Engine (NSE) with 600+ scripts
5. Multiple output formats (text, XML, grepable)
6. Timing templates for speed vs stealth tradeoff

## 🔬 Typical Workflow (Practical 2 — service enumeration)
```bash
nmap -sn 192.168.56.0/24              # 1. discover live hosts
sudo nmap -sS -p- 192.168.56.101      # 2. full SYN scan, all 65535 ports
sudo nmap -sV -O 192.168.56.101       # 3. version + OS detection on found ports
nmap -sV --script=default 192.168.56.101 -oN scan.txt   # 4. NSE + save output
```
Build the service inventory from the results, then research versions (e.g. take "vsftpd 2.3.4" to a CVE lookup — **without exploiting**, per the lab's defender framing).

## ⚠️ Gotchas / Limitations
- **`-sS` and `-O` need root** (raw packets); `-sT` works without but is noisy/logged.
- **Scanning without authorisation is illegal** — only scan targets you own or are permitted to (Metasploitable 2 in an isolated host-only network for the lab).
- `filtered` results mean a firewall/IDS is in the way — the port state is genuinely unknown, not "closed".
- Aggressive timing (`-T4/-T5`) is faster but louder and can miss ports on lossy links; IDS like [[Snort]]/[[Suricata]] will flag fast scans.

## 💻 Installation
```bash
# Linux
sudo apt install nmap

# Kali Linux — pre-installed

# Verify
nmap --version
```

## ⚡ Quick Reference
→ [[Nmap Commands Cheatsheet]]

## 🔗 Related Notes
- [[Network Traffic Analysis]]
- [[Reconnaissance & Scanning]]
- [[Wireshark]] — Packet-level analysis after scanning
