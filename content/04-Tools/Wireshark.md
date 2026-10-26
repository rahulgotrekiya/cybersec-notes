---
type: tool
subject: shared
tags: [cybersecurity, tool, wireshark, network-analysis, packet-capture]
status: learning
created: 2026-07-02
---

# Wireshark

> 🔧 **Tool Note** — Category: Network Analysis | Open Source

---

## 📌 Quick Overview
**What is it?** Open-source network protocol analyser for capturing and inspecting network traffic in real-time.

**Official Website:** https://www.wireshark.org/
**License:** GNU GPLv2 (Free)
**Used in:** CSF & NS, EHPT, DFIR

## 🎯 What Problem Does It Solve?
- See exactly what's happening on a network at the packet level
- Identify cleartext credentials, malicious traffic, data exfiltration
- Reconstruct attack timelines from network captures
- Verify encryption and security controls

## ⚙️ How It Works
Wireshark is a **GUI on top of a packet-capture library** — it doesn't generate traffic, it copies packets the network card sees:

```
NIC → capture driver (libpcap/npcap) → promiscuous mode → capture buffer →
      dissectors decode each layer → display (with filters) → pcap file
```

1. **Capture library:** `libpcap` (Linux/macOS) or `Npcap` (Windows) taps the interface below the OS network stack and copies raw frames.
2. **Promiscuous mode:** normally a NIC ignores frames not addressed to it; promiscuous mode tells it to keep **all** frames it sees. (On Wi-Fi, **monitor mode** goes further and captures raw 802.11 management/other-device frames.)
3. **Dissectors:** Wireshark peels each packet layer by layer (Ethernet → IP → TCP → HTTP…), one dissector per protocol, turning bytes into the readable tree you click through.
4. **TCP stream reassembly:** a single HTTP response is split across many packets; Wireshark can stitch them back — **Follow → TCP Stream** shows the full conversation (this is how you read HTTP credentials/POST data).

**Two filter types — don't confuse them:**
| | **Capture filter** | **Display filter** |
|---|---|---|
| When | *before* capture (BPF syntax) | *after* capture |
| Purpose | limit what's recorded | limit what's shown |
| Example | `tcp port 80` | `http.request.method == "POST"` |
| Reversible? | No — dropped forever | Yes — just re-type |

**Why it sees cleartext:** if traffic isn't encrypted (HTTP, FTP, Telnet, DNS), the payload is right there in the packets — that's why the labs recover HTTP form credentials, and why HTTPS/TLS matters (see [[OSI Model & TCP-IP Security]]).

## 🚀 Key Features
1. Live packet capture on any interface
2. Deep inspection of hundreds of protocols
3. Powerful display filter engine
4. TCP/UDP stream reconstruction
5. Protocol hierarchy statistics
6. Export objects (files transferred over HTTP/SMB)
7. Read/write pcap and pcapng files

## 🔬 Common Display Filters
```
ip.addr == 192.168.1.10        # any traffic to/from this host
tcp.port == 80                 # by port
http                           # only HTTP
dns                            # only DNS queries/responses
http.request.method == "POST"  # form submissions (creds!)
tcp.flags.syn == 1 && tcp.flags.ack == 0   # SYN packets (scan/handshake starts)
tcp.analysis.retransmission    # network trouble / oddities
```
**Analyse workflow:** apply filter → right-click a packet → **Follow → TCP Stream** → read the conversation. **Statistics → Protocol Hierarchy** and **Conversations** summarise what's on the wire.

## ⚠️ Gotchas / Limitations
- **Encrypted traffic (HTTPS/TLS) shows only metadata** (IPs, SNI, cert) — not payload, unless you supply session keys (`SSLKEYLOGFILE`).
- Capturing needs privileges; on Linux add yourself to the `wireshark` group (see install) rather than running as root.
- On a **switched** network you only see your own traffic + broadcast unless you use a SPAN/mirror port or tap — a switch isn't a hub.
- Huge captures are memory-heavy; use **capture filters** to keep files manageable.
- Capturing traffic you're not authorised to monitor can be **illegal** — only capture on networks you own/are permitted to.

## 💻 Installation
```bash
# Linux
sudo apt install wireshark
# Allow non-root capture:
sudo usermod -aG wireshark $USER

# Kali Linux — pre-installed
```

## ⚡ Quick Reference
→ [[Wireshark Commands Cheatsheet]]

## 🔗 Related Notes
- [[Network Traffic Analysis]]
- [[Network & Email Forensics]]
- [[Nmap]] — Complements Wireshark for network analysis
