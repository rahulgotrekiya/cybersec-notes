---
type: concept
subject: csf-ns
tags: [cybersecurity, concept, csf-ns, proxy, reverse-proxy, web-filtering, unit-3]
status: learning
created: 2026-09-04
---

# Proxy Servers

> 📝 **Concept Note** — CSF & NS Unit III Deep Dive | Source: MCA Sem III Syllabus

---

## The Problem It Solves

A [[Firewalls|firewall]] mostly answers "may these two addresses talk?". A **proxy** goes further: it **terminates the connection and makes its own**, so it fully understands the application-layer conversation and can inspect, cache, filter, log, or rewrite it.

A proxy is an **intermediary that breaks the direct connection** between client and server. Nothing flows end-to-end; there are always **two** connections.

---

## ⚙️ How It Actually Works

```
WITHOUT a proxy
Client ───────────────────► Server        (one connection)

WITH a proxy
Client ────► Proxy ────► Server           (TWO separate connections)
        conn 1      conn 2
```

1. Client sends its request to the **proxy** instead of the server.
2. Proxy terminates that connection and **reads the full application request** (e.g. `GET /page HTTP/1.1  Host: example.com`).
3. Proxy applies policy — allowed? cached? malware-scanned? log it.
4. Proxy opens a **new** connection to the real server **using its own IP**.
5. Response returns to the proxy, which inspects/caches it, then passes it to the client.

Because the proxy fully parses the request, it can make decisions a packet-level firewall cannot: block one URL path but allow the rest of a site, strip a malicious file from a response, or enforce SafeSearch.

**The server never sees the client's IP** (it sees the proxy's) — that's the direction-dependent anonymity property.

---

## Forward Proxy vs Reverse Proxy

This is the distinction most often examined. **Who is the proxy protecting/serving?**

```
FORWARD PROXY — protects/controls the CLIENTS
[Internal users] ──► [Forward Proxy] ──► Internet
                      ▲ sits in front of clients
                      organisation controls it

REVERSE PROXY — protects the SERVERS
Internet users ──► [Reverse Proxy] ──► [Internal web servers]
                    ▲ sits in front of servers
                    server owner controls it
```

| | **Forward Proxy** | **Reverse Proxy** |
|---|---|---|
| Sits in front of | Clients | Servers |
| Hides | The **client** from the server | The **server** from the client |
| Configured by | Client/network admin | Server operator |
| Client aware? | Sometimes (explicit) | **Never** — looks like the real site |
| Purpose | Filtering, monitoring, caching, access control | Load balancing, TLS termination, WAF, DDoS |
| Examples | Squid, corporate web gateway | Nginx, HAProxy, Cloudflare |

**Memory hook:** a *forward* proxy forwards **your** traffic out; a *reverse* proxy handles traffic coming **in** to someone's servers.

---

## Forward Proxy in the Enterprise

### Web filtering
Blocks categories (gambling, malware, adult), specific domains, or file types. Enforced **before** the request leaves the network.

**Web filter vs proxy — the distinction the syllabus wants:**
- A **web filter** is a *function*: decide allowed/blocked by URL/category.
- A **proxy** is an *architecture*: terminate and relay connections.
- A proxy is the usual **place** a web filter runs, but filtering can also happen via DNS filtering or firewall URL categories, with no proxy involved.

### Transparent vs Explicit

| | **Explicit** | **Transparent (intercepting)** |
|---|---|---|
| Client config | Browser/OS points at proxy (IP:port or PAC file) | **None** |
| User awareness | Aware | **Unaware** |
| How traffic arrives | Client deliberately sends it | Network **redirects** port 80/443 to the proxy |
| Weakness | User can remove the setting | Harder to bypass; can break some apps/TLS |

Transparent proxying works by the router/firewall diverting traffic to the proxy — the client believes it's talking to the real server.

### Caching
Stores frequently requested content locally: faster for users, less bandwidth. Historically the main reason proxies existed; less impactful now that almost everything is dynamic and encrypted.

### Other benefits
- **Logging/accountability** — a per-user record of web access.
- **Bandwidth control** — throttle streaming.
- **DLP** — inspect uploads for sensitive data leaving.

---

## 🔐 TLS/SSL Interception — how it really works

Most traffic is HTTPS, which a proxy cannot read... unless it performs a **deliberate, sanctioned man-in-the-middle**:

1. Organisation generates its own **internal Certificate Authority (CA)**.
2. That CA certificate is installed as **trusted** on every managed device (via group policy).
3. When you visit `https://bank.com`, the proxy intercepts, connects to the real bank itself, and then **generates a certificate for `bank.com` on the fly**, signed by the internal CA.
4. Your browser checks the certificate, sees it chains to a CA it trusts, and shows the padlock — **no warning**.
5. The proxy now holds cleartext in the middle: it can scan for malware and enforce DLP.

```
You ══TLS #1══► [Proxy: decrypt → inspect → re-encrypt] ══TLS #2══► bank.com
     (cert signed by                                        (real cert)
      company CA)
```

> ⚠️ **The trade-offs are serious.** The proxy sees *everything*, including employees' banking and health traffic. It becomes a catastrophic single point of compromise. It breaks **certificate pinning** (many mobile/banking apps refuse to work), and it defeats the end-to-end guarantee TLS is supposed to provide. Most organisations therefore **exempt** finance/health/government categories from interception.

---

## Reverse Proxy Duties

Sitting in front of servers, it typically provides:

1. **Load balancing** — distribute requests across a server pool (round-robin, least-connections), with health checks to skip dead nodes.
2. **TLS termination** — handle the expensive encryption centrally; back-end servers serve plain HTTP internally and stay simple.
3. **Web Application Firewall (WAF)** — inspect HTTP for SQLi/XSS payloads and block them before they reach the app.
4. **DDoS absorption** — a large provider (Cloudflare, Akamai) soaks up volumetric floods; the origin IP is never exposed.
5. **Caching / CDN** — serve static assets from edge locations near users.
6. **Single entry point** — one place for authentication, logging, and rate limiting.

---

## Proxy vs VPN vs NAT

| | **Proxy** | **[[VPN]]** | **NAT** |
|---|---|---|---|
| Layer | 7 (application) | 3 (network) | 3 |
| Scope | Usually one app (browser) | **All** traffic on the device | All traffic |
| Encrypts? | Not inherently | ✅ **Yes** | ❌ No |
| Purpose | Filter, cache, control, inspect | Secure private connectivity | Address conservation |
| Hides your IP from destination | ✅ | ✅ | ✅ |

> **Exam trap:** a proxy is **not** an encryption tool. It may relay HTTPS, but the proxy itself doesn't provide the confidentiality — TLS does. If a question asks "which encrypts all traffic from the device", the answer is **VPN**.

---

## ⚠️ Gotchas / Exam Traps

- **Forward = protects clients; reverse = protects servers.** State who is in front of whom.
- **Transparent proxy** requires no client configuration — that's its defining trait.
- A proxy creates **two connections**; nothing is end-to-end.
- **TLS inspection requires installing a trusted internal CA** on endpoints; without that, browsers show certificate warnings.
- A reverse proxy improves availability and adds a WAF, but is itself a chokepoint that must be highly available.

---

## 📺 Learn by Video
*Supplementary only — these help you visualise. The technical content above is written independently and stands on its own.*

- 🔍 **Proxy vs Reverse Proxy** (Hussein Nasser explains this best) — [search](https://www.youtube.com/results?search_query=Hussein+Nasser+proxy+vs+reverse+proxy)
- 🔍 Proxy server explained — [search](https://www.youtube.com/results?search_query=proxy+server+explained+animated)
- 🔍 Reverse proxy / Nginx — [search](https://www.youtube.com/results?search_query=NetworkChuck+reverse+proxy)

---

## Related Concepts

- [[Network Defence Mechanisms]] — Unit III hub
- [[Firewalls]] — NGFWs perform similar L7 inspection
- [[VPN]] — commonly confused; different layer and purpose
- [[DMZ & Network Segmentation]] — reverse proxies usually live in the DMZ
- [[Burp Suite]] — an *intercepting* proxy used offensively
