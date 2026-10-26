---
type: tool
subject: shared
tags: [cybersecurity, tool, netstat, ss, host-forensics, soc, csf-ns]
status: learning
created: 2026-08-18
---

# netstat & ss

> 🔧 **Tool Note** — Category: Host Network Inspection (built-in CLI) | Free / OS-native

---

## 📌 Quick Overview
**What are they?** Command-line tools that show the **network connections and listening ports of the local machine** — every socket the OS currently has open. `netstat` is the classic (cross-platform) tool; `ss` ("socket statistics") is its faster modern Linux replacement.

**On Windows:** `netstat`
**On Linux:** `ss` (preferred) or `netstat` (from `net-tools`, often deprecated)
**Used in:** CSF & NS (Practical 6 — spotting a compromised host beaconing out)
**Related concept:** [[Network Traffic Analysis]]

## 🎯 What Problem Does It Solve?
- Answer "**what is my machine talking to right now?**" and "**what ports am I listening on?**"
- Spot a suspicious **outbound** connection (malware C2 / beaconing) from the host itself
- Find unexpected **listeners** (a backdoor shell waiting for connections)
- Map a connection back to the **process** that owns it

Where [[Wireshark]] shows traffic *on the wire*, netstat/ss shows sockets *from the OS's point of view* — including which **process** owns each one, which Wireshark can't tell you.

## ⚙️ How It Works
The OS kernel keeps a table of every active socket (local IP:port ↔ remote IP:port + state + owning process). These tools **read that table** — they do **not** send packets:

- **`netstat`** (Linux) reads pseudo-files under **`/proc/net/`** (`tcp`, `udp`, `tcp6`…) and, with `-p`, walks `/proc/<pid>/fd/` to map a socket to its process. Parsing all of `/proc` is slow on busy hosts.
- **`ss`** asks the kernel directly over a **netlink** socket (`NETLINK_SOCK_DIAG`) — no `/proc` text parsing — so it's much faster and can filter kernel-side.

Because they read live kernel state, the output is a **point-in-time snapshot** of connections; run repeatedly (or `watch`) to see beaconing patterns.

### TCP connection states you'll see
| State | Meaning |
|---|---|
| `LISTEN` | Port open, waiting for inbound connections (a server/backdoor) |
| `ESTABLISHED` | Active, connected session |
| `SYN_SENT` | This host is trying to connect out (handshake started) |
| `TIME_WAIT` | Connection closed, cleaning up |
| `CLOSE_WAIT` | Remote closed; local app hasn't yet |

An unexpected `LISTEN` on a high port, or repeated `SYN_SENT`/`ESTABLISHED` to one odd external IP:port, are classic red flags.

## 🔬 Key Commands
```bash
# ── Linux (ss) ──
ss -tulpn      # TCP+UDP, Listening, show Process, Numeric (no DNS) — the go-to
ss -tan        # all TCP sockets, numeric
ss -tp state established         # established TCP with owning process
ss dst 203.0.113.10              # connections to a specific IP

# ── Linux (netstat, legacy) ──
netstat -tulpn                   # same idea as ss -tulpn
netstat -antp

# ── Windows ──
netstat -ano                     # all + numeric + owning PID
netstat -anob                    # also show the executable (needs admin)
# then map PID → process:
tasklist /fi "PID eq 1234"
```
**Flag mnemonic:** `t`=TCP, `u`=UDP, `l`=listening, `p`=process, `n`=numeric, `a`=all.

## 🔬 Typical Workflow (Practical 6 — C2 beaconing)
1. `ss -tunp` (or `netstat -ano` on Windows) → list active connections.
2. Spot a repeating outbound connection to a **non-standard port** at regular intervals → possible **C2 beacon**.
3. Note the **owning process/PID** — is it something that should be talking to the Internet?
4. Take the remote IP/hash to [[VirusTotal]] for reputation.
5. Correlate with [[Wireshark]] to see the payload/timing, and log the incident.

## ⚠️ Gotchas / Limitations
- **Snapshot only** — short-lived connections between runs are missed; sample repeatedly.
- Seeing the **owning process** usually needs **admin/root** (`-p` / `-b`).
- Shows *that* a connection exists, not *what's inside it* — pair with [[Wireshark]] for payload.
- `netstat` on Linux is deprecated; prefer `ss`. Malware on a rooted host can also hide from these tools (rootkits) — corroborate with network-side capture.

## 🔗 Related Notes
- [[Network Traffic Analysis]] — host-side vs wire-side visibility
- [[Wireshark]] — packet-level view of the same connections
- [[VirusTotal]] — reputation-check the remote IPs you find
- [[SOC Operations & Incident Lifecycle]] — C2 beaconing detection & triage
