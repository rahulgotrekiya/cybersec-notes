---
type: lab
subject: csf-ns
tags: [cybersecurity, lab, csf-ns, wireshark]
status: learning
created: 2026-07-02
---

# Lab: Wireshark — Capturing and Filtering Live Traffic

> 🔬 **Lab Practical** — CSF & NS Practical 1 | Est. time: 3 hrs

---

## 🎯 Objective
Capture live network traffic, apply display filters, identify HTTP credentials and DNS queries.

## 📋 Prerequisites
- [ ] Lab machine with internet access
- [ ] Wireshark installed

## 📝 Procedure
1. Start live capture on active interface
2. Browse websites to generate traffic (2-3 min)
3. Apply filters: `http`, `dns`, `tcp.port == 80`
4. Right-click HTTP packet → Follow → TCP Stream
5. Filter `dns` → examine third-party domains
6. Statistics → Protocol Hierarchy

## ✅ Expected Output
- [ ] TCP Stream screenshot (HTTP request/response)
- [ ] DNS query list
- [ ] Protocol Hierarchy statistics

## 💭 Observation Questions
1. Why can Wireshark see HTTP form data in plaintext? What prevents this?
2. Which third-party domains appear in DNS queries besides the sites you visited?

## 🔗 Related
- [[Wireshark]] | [[Wireshark Commands Cheatsheet|Cheatsheet]] ⚡
