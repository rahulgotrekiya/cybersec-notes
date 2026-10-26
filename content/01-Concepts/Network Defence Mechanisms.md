---
type: concept
subject: csf-ns
tags: [cybersecurity, concept, csf-ns, concepts, firewalls, ids, ips, vpn, zero-trust, network-security, wireless-security]
status: learning
created: 2026-07-02
---

# Network Defence Mechanisms

> 📝 **Concept Note** — CSF & NS Unit III | Source: MCA Sem III Syllabus

---

## The Problem It Solves

A network without defence is like a building with no walls, no doors, and no locks. Network defence mechanisms provide the walls, doors, locks, guards, and surveillance cameras of the digital world.

---

## 🧭 Unit III Deep Dives

> This note is the **Unit III overview**. Each topic below has a dedicated deep-dive note explaining **how it actually works** at mechanism level, plus supplementary videos.

| Deep dive | What's inside |
|---|---|
| [[Firewalls]] | 5-tuple matching, the **stateful connection table**, DPI/NGFW app-ID, rule order & implicit deny, egress filtering, why NAT ≠ security |
| [[IDS & IPS]] | Detection pipeline, **why normalisation defeats evasion**, passive SPAN vs inline placement, signature vs anomaly, FP/FN and tuning |
| [[DMZ & Network Segmentation]] | **North-south vs east-west**, lateral movement, single vs dual-firewall DMZ, **802.1Q VLAN tagging**, VLAN hopping, micro-segmentation |
| [[VPN]] | Encapsulation, **IPsec IKE phase 1/2**, ESP vs AH, tunnel vs transport mode, SSL/TLS VPN, WireGuard, split tunnelling, VPN ≠ anonymity |
| [[Proxy Servers]] | Two-connection model, forward vs reverse, transparent vs explicit, **TLS interception via internal CA**, WAF/load balancing |
| [[Zero Trust Architecture]] | **NIST SP 800-207 PE/PA/PEP**, continuous verification, least privilege, assume breach, **ZTNA vs VPN** |
| [[Wireless Security]] | **WPA2 4-way handshake (PTK/GTK, nonces, MIC)**, offline cracking, 802.1X/RADIUS, **WPA3 SAE**, evil twin, deauth, **KRACK** |

---

## Firewalls

### What is a Firewall?
A network security device that monitors and controls incoming and outgoing traffic based on predetermined security rules.

### Types of Firewalls

#### 1. Packet Filtering Firewall
- Examines individual packets against rules (source/dest IP, port, protocol)
- **Stateless** — doesn't track connection state
- Fast but limited

#### 2. Stateful Inspection Firewall
- Tracks **state of active connections**
- Understands response packets belong to allowed requests
- More secure than packet filtering

#### 3. Application-Layer / Next-Gen Firewall (NGFW)
- Inspects actual application data (Layer 7)
- Deep Packet Inspection (DPI)
- Includes IPS, antivirus, URL filtering

### Firewall Rules

Rules processed **top to bottom** — first match wins:

```
Priority  Action   Source IP        Dest IP         Port    Protocol
1         ALLOW    192.168.1.0/24  Any             443     TCP
2         ALLOW    10.0.0.5        192.168.1.100   22      TCP
3         DENY     Any             Any             22      TCP
4         ALLOW    Any             192.168.1.50    80      TCP
5         DENY     Any             Any             Any     Any    ← Default DENY
```

> 💡 **Best practice:** End every rule set with "DENY ALL" — whitelist approach.

### DMZ Architecture

```
                   Internet
                      │
                 ┌────▼────┐
                 │ External │
                 │ Firewall │
                 └────┬────┘
                      │
              ┌───────▼───────┐
              │      DMZ      │
              │  Web Server   │
              │  Mail Server  │
              └───────┬───────┘
                      │
                 ┌────▼────┐
                 │ Internal │
                 │ Firewall │
                 └────┬────┘
                      │
              ┌───────▼───────┐
              │  Internal LAN  │
              │  Databases     │
              └───────────────┘
```

**Why DMZ?** If public-facing server is compromised, attacker is still blocked from internal network.

---

## Intrusion Detection & Prevention Systems

### IDS vs IPS

| Feature | IDS (Detection) | IPS (Prevention) |
|---------|-----------------|-------------------|
| **Action** | Detects and ALERTS | Detects and BLOCKS |
| **Placement** | Passive (copy of traffic) | Inline (traffic flows through) |
| **Risk** | May miss blocking | May block legitimate traffic |
| **Analogy** | Security camera | Security guard |

### Detection Methods

**Signature-Based:** Compares against known attack patterns. Fast, accurate for known threats. ❌ Cannot detect zero-day attacks.

**Anomaly-Based:** Establishes baseline of normal behaviour, alerts on deviation. ✅ Can detect unknown attacks. ❌ Higher false positives.

### HIDS vs NIDS

| Feature | HIDS (Host-based) | NIDS (Network-based) |
|---------|-------------------|---------------------|
| **Monitors** | Single host | Entire network segment |
| **Sees** | File changes, logs, processes | Network packets |
| **Example** | OSSEC, Wazuh | [[Snort]], [[Suricata]] |

**Common NIDS tools:** [[Snort]] (the original signature-based engine) and [[Suricata]] (modern, multi-threaded, also does full network security monitoring). Both match traffic against **rules** — see those notes for how the detection pipeline works.

### Reducing False Positives
- Tune rules for your environment
- Whitelist known-good traffic
- Set alert thresholds
- Regular rule review and updates

---

## VPNs (Virtual Private Networks)

### Types

**Site-to-Site:** Connects entire networks (office to office)
```
Mumbai LAN ──── [Encrypted Tunnel] ──── Delhi LAN
```

**Remote Access:** Individual user connects to corporate network
```
Employee Laptop ──── [Encrypted Tunnel] ──── Corporate Network
```

### VPN Protocols

| Protocol | Security | Speed | Use Case |
|----------|----------|-------|----------|
| **IPsec** | Very strong | Good | Site-to-site, corporate |
| **SSL/TLS VPN** | Strong | Good | Remote access, browser-based |
| **WireGuard** | Very strong | Fastest | Modern, all purposes |

### Split Tunnelling
- **Full tunnel:** ALL traffic goes through VPN (more secure, slower)
- **Split tunnel:** Only corporate traffic through VPN; internet goes direct (faster, less secure)

> ⚠️ **VPN does NOT make you anonymous:** Provider can see traffic, DNS leaks possible, endpoint still vulnerable.

---

## Proxy Servers

**Forward Proxy:** Between internal users and internet — hides user IPs, filters content, caches pages.

**Reverse Proxy:** Between internet and internal servers — hides server IPs, load balancing, DDoS protection. Examples: Nginx, Cloudflare.

**Transparent vs Explicit:** Transparent = users unaware; Explicit = browser configured manually.

---

## Network Segmentation

### Flat vs Segmented

**Flat (BAD):** All devices on one network — attacker accesses everything.

**Segmented (GOOD):** VLANs separate users, servers, databases. Firewall rules control inter-VLAN traffic. Limits **lateral movement**.

### Micro-Segmentation
Even more granular than VLANs — individual workloads have own security policies. Common in cloud/data centres. Zero Trust at network level.

---

## Zero Trust Architecture

**"Never trust, always verify"**

| Aspect | Traditional | Zero Trust |
|--------|------------|------------|
| Inside network | Trusted | Untrusted |
| Authentication | At perimeter | Every request |
| Access | Broad once inside | Minimum needed |
| Assumption | Perimeter is secure | Breach has occurred |

### Key Principles
1. **Verify explicitly** — Authenticate based on all data points
2. **Least privilege** — Minimum permissions needed
3. **Assume breach** — Design as if attackers are already inside

---

## Wireless Security

### WPA2 vs WPA3

| Feature | WPA2 | WPA3 |
|---------|------|------|
| **Key Exchange** | PSK | SAE (resistant to offline attacks) |
| **Forward Secrecy** | No | Yes |
| **Open Networks** | No encryption | OWE (individual encryption) |

### Common Wi-Fi Attacks

| Attack | How It Works | Defence |
|--------|-------------|---------|
| **Evil Twin** | Fake AP with same SSID | VPN, verify AP |
| **Deauthentication** | Disconnect clients forcibly | WPA3, 802.11w |
| **KRACK** | Exploits WPA2 handshake | Patch firmware, WPA3 |

### Securing Wireless
1. Use WPA3 (or WPA2 with strong passphrase)
2. Disable WPS
3. Change default SSID and admin credentials
4. Separate guest network
5. Keep firmware updated

---

## Related Concepts

- [[CIA Triad]] — What defences protect
- [[OSI Model & TCP-IP Security]] — Which layer each defence operates at
- [[Network Traffic Analysis]] — Monitoring with tools
- [[CSF & NS MOC]] — Subject navigation
