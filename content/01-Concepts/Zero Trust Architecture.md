---
type: concept
subject: csf-ns
tags: [cybersecurity, concept, csf-ns, zero-trust, ztna, identity, unit-3]
status: learning
created: 2026-09-04
---

# Zero Trust Architecture

> 📝 **Concept Note** — CSF & NS Unit III Deep Dive | Source: MCA Sem III Syllabus

---

## The Problem It Solves

Traditional security is the **castle-and-moat** model: a hard perimeter with a soft, trusting interior. Get past the [[Firewalls|firewall]] and you're "inside", and inside is trusted.

Three things broke that model:

1. **The perimeter dissolved.** Cloud apps, SaaS, remote work and personal devices mean the assets and the users are no longer inside any wall.
2. **Attackers get inside anyway** — one phishing email is enough. Then implicit internal trust hands them free lateral movement.
3. **Insiders** were always inside the wall.

Zero Trust removes the idea of a trusted zone: **"Never trust, always verify."** Trust is never granted because of *where a request comes from* — only because of *who and what is making it, verified right now*.

---

## ⚙️ How It Actually Works — the NIST SP 800-207 model

Zero Trust is an **architecture**, not a product. The reference model splits into a **control plane** (decides) and a **data plane** (enforces).

```
                 ┌──────────── CONTROL PLANE ────────────┐
                 │                                       │
                 │   ┌───────────────┐                   │
   Signals ─────►│   │ POLICY ENGINE │  decides GRANT/   │
   • identity    │   │     (PE)      │  DENY per request │
   • device      │   └───────┬───────┘                   │
   • location    │           │ decision                  │
   • behaviour   │   ┌───────▼──────────┐                │
   • threat intel│   │POLICY ADMIN (PA) │ issues/revokes │
                 │   └───────┬──────────┘ the session    │
                 └───────────┼───────────────────────────┘
                             │ configures
   ┌─────────┐      ┌────────▼────────┐      ┌──────────┐
   │  USER   │─────►│ POLICY ENFORCE- │─────►│ RESOURCE │
   │ +DEVICE │      │  MENT POINT(PEP)│      │ (app/data)│
   └─────────┘      └─────────────────┘      └──────────┘
                       DATA PLANE
```

- **Policy Engine (PE)** — the brain. For **each** access request it evaluates signals against policy and returns allow/deny. Modern PEs score risk dynamically rather than applying a static rule.
- **Policy Administrator (PA)** — executes the decision: establishes or tears down the session, issues the credential/token.
- **Policy Enforcement Point (PEP)** — the gate that sits in the traffic path (a gateway, proxy, or agent). It permits, monitors, and terminates connections. **The user never connects to the resource directly** — the PEP brokers every connection.

### The critical property
Verification is **per-request and continuous**, not once at login. If your device posture degrades mid-session (antivirus disabled, jailbreak detected) or behaviour becomes anomalous (sudden bulk download), the session is **re-evaluated and can be revoked immediately**.

---

## The Core Principles

### 1. Verify explicitly
Authenticate and authorise on **all available signals**, not just a password:
- **Identity** — who (strong auth, **MFA** is effectively mandatory)
- **Device** — is it managed, encrypted, patched, running EDR? (**device posture**)
- **Location / network** — impossible travel? unusual country?
- **Behaviour** — does this match the user's normal pattern?
- **Data sensitivity** — accessing payroll demands more assurance than the lunch menu.

### 2. Least privilege access
Grant the **minimum** access, for the **shortest time**:
- **Just-in-time (JIT)** — elevate only when needed, automatically expire.
- **Just-enough-access (JEA)** — only the specific resource, not the whole subnet.
- Per-application access, never "you're on the VPN so you can reach the whole network".

### 3. Assume breach
Design as though attackers are **already inside**:
- **Micro-segmentation** so a compromise can't spread ([[DMZ & Network Segmentation]]).
- **Encrypt everything**, including internal east-west traffic.
- Comprehensive logging and monitoring feeding the [[SOC Operations & Incident Lifecycle|SOC]].
- Minimise blast radius by default.

---

## Traditional vs Zero Trust

| | Traditional (perimeter) | Zero Trust |
|---|---|---|
| Trust basis | **Network location** ("inside") | **Identity + device + context** |
| Verification | Once, at the perimeter | **Continuously, every request** |
| Access granted | Broad network access | Per-resource, least privilege |
| Default posture | Allow inside, deny outside | **Deny by default, everywhere** |
| Assumption | The perimeter holds | **A breach has already happened** |
| Lateral movement | Easy once inside | Blocked by micro-segmentation |

---

## ZTNA vs Traditional VPN

This is the most practical, most examinable comparison.

| | **Traditional [[VPN]]** | **ZTNA** (Zero Trust Network Access) |
|---|---|---|
| Grants | Access to the **network** | Access to a **specific application** |
| After connecting | You're "on the LAN" — can scan/reach everything permitted | You can reach only what policy allows; other apps are invisible |
| Trust check | At connection time | Continuous, per request |
| Visibility of resources | Whole subnets discoverable | **Apps are hidden/dark** unless authorised |
| Compromised account impact | Broad lateral movement | Confined to that one app |

The core failure of VPN-as-security: it authenticates you **once** and then places you **inside** a trusted network. ZTNA never puts you "inside" anything — the broker connects you to one app at a time.

---

## Implementing It (realistically)

Zero Trust is a multi-year journey, not a purchase. A sane order:

1. **Identity first** — strong IdP, **MFA everywhere**, SSO. Without solid identity nothing else works.
2. **Device inventory & posture** — you can't evaluate what you can't see; enforce managed/compliant devices.
3. **Classify resources** — know what data matters, so policy can be risk-proportionate.
4. **Segment** — start with the crown jewels; move toward micro-segmentation.
5. **Enforce least privilege** — remove standing admin, add JIT elevation.
6. **Monitor continuously** — feed everything to the SIEM; use analytics for anomalies.

> ⚠️ Vendors sell "Zero Trust products". There is no such thing as a Zero Trust box — it is an architecture and an operating philosophy that spans identity, device, network, and data.

---

## ⚠️ Gotchas / Exam Traps

- Zero Trust ≠ "no trust at all" — it means **no *implicit* trust based on network location**. Trust is granted, but always **earned and verified**.
- It is **not a product** you install.
- It doesn't eliminate the network perimeter or firewalls; it **stops relying on them as the trust boundary**.
- **MFA alone is not Zero Trust** — it's one signal in "verify explicitly".
- Know the three PE/PA/PEP components and that **verification is continuous**, not one-time.

---

## 📺 Learn by Video
*Supplementary only — these help you visualise. The technical content above is written independently and stands on its own.*

- 🔍 **IBM Technology — "Zero Trust Explained"** — [search](https://www.youtube.com/results?search_query=IBM+Technology+zero+trust+explained)
- 🔍 Zero Trust for Security+ — [search](https://www.youtube.com/results?search_query=Professor+Messer+zero+trust)
- 🔍 ZTNA vs VPN — [search](https://www.youtube.com/results?search_query=ZTNA+vs+VPN+explained)

---

## Related Concepts

- [[Network Defence Mechanisms]] — Unit III hub
- [[DMZ & Network Segmentation]] — micro-segmentation is Zero Trust at the network layer
- [[VPN]] — what ZTNA is replacing
- [[CIA Triad]] — the goals Zero Trust protects
- [[What is Cybersecurity]] — least privilege, defence in depth
