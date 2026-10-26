---
type: reference
subject: shared
tags: [cybersecurity, reference, cheatsheet, burp-suite, web-security, quickref]
status: learning
created: 2026-07-02
---

# Burp Suite Cheatsheet ⚡

> ⚡ **Quick Reference** — Burp Suite Community Edition for web application testing

---

## Setup
1. Start Burp Suite → Temporary Project → Use Burp defaults
2. Configure browser proxy: `127.0.0.1:8080` (use FoxyProxy)
3. Visit `http://burpsuite` in browser → Download CA certificate → Install in browser

## Proxy
| Action | How |
|--------|-----|
| Intercept ON | Proxy → Intercept → Intercept is on |
| Forward request | Click "Forward" |
| Drop request | Click "Drop" |
| Send to Repeater | Right-click → Send to Repeater |
| Send to Intruder | Right-click → Send to Intruder |

## Repeater (Manual Testing)
1. Send intercepted request to Repeater (Ctrl+R)
2. Modify parameters (e.g., inject SQL payload)
3. Click "Send"
4. Analyse response
5. Iterate with different payloads

## Intruder (Automated Attacks)
**Attack Types:**
| Type | Use |
|------|-----|
| **Sniper** | Test ONE position with payload list |
| **Battering Ram** | Same payload in ALL positions |
| **Pitchfork** | Different lists for each position (parallel) |
| **Cluster Bomb** | All combinations of all lists |

**Workflow:**
1. Send request to Intruder
2. Mark positions with § markers: `username=§admin§&password=§pass§`
3. Set payload lists (e.g., wordlist for passwords)
4. Start Attack
5. Sort by response length/status to find successes

## Decoder
| Encode/Decode | Example |
|---------------|---------|
| Base64 encode | `admin:password` → `YWRtaW46cGFzc3dvcmQ=` |
| URL encode | `admin' OR '1'='1` → `admin%27+OR+%271%27%3D%271` |
| HTML decode | `&lt;script&gt;` → `<script>` |

## Target Scope
Target → Scope → Add URL to scope → Only test in-scope targets

## Keyboard Shortcuts
| Shortcut | Action |
|----------|--------|
| Ctrl+R | Send to Repeater |
| Ctrl+I | Send to Intruder |
| Ctrl+Shift+T | Switch to Target tab |
