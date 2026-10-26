---
type: lab
subject: dfir
tags: [cybersecurity, lab, dfir, registry, windows-forensics, persistence]
status: learning
created: 2026-07-02
---

# Lab: Windows Registry Forensic Analysis

> 🔬 **Lab Practical** — DFIR Practical 3 | Est. time: 4 hrs

---

## 🎯 Objective
Extract forensic artefacts from Windows Registry hives to determine program execution history, USB connections, and persistence mechanisms.

## 📝 Procedure

### Step 1: USB Device History
- Navigate to SYSTEM hive → `ControlSet001\Enum\USBSTOR`
- Extract: Vendor, Product, Serial Number, timestamps
- Cross-reference with MountedDevices for drive letters

### Step 2: Program Execution (UserAssist)
- Navigate to NTUSER.DAT → `Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist`
- Decode ROT13 encoded program names
- Note execution counts and last run times

### Step 3: Persistence Mechanisms
- Check Run/RunOnce keys:
  - `SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
  - `SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce`
- Note any suspicious entries

### Step 4: Recent Documents
- NTUSER.DAT → `Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs`
- List recently opened files with timestamps

### Step 5: Complete Artefact Table

| Artefact | Registry Key | Value | Forensic Significance |
|----------|-------------|-------|----------------------|
| USB Device 1 | USBSTOR | | |
| USB Device 2 | USBSTOR | | |
| Program 1 | UserAssist | | |
| Suspicious Run entry | Run | | |

## 💭 Observation Questions
1. USBSTOR shows a USB drive connected at 23:49 (matching your timeline hypothesis). What additional Registry info confirms data was copied to it?
2. You find a RunOnce entry pointing to `C:\Users\Public\svchost32.exe`. Why is this suspicious even though the filename looks legitimate?

## 🔗 Related Notes
- [[Windows & Linux Forensics]]
- [[Forensic Artifacts Reference|Forensic Artifacts]] ⚡
