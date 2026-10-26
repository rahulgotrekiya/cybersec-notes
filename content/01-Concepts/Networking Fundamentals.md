---
type: concept
subject: csf-ns
tags:
  - cybersecurity
  - concept
  - csf-ns
  - networking
  - osi-model
  - tcp-ip
  - devices
  - topology
status: done
created: 2026-07-16
---

# Networking Fundamentals

> 📝 **Concept Note** — CSF & NS Unit II | Foundation for all network security topics

---

## 1. Networking Devices

### Router
- Operates at **Layer 3 (Network)** of OSI model
- Connects **different networks** together (e.g., your home LAN → the Internet)
- Uses **IP addresses** to forward packets between networks
- Makes routing decisions based on a **routing table**
- Each interface has its own IP address
- Performs **NAT** (Network Address Translation) in home/office setups

```
Home LAN (192.168.1.x)  ←→  Router  ←→  ISP / Internet
```

**Security relevance:** Routers are the first line of defence — misconfigured routers expose entire networks. Attackers target default credentials and firmware vulnerabilities.

---

### Switch
- Operates at **Layer 2 (Data Link)** of OSI model
- Connects **devices within the same network** (LAN)
- Uses **MAC addresses** to forward frames to the correct port
- Maintains a **MAC address table** (CAM table) mapping MAC → port
- Creates a **dedicated collision domain** per port (unlike Hub)
- Layer 3 switches can also do routing

```
PC1 ─┐
PC2 ─┤── Switch ── Router ── Internet
PC3 ─┘
      All on same LAN (192.168.1.x)
```

**Security relevance:** 
- **MAC flooding attack** — overflows CAM table → switch behaves like a hub → attacker sees all traffic
- **VLAN hopping** — attacker escapes their VLAN into another
- **ARP spoofing** — poisons ARP cache to redirect traffic through attacker

---

### Hub
- Operates at **Layer 1 (Physical)** of OSI model
- The "dumb" ancestor of the switch
- Receives a signal on one port → **broadcasts it to ALL other ports**
- No intelligence — does not look at MAC or IP addresses
- All connected devices share **one collision domain**
- Largely obsolete — replaced by switches

```
PC1 sends frame → Hub → PC2, PC3, PC4 all receive it
                        ↑ Major security risk!
```

**Security relevance:** Any device on a hub-connected network can **sniff all traffic** with Wireshark — no attack needed. This is why passive eavesdropping was trivial on old networks.

| Feature | Hub | Switch |
|---------|-----|--------|
| Layer | 1 (Physical) | 2 (Data Link) |
| Addressing | None | MAC address |
| Sends frame to | All ports | Specific port |
| Collision domain | 1 shared | 1 per port |
| Security | ❌ Very poor | ✅ Better |

---

### Modem
- **Mo**dulator / **Dem**odulator
- Converts **digital signals ↔ analog signals** for transmission over telephone lines, cable, or fiber
- Sits at the boundary between your home network and the ISP
- DSL modem, Cable modem, Fiber ONT are common types
- Often combined with a router in home "modem-router" combo devices

```
Digital (PC) → Modem (modulates) → Analog (phone line) → ISP → Internet
Internet → ISP → Analog (phone line) → Modem (demodulates) → Digital (PC)
```

---

### Bridge
- Operates at **Layer 2 (Data Link)** of OSI model
- Connects **two network segments** and filters traffic between them
- Learns MAC addresses and decides whether to forward or block a frame
- Reduces collision domains by dividing a large network into segments
- A switch is essentially a **multi-port bridge**

```
Segment A ──── Bridge ──── Segment B
(PCs 1-5)  filter by MAC  (PCs 6-10)
```

### Device Summary Table

| Device | OSI Layer | Addresses Used | Primary Function |
|--------|-----------|----------------|-----------------|
| **Hub** | Layer 1 — Physical | None | Broadcast signal to all ports |
| **Bridge** | Layer 2 — Data Link | MAC address | Connect two LAN segments |
| **Switch** | Layer 2 — Data Link | MAC address | Forward frames to correct port |
| **Router** | Layer 3 — Network | IP address | Connect different networks |
| **Modem** | Layer 1 — Physical | N/A | Convert digital ↔ analog |

---

## 2. Network Topologies

Network topology = the **physical or logical arrangement** of devices in a network.

---

### Bus Topology

```
PC1 ── PC2 ── PC3 ── PC4 ── PC5
├─────────────────────────────┤
              Bus (Backbone cable)
```

- All devices connected to a **single central cable** (the bus/backbone)
- Data travels in both directions along the cable
- **Terminators** at each end absorb signals to prevent reflection
- If the backbone cable fails → **entire network goes down**

| Pros | Cons |
|------|------|
| Simple and cheap to set up | Single point of failure (backbone) |
| Less cable needed | Performance degrades with more devices |
| Easy to extend | Difficult to troubleshoot |

**Security:** All traffic visible to all devices on the bus — easy to sniff.

---

### Star Topology

```
        PC1
         │
PC4 ──── Switch/Hub ──── PC2
         │
        PC3
```

- All devices connect to a **central hub or switch**
- Most common topology in modern LANs
- If one device/cable fails → **only that device** is affected
- If the central switch fails → entire network goes down

| Pros | Cons |
|------|------|
| Easy to add/remove devices | Central device = single point of failure |
| Easy to troubleshoot | More cable needed than bus |
| Failure isolation (one device fails, rest work) | Cost of central switch |

**Security:** With a switch (not hub), traffic is isolated per port — better than bus/hub topologies.

---

### Ring Topology

```
    PC1
   /    \
PC4      PC2
   \    /
    PC3
```

- Devices connected in a **closed loop** — each device connects to exactly two neighbours
- Data travels in **one direction** (unidirectional) or two directions (bidirectional/dual ring)
- Uses **token passing** — a device can only transmit when it holds the token
- **FDDI** (Fiber Distributed Data Interface) used dual ring for fault tolerance

| Pros | Cons |
|------|------|
| No collisions (token passing) | One break in ring = network fails |
| Equal access for all devices | Difficult to add/remove devices |
| Predictable performance | Slower than star topology |

---

### Mesh Topology

```
PC1 ─── PC2
│ ╲   ╱ │
│   ╳   │
│ ╱   ╲ │
PC3 ─── PC4
```

- Every device connects to **every other device**
- **Full mesh:** n(n-1)/2 connections for n devices
- **Partial mesh:** only some devices have redundant connections

| Pros | Cons |
|------|------|
| Highly redundant — no single point of failure | Extremely expensive (many cables) |
| Multiple paths for data | Complex configuration |
| Best fault tolerance | Difficult to manage |

**Used in:** The Internet itself is a form of partial mesh. Core network infrastructure.

---

### Tree Topology (Hierarchical)

```
          Root Switch
         /            \
   Switch-A          Switch-B
   /     \           /     \
PC1     PC2       PC3     PC4
```

- Hierarchical arrangement — **combination of star topologies**
- Root node at the top, branches below
- Used in **large enterprise networks** — campus networks, ISP backbones

| Pros | Cons |
|------|------|
| Scalable and easy to expand | Root node failure = entire network down |
| Easy to manage hierarchically | More cabling than bus |
| Suitable for large networks | Higher cost |

---

### Hybrid Topology
- **Combination of two or more topologies**
- Most real-world networks are hybrid
- Example: A company might use star topology within each floor, connected via a ring backbone

```
Floor 1 (Star) ─┐
Floor 2 (Star) ─┼── Ring backbone ── Internet (Router)
Floor 3 (Star) ─┘
```

---

### Topology Comparison

| Topology | Fault Tolerance | Cost | Scalability | Real-world Use |
|----------|----------------|------|-------------|---------------|
| **Bus** | ❌ Poor | ✅ Low | ❌ Poor | Old coax networks, obsolete |
| **Star** | ✅ Good | Medium | ✅ Good | Modern LAN (most common) |
| **Ring** | ❌ Medium | Medium | ❌ Poor | SONET, FDDI, older networks |
| **Mesh** | ✅✅ Best | ❌ High | ❌ Complex | Internet core, WAN |
| **Tree** | Medium | Medium | ✅✅ Best | Enterprise campus networks |
| **Hybrid** | Varies | Varies | ✅✅ Best | Large enterprise networks |

---

## 3. Communication Types

### Simplex

```
Sender ──────────────────► Receiver
       (one way only)
```

- Communication flows in **one direction only**
- Receiver cannot send back any data
- **Examples:** Television broadcast, AM/FM radio, keyboard → CPU

**Security relevance:** Log forwarding from sensor → SIEM can be designed as simplex to prevent a compromised SIEM from sending commands back to sensors.

---

### Half-Duplex

```
PC1 ──────────────────────► PC2   (PC1 sending)
PC1 ◄────────────────────── PC2   (PC2 sending — but NOT simultaneously)
```

- Communication in **both directions, but not at the same time**
- One party must stop transmitting before the other can start
- **Examples:** Walkie-talkie ("over"), older WiFi (802.11), CB radio

**Network context:** Early Ethernet with hubs was effectively half-duplex — only one device could transmit at a time without collision.

---

### Full-Duplex

```
PC1 ──────────────────────► PC2   (simultaneously)
PC1 ◄────────────────────── PC2   (simultaneously)
```

- Communication in **both directions simultaneously**
- Both parties can send and receive at the same time
- **Examples:** Telephone call, modern Ethernet (switches), mobile phones

**Network context:** Modern switched Ethernet is full-duplex — the switch gives each device a dedicated channel, eliminating collisions entirely. This **doubles effective bandwidth**.

| Feature | Simplex | Half-Duplex | Full-Duplex |
|---------|---------|-------------|-------------|
| Direction | One-way | Both, not simultaneous | Both, simultaneously |
| Example | Radio broadcast | Walkie-talkie | Phone call |
| Efficiency | Low | Medium | High |
| Collision possible? | N/A | Yes | No |

---

## 4. Transmission Models

### Analog Transmission

- Data represented as a **continuous waveform** — varies smoothly over time
- Characterised by **amplitude, frequency, and phase**
- Susceptible to **noise and distortion** over long distances — signal degrades
- Traditional telephone (PSTN), AM/FM radio, early modems

```
Analog signal (continuous wave):
     ╭─╮     ╭─╮     ╭─╮
────╯   ╰───╯   ╰───╯   ╰────
```

**Signal degradation:** As analog signal travels, noise adds on top of it — you can't perfectly distinguish original signal from noise. Amplifiers boost the signal but also boost the noise.

---

### Digital Transmission

- Data represented as **discrete binary values** — 0 (low voltage) or 1 (high voltage)
- Signal is either **ON or OFF** — no in-between states
- Can be **regenerated perfectly** — repeaters restore the original signal, not the noise
- More resistant to noise, enables encryption, supports compression
- Modern Ethernet, fiber optics, WiFi, USB, HDMI — all digital

```
Digital signal (discrete):
     ┌───┐   ┌─┐ ┌─────┐
─────┘   └───┘ └─┘     └─────
  0    1    0   1    1    0
```

**Regeneration vs Amplification:**
```
Analog: Signal + Noise → Amplifier → Louder Signal + Louder Noise ❌
Digital: Signal + Noise → Repeater → Clean original signal restored ✅
```

| Feature | Analog | Digital |
|---------|--------|---------|
| Representation | Continuous wave | Discrete 0s and 1s |
| Noise immunity | Poor — noise accumulates | Excellent — signal regenerated |
| Bandwidth efficiency | Lower | Higher |
| Security (encryption) | Difficult | Easy ✅ |
| Example | Old telephone, radio | Ethernet, WiFi, fiber |

---

## 5. OSI Model (Open Systems Interconnection)

The OSI model is a **conceptual framework** that standardises how different network systems communicate. Developed by ISO in 1984.

> **Mnemonic (top to bottom):** **A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing
> Application → Presentation → Session → Transport → Network → Data Link → Physical

```
┌────────────────────────────────────────────┐
│  7. Application   │ HTTP, FTP, DNS, SMTP   │  ← User/App interaction
│  6. Presentation  │ SSL/TLS, JPEG, ASCII   │  ← Data format/encryption
│  5. Session       │ NetBIOS, RPC, SQL      │  ← Session management
│  4. Transport     │ TCP, UDP               │  ← End-to-end delivery
│  3. Network       │ IP, ICMP, ARP, OSPF    │  ← Logical addressing/routing
│  2. Data Link     │ Ethernet, MAC, PPP     │  ← Physical addressing/frames
│  1. Physical      │ Cables, Hubs, Bits     │  ← Raw bit transmission
└────────────────────────────────────────────┘
```

---

### Layer 1 — Physical Layer

- Transmits **raw bits** (0s and 1s) over a physical medium
- Deals with: voltage levels, cable types, connectors, pin layouts, signal timing
- **Devices:** Hub, repeater, cable, modem
- **Protocols/Standards:** Ethernet physical (RJ-45), RS-232, USB, Bluetooth physical layer

**Security:** Physical layer attacks — wiretapping, cable cutting, hardware keyloggers, rogue devices.

---

### Layer 2 — Data Link Layer

- Provides **node-to-node** data transfer within the same network
- Packages bits into **frames**, adds MAC addresses (source + destination)
- Handles **error detection** (CRC checksum) and **flow control**
- Divided into two sublayers:
  - **MAC** (Media Access Control) — physical addressing, media access
  - **LLC** (Logical Link Control) — flow control, error checking
- **Devices:** Switch, bridge, NIC
- **Protocols:** Ethernet, PPP, HDLC, ARP (partially)

**Security:** ARP poisoning, MAC spoofing, MAC flooding, VLAN hopping all target Layer 2.

---

### Layer 3 — Network Layer

- Handles **logical addressing** (IP addresses) and **routing** between networks
- Determines the **best path** for data to travel from source to destination
- Breaks data into **packets**, adds IP header (source IP, destination IP, TTL)
- **Devices:** Router, Layer 3 switch
- **Protocols:** IPv4, IPv6, ICMP, OSPF, BGP, RIP

**Security:** IP spoofing, routing attacks, ICMP-based attacks (ping flood, smurf attack), route poisoning.

---

### Layer 4 — Transport Layer

- Provides **end-to-end** communication between applications
- **Segmentation** — breaks large data into segments, reassembles at destination
- Two main protocols with different characteristics:

| Feature | TCP | UDP |
|---------|-----|-----|
| Connection | Connection-oriented (3-way handshake) | Connectionless |
| Reliability | Guaranteed delivery (ACK) | Best-effort |
| Order | In-order delivery | No ordering |
| Speed | Slower (overhead) | Faster |
| Use case | HTTP, SSH, FTP, email | DNS, video streaming, VoIP, gaming |

- **Port numbers** identify specific applications (80=HTTP, 443=HTTPS, 22=SSH)

**Security:** SYN flood (TCP handshake exploit), port scanning (Nmap targets Layer 4), firewall rules operate at Layer 4.

---

### Layer 5 — Session Layer

- Manages **sessions** (connections) between applications
- Establishes, maintains, and terminates sessions
- Handles **session checkpointing** — ability to resume after a failure
- **Protocols:** NetBIOS, RPC (Remote Procedure Call), PPTP, SQL sessions

**Security:** Session hijacking attacks target Layer 5 — stealing a valid session token to impersonate an authenticated user.

---

### Layer 6 — Presentation Layer

- **Translates data** between the application format and the network format
- Handles: **Encryption/decryption**, data compression, character encoding (ASCII, Unicode)
- Ensures data sent by one system can be read by another (format independence)
- **Protocols:** SSL/TLS (encryption), JPEG/PNG (image format), MPEG, ASCII, UTF-8

**Security:** SSL/TLS stripping attacks, weak cipher negotiation attacks operate at Layer 6. This is where HTTPS encryption happens.

---

### Layer 7 — Application Layer

- The layer **closest to the end user**
- Provides **network services directly to applications**
- Not the application itself — but the protocols applications use
- **Protocols:** HTTP/HTTPS, FTP, SFTP, DNS, SMTP, POP3, IMAP, SSH, Telnet, SNMP, DHCP

**Security:** Most web attacks target Layer 7 — SQLi, XSS, CSRF, directory traversal, credential brute force. WAFs (Web Application Firewalls) protect Layer 7.

---

### OSI Layer Data Units (PDUs)

| Layer | PDU Name | What it Contains |
|-------|----------|-----------------|
| 7. Application | Data | Application payload |
| 6. Presentation | Data | Formatted/encrypted data |
| 5. Session | Data | Session info |
| 4. Transport | **Segment** (TCP) / **Datagram** (UDP) | Port numbers + data |
| 3. Network | **Packet** | IP header + segment |
| 2. Data Link | **Frame** | MAC header + packet |
| 1. Physical | **Bits** | Raw 0s and 1s |

### Encapsulation (Sender) vs Decapsulation (Receiver)

```
SENDER (adding headers going down):
App Data → [Session][Presentation][Application Data]
         → [Transport Header][Data]  = Segment
         → [Network Header][Segment] = Packet
         → [MAC Header][Packet][FCS] = Frame
         → 01010110101...              = Bits

RECEIVER (stripping headers going up): reverse process
```

---

## 6. TCP/IP Model

The TCP/IP model is the **practical model** that actually runs the Internet. Developed by DARPA in the 1970s.

Unlike OSI (7 layers), TCP/IP has **4 layers** — it collapses several OSI layers together.

```
TCP/IP Model              OSI Equivalent
┌──────────────────┐      ┌────────────────────┐
│  4. Application  │  =   │ 7. Application     │
│                  │      │ 6. Presentation    │
│                  │      │ 5. Session         │
├──────────────────┤      ├────────────────────┤
│  3. Transport    │  =   │ 4. Transport       │
├──────────────────┤      ├────────────────────┤
│  2. Internet     │  =   │ 3. Network         │
├──────────────────┤      ├────────────────────┤
│  1. Network      │  =   │ 2. Data Link       │
│     Access       │      │ 1. Physical        │
└──────────────────┘      └────────────────────┘
```

### Layer 1 — Network Access Layer (Link Layer)
- Combines OSI **Data Link + Physical**
- Handles physical transmission of data on the local network
- MAC addressing, frame creation, physical medium
- **Protocols:** Ethernet, Wi-Fi (802.11), ARP

### Layer 2 — Internet Layer
- Corresponds to OSI **Network Layer**
- Logical addressing and routing across networks
- **Protocols:** **IP** (IPv4/IPv6), ICMP, IGMP

### Layer 3 — Transport Layer
- Corresponds to OSI **Transport Layer**
- End-to-end data transfer between applications
- **Protocols:** **TCP**, **UDP**

### Layer 4 — Application Layer
- Combines OSI **Session + Presentation + Application**
- All application-level protocols
- **Protocols:** HTTP, HTTPS, DNS, FTP, SSH, SMTP, DHCP, Telnet

---

### OSI vs TCP/IP — Quick Comparison

| Feature | OSI Model | TCP/IP Model |
|---------|-----------|-------------|
| Layers | 7 | 4 |
| Type | Conceptual/theoretical | Practical/implemented |
| Developed by | ISO (1984) | DARPA (1970s) |
| Purpose | Universal reference framework | Internet communication |
| Application protocols | Layers 5, 6, 7 | Single Application layer |
| Transport protocols | Layer 4 | Layer 3 (Transport) |
| Physical | Layer 1 | Part of Network Access |
| Used for | Learning, troubleshooting | Actual internet |

> 💡 **Exam tip:** OSI is the *theoretical* model you use to understand and explain networking. TCP/IP is the *real* model the internet actually uses. Both are tested together.

---

## 7. How It All Connects — Security Mapping

| OSI Layer | TCP/IP Layer | Devices | Attacks | Defences |
|-----------|-------------|---------|---------|---------|
| 7 Application | Application | Proxy, WAF | SQLi, XSS, phishing | WAF, input validation, HTTPS |
| 6 Presentation | Application | — | SSL stripping | TLS 1.3, HSTS |
| 5 Session | Application | — | Session hijacking | Secure cookies, re-auth |
| 4 Transport | Transport | Firewall | SYN flood, port scan | Firewall, rate limiting |
| 3 Network | Internet | Router | IP spoofing, ICMP flood | ACLs, ingress filtering |
| 2 Data Link | Network Access | Switch | ARP poison, MAC flood | Dynamic ARP inspection, port security |
| 1 Physical | Network Access | Hub | Wiretapping, hardware tap | Physical security, encrypted cable |

---

## Related Notes
- [[OSI Model & TCP-IP Security]] — Security analysis per layer
- [[Network Traffic Analysis]] — Wireshark captures at Layer 2-4
- [[Network Defence Mechanisms]] — Firewalls, IDS/IPS (Layers 3-7)
- [[Common Ports & Protocols Reference|Common Ports]] ⚡ — Layer 4 port reference
