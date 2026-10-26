---
type: concept
subject: csf-ns
tags: [cybersecurity, concept, csf-ns, vpn, ipsec, tls, tunnelling, unit-3]
status: learning
created: 2026-09-04
---

# VPN (Virtual Private Network)

> 📝 **Concept Note** — CSF & NS Unit III Deep Dive | Source: MCA Sem III Syllabus

---

## The Problem It Solves

The Internet is a **shared, untrusted** medium. Anything you send crosses routers owned by strangers, and anyone on the path (ISP, café Wi-Fi operator, a MitM attacker) can read or alter unencrypted traffic.

A VPN creates a **private, encrypted tunnel across that public network** — so two endpoints communicate as if directly connected on a private LAN, even though the packets traverse the open Internet.

Three guarantees: **Confidentiality** (encrypted), **Integrity** (tamper-evident), **Authentication** (you're talking to the real peer). Straight back to the [[CIA Triad]].

---

## ⚙️ How It Actually Works — Encapsulation

The central mechanism is **encapsulation**: your original packet is encrypted and then wrapped inside a *new* packet addressed to the VPN gateway.

```
ORIGINAL PACKET (private, would never route over the Internet)
┌──────────────────┬─────────────────────┐
│ IP: 10.0.0.5 →   │  data               │
│     10.0.1.20    │                     │
└──────────────────┴─────────────────────┘
                    │
                    ▼  encrypt the whole thing, then add a new header
NEW PACKET ON THE WIRE
┌──────────────────┬──────────────────────────────────────┐
│ IP: 203.0.113.1 →│ ESP │ ▓▓▓ ENCRYPTED ORIGINAL ▓▓▓ │ MIC│
│    198.51.100.7  │ hdr │  (headers AND data hidden) │    │
└──────────────────┴──────────────────────────────────────┘
   real routable IPs         an observer sees only this
```

An observer on the path sees only "gateway A is sending encrypted data to gateway B". The internal addresses, ports, protocols, and content are all hidden. At the far end the gateway decrypts, strips the outer header, and releases the original packet onto the private network.

---

## Types of VPN

### Site-to-Site
Connects **entire networks**, gateway-to-gateway. Individual devices know nothing about it — the routers do all the work.

```
Mumbai Office LAN ──[GW]══ encrypted tunnel ══[GW]── Delhi Office LAN
  10.1.0.0/16                                          10.2.0.0/16
```
Always on. Typically **IPsec**. Used to link branch offices or an office to a cloud VPC.

### Remote Access
An **individual user** connects into the corporate network from anywhere. Requires **client software** (or a browser for SSL VPN) and per-user authentication (ideally MFA).

```
Employee laptop (café Wi-Fi) ══ encrypted tunnel ══► [VPN Concentrator] ── Corporate LAN
```

---

## IPsec — in depth

**IPsec** is not one protocol but a *suite* operating at **Layer 3**, so it protects *all* IP traffic regardless of application.

### The two phases (IKE)

**IKE (Internet Key Exchange)** sets up the tunnel before any data flows. The problem it solves: how do two parties agree on a secret key over a channel an eavesdropper is watching?

**Phase 1 — build a secure management channel**
- Peers authenticate each other (pre-shared key or digital certificates).
- Perform a **Diffie-Hellman** exchange → both derive a shared secret *without ever transmitting it*.
- Result: an **IKE SA**, a secure channel used only to negotiate phase 2.
- Modes: *Main mode* (6 messages, identities encrypted) vs *Aggressive mode* (3 messages, faster, identity exposed — weaker).

**Phase 2 — build the actual data tunnel**
- Using the protected phase-1 channel, negotiate the encryption/hashing for real traffic.
- Result: **IPsec SAs** that carry your data.
- *Perfect Forward Secrecy (PFS)* optionally runs a fresh DH here, so compromising one key doesn't expose past sessions.

> **IKEv2** (modern) does this in far fewer messages, supports MOBIKE (survives changing networks — Wi-Fi → mobile data), and is what you should cite as current.

**Security Association (SA):** the agreed set of parameters (algorithms, keys, lifetime) for one direction. SAs are **unidirectional**, so a two-way tunnel has **two** SAs.

### ESP vs AH

| | **ESP** (Encapsulating Security Payload) | **AH** (Authentication Header) |
|---|---|---|
| Encryption | ✅ Yes | ❌ **None** |
| Integrity/auth | ✅ Yes | ✅ Yes |
| Protocol no. | 50 | 51 |
| Works through NAT | ✅ (with NAT-T) | ❌ Breaks |
| Used today | **Almost always** | Effectively obsolete |

> ⚠️ **Exam trap:** **AH does not encrypt.** It only proves integrity and origin. If a question asks which provides confidentiality, the answer is **ESP**.

### Tunnel vs Transport mode

| | **Tunnel mode** | **Transport mode** |
|---|---|---|
| Protects | **Entire original packet** (incl. its IP header) | Payload only; original IP header kept |
| New IP header | ✅ Added | ❌ No |
| Hides internal IPs | ✅ Yes | ❌ No |
| Used for | **Site-to-site & remote access VPNs** | Host-to-host on a trusted LAN |

Tunnel mode is what "VPN" normally means.

---

## SSL/TLS VPN

Operates higher up (**Layer 4–7**) using the same TLS that secures HTTPS.

- **Clientless** — user logs into a web portal; specific internal web apps are proxied through the browser. Nothing to install.
- **Client-based** (e.g. OpenVPN) — a lightweight client gives full network access.

**Why it's often preferred for remote access:** it runs over **TCP/UDP 443**, which is open on essentially every network — hotel Wi-Fi, corporate guest networks, and restrictive firewalls that block IPsec's ports (UDP 500/4500, protocol 50) let it straight through.

| | IPsec | SSL/TLS VPN |
|---|---|---|
| Layer | 3 (network) | 4–7 (transport/app) |
| Scope | All IP traffic | Per-application or full (client) |
| Client needed | Usually | Optional (browser works) |
| Firewall friendliness | Often blocked | **Excellent (443)** |
| Best for | Site-to-site | **Remote access** |

### WireGuard
Modern alternative: ~4,000 lines of code (vs IPsec/OpenVPN's hundreds of thousands), fixed modern cryptography (no negotiation, so no downgrade attacks), runs in the Linux kernel. **Faster, simpler, easier to audit** — rapidly becoming the default.

---

## Split vs Full Tunnelling

```
FULL TUNNEL                          SPLIT TUNNEL
laptop ──all traffic──► VPN ──► net  laptop ──corp traffic──► VPN ──► corp
                         │                  └──everything else──► Internet
                    corp + internet
```

| | Full tunnel | Split tunnel |
|---|---|---|
| Corporate traffic | Via VPN | Via VPN |
| Internet traffic | **Via VPN** (inspected/filtered) | **Direct** |
| Security | ✅ Higher — all traffic subject to corporate controls | ⚠️ Lower — direct traffic is uninspected |
| Performance | Slower, loads the concentrator | Faster |
| Risk | — | An infected laptop can be **simultaneously** on the Internet and your LAN — a bridge into the network |

---

## ⚠️ A VPN Does NOT Make You Anonymous

Frequently examined, widely misunderstood. A VPN **moves** trust; it doesn't remove it.

- **Your VPN provider sees everything** the ISP used to. You've swapped who you trust. A "no-logs" claim is a promise, not a technical guarantee.
- **DNS leaks** — if DNS queries go outside the tunnel, your ISP still sees every domain you visit.
- **The endpoint is unchanged** — malware on your laptop, or logging into your own accounts, identifies you regardless.
- **Browser fingerprinting and cookies** track you across IP changes.
- **Traffic-correlation** by a global observer can de-anonymise timing/volume patterns.
- **It protects data in transit, not the destination** — a phishing site over VPN is still a phishing site.

> 💡 A **kill switch** blocks all traffic if the tunnel drops, preventing accidental cleartext leakage — a genuine feature worth knowing.

---

## ⚠️ Gotchas / Exam Traps

- **AH ≠ encryption.** ESP provides confidentiality.
- **Tunnel mode** protects the whole packet and is used for site-to-site; transport mode does not hide internal IPs.
- IKE **Phase 1 secures the negotiation; Phase 2 secures the data.**
- SAs are **unidirectional** — two per bidirectional tunnel.
- Split tunnelling is a **performance-vs-security** trade-off, and its worst risk is bridging an untrusted network into the corporate LAN.
- VPN ≠ anonymity, and VPN ≠ endpoint security.

---

## 📺 Learn by Video
*Supplementary only — these help you visualise. The technical content above is written independently and stands on its own.*

- ▶️ **"What is a VPN (and How Does it Work)? | VPN Explained"** — https://www.youtube.com/watch?v=7uKIB1TxGc4
- ▶️ **"IPsec Explained"** — https://www.youtube.com/watch?v=xTH1ZA_qUvA
- 🔍 WireGuard — [search](https://www.youtube.com/results?search_query=NetworkChuck+wireguard)

---

## Related Concepts

- [[Network Defence Mechanisms]] — Unit III hub
- [[CIA Triad]] — confidentiality/integrity guarantees a VPN provides
- [[Hash & Encryption Reference]] — the crypto primitives underneath ⚡
- [[Zero Trust Architecture]] — ZTNA as the modern replacement for broad VPN access
- [[Proxy Servers]] — often confused with VPNs; different scope
