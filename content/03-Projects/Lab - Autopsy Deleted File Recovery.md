---
type: lab
subject: dfir
tags: [cybersecurity, lab, dfir, autopsy, file-recovery, timeline]
status: learning
created: 2026-07-02
---

# Lab: Autopsy — Deleted File Recovery & Activity Timeline

> 🔬 **Lab Practical** — DFIR Practical 2 | Est. time: 4 hrs

---

## 🎯 Objective
Load a pre-seeded forensic image in Autopsy; recover deleted files; extract recent activity artefacts; build a chronological timeline.

## 📝 Procedure

### Step 1: Create Case in Autopsy
1. Open Autopsy → New Case
2. Enter case name and number
3. Add Data Source → Disk Image (.E01)
4. Enable all ingest modules
5. Wait for processing to complete

### Step 2: Recover Deleted Files
1. Navigate to "Deleted Files" in tree view
2. Look for deleted documents, images, executables
3. Right-click → Extract File(s) → Save to analysis folder
4. Note: file path, original name, timestamps

### Step 3: Extract Activity Artefacts
- **Recent Documents:** Check Recent Documents module
- **Browser History:** Web History module → URLs, timestamps, search queries
- **USB Devices:** Check extracted registry artefacts
- **Downloads:** Check Downloads folder and browser downloads

### Step 4: Build Timeline
1. Use Timeline module in Autopsy
2. Or export to CSV for manual analysis
3. Create chronological narrative (min 5 events)

| # | Timestamp | Event | Source | Significance |
|---|-----------|-------|--------|-------------|
| 1 | | | | |
| 2 | | | | |

## ✅ Expected Output
- [ ] Deleted file recovery screenshot
- [ ] Recent documents list
- [ ] Browser history screenshot
- [ ] USB device table
- [ ] Timeline CSV with 5-event narrative

## 💭 Observation Questions
1. In NTFS, when does deleted data become truly unrecoverable?
2. Timeline shows document access at 23:47 and USB connection at 23:49. What hypothesis does this support?

## 🔗 Related Notes
- [[Autopsy]]
- [[Windows & Linux Forensics]]
- [[Forensic Artifacts Reference|Forensic Artifacts]] ⚡
