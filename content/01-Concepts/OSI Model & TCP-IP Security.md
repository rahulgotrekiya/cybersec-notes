---
type: concept
subject: csf-ns
tags:
  - cybersecurity
  - concept
  - csf-ns
  - concepts
  - osi-model
  - tcp-ip
  - networking
  - ipv4
  - dns
  - ports
status: done
created: 2026-07-02
---

# OSI Model & TCP-IP Security

> 📝 **Concept Note** — CSF & NS Unit II | Source: MCA Sem III Syllabus

---

## The Problem It Solves

To defend a network, you must understand **how** networks work at every layer. Each layer has its own vulnerabilities and defences. Without this knowledge, you can't:
- Identify which layer an attack is targeting
- Deploy the right defence at the right layer
- Analyse network traffic effectively
- Troubleshoot security issues

---

## OSI Model — 7 Layers with Security Context

The Open Systems Interconnection (OSI) model divides network communication into 7 layers:

```
┌──────────────────────────────────────────────────┐
│ Layer 7  APPLICATION    │ HTTP, DNS, FTP, SMTP   │
├──────────────────────────────────────────────────┤
│ Layer 6  PRESENTATION   │ SSL/TLS, Encryption    │
├──────────────────────────────────────────────────┤
│ Layer 5  SESSION        │ Session management     │
├──────────────────────────────────────────────────┤
│ Layer 4  TRANSPORT      │ TCP, UDP               │
├──────────────────────────────────────────────────┤
│ Layer 3  NETWORK        │ IP, ICMP, Routers      │
├──────────────────────────────────────────────────┤
│ Layer 2  DATA LINK      │ MAC, Switches, ARP     │
├──────────────────────────────────────────────────┤
│ Layer 1  PHYSICAL       │ Cables, Hubs, Signals  │
└──────────────────────────────────────────────────┘
```

> 💡 Memory trick: **"All People Seem To Need Data Processing"** (top to bottom) or **"Please Do Not Throw Sausage Pizza Away"** (bottom to top)

---

### Layer 7 — Application Layer

**What it does:** Provides network services directly to user applications (email, web browsers, file transfer)

**Protocols:** HTTP, HTTPS, FTP, SMTP, DNS, SSH, Telnet, SNMP

**Security attacks at this layer:**
- **HTTP injection** — Injecting malicious code into web requests
- **SQL injection** — Manipulating database queries through web forms
- **Cross-Site Scripting (XSS)** — Injecting scripts into web pages
- **DNS poisoning** — Redirecting DNS queries to malicious servers
- **Phishing** — Fraudulent applications/emails

**Defences:**
- Web Application Firewalls (WAF)
- Input validation and output encoding
- HTTPS (TLS encryption)
- DNS Security Extensions (DNSSEC)

---

### Layer 4 — Transport Layer

**What it does:** End-to-end communication, reliability (TCP), speed (UDP)

**Protocols:** TCP (reliable, connection-oriented), UDP (fast, connectionless)

**Security attacks at this layer:**
- **TCP SYN Flood** — Sends thousands of half-open connections, exhausting server resources
  ```
  Attacker → SYN → Server
  Server → SYN-ACK → (attacker never responds)
  Server waits... resources consumed
  × 1000s of connections = server crashes
  ```
- **UDP flood** — Overwhelming target with UDP packets
- **Port scanning** — Discovering open services

**Defences:**
- SYN cookies
- Rate limiting
- Firewall rules (block unused ports)
- TLS for encrypted transport

---

### Layer 3 — Network Layer

**What it does:** Routing packets between networks, logical addressing (IP addresses)

**Protocols:** IP (IPv4/IPv6), ICMP, IGMP

**Security attacks at this layer:**
- **IP spoofing** — Forging the source IP address in packets
- **ICMP flood (Ping of Death)** — Overwhelming with ping requests
- **Route manipulation** — Redirecting traffic through attacker's network
- **IP fragmentation attacks** — Exploiting packet reassembly

**Defences:**
- Ingress/egress filtering
- IPsec (encrypted IP communication)
- Anti-spoofing rules on routers
- Access Control Lists (ACLs)

---

### Layer 2 — Data Link Layer

**What it does:** Node-to-node data transfer, MAC addressing, error detection

**Protocols:** Ethernet, ARP, PPP

**Security attacks at this layer:**
- **ARP spoofing/poisoning** — Associating attacker's MAC with a legitimate IP
  ```
  Normal:  Gateway IP 192.168.1.1 → MAC AA:BB:CC:DD:EE:FF
  Attack:  Gateway IP 192.168.1.1 → MAC [ATTACKER'S MAC]
  Result:  All traffic meant for gateway goes to attacker (MitM)
  ```
- **MAC flooding** — Overwhelming switch's MAC table, causing it to act like a hub
- **VLAN hopping** — Escaping VLAN isolation

**Defences:**
- Dynamic ARP Inspection (DAI)
- Port security (limit MACs per port)
- 802.1X authentication
- VLAN configuration best practices

---

## Attack Mapping to OSI Layers

| Attack | Layer | Protocol Exploited | Defence |
|--------|-------|--------------------|---------|
| ARP Spoofing | L2 | ARP | DAI, static ARP |
| MAC Flooding | L2 | Ethernet | Port security |
| IP Spoofing | L3 | IP | Ingress filtering |
| ICMP Flood | L3 | ICMP | Rate limiting |
| TCP SYN Flood | L4 | TCP | SYN cookies |
| Port Scanning | L4 | TCP/UDP | Firewall rules |
| HTTP Injection | L7 | HTTP | WAF, input validation |
| DNS Poisoning | L7 | DNS | DNSSEC |
| SQL Injection | L7 | HTTP | Parameterised queries |

---

## TCP/IP Model

The practical model used on the internet — maps to OSI:

```
TCP/IP Model          OSI Model
┌──────────────┐     ┌──────────────┐
│ Application  │  ←  │ Application  │
│              │  ←  │ Presentation │
│              │  ←  │ Session      │
├──────────────┤     ├──────────────┤
│ Transport    │  ←  │ Transport    │
├──────────────┤     ├──────────────┤
│ Internet     │  ←  │ Network      │
├──────────────┤     ├──────────────┤
│ Network      │  ←  │ Data Link    │
│ Access       │  ←  │ Physical     │
└──────────────┘     └──────────────┘
```

>💡 Memory trick: "NITA" (bottom to top)


---

## IPv4 Addressing

### Structure
- **32-bit** address, written as 4 octets: `192.168.1.100`
- Each octet = 8 bits (0–255)

### Classes (classful — largely historical)

| Class | Range | Default Mask | Purpose |
|-------|-------|-------------|---------|
| A | 1.0.0.0 – 126.255.255.255 | /8 (255.0.0.0) | Large networks |
| B | 128.0.0.0 – 191.255.255.255 | /16 (255.255.0.0) | Medium networks |
| C | 192.0.0.0 – 223.255.255.255 | /24 (255.255.255.0) | Small networks |
| D | 224.0.0.0 – 239.255.255.255 | — | Multicast |
| E | 240.0.0.0 – 255.255.255.255 | — | Experimental |

### Private vs Public IP Ranges

| Private Range | CIDR | Commonly Used For |
|--------------|------|-------------------|
| 10.0.0.0 – 10.255.255.255 | 10.0.0.0/8 | Large enterprise LANs |
| 172.16.0.0 – 172.31.255.255 | 172.16.0.0/12 | Medium networks |
| 192.168.0.0 – 192.168.255.255 | 192.168.0.0/16 | Home/small office |

> 💡 Private IPs are **not routable on the internet**. NAT (Network Address Translation) converts private → public when accessing the internet.

### Subnetting Basics (CIDR)
**CIDR notation** (`/n`) says how many leading bits are the **network** part; the rest are **host** bits. An IPv4 address is 32 bits, so hosts = 2^(32−n).

- **/24** = 256 addresses (254 usable) — `255.255.255.0`
- **/16** = 65,536 addresses — `255.255.0.0`
- **/8** = 16.7 million addresses — `255.0.0.0`

> 2 addresses are never usable for hosts: the **network address** (all host bits 0) and the **broadcast** (all host bits 1). That's why /24 gives 256 − 2 = **254** usable.

**Worked example — `192.168.1.0/24`:**
- Network bits = 24, host bits = 8 → 2⁸ = 256 addresses
- **Network address:** `192.168.1.0` · **Broadcast:** `192.168.1.255`
- **Usable host range:** `192.168.1.1` → `192.168.1.254`
- Split it into two **/25** subnets (host bits 7 → 128 each): `192.168.1.0/25` (`.1`–`.126`) and `192.168.1.128/25` (`.129`–`.254`). Smaller subnets = tighter [[Network Defence Mechanisms|segmentation]], limiting lateral movement.

**Rule of thumb:** bigger `/n` = smaller network. `/32` is a single host; `/30` (4 addresses, 2 usable) is common for point-to-point links.

### IPv6 Overview
- **128-bit** addresses (vs 32-bit IPv4)
- Written as 8 groups of 4 hex digits: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`
- Massive address space: 3.4 × 10³⁸ addresses
- Built-in IPsec support

---

## Common Ports — Security Critical

| Port | Protocol | Service | Security Note |
|------|----------|---------|--------------|
| 22 | TCP | SSH | Secure remote access ✅ |
| 23 | TCP | Telnet | **Unencrypted** — never use ❌ |
| 25 | TCP | SMTP | Email sending — often targeted for spam |
| 53 | TCP/UDP | DNS | Targeted for DNS attacks, tunnelling |
| 80 | TCP | HTTP | **Unencrypted** web traffic |
| 443 | TCP | HTTPS | Encrypted web traffic ✅ |
| 3389 | TCP | RDP | Remote Desktop — common attack target |

> 🔗 Full reference: [[Common Ports & Protocols Reference]]

---

## DNS Resolution Process

```
1. User types www.example.com in browser
2. Browser checks local cache → Not found
3. OS checks hosts file (/etc/hosts) → Not found
4. Query sent to Recursive DNS Resolver (ISP's DNS)
5. Resolver checks its cache → Not found
6. Resolver queries Root DNS Server → "Ask .com TLD server"
7. Resolver queries .com TLD Server → "Ask example.com's authoritative server"
8. Resolver queries Authoritative DNS Server → "IP is 93.184.216.34"
9. Resolver caches the result and returns IP to browser
10. Browser connects to 93.184.216.34
```

### DNS Record Types

| Record | Purpose | Example |
|--------|---------|---------|
| **A** | Maps domain to IPv4 | example.com → 93.184.216.34 |
| **AAAA** | Maps domain to IPv6 | example.com → 2606:2800:220:1:... |
| **MX** | Mail server for domain | example.com → mail.example.com |
| **CNAME** | Alias for another domain | www.example.com → example.com |
| **TXT** | Text records (SPF, DKIM) | SPF records for email auth |
| **NS** | Authoritative name servers | ns1.example.com |

---

## HTTPS — What It Protects and What It Doesn't

### What HTTPS Protects ✅
- **Encryption** of data in transit (confidentiality)
- **Integrity** verification (data not tampered with)
- **Authentication** of server identity (via SSL/TLS certificate)

### What HTTPS Does NOT Protect ❌
- Does not guarantee the **website is legitimate** (attackers can get HTTPS certificates)
- Does not protect **data at rest** on the server
- Does not prevent **server-side vulnerabilities** (SQLi, XSS still possible)
- Does not hide **which domain** you're visiting (SNI reveals it)
- Does not protect against **malware on the endpoint**

---

## Related Concepts

- [[What is Cybersecurity]] — The broader context
- [[CIA Triad]] — Security properties per layer
- [[Network Defence Mechanisms]] — Controls at each layer
- [[Network Traffic Analysis]] — Analysing traffic at various layers
- [[CSF & NS MOC]] — Subject navigation

---

## Tags

#cybersecurity #csf-ns #concepts #osi-model #tcp-ip #networking #ipv4 #dns #ports

---

## Resources

- Course: MCA Sem III — CSF & NS, Unit II
- "Network Security Essentials" — William Stallings
- Related: [[00-Cyber Security MOC|Cyber Security MOC]] for navigation
