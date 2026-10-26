---
type: tool
subject: shared
tags: [cybersecurity, tool, snort, ids, ips, network-security, csf-ns]
status: learning
created: 2026-08-18
---

# Snort

> 🔧 **Tool Note** — Category: Network Intrusion Detection/Prevention (NIDS/NIPS) | Open Source

---

## 📌 Quick Overview
**What is it?** The original open-source, signature-based network IDS/IPS. It inspects packets against a database of **rules** and raises an alert (IDS mode) or drops the traffic (IPS mode) when a packet matches a known attack pattern.

**Official Website:** https://www.snort.org/
**License:** GPLv2 (free); maintained by Cisco Talos
**Used in:** CSF & NS (Unit III — IDS/IPS)
**Related concept:** [[Network Defence Mechanisms]]

## 🎯 What Problem Does It Solve?
- Detects known attacks (exploits, malware C2, scans, policy violations) as they cross the network
- Gives a SOC visibility into *what* is happening on the wire, not just *that* something happened
- Can actively block traffic inline (IPS mode)
- Turns community threat research (rules) into automated detection

## ⚙️ How It Works
Snort processes every packet through a **pipeline**:

```
Packet → DAQ → Decoder → Preprocessors → Detection Engine → Output/Alert
```

1. **DAQ (Data AcQuisition):** the abstraction layer that grabs packets — from `libpcap` (passive tap/SPAN, IDS mode) or inline via `afpacket`/`nfq` (IPS mode, sits *between* segments so it can drop).
2. **Decoder:** parses raw bytes into protocol structures (Ethernet → IP → TCP/UDP → payload) and flags malformed packets.
3. **Preprocessors** (Snort 2) / **Inspectors** (Snort 3): normalise traffic *before* rule matching so attackers can't evade with tricks — e.g. `frag3` reassembles fragmented IP, `stream` reassembles TCP sessions, `http_inspect` normalises URL encoding. Without this, an attack split across packets would slip past.
4. **Detection Engine:** matches the normalised packet against loaded **rules**. This is the heart — fast multi-pattern matching (Aho-Corasick) first narrows candidates, then full rule options are checked.
5. **Output:** on match, writes an alert (fast/full/`unified2` binary → read by Barnyard2 → SIEM), logs the packet, or drops it (IPS).

**Detection is signature-based:** Snort only catches what a rule describes. Unknown/zero-day attacks with no rule = no alert. (Contrast with **anomaly-based** detection — see [[Suricata]] and [[Network Defence Mechanisms]].)

**Snort 2 vs Snort 3:** Snort 3 (current) is a rewrite — multi-threaded, faster, simpler config, but the classic single-thread Snort 2 rule syntax below still applies conceptually.

## 🧩 Anatomy of a Rule
```
alert tcp any any -> 192.168.1.0/24 80 (msg:"Possible SQLi"; content:"UNION SELECT"; nocase; sid:1000001; rev:1;)
└rule   └proto     └src   └dst        └port  └────────── rule options ──────────┘
 header │           addr/port          │
        action                         detection + metadata
```
- **Action:** `alert` (log+alert), `log`, `drop`/`reject` (IPS), `pass` (whitelist).
- **Header:** protocol, source/dest IP + port, **direction** (`->` one-way, `<>` bidirectional).
- **Options:** `content` (byte/string to match), `nocase`, `pcre` (regex), `flow` (session state), `sid` (unique rule ID), `rev` (rule version), `msg` (alert text).

## 🚀 Key Features
1. Three run modes: **sniffer**, **packet logger**, **NIDS/NIPS**
2. Huge rule ecosystem — **Talos** (official), **ET Open** (Emerging Threats, community)
3. Inline IPS blocking
4. Preprocessors that defeat evasion (fragmentation, session splitting)
5. Rule language readable/writable by analysts

## 🔬 Typical Workflow
```bash
# Sniffer mode (like tcpdump)
snort -v

# Packet logger
snort -dev -l ./log

# NIDS mode with a config + ruleset
snort -c /etc/snort/snort.conf -i eth0 -A fast

# Test a rule file / config without running
snort -T -c /etc/snort/snort.conf
```
Alerts land in `/var/log/snort/alert`. In production the flow is **Snort → unified2 → Barnyard2 → Splunk/SIEM** so analysts triage in one place ([[Splunk]], [[SOC Operations & Incident Lifecycle]]).

## ⚠️ Gotchas / Limitations
- **Signature-based only** — no rule, no detection. Keep rules updated (PulledPork/manual).
- Can't inspect **encrypted** payloads (HTTPS/TLS) — sees metadata, not content.
- Noisy rulesets → **false-positive** flood; tuning (disabling/threshold-ing rules) is a real job (Unit III).
- Snort 2 is single-threaded → a busy link can drop packets; use Snort 3 or [[Suricata]] for multi-core.

## 🔗 Related Notes
- [[Suricata]] — modern multi-threaded alternative, runs Snort rules
- [[Network Defence Mechanisms]] — IDS vs IPS, signature vs anomaly
- [[Splunk]] — where Snort alerts get correlated
- [[Wireshark]] — manual packet analysis of what Snort flags
