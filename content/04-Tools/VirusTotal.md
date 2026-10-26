---
type: tool
subject: shared
tags: [cybersecurity, tool, virustotal, threat-intel, ioc, soc, csf-ns]
status: learning
created: 2026-08-18
---

# VirusTotal

> 🔧 **Tool Note** — Category: Threat Intelligence / IoC Reputation | Freemium (web + API)

---

## 📌 Quick Overview
**What is it?** A free online service that analyses files, URLs, IPs, domains, and hashes against **70+ antivirus engines and sandboxes at once**, then aggregates the verdicts. It's the fastest way for an analyst to check whether an **Indicator of Compromise (IoC)** is known-bad.

**Official Website:** https://www.virustotal.com/
**License:** Free (public); paid **VT Enterprise** for private/bulk/retro hunting. Owned by Google (Chronicle).
**Used in:** CSF & NS (Practical 6 — reputation lookup during triage)
**Related concept:** [[SOC Operations & Incident Lifecycle]] (IoCs, threat feeds)

## 🎯 What Problem Does It Solve?
- One-stop verdict: "is this file/IP/domain malicious?" without installing 70 AV products
- Contextualise a SOC alert — turn a raw hash or IP into a reputation + threat name
- Pivot: from one IoC to related files, domains, and infrastructure (the **Relations** graph)
- Access community + sandbox behaviour without running the malware yourself

## ⚙️ How It Works
```
You submit  →  hash | file | URL | IP | domain
                 │
      ┌──────────┴───────────┐
   HASH known?            new FILE
   look up cached         detonated + scanned by
   report (instant)       70+ engines & sandboxes
                 │                │
                 └──────► aggregated report ◄──────┘
        detection ratio  +  behaviour  +  relations  +  community
```

1. **Hash lookup (no upload):** every file has a unique fingerprint ([[Hash & Encryption Reference|hash]] — MD5/SHA-256). If anyone ever submitted that file, VT already has a report. You can paste **just the hash** and get the verdict **without uploading the file** — critical when the file is sensitive/confidential.
2. **File/URL submission:** if unknown, VT runs it through all engines and **sandbox detonation** (executes it in an isolated VM to record behaviour — files created, connections made, registry changes).
3. **Aggregation:** results become a **detection ratio** (e.g. `48 / 72` engines flag it), plus engine-specific malware names, sandbox behaviour, and **Relations** (contacted IPs/domains, dropped files) you can pivot through.

**Why detections vary:** each engine uses different signatures/heuristics, so ratios differ and a low ratio ≠ safe (could be brand-new malware) while a couple of hits might be **false positives**. Read the *names* and behaviour, don't just count.

## 🔬 Typical Workflow (Practical 6 triage)
1. From [[netstat & ss]] you found the host beaconing to `203.0.113.10`.
2. Search that **IP** in VirusTotal → check the detection ratio + which vendors flag it + what domains resolve to it.
3. Grab the **hash** of the suspicious process/file → look it up → get the malware family name.
4. Record the verdict in your incident note as supporting evidence; feed the confirmed IoCs to your [[SOC Operations & Incident Lifecycle|SOC]] blocklist.

## 🖥️ API (automation)
```bash
# Look up a file hash (public API)
curl -s --request GET \
  --url https://www.virustotal.com/api/v3/files/<SHA256> \
  --header "x-apikey: <API_KEY>"
# Also: /ip_addresses/<ip>, /domains/<domain>, /urls/<id>
```

## ⚠️ Gotchas / Limitations — read this
- **Privacy: public uploads are shared.** Anyone with VT access (incl. adversaries and other researchers) can download files you upload. **Never upload confidential/internal documents** — look up the **hash** instead. Attackers also watch VT to see if their malware was caught.
- **Detection ratio is a signal, not a verdict** — 0/72 can still be a fresh 0-day; 2/72 can be a false positive. Interpret, don't count.
- Public API is **rate-limited** (4 requests/min, 500/day) — fine for manual triage, not bulk.
- A malicious verdict on an IP/domain may be **stale** or shared-hosting collateral — corroborate.

## 🔗 Related Notes
- [[SOC Operations & Incident Lifecycle]] — IoCs, threat feeds, alert triage
- [[Hash & Encryption Reference]] — how file hashes work (the lookup key) ⚡
- [[netstat & ss]] — source of IPs/processes to look up
- [[Shodan]] — complementary OSINT/reputation source
