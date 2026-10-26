---
type: lab
subject: dfir
tags: [cybersecurity, lab, dfir, volatility, memory-forensics, malware]
status: learning
created: 2026-07-02
---

# Lab: Memory Forensics with Volatility 3

> 🔬 **Lab Practical** — DFIR Practical 6 | Est. time: 4 hrs

---

## 🎯 Objective
Analyse a Windows memory dump to identify suspicious processes, network connections, and indicators of malware.

## 📝 Procedure

### Step 1: System Information
```bash
python3 vol.py -f memory.raw windows.info
```

### Step 2: List Processes
```bash
# Process list
python3 vol.py -f memory.raw windows.pslist

# Process tree (parent-child)
python3 vol.py -f memory.raw windows.pstree

# Scan for hidden processes
python3 vol.py -f memory.raw windows.psscan
```
→ Look for: unusual parent-child relationships (cmd.exe from svchost.exe)

### Step 3: Network Connections
```bash
python3 vol.py -f memory.raw windows.netscan
```
→ Look for: connections to external IPs, unusual ports

### Step 4: Detect Malware Indicators
```bash
# Check for injected code
python3 vol.py -f memory.raw windows.malfind

# List DLLs of suspicious process
python3 vol.py -f memory.raw windows.dlllist --pid <PID>
```

### Step 5: String Analysis
```bash
strings memory.raw | grep -i "http://"
strings memory.raw | grep -i "cmd.exe"
strings memory.raw | grep -i "password"
```
→ Top 10 interesting strings

### Step 6: VirusTotal Lookup
- Extract suspicious file hash
- Search on https://www.virustotal.com
- Record detection ratio

### Step 7: Written Analysis
Write 1 paragraph describing:
- What malicious process was found
- What it was doing (network connections, injected code)
- Classification (trojan, backdoor, RAT, etc.)

## ✅ Expected Output
- [ ] Process tree with suspicious process highlighted
- [ ] Network connections table with anomaly identified
- [ ] Strings output top 10
- [ ] VirusTotal result screenshot
- [ ] Written analysis paragraph

## 💭 Observation Questions
1. Process tree shows cmd.exe spawned from svchost.exe. Why is this suspicious? What legitimate reasons could cause this?
2. Strings output contains `http://malicious-c2.com/beacon`. What type of malware behaviour does this suggest?

## 🔗 Related Notes
- [[Volatility]] | [[Volatility Commands Cheatsheet|Cheatsheet]] ⚡
- [[Incident Response & Malware Analysis|IR & Malware Analysis]]
