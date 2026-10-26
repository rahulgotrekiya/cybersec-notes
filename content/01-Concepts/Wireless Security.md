---
type: concept
subject: csf-ns
tags: [cybersecurity, concept, csf-ns, wireless, wpa2, wpa3, wifi-attacks, unit-3]
status: learning
created: 2026-09-04
---

# Wireless Security

> 📝 **Concept Note** — CSF & NS Unit III Deep Dive | Source: MCA Sem III Syllabus

---

## The Problem It Solves

Wired networks have a physical security property: to attack them you must **plug something in**. Wi-Fi broadcasts in every direction through walls and out into the car park. **Anyone within radio range can passively capture every frame** — no compromise required.

So wireless security cannot rely on physical access control. It must assume the attacker is already listening and solve everything with cryptography and authentication.

---

## The Evolution — and why each generation broke

| | **WEP** (1997) | **WPA** (2003) | **WPA2** (2004) | **WPA3** (2018) |
|---|---|---|---|---|
| Cipher | RC4 | RC4 + TKIP | **AES-CCMP** | AES-GCMP |
| Key exchange | Static key | 4-way handshake | 4-way handshake | **SAE (Dragonfly)** |
| Forward secrecy | ❌ | ❌ | ❌ | ✅ |
| Status | **Broken** | Deprecated | Vulnerable (KRACK) but patchable | Current |

- **WEP** failed because its 24-bit **initialisation vector (IV)** was far too short. On a busy network IVs repeat within hours, and reused keystream lets an attacker recover the key. WEP is crackable in **minutes** — never acceptable.
- **WPA** was a stopgap on the same hardware: TKIP rotated keys per packet, but kept RC4 and is now broken too.
- **WPA2** moved to **AES-CCMP** — the cipher itself remains sound. Its weakness is the **handshake** (KRACK) and offline cracking of weak passphrases.
- **WPA3** fixes the handshake itself.

---

## ⚙️ How It Actually Works — the WPA2 4-Way Handshake

This is the heart of Unit III's wireless section. The goal: **both sides prove they know the password and derive fresh session keys — without ever sending the password over the air.**

**Starting point:** both the client and AP already know the **PMK** (Pairwise Master Key), derived from the passphrase + SSID via PBKDF2. The PMK itself is *never* transmitted.

```
CLIENT (supplicant)                         ACCESS POINT (authenticator)
        │                                              │
        │  ◄─── (1) ANonce ─────────────────────────── │  AP sends random number
        │                                              │
   Now client has: PMK + ANonce + SNonce + both MACs   │
   → derives the PTK (session key)                     │
        │                                              │
        │  ─── (2) SNonce + MIC ──────────────────────►│  client's random + proof
        │                                              │
        │                          AP now derives the same PTK,
        │                          verifies MIC → client knew the PMK ✅
        │                                              │
        │  ◄─── (3) GTK (encrypted) + MIC ──────────── │  group key for broadcast
        │                                              │
        │  ─── (4) ACK ───────────────────────────────►│
        │                                              │
        └────────── encrypted session begins ──────────┘
```

**The pieces:**
- **ANonce / SNonce** — random "numbers used once" from AP and client. They guarantee **every session derives different keys**, even with the same password.
- **PTK (Pairwise Transient Key)** — the actual session key encrypting *your* unicast traffic. Derived as `PTK = f(PMK, ANonce, SNonce, AP MAC, client MAC)`. Because both sides have all five inputs, **both compute the same PTK independently — it is never transmitted**.
- **MIC (Message Integrity Code)** — proves the sender knows the PTK (and therefore the PMK/password) without revealing it.
- **GTK (Group Temporal Key)** — shared by all clients, used for broadcast/multicast; re-keyed when a device leaves.

### Why this handshake is attackable
The handshake is sent **in the clear** (it must be — no keys are established yet). An attacker who captures all four messages gets the ANonce, SNonce, MACs, and MIC. They cannot reverse the MIC, but they **can guess offline**:

```
for each candidate password:
    PMK  = PBKDF2(password, SSID)
    PTK  = f(PMK, ANonce, SNonce, MACs)
    if computed MIC == captured MIC:  → password found
```

This is **offline brute force** — unlimited attempts, no interaction with the network, no lockout. **The only defence is a long, high-entropy passphrase.** A dictionary word falls in seconds; a 20-character random passphrase is infeasible.

> 💡 This is why attackers send **deauth** frames: to force a reconnection so they can capture a fresh handshake. (Also why **PMKID** attacks matter — on some APs a single frame yields crackable material without waiting for any client.)

---

## WPA2-Personal vs WPA2-Enterprise

| | **Personal (PSK)** | **Enterprise (802.1X)** |
|---|---|---|
| Credential | One shared passphrase for everyone | **Per-user** credentials/certificates |
| Auth backend | None | **RADIUS** server + EAP |
| Revoking one user | Change password for **everyone** | Disable that one account |
| Attacker who knows the password | Can decrypt others' traffic (with handshake) | Cannot — keys are per-user |
| Use | Home, small office | **Any real organisation** |

**802.1X flow:** the client (**supplicant**) authenticates through the AP (**authenticator**) to a **RADIUS server** (**authentication server**). The AP passes traffic only after the server approves, and a **unique PMK per user per session** is issued. EAP variants: **EAP-TLS** (certificates both sides — strongest), PEAP/EAP-TTLS (server cert + user password).

---

## WPA3 — how it fixes things

**SAE (Simultaneous Authentication of Equals)**, a.k.a. **Dragonfly**, replaces the PSK handshake:

- It is a **password-authenticated key exchange**. The captured exchange contains **no material that can be brute-forced offline** — each password guess requires a **fresh live interaction** with the AP, which can be rate-limited and logged.
- **Forward secrecy**: each session uses fresh ephemeral keys. Capturing traffic today and learning the password *later* still doesn't decrypt it. (Under WPA2 it does — a critical difference.)
- **Protected Management Frames (802.11w) are mandatory**, which kills classic deauth attacks.
- **OWE (Opportunistic Wireless Encryption)** for open networks: encrypts café/airport Wi-Fi individually per client even with no password. Not authentication — but it stops trivial passive sniffing.

---

## Common Wi-Fi Attacks

### Evil Twin
Attacker runs a rogue AP broadcasting the **same SSID** as a legitimate one, often with a stronger signal. Devices prefer the stronger signal and **auto-connect** (especially to remembered networks). All victim traffic then flows through the attacker.

Usually paired with a **captive portal** that phishes credentials. **Defences:** always use HTTPS/[[VPN]], disable auto-connect to open networks, use 802.1X (the client validates the server certificate, so a fake AP fails), and WIPS to detect rogue APs.

### Deauthentication attack
In WPA2, **management frames are unauthenticated**. Anyone can forge a "deauthenticate" frame claiming to be the AP, and the client obeys and disconnects.

Used to (a) deny service, or (b) — more importantly — **force a reconnection to capture the 4-way handshake** for offline cracking, or push victims onto an evil twin.

**Defence: 802.11w Protected Management Frames (PMF)**, which authenticates management frames. Optional in WPA2, **mandatory in WPA3**.

### KRACK (Key Reinstallation Attack, 2017)
Targets the handshake **protocol logic**, not the AES cipher.

**Mechanism:** message 3 of the handshake may be retransmitted if the AP doesn't receive an ACK. An attacker in a MitM position **replays message 3**. The client **reinstalls the already-in-use PTK**, which **resets the nonce/packet-number counter** to zero. Reusing a nonce with the same key means the same keystream encrypts different plaintexts — enabling decryption and, in some cases, packet forgery.

**Key points:** it did **not** break AES and does **not** reveal the Wi-Fi password. It was fixed in **software** — patch clients *and* APs. Android/Linux (wpa_supplicant) were worst hit because they reinstalled an all-zero key.

### Others worth knowing
- **WPS PIN** — the 8-digit PIN is validated in two halves, reducing brute force to ~11,000 attempts. **Always disable WPS.**
- **Rogue AP** — an employee plugging in an unauthorised AP, bypassing all perimeter controls.
- **Karma/probe attacks** — devices broadcast probes for remembered SSIDs; an attacker answers "yes, that's me".

---

## Securing a Wireless Network

1. **Use WPA3** where supported; otherwise **WPA2-AES** (never WEP/TKIP).
2. **Long random passphrase** (20+ chars) — the *only* defence against offline cracking on PSK.
3. **WPA2/WPA3-Enterprise (802.1X)** for any organisation — per-user credentials and revocation.
4. **Disable WPS.**
5. **Enable 802.11w (PMF)** to stop deauth.
6. Change default **admin credentials** and SSID; keep **firmware patched** (KRACK et al.).
7. **Separate guest and IoT SSIDs** onto isolated VLANs ([[DMZ & Network Segmentation]]).
8. Deploy **WIDS/WIPS** to detect rogue and evil-twin APs.
9. Reduce transmit power / place APs so coverage doesn't flood the car park.

> ⚠️ **Not real security:** hiding the SSID (it's still in probe/beacon traffic) and **MAC filtering** (MACs are sent in cleartext and trivially spoofed). Know these as classic exam distractors.

---

## ⚠️ Gotchas / Exam Traps

- **The PMK/password is never transmitted** in the 4-way handshake — the MIC proves knowledge of it.
- Capturing a handshake enables **offline** brute force; passphrase strength is the whole defence.
- **KRACK broke the handshake logic (nonce reuse), not AES**, and does not disclose the password.
- **Deauth works because WPA2 management frames are unauthenticated** → fixed by 802.11w/WPA3.
- **WPA3's SAE gives forward secrecy**; WPA2 does not — learning the password later decrypts old WPA2 captures.
- **SSID hiding and MAC filtering are not security controls.**

---

## 📺 Learn by Video
*Supplementary only — these help you visualise. The technical content above is written independently and stands on its own.*

- ▶️ **"WiFi (Wireless) Password Security — WEP, WPA, WPA2, WPA3, WPS Explained"** — https://www.youtube.com/watch?v=WZaIfyvERcA
- ▶️ **"Next-Gen Wi-Fi Security — WPA3 Explained"** — https://www.youtube.com/watch?v=aPoe4WtX2mU
- 🔍 KRACK attack (Computerphile) — [search](https://www.youtube.com/results?search_query=Computerphile+KRACK+attack)
- 🔍 4-way handshake / Wi-Fi cracking — [search](https://www.youtube.com/results?search_query=David+Bombal+wifi+4-way+handshake)

---

## Related Concepts

- [[Network Defence Mechanisms]] — Unit III hub
- [[Hash & Encryption Reference]] — PBKDF2, AES, MIC primitives ⚡
- [[DMZ & Network Segmentation]] — isolating guest/IoT SSIDs
- [[VPN]] — protects you on untrusted/open Wi-Fi
- [[OSI Model & TCP-IP Security]] — Layer 2 attacks
