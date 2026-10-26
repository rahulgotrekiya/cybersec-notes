---
type: concept
subject: csf-ns
tags: [cybersecurity, concept, csf-ns, ids, ips, nids, hids, detection, unit-3]
status: learning
created: 2026-09-04
---

# IDS & IPS

> 📝 **Concept Note** — CSF & NS Unit III Deep Dive | Source: MCA Sem III Syllabus

---

## The Problem It Solves

A [[Firewalls|firewall]] decides *allowed or not* based mostly on addresses and ports. But plenty of attacks arrive over **traffic the firewall is supposed to permit** — a SQL injection inside a legitimate HTTP request to your public web server on port 80.

An IDS/IPS looks **inside** permitted traffic for evidence of attack. Firewall = "who may come in". IDS/IPS = "is what's coming in malicious".

---

## ⚙️ How It Actually Works

### The detection pipeline

```
   Traffic
      │
      ▼
1. ACQUISITION   copy packets (SPAN/tap) or receive inline
      │
      ▼
2. DECODE        parse Ethernet → IP → TCP → HTTP into fields
      │
      ▼
3. NORMALISE     reassemble IP fragments, rebuild TCP streams,
      │          decode URL/Unicode encoding  ← defeats evasion
      ▼
4. DETECT        match against signatures / baseline / behaviour rules
      │
      ▼
5. RESPOND       IDS → alert + log     IPS → drop/reset + alert
```

**Step 3 is the step people skip, and it's the important one.** An attacker can split `UNION SELECT` across two TCP segments, or write `%55NION` instead of `UNION`. A naive pattern-matcher sees neither. Normalisation reassembles and canonicalises traffic *before* matching, so the engine compares against what the destination will actually process. Without it, evasion is trivial.

### Where the sensor sits — this defines IDS vs IPS

```
	IDS (passive)                    IPS (inline)
                                     
Internet─┬─Firewall──LAN      Internet──Firewall──IPS──LAN
         │                                  │
      SPAN/TAP                     traffic flows THROUGH
         │                            it; it can drop
   [IDS]  ← sees a COPY
```

- **IDS is out-of-band.** It receives a **copy** of traffic from a switch **SPAN port** or a physical **network TAP**. It can alert, but the original packet has already reached the target. It cannot block; it can only tell you (or send a TCP `RST`, which is unreliable and racy).
- **IPS is in-line.** All traffic physically passes through it, so it can **drop** the malicious packet before delivery.

**The trade-off:**

| | IDS | IPS |
|---|---|---|
| Position | Passive copy | In the traffic path |
| Action | Alert only | Alert **and block** |
| Latency added | None | Yes (inspection delay) |
| Cost of false positive | Analyst wastes time | **Legitimate traffic is blocked** |
| Failure risk | None to traffic | Becomes a single point of failure |

**Fail-open vs fail-closed** (an IPS-only question): if the IPS dies, does traffic keep flowing (**fail-open** — availability preserved, security lost) or stop (**fail-closed** — secure, but outage)? This is a business decision, and it's a classic exam question.

---

## Detection Methods

### Signature-based
Matches traffic against a database of **known** attack patterns (a byte string, regex, or header combination) — exactly how [[Snort]] and [[Suricata]] rules work.

✅ Accurate, low false positives, alert names the exact threat.
❌ **Blind to anything without a signature** — zero-days, novel malware, or a trivially modified variant. Requires constant rule updates.

### Anomaly-based
Builds a **statistical baseline** of "normal" over a learning period (typical bandwidth, protocols, ports, active hours, peer hosts), then alerts on deviation — e.g. a workstation that suddenly uploads 4 GB at 3 a.m.

✅ **Can catch unknown/zero-day attacks** and insider misuse.
❌ **Noisy.** If the baseline is learned while the network is already compromised, malicious activity becomes "normal". Legitimate change (a new app rollout) triggers alerts.

### Behaviour / heuristic-based
Looks for known bad *sequences* rather than fixed strings — 200 failed logons then one success then a new service installed. Sits between the two above and underpins modern EDR/NDR.

| | Signature | Anomaly |
|---|---|---|
| Detects known attacks | ✅ Excellent | ⚠️ Sometimes |
| Detects zero-days | ❌ No | ✅ Yes |
| False positives | Low | **High** |
| Needs training period | No | Yes |
| Maintenance | Rule updates | Baseline re-tuning |

**Real deployments use both.**

---

## NIDS vs HIDS

| | **NIDS** (network) | **HIDS** (host) |
|---|---|---|
| Sees | Packets on a segment | Files, logs, processes, registry on one machine |
| Covers | Many hosts at once | Only its host |
| Encrypted traffic | ❌ Blind to payload | ✅ Sees data after decryption |
| Detects | Scans, exploits, C2 on the wire | File tampering, privilege escalation, local persistence |
| Examples | [[Snort]], [[Suricata]] | OSSEC, Wazuh, modern EDR |

They are complementary: NIDS sees the intrusion arriving, HIDS sees what it did after landing. **A NIDS cannot see inside TLS** — an increasingly large blind spot, which is why host-level visibility matters more every year.

---

## False Positives, False Negatives, and Tuning

| | Attack actually happened | No attack |
|---|---|---|
| **Alert raised** | ✅ True Positive | ❌ **False Positive** (noise) |
| **No alert** | ☠️ **False Negative** (missed) | ✅ True Negative |

- **False positive** — wastes analyst time. In volume it causes **alert fatigue**, and fatigued analysts start ignoring alerts, including real ones. This is how breaches get missed in organisations that "had an alert".
- **False negative** — the attack succeeded silently. Worse per-incident, but FP volume is the more common practical failure.

**Tuning** (Unit III explicitly asks this):
1. **Disable irrelevant rules** — don't run IIS signatures on a Linux-only estate.
2. **Whitelist known-good** — your own vulnerability scanner will look exactly like an attacker; suppress it by source IP.
3. **Thresholding / rate-limiting** — alert once per 60s per source, not on all 10,000 packets.
4. **Tune the baseline** to your real business hours and traffic.
5. **Review continuously** — an untuned IDS becomes shelfware within weeks.

> 💡 An IPS **must** be tuned harder than an IDS: a false positive on an IDS is noise, but on an IPS it is an **outage**. Standard practice is to deploy new rules in alert-only mode first, then switch to blocking once proven clean.

---

## ⚠️ Gotchas / Exam Traps

- **IDS detects, IPS prevents** — and placement is *why* (passive copy vs inline). Say both parts.
- **Encrypted traffic defeats a NIDS payload check.** It still sees metadata (IPs, sizes, timing, TLS SNI/JA3) which is often enough to spot beaconing.
- An IDS/IPS is **not** a firewall replacement — the firewall does coarse allow/deny, the IDS/IPS inspects what was allowed.
- Signature-based ≠ "detects everything known". A one-byte change can evade a poorly written signature.
- IPS in fail-open mode is a **security** gap; in fail-closed it is an **availability** risk.

---

## 📺 Learn by Video
*Supplementary only — these help you visualise. The technical content above is written independently and stands on its own.*

- ▶️ **"IDS & IPS Explained in 3 minutes"** — https://www.youtube.com/watch?v=NLLN4ynrono
- ▶️ **"IDS vs IPS: Which to Use and When"** — https://www.youtube.com/watch?v=wQSd_piqxQo
- 🔍 Snort hands-on — [search](https://www.youtube.com/results?search_query=NetworkChuck+snort+IDS)

---

## Related Concepts

- [[Network Defence Mechanisms]] — Unit III hub
- [[Snort]] · [[Suricata]] — the actual engines and their rule pipelines
- [[Firewalls]] — coarse allow/deny at the boundary
- [[SOC Operations & Incident Lifecycle]] — who triages these alerts
- [[Network Traffic Analysis]] — manual investigation of what fired
