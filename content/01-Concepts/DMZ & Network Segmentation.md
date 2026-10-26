---
type: concept
subject: csf-ns
tags: [cybersecurity, concept, csf-ns, dmz, segmentation, vlan, micro-segmentation, unit-3]
status: learning
created: 2026-09-04
---

# DMZ & Network Segmentation

> 📝 **Concept Note** — CSF & NS Unit III Deep Dive | Source: MCA Sem III Syllabus

---

## The Problem It Solves

A perimeter [[Firewalls|firewall]] protects the boundary — but once an attacker is **inside**, a flat network lets them reach everything. Segmentation answers the question: *"assuming they get in, how far can they get?"*

It is the practical implementation of **assume breach** ([[Zero Trust Architecture]]) and the main control against **lateral movement**.

---

## ⚙️ How It Actually Works

### The core insight: north-south vs east-west traffic

```
                    Internet
                       │
                       │  NORTH-SOUTH  (in/out of the network)
                       │  ← firewalls inspect this well
              ┌────────▼────────┐
              │    Firewall     │
              └────────┬────────┘
                       │
     ┌─────────┬───────┴───────┬─────────┐
     │         │               │         │
  [Server A]─[Server B]────[Server C]─[Workstation]
     └─────────┴───────────────┴─────────┘
          EAST-WEST (server to server, inside)
          ← a perimeter firewall NEVER SEES THIS
```

Perimeter firewalls inspect **north-south** traffic. Traffic *between* internal hosts (**east-west**) never crosses the firewall, so it is completely uninspected. That is exactly the traffic an attacker uses after landing on one machine. Segmentation forces east-west traffic through a control point.

### Lateral movement — what segmentation actually stops

1. Attacker phishes one workstation → gets a foothold.
2. Scans the local subnet — on a **flat** network they see file servers, databases, domain controllers, printers, everything.
3. Reuses stolen credentials / exploits an unpatched internal box → moves to it.
4. Repeats until they reach the domain controller or the database.

The initial compromise is rarely the valuable machine. **The breach happens during step 2–4.** Segmentation breaks the chain: the attacker on the user VLAN simply cannot reach the database VLAN, because a firewall/ACL between them denies it.

---

## Flat vs Segmented Networks

| | Flat network | Segmented network |
|---|---|---|
| Layout | Everything in one broadcast domain | Split into zones by function/sensitivity |
| Blast radius | **Entire network** | One zone |
| Internal traffic control | None | Firewall/ACL between zones |
| Broadcast noise | High | Contained |
| Compliance | Fails (e.g. PCI-DSS) | Scopes audit to the card zone only |

**Typical zones:** User workstations · Servers · Databases · DMZ · Management · Guest Wi-Fi · IoT/OT.

> 💡 Practical rule: put the **guest Wi-Fi and IoT devices** on their own isolated segment with internet-only access. A compromised smart TV or camera then cannot touch your servers — this is the single most common real-world win.

---

## The DMZ (Demilitarised Zone)

A **DMZ** is a separate network zone for services that **must** be reachable from the Internet — web, mail, DNS, public API.

**The dilemma it resolves:** a public web server must accept connections from anyone, which makes it the most likely thing to be compromised. If it sits on the internal LAN, compromising it hands the attacker your LAN. If you disconnect it, it can't serve anyone.

**The DMZ answer:** put it in a third zone that is reachable from the Internet but **whose access into the internal LAN is tightly restricted**. When (not if) it is compromised, the attacker is stuck in a low-value zone.

### Single-firewall (three-legged) DMZ

```
                Internet
                    │
              ┌─────▼─────┐
              │  Firewall │  (3 interfaces)
              └──┬─────┬──┘
                 │     │
        ┌────────▼─┐ ┌─▼──────────┐
        │   DMZ    │ │ Internal   │
        │ Web/Mail │ │    LAN     │
        └──────────┘ └────────────┘
```
One firewall, three interfaces (Internet / DMZ / LAN). Cheaper and simpler — but it is a **single point of failure**, and one misconfiguration or firewall compromise exposes everything.

### Dual-firewall (screened subnet) DMZ — more secure

```
   Internet ──► [ External Firewall ] ──► DMZ ──► [ Internal Firewall ] ──► LAN
                  permissive-ish            strict, deny most
```
Two firewalls, ideally **from different vendors** — a zero-day in one vendor's product doesn't breach both. The external firewall is relatively permissive (it must allow 80/443 in); the internal one is strict.

### The rules that make a DMZ a DMZ

| From | To | Allowed? |
|---|---|---|
| Internet | DMZ | ✅ Yes (80/443 only) |
| DMZ | Internet | ⚠️ Limited (updates only) |
| **DMZ** | **Internal LAN** | 🚫 **Denied by default** — the critical rule |
| Internal LAN | DMZ | ✅ Yes (admin/management) |
| Internet | Internal LAN | 🚫 Never directly |

If the web server needs a database, the database stays **on the internal side**, and you allow *only* the DMZ web server to reach *only* the DB server on *only* the DB port — never a general DMZ→LAN allow.

---

## VLANs — how segmentation is actually implemented

A **VLAN** splits one physical switch into multiple isolated logical networks. Two devices on the same switch but different VLANs cannot talk directly at Layer 2, as if they were on separate switches.

**802.1Q tagging** — the mechanism:
- A **4-byte VLAN tag** (containing a 12-bit **VLAN ID**, so 1–4094) is inserted into the Ethernet frame header.
- **Access port** — connects to an end device (a PC). Traffic is **untagged**; the switch assigns the port's VLAN internally.
- **Trunk port** — connects switch-to-switch or switch-to-router. Carries **many VLANs**, so frames are **tagged** to identify which VLAN each belongs to.

```
PC (VLAN 10) ──access(untagged)──► Switch A ══trunk(tagged)══► Switch B ──access──► PC (VLAN 10)
                                                 carries VLANs 10,20,30
```

**Inter-VLAN routing:** VLANs are separate Layer 2 domains, so traffic between them needs a **Layer 3** device (router or L3 switch) — which is precisely where you apply **ACLs/firewall rules**. That routing chokepoint is what turns a VLAN into a security control.

> ⚠️ **A VLAN by itself is not security — it's separation.** Without ACLs on the inter-VLAN routing, all VLANs can still reach each other; you've only reduced broadcast traffic.

### VLAN hopping (the attack)
- **Switch spoofing** — attacker's machine pretends to be a switch and negotiates a trunk (via DTP), gaining access to all VLANs. **Fix:** disable DTP/auto-trunking; set access ports to `switchport mode access` explicitly.
- **Double tagging** — attacker sends a frame with two VLAN tags; the first switch strips the outer tag and forwards it into the victim VLAN. **Fix:** never use the **native VLAN** for user data; change it from the default VLAN 1.

---

## Micro-Segmentation

VLANs segment by **network zone**. Micro-segmentation goes to the **individual workload**: each VM/container/app gets its own policy, enforced at the host or hypervisor rather than at a network chokepoint.

- Policies follow the workload wherever it moves (essential in cloud/virtualised estates).
- Enforced by identity/tags ("web tier may talk to app tier on 8080") not IP subnets.
- Enables **default-deny east-west** — the practical realisation of [[Zero Trust Architecture]] at the network layer.

| | VLAN segmentation | Micro-segmentation |
|---|---|---|
| Granularity | Subnet/zone | Single workload |
| Enforced at | Router/firewall | Hypervisor/host agent |
| Follows workload | ❌ | ✅ |
| East-west default | Usually allowed within zone | Default deny |

---

## ⚠️ Gotchas / Exam Traps

- **"Why not put the web server on the LAN?"** — because it is Internet-exposed and therefore most likely to be compromised; on the LAN, its compromise = LAN compromise. The DMZ contains the blast radius.
- Segmentation limits **lateral movement**; it does not prevent the initial compromise.
- **VLAN ≠ security** without inter-VLAN ACLs.
- A DMZ with a general **DMZ → internal LAN allow** rule defeats the entire purpose.
- Flat networks fail compliance regimes (PCI-DSS) because audit scope becomes the whole network.

---

## 📺 Learn by Video
*Supplementary only — these help you visualise. The technical content above is written independently and stands on its own.*

- ▶️ **"VLAN Explained"** — https://www.youtube.com/watch?v=jC6MJTh9fRE
- ▶️ **"VLANs, Tagging, Trunking, VxLAN, & Native VLAN"** (the 802.1Q detail) — https://www.youtube.com/watch?v=MNZa-S4E-To
- 🔍 DMZ / screened subnet — [search](https://www.youtube.com/results?search_query=DMZ+network+explained+screened+subnet)

---

## Related Concepts

- [[Network Defence Mechanisms]] — Unit III hub
- [[Firewalls]] — enforces the rules between zones
- [[Zero Trust Architecture]] — micro-segmentation is Zero Trust at network level
- [[OSI Model & TCP-IP Security]] — L2 vs L3 boundaries
- [[Networking Fundamentals]] — switches, broadcast domains, subnets
