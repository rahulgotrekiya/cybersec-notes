---
type: concept
subject: csf-ns
tags: [cybersecurity, concept, csf-ns, firewall, network-security, ngfw, unit-3]
status: learning
created: 2026-09-04
---

# Firewalls

> 📝 **Concept Note** — CSF & NS Unit III Deep Dive | Source: MCA Sem III Syllabus

---

## The Problem It Solves

Every network needs to talk to the outside world, but "connected to everything" means "attackable by everything". A firewall is the **policy enforcement point** at a network boundary: it decides, for every single packet, whether it is allowed to pass.

The core idea: **default deny**. Everything is forbidden unless a rule explicitly permits it.

---

## ⚙️ How It Actually Works

### The 5-tuple — what a firewall actually looks at

Every rule decision is based on values pulled from the packet headers, classically the **5-tuple**:

| Field | From | Example |
|---|---|---|
| Source IP | IP header | `192.168.1.50` |
| Destination IP | IP header | `93.184.216.34` |
| Source port | TCP/UDP header | `51423` (random/ephemeral) |
| Destination port | TCP/UDP header | `443` |
| Protocol | IP header | `TCP` |

Plus **direction** (which interface the packet arrived on). The firewall compares this tuple to its rule list.

### Rule evaluation — top to bottom, first match wins

```
Packet arrives
     │
     ▼
Rule 1: match? ──yes──► apply action (ALLOW/DENY) ──► STOP
     │ no
     ▼
Rule 2: match? ──yes──► apply action ──► STOP
     │ no
     ▼
   ...
     ▼
Implicit DENY ALL  ◄── nothing matched, so drop
```

**This is why rule order is everything.** A permissive rule placed above a restrictive one makes the restrictive rule dead code — the packet never reaches it.

```
1  ALLOW  Any → Any        port 22   TCP    ← everyone can SSH
2  DENY   Any → 10.0.0.5   port 22   TCP    ← DEAD RULE, never reached
```

**DROP vs REJECT:**
- **DROP** — silently discard, no reply. Attacker's scan times out (`filtered`). Stealthier, slower for the scanner.
- **REJECT** — send back `TCP RST` or ICMP unreachable. Faster failure for legitimate users, but confirms the host exists.

---

## Types of Firewalls

### 1. Packet-Filtering (Stateless)

Examines each packet **in isolation** against the 5-tuple. Has **no memory** of previous packets.

**The problem this creates:** you request a web page. Your packet goes out to port 443. The reply comes back *from* port 443 *to* your random ephemeral port (e.g. 51423). A stateless firewall has no idea this reply is related to your request — so you must write a second, permanent rule allowing inbound traffic to ports 1024–65535. **That hole is enormous** and an attacker can send packets into it directly.

✅ Very fast, low resource. ❌ Blind to context; can't tell a reply from an unsolicited packet.

### 2. Stateful Inspection — the state table

This is the key mechanism to understand. A stateful firewall keeps a **connection state table** in memory:

```
STATE TABLE
┌──────────────┬──────┬─────────────┬──────┬─────────┬───────┐
│ Source IP    │SPort │Dest IP      │DPort │Protocol │ State │
├──────────────┼──────┼─────────────┼──────┼─────────┼───────┤
│ 192.168.1.50 │51423 │93.184.216.34│443   │TCP      │ ESTAB │
└──────────────┴──────┴─────────────┴──────┴─────────┴───────┘
```

**How the flow works:**
1. Your outbound packet matches an ALLOW rule → firewall creates a **state table entry** for that connection.
2. The reply arrives. The firewall checks the state table **before** the rule list.
3. The reply's tuple matches an existing entry (reversed) → **allowed automatically**, no inbound rule needed.
4. Connection closes (`FIN`/`RST`) or times out → entry removed, hole closes.

So you only write **one rule, in one direction** — return traffic is handled implicitly. The inbound "hole" exists only for the lifetime of a connection *you* initiated.

It also tracks **TCP state** (`SYN_SENT` → `ESTABLISHED` → `FIN_WAIT`), so an out-of-nowhere `ACK` packet with no matching handshake is dropped as invalid — this defeats a range of scanning and spoofing tricks.

> ⚠️ The state table is finite. Flooding it with half-open connections (**SYN flood**) can exhaust it — a denial-of-service against the firewall itself. Mitigated with SYN cookies and aggressive timeouts.

### 3. Application-Layer / Next-Generation Firewall (NGFW)

Operates up to **Layer 7** — it parses the actual application payload, not just headers.

- **Deep Packet Inspection (DPI)** — reads the content, so it can tell HTTP from SSH *even on the same port*.
- **Application identification** — recognises "this is BitTorrent" or "this is Facebook chat" by traffic signature, regardless of port. Port 443 is no longer a free pass.
- **User identity** — integrates with AD/LDAP, so rules become "the Finance *group* may reach the accounting app", not "IP 10.0.0.7 may".
- **TLS/SSL inspection** — decrypts, inspects, re-encrypts (see [[Proxy Servers]] for how the certificate trick works).
- Usually bundles an [[IDS & IPS|IPS]], antivirus, and URL filtering.

✅ Real control over modern traffic. ❌ CPU-expensive; TLS inspection raises privacy and breakage issues.

### Comparison

| | Packet Filter | Stateful | NGFW |
|---|---|---|---|
| OSI layer | 3–4 | 3–4 (+ state) | 3–7 |
| Remembers connections | ❌ | ✅ | ✅ |
| Sees payload | ❌ | ❌ | ✅ |
| Blocks app on odd port | ❌ | ❌ | ✅ |
| Speed | Fastest | Fast | Slowest |

---

## Ingress vs Egress Filtering

- **Ingress** (inbound) — what everyone remembers: keep attackers out.
- **Egress** (outbound) — what most organisations neglect, and what stops real damage. If a machine is compromised, egress rules block its **C2 callback** and **data exfiltration**. Blocking outbound traffic except through approved proxies/DNS is one of the highest-value controls available.

---

## Firewalls and NAT

**NAT (Network Address Translation)** rewrites private source addresses to one public IP and tracks the mapping in a translation table. Because inbound packets have no existing mapping, NAT *incidentally* blocks unsolicited inbound connections.

> ⚠️ **Exam trap:** NAT is **not** a security control. It is an address-conservation mechanism whose side effect resembles a firewall. It does no policy enforcement, inspects nothing, and does nothing about outbound/malicious traffic. Never present NAT as a substitute for a firewall.

---

## ⚠️ Gotchas / Exam Traps

- **`ALLOW TCP ANY ANY 443` is dangerous** even though 443 is "just HTTPS". Port ≠ application. Malware routinely tunnels C2 over 443 precisely because that rule exists everywhere, and the traffic is encrypted so a basic firewall can't inspect it. You need an NGFW (app-ID) or TLS inspection to actually control it.
- A firewall **cannot inspect encrypted payloads** without TLS interception.
- A firewall does **not** stop an insider, a phished credential, or a malicious file the user was allowed to download — the traffic was *permitted*.
- **Rule order bugs** and forgotten `ANY` rules are the most common real-world misconfiguration.
- Firewalls at the perimeter do nothing about **east-west** (internal) traffic — that needs [[DMZ & Network Segmentation|segmentation]].

---

## 📺 Learn by Video
*Supplementary only — these help you visualise. The technical content above is written independently and stands on its own.*

- ▶️ **"What is a Firewall?"** — https://www.youtube.com/watch?v=kDEX1HXybrU
- ▶️ **PowerCert Animated Videos** (great animated networking channel) — https://www.youtube.com/@PowerCertAnimatedVideos
- 🔍 Stateful vs stateless deep dive — [search](https://www.youtube.com/results?search_query=Practical+Networking+stateful+firewall)

---

## Related Concepts

- [[Network Defence Mechanisms]] — Unit III hub
- [[IDS & IPS]] — detection layer that complements blocking
- [[DMZ & Network Segmentation]] — where firewalls are placed
- [[Proxy Servers]] — application-layer alternative, TLS inspection
- [[OSI Model & TCP-IP Security]] — which layer each type operates at
