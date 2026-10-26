---
type: concept
subject: csf-ns
tags: [cybersecurity, concept, csf-ns, concepts, wireshark, nmap, splunk, traffic-analysis, network-security]
status: learning
created: 2026-07-02
---

# Network Traffic Analysis

> 📝 **Concept Note** — CSF & NS Unit IV | Source: MCA Sem III Syllabus

---

## The Problem It Solves

You can't defend what you can't see. Network traffic analysis gives security analysts **visibility** into what's happening on the network — who's talking to whom, what protocols are being used, and whether anything looks malicious.

---

## Wireshark — Packet Capture & Analysis

### What is Wireshark?
An open-source packet analyser that captures and interactively displays network traffic in real-time.

> 🔗 Deep dive: [[Wireshark]]

### Key Capabilities
- Capture live traffic on any network interface
- Load and analyse pcap files
- Apply display filters to isolate specific traffic
- Follow TCP/UDP streams to reconstruct conversations
- View protocol hierarchy statistics
- Export specific packets for further analysis

### Display Filters (Most Used)

```
ip.addr == 192.168.1.10       # All traffic to/from this IP
tcp.port == 80                 # All HTTP traffic
http                           # Only HTTP protocol
dns                            # Only DNS queries/responses
tcp.flags.syn == 1             # Only SYN packets (connection attempts)
http.request.method == "POST"  # Only HTTP POST requests
!(arp)                         # Exclude ARP traffic
ip.src == 10.0.0.5 && tcp.port == 443  # HTTPS from specific source
```

### Following a TCP Stream
Right-click a packet → Follow → TCP Stream → Shows complete conversation in readable text

**Security use cases:**
- See HTTP credentials sent in **cleartext** (never use plain HTTP for login)
- Reconstruct file transfers
- Read complete email conversations
- Identify data exfiltration content

### Identifying Suspicious Traffic
- Unusual DNS queries (long subdomains = possible DNS tunnelling)
- Traffic to known-bad IPs — reputation-check with [[VirusTotal]]
- HTTP traffic containing SQL injection patterns
- Beaconing patterns (regular interval connections to same host) — see it host-side with [[netstat & ss]], confirm the remote IP on [[VirusTotal]]
- Large data transfers to external IPs

> 💡 **Wire-side vs host-side:** [[Wireshark]] shows traffic on the network; [[netstat & ss]] shows the same connections from the host's OS *and the owning process*. Use both. For an attacker's external view of what you expose, see [[Shodan]].

---

## Nmap — Network Scanning

### What is Nmap?
A network scanner that discovers hosts, open ports, running services, and OS details on a network.

> 🔗 Deep dive: [[Nmap]]

### Why Port Scanning Matters
- **Defenders:** Know what's exposed, verify firewall rules, find unauthorized services
- **Attackers:** Reconnaissance — find entry points

### Scan Types

| Scan | Flag | Description | Stealth | Speed |
|------|------|------------|---------|-------|
| TCP SYN (Half-open) | `-sS` | Sends SYN, reads response, doesn't complete handshake | High | Fast |
| TCP Connect | `-sT` | Completes full TCP handshake | Low (logged) | Medium |
| UDP | `-sU` | Scans UDP ports | Medium | Slow |
| Host Discovery | `-sn` | Ping sweep — find live hosts only | N/A | Very fast |
| Version Detection | `-sV` | Identify service versions | Low | Medium |
| OS Fingerprinting | `-O` | Identify operating system | Low | Medium |

### Common Commands

```bash
# Quick scan of target
nmap 192.168.1.10

# SYN scan (stealthy, needs root)
sudo nmap -sS 192.168.1.10

# Full scan with version and OS detection
sudo nmap -sS -sV -O 192.168.1.10

# Scan all ports
nmap -p- 192.168.1.10

# Aggressive scan (OS, version, scripts, traceroute)
sudo nmap -A 192.168.1.10

# Save output to file
nmap -oN scan_results.txt 192.168.1.10

# NSE scripts
nmap --script=default 192.168.1.10
nmap --script=vuln 192.168.1.10
```

### Interpreting Results

```
PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 7.6
80/tcp   open  http        Apache 2.4.29
443/tcp  open  ssl/https   Apache 2.4.29
3306/tcp open  mysql       MySQL 5.7.25
8080/tcp open  http-proxy  
```

**Port states:**
- **open** — Service accepting connections
- **closed** — No service listening, but port reachable
- **filtered** — Firewall blocking, can't determine state

---

## Splunk — SIEM Basics

### What is a SIEM?
**Security Information and Event Management** — collects, normalises, correlates, and analyses logs from across the entire infrastructure in one place.

> 🔗 Deep dive: [[Splunk]]

### Why SIEM Matters
Without SIEM, logs are scattered across hundreds of devices. With SIEM:
- **Centralised visibility** — all logs in one place
- **Correlation** — connect events across devices
- **Alerting** — automated detection of suspicious patterns
- **Search** — query across all logs instantly

### Splunk Free Basics

**Adding a data source:**
Settings → Data Inputs → Upload/Monitor → Select log file

**Basic SPL (Search Processing Language):**

```spl
# Search all events
index=main

# Search by keyword
index=main "failed login"

# Filter by source type
index=main sourcetype="WinEventLog:Security"

# Search specific Event ID
index=main EventCode=4625

# Count events
index=main EventCode=4625 | stats count

# Top 10 source IPs
index=main EventCode=4625 | top src_ip

# Timeline chart
index=main EventCode=4625 | timechart count by src_ip
```

### Key Concepts
- **Index** — Where data is stored (like a database)
- **Source type** — Format of the data (Windows Event Log, syslog, etc.)
- **Events** — Individual log entries
- **Fields** — Key-value pairs extracted from events

---

## Network Reconnaissance — Defender's View

### Defensive Use of Network Tools

**netstat / ss (Linux):** → full tool note: [[netstat & ss]]
```bash
# What's listening on this server?
ss -tulpn

# Active connections
ss -t state established

# Find process using a port
ss -tulpn | grep :8080
```

**Common command-line tools:**

| Tool | Purpose |
|------|---------|
| `netstat -tulpn` | Show listening ports with PIDs |
| `ss -tulpn` | Modern replacement for netstat |
| `tcpdump` | Command-line packet capture |
| `arp -a` | Show ARP cache |
| `traceroute` | Trace packet route |

### Ethics of Scanning
- **Always** get written authorization before scanning
- Scanning without permission is **illegal** under IT Act
- Even defensive scanning of your own network should be documented
- Test environments should be **isolated** (host-only networks)

---

## Related Concepts

- [[OSI Model & TCP-IP Security]] — Understanding what you're seeing in captures
- [[Network Defence Mechanisms]] — What the traffic passes through
- [[SOC Operations & Incident Lifecycle]] — Using these tools in a SOC
- [[Wireshark Commands Cheatsheet|Wireshark Cheatsheet]] ⚡
- [[Nmap Commands Cheatsheet|Nmap Cheatsheet]] ⚡
- [[CSF & NS MOC]] — Subject navigation
