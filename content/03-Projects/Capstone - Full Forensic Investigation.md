---
type: capstone
subject: dfir
tags: [cybersecurity, capstone, dfir, forensic-investigation, report, chain-of-custody]
status: learning
created: 2026-07-02
---

# Capstone: Full Forensic Investigation Report

> 🚀 **Capstone Project** — DFIR Weeks 11-13 | Est. time: 12 hrs | 30 marks

---

## 🎯 Project Goal
Conduct a complete forensic investigation on a faculty-provided evidence package and produce a legally defensible investigation report.

---

## 📋 Evidence Package (Provided by Faculty)
- Pre-seeded Windows disk image (.E01)
- Memory dump (.raw)
- Network PCAP
- Scenario: **Simulated insider data exfiltration incident**

**You must determine:**
1. What data was accessed?
2. When was it accessed?
3. How was it exfiltrated?
4. Was malware involved?

---

## 📝 Investigation Process

### Phase 1: Evidence Intake & Chain of Custody
1. Record all evidence items with descriptions
2. Verify hash values of all evidence files
3. Fill out chain of custody form
4. Create working copies — never analyse originals

### Phase 2: Disk Forensics (Autopsy)
1. Load disk image in Autopsy
2. Run all ingest modules
3. Examine:
   - Recently accessed files (timestamps around incident)
   - Deleted files in unallocated space
   - USB device connections (Registry → USBSTOR)
   - Browser history and downloads
   - Prefetch files (program execution)
   - Registry Run keys (persistence)

### Phase 3: Memory Forensics (Volatility)
```bash
# Process analysis
python3 vol.py -f memory.raw windows.pstree
python3 vol.py -f memory.raw windows.psscan

# Network connections
python3 vol.py -f memory.raw windows.netscan

# Malware indicators
python3 vol.py -f memory.raw windows.malfind

# Command line history
python3 vol.py -f memory.raw windows.cmdline
```

### Phase 4: Network Forensics (Wireshark)
1. Open PCAP in Wireshark
2. Identify:
   - Data exfiltration (large outbound transfers)
   - C2 communications (beaconing)
   - Credential theft (cleartext protocols)
   - DNS anomalies (tunnelling)
3. Build network timeline

### Phase 5: Correlation
- Cross-reference disk, memory, and network findings
- Build unified timeline from all sources
- Identify the complete attack chain

### Phase 6: Report Writing

---

## 📝 Report Structure

### 1. Case Details & Chain of Custody
| Field | Content |
|-------|---------|
| Case Number | |
| Date of Investigation | |
| Examiner | |
| Requesting Authority | |
| Evidence Items | List all items with hashes |

### 2. Scope & Methodology
- What was examined
- Investigation approach
- Limitations

### 3. Tools Used (with version numbers!)

| Tool | Version | Purpose |
|------|---------|---------|
| Autopsy | 4.x | Disk forensics |
| Volatility | 3.x | Memory forensics |
| Wireshark | 4.x | Network forensics |
| Splunk Free | x.x | Log correlation |

### 4. Findings Timeline (Chronological)

| # | Timestamp | Source | Artefact | Finding | Significance |
|---|-----------|--------|----------|---------|-------------|
| 1 | | Disk | | | |
| 2 | | Memory | | | |
| 3 | | Network | | | |

### 5. Detailed Findings
Each finding includes:
- **Artefact:** What was found
- **Location:** Where it was found (file path, registry key, packet #)
- **Timestamp:** When it occurred
- **Forensic Significance:** What it proves

### 6. Conclusion
- Narrative of what happened based on evidence
- Who (which account/system)
- What data was compromised
- How it was exfiltrated
- Whether malware was involved

### 7. Recommendations
- Prevent recurrence
- Improve detection
- Policy changes

### 8. Appendix
- Hash values of all evidence items
- Chain of custody form (completed)
- Raw tool outputs (referenced in findings)

---

## 📊 Assessment Criteria (30 marks)

| Criteria | Marks | What's Evaluated |
|----------|-------|-----------------|
| Artefact identification accuracy | 12 | Did you find the key evidence? |
| Timeline reconstruction quality | 8 | Logical, complete, evidence-backed |
| Report legal defensibility & writing | 10 | Chain of custody, professional format, objective language |

---

## ✅ Deliverables Checklist
- [ ] Chain of custody form (completed)
- [ ] Hash values of all evidence items verified
- [ ] Disk forensic findings documented
- [ ] Memory forensic findings documented
- [ ] Network forensic findings documented
- [ ] Unified timeline (chronological, artefact-sourced)
- [ ] Detailed findings (each with artefact, location, timestamp, significance)
- [ ] Conclusion with narrative
- [ ] Recommendations
- [ ] Appendix with hash values and chain of custody

---

## 💡 Tips
- **Start with the timeline** — establish the sequence of events first
- **Corroborate findings** — evidence from one source should be confirmed by another
- **State facts, not opinions** — "The registry shows..." not "I believe..."
- **Document your methodology** — every step you took
- **Tool versions matter** — courts require knowing exactly which tool version was used

---

## 🔗 Related Notes
- [[Digital Forensics Fundamentals]]
- [[Evidence Acquisition & Imaging|Evidence Acquisition]]
- [[Windows & Linux Forensics]]
- [[Network & Email Forensics]]
- [[Incident Response & Malware Analysis|IR & Malware Analysis]]
- [[Autopsy]] | [[Volatility]] | [[Wireshark]]
- [[Forensic Artifacts Reference|Forensic Artifacts]] ⚡
- [[DFIR MOC]]
