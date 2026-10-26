---
type: lab
subject: dfir
tags: [cybersecurity, lab, dfir, ftk-imager, evidence-acquisition, chain-of-custody]
status: learning
created: 2026-07-02
---

# Lab: Evidence Acquisition with FTK Imager

> 🔬 **Lab Practical** — DFIR Practical 1 | Est. time: 3 hrs

---

## 🎯 Objective
Capture volatile memory and create a forensic image of a USB device; verify integrity with hash values; complete chain-of-custody form.

## 📝 Procedure

### Step 1: RAM Capture
1. Open FTK Imager
2. File → Capture Memory
3. Select destination path
4. Include pagefile if option available
5. Click "Capture Memory"
6. Record file size of RAM capture

### Step 2: Create Forensic Disk Image
1. File → Create Disk Image
2. Select Source → Physical Drive → USB drive
3. Select Image Type → E01 (EnCase format)
4. Set destination and case metadata:
   - Case Number, Evidence Number, Examiner Name
5. Start imaging
6. FTK calculates MD5 and SHA-1 automatically

### Step 3: Verify Integrity
- Compare generated hash with verification hash
- Both must match exactly
- Screenshot the verification report

### Step 4: Chain of Custody
Fill out the following fields:
| Field | Value |
|-------|-------|
| Case Number | |
| Evidence Description | |
| Date/Time Collected | |
| Collected By | |
| Location Found | |
| MD5 Hash | |
| SHA-1 Hash | |
| Storage Location | |

## ✅ Expected Output
- [ ] FTK Imager verification report PDF with matching hashes
- [ ] Chain-of-custody fields filled in screenshot
- [ ] RAM capture file size recorded

## 💭 Observation Questions
1. You accidentally boot from the suspect USB before imaging. Which files have timestamps changed? What data might be written?
2. Why must you record both MD5 AND SHA-1 — why is one hash algorithm not sufficient in court?

## 🔗 Related Notes
- [[FTK Imager]]
- [[Evidence Acquisition & Imaging]]
- [[Hash & Encryption Reference|Hash Reference]] ⚡
