---
type: tool
subject: shared
tags: [cybersecurity, tool, suricata, ids, ips, nsm, csf-ns]
status: learning
created: 2026-08-18
---

# Suricata

> 🔧 **Tool Note** — Category: IDS / IPS / Network Security Monitoring | Open Source

---

## 📌 Quick Overview
**What is it?** A high-performance, **multi-threaded** open-source engine that does IDS, IPS, and full **Network Security Monitoring (NSM)** in one tool. Think of it as "Snort's modern cousin" — it can even run Snort/Talos and ET rulesets, but adds protocol logging, file extraction, and rich JSON output.

**Official Website:** https://suricata.io/
**License:** GPLv2; maintained by the **OISF** (Open Information Security Foundation)
**Used in:** CSF & NS (Unit III — IDS/IPS overview)
**Related concept:** [[Network Defence Mechanisms]]

## 🎯 What Problem Does It Solve?
- Signature-based detection like [[Snort]], but scales across **all CPU cores** for high-throughput links
- Produces not just alerts but a full **record of network activity** (every DNS query, TLS handshake, HTTP request) for threat hunting
- Extracts files transferred over the wire for malware analysis
- One engine for detection **and** logging, reducing tool sprawl

## ⚙️ How It Works
Same conceptual pipeline as [[Snort]] (acquire → decode → normalise → match → output) but with two big architectural differences:

1. **Multi-threading:** Suricata splits work across threads and uses **flow-based load balancing** — packets from the same connection go to the same thread — so an 8-core box inspects ~8× the traffic Snort 2 can on one core.
2. **Automatic protocol detection:** it identifies the application protocol by **behaviour, not port** (e.g. HTTP on port 8080, or TLS on a random port) — so it isn't fooled by services on non-standard ports.

Beyond alerting, every parsed event is written to **EVE JSON** (`eve.json`) — one structured line per event:

```
Packet → Decode → Flow tracking → App-layer parsers (HTTP/TLS/DNS/SMB…) →
         ├── Detection engine (rules) → alert events
         └── Logging → dns / tls / http / fileinfo events  → eve.json → SIEM
```

Because it logs *all* protocol metadata (not only rule hits), Suricata doubles as an **NSM sensor** — you can answer "what TLS certs did this host see?" even with zero alerts. That EVE JSON drops straight into [[Splunk]]/ELK.

## 🆚 Suricata vs Snort
| | Snort (2) | Suricata |
|---|---|---|
| Threading | Single-threaded | **Multi-threaded** |
| Rules | Talos, ET | **Runs Snort rules** + its own keywords |
| Protocol logging | Alert-focused | **Full NSM** (DNS/TLS/HTTP/flow logs) |
| File extraction | Limited | **Built-in** (`fileinfo`, file-store) |
| Output | unified2 | **EVE JSON** (SIEM-friendly) |
| Detection type | Signature | Signature + some anomaly/protocol-anomaly |

Both are **signature-based at heart** — neither is a true ML anomaly engine.

## 🚀 Key Features
1. Multi-threaded, hardware-accelerated inspection
2. EVE JSON logging of DNS, TLS (incl. JA3), HTTP, SMB, flow records
3. File extraction + MD5/SHA hashing for [[VirusTotal]] lookups
4. Runs in IDS (passive) or IPS (inline, `nfqueue`) mode
5. Lua scripting for complex detection

## 🔬 Typical Workflow
```bash
# Test config + rules
suricata -T -c /etc/suricata/suricata.yaml

# IDS mode on an interface
suricata -c /etc/suricata/suricata.yaml -i eth0

# Read a pcap offline (great for lab / malicious.pcap analysis)
suricata -c /etc/suricata/suricata.yaml -r malicious.pcap

# Update rules (Emerging Threats etc.)
suricata-update
```
Watch `/var/log/suricata/fast.log` for alerts and `eve.json` for the full event stream.

## ⚠️ Gotchas / Limitations
- Still **signature-dependent** for alerts — anomaly coverage is limited; unknown attacks need a rule.
- Cannot read **encrypted payloads** — but it *does* log TLS metadata (SNI, cert, JA3) which is often enough to spot bad infrastructure.
- More config surface (`suricata.yaml`) than Snort; EVE JSON can be high-volume — size your SIEM ingest.
- Inline IPS mode needs careful placement so it doesn't become a single point of failure.

## 🔗 Related Notes
- [[Snort]] — the signature-based predecessor; shares rule language
- [[Network Defence Mechanisms]] — IDS vs IPS, HIDS vs NIDS, tuning
- [[Splunk]] — ingesting EVE JSON for correlation
- [[VirusTotal]] — checking hashes of files Suricata extracts
