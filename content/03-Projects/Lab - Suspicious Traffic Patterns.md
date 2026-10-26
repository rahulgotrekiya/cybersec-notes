---
type: lab
subject: csf-ns
tags: [cybersecurity, lab, csf-ns, wireshark, nmap, c2-detection]
status: learning
created: 2026-07-02
---

# Lab: Identifying Suspicious Network Traffic Patterns

> 🔬 **Lab Practical** — CSF & NS Practical 6 | Est. time: 3 hrs

---

## 🎯 Objective
Use Wireshark and Nmap to identify suspicious indicators from a compromised machine phoning home.

## 📝 Procedure
1. Capture traffic with Wireshark — look for beaconing patterns
2. Use `netstat`/`ss` to find suspicious outbound connections
3. Run Nmap against suspicious destination
4. Look up suspicious IP on VirusTotal
5. Write incident note

## 💭 Observation Questions
1. What is C2 beaconing and why do attackers use regular intervals?
2. How could an attacker use port 443 to hide C2 traffic from a firewall?
