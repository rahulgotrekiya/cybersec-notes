---
type: lab
subject: csf-ns
tags: [cybersecurity, lab, csf-ns, wireshark, pcap-analysis]
status: learning
created: 2026-07-02
---

# Lab: Analysing a Malicious PCAP File

> 🔬 **Lab Practical** — CSF & NS Practical 3 | Est. time: 4 hrs

---

## 🎯 Objective
Load a faculty-provided malicious traffic capture, identify attack type, extract credentials, and build a timeline.

## 📝 Procedure
1. Open malicious.pcap in Wireshark
2. Filter `ftp` → extract cleartext credentials
3. Filter `http.request.method == "POST"` → find SQLi payload
4. Filter `dns` → identify DNS anomalies
5. Build attack timeline (min 5 events)

## 💭 Observation Questions
1. FTP credentials were visible in plaintext. What protocol would replace FTP?
2. What is DNS tunnelling and why is it difficult to detect with a simple firewall rule?
