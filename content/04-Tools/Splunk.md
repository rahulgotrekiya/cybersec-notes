---
type: tool
subject: shared
tags: [cybersecurity, tool, splunk, siem, soc, log-analysis]
status: learning
created: 2026-07-02
---

# Splunk

> 🔧 **Tool Note** — Category: SIEM Platform | Commercial (Free tier available)

---

## 📌 Quick Overview
**What is it?** A Security Information and Event Management (SIEM) platform that collects, indexes, searches, and visualises machine-generated data (logs).

**Official Website:** https://www.splunk.com/
**License:** Splunk Free (500MB/day) / Enterprise (paid)
**Used in:** CSF & NS, DFIR

## 🎯 What Problem Does It Solve?
- Centralise logs from hundreds of sources into one searchable platform
- Correlate events across devices (firewall + server + endpoint)
- Detect security incidents through automated alerting
- Create dashboards for SOC monitoring
- Search and analyse historical log data

## ⚙️ How It Works
Splunk's job is **schema-on-read**: it ingests raw logs as-is, then applies structure at *search* time. Three roles (all in one process for Splunk Free, split across servers in Enterprise):

```
Data sources (Windows evtx, syslog, firewall, app logs)
        │  (Universal Forwarder ships the data)
        ▼
   [ Forwarder ] → [ Indexer ] → [ Search Head ]
                     stores +       runs SPL,
                     indexes        dashboards, alerts
```

1. **Forwarder** — a lightweight agent on each endpoint that streams logs to the indexer. (In the lab you often just upload a file directly instead.)
2. **Indexer** — the engine. At **index time** it breaks the stream into **events** (usually one per line), stamps each with a **timestamp**, tags a **sourcetype** (e.g. `WinEventLog:Security`), and builds indexes so searches are fast. Data lands in an **index** (default `main`).
3. **Search Head** — where you run **SPL**. At **search time** Splunk extracts **fields** (like `EventCode`, `src_ip`) from the raw event on demand — so you can pull out new fields later without re-ingesting.

**Index-time vs search-time** is the key idea: heavy structure isn't fixed on ingest (that's what makes Splunk flexible with messy logs) — fields are teased out when you search.

### SPL is a pipeline
Like Unix pipes, each `|` passes results to the next command:
```
index=main sourcetype=WinEventLog:Security EventCode=4625   ← search (get events)
| stats count by src_ip                                     ← transform (aggregate)
| sort -count                                               ← order
```
`search → transform (stats/timechart/top) → format (sort/table)`. Master `stats`, `timechart`, and `top` and you can build most SOC detections.

## 🚀 Key Features
1. SPL (Search Processing Language) for powerful queries
2. Real-time alerting and correlation rules
3. Dashboard creation with charts and tables
4. Data ingestion from files, syslog, APIs
5. Source type auto-detection
6. Saved searches and scheduled reports

## 💻 Installation
- Download Splunk Free from splunk.com
- Install on Linux/Windows
- Access at `http://localhost:8000`
- Default credentials: set during install

## 📝 Basic SPL
```spl
# Search all events
index=main

# Filter by event code
index=main EventCode=4625

# Count failures by IP
index=main EventCode=4625 | stats count by src_ip | sort -count

# Timeline chart
index=main EventCode=4625 | timechart count
```

## 🔬 Typical Workflow (Practical 5 — brute-force triage)
```spl
# 1. Failed logons over time — spot the spike
index=main sourcetype=WinEventLog:Security EventCode=4625 | timechart count

# 2. Which source IPs are hammering us
index=main EventCode=4625 | stats count by src_ip | sort -count

# 3. Did any brute-force succeed? (4624 = successful logon)
index=main EventCode=4624 src_ip=<attacker_ip>

# 4. Did they create an account (4720) or install a service (7045)? — persistence
index=main (EventCode=4720 OR EventCode=7045)
```
Pattern: many `4625` from one IP → a `4624` from that IP → `4720/7045` = brute-force that **succeeded** and established persistence. See [[Windows Event IDs Reference]] and [[SOC Operations & Incident Lifecycle]].

## ⚠️ Gotchas / Limitations
- **Splunk Free = 500 MB/day** ingest, and has **no authentication, no alerting, no distributed search** — fine for labs, not production.
- **Timestamps matter** — if Splunk parses the wrong time field, events sort wrongly; check the `_time` column.
- Field names depend on sourcetype (`src_ip` vs `Source_Network_Address`) — inspect the event first (`| table _raw`) before writing `stats by`.
- Search-time field extraction means a bad search returns nothing rather than an error — verify with a broad search first, then narrow.

## ⚡ Quick Reference
→ [[Splunk SPL Cheatsheet]]

## 🔗 Related Notes
- [[SOC Operations & Incident Lifecycle|SOC Operations]]
- [[Network Traffic Analysis]]
- [[Windows Event IDs Reference|Windows Event IDs]] ⚡
