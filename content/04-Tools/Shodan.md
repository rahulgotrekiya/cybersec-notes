---
type: tool
subject: shared
tags: [cybersecurity, tool, shodan, osint, reconnaissance, csf-ns]
status: learning
created: 2026-08-18
---

# Shodan

> 🔧 **Tool Note** — Category: OSINT / Attack-Surface Discovery | Freemium (web + API)

---

## 📌 Quick Overview
**What is it?** A **search engine for Internet-connected devices**. Where Google indexes web *pages*, Shodan indexes the *services* running on public IP addresses — servers, routers, webcams, ICS/SCADA, databases — by grabbing and cataloguing their **banners**.

**Official Website:** https://www.shodan.io/
**License:** Free tier (limited results/filters); paid membership + API credits
**Used in:** CSF & NS (prerequisites list Shodan lookups; passive OSINT recon)
**Related concepts:** [[What is Cybersecurity]] (attack surface), [[Network Traffic Analysis]]

## 🎯 What Problem Does It Solve?
- See your organisation's **external attack surface** the way an attacker does — what's exposed to the Internet
- Find devices with default credentials, exposed databases, outdated/vulnerable service versions
- Research how widespread a vulnerability is (e.g. "how many hosts still run vsftpd 2.3.4?")
- Do all of this **passively** — without sending a single packet to the target yourself

## ⚙️ How It Works
Shodan runs a fleet of **crawlers** distributed worldwide that continuously scan the IPv4 Internet:

```
Shodan scanners → connect to (IP : port) across the Internet →
  grab the service BANNER (the greeting/metadata a service returns) →
  parse + enrich (geo, org, OS, product, version, CVEs) →
  index into a searchable database  ←  YOU query this database
```

1. **Banner grabbing:** when you connect to a service it often announces itself — an SSH server sends its version string, a web server returns HTTP headers + `Server:`, an RTSP camera returns its model. Shodan opens connections across common ports (21, 22, 23, 80, 443, 3389, 502…) and stores these banners.
2. **Enrichment:** each banner is tagged with location, owning organisation (from IP WHOIS), detected product/version, and any **CVEs** known for that version.
3. **Indexing:** results go into a database you search with **filters**.

**Why it's "passive" recon for you:** the target was scanned by *Shodan's* infrastructure, possibly weeks ago. When you search, you query Shodan — the target's logs never see you. That's the key OSINT distinction from actively running [[Nmap]] against them (which *does* touch the target and can be logged/illegal without authorisation).

## 🔎 Search Filters (the real skill)
```
apache country:"IN"            # Apache servers in India
port:3389 os:"Windows"         # exposed RDP on Windows
product:MySQL                  # MySQL instances
"default password"             # banners literally mentioning default password
http.title:"Dashboard"         # web apps with Dashboard in the title
vuln:CVE-2021-44228            # hosts flagged with Log4Shell (paid)
org:"Example Corp" net:203.0.113.0/24   # scope to an org / subnet
```
Filters combine — this is what makes it powerful vs a plain keyword.

## 🖥️ CLI & API
```bash
pip install shodan
shodan init <API_KEY>
shodan host 8.8.8.8          # everything Shodan knows about an IP
shodan search "port:22 country:IN" --limit 10
shodan count apache          # how many results (cheap, no result credits)
```

## ⚠️ Gotchas / Limitations
- Data is a **snapshot** — a host may have changed/patched since last scanned; verify before acting.
- Free tier heavily limits filters and result counts; `vuln:` needs a paid plan.
- **Ethics/legality:** *querying* Shodan is fine; using findings to access a system without authorisation is a crime. It's a recon aid, not a licence. See [[What is Cybersecurity]] on ethics.
- Banners can be spoofed/honeypots — a "vulnerable" banner isn't proof of exploitability.

## 🔗 Related Notes
- [[Nmap]] — active scanning; Shodan is the passive, pre-scanned counterpart
- [[What is Cybersecurity]] — attack surface concept
- [[Network Defence Mechanisms]] — reducing what you expose (segmentation, firewalls)
- [[VirusTotal]] — another OSINT/reputation lookup source
