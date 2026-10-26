---
type: tool
subject: shared
tags: [cybersecurity, tool, ftk-imager, forensic-imaging, evidence, dfir]
status: learning
created: 2026-07-02
---

# FTK Imager

> 🔧 **Tool Note** — Category: Forensic Imaging | Free

---

## 📌 Quick Overview
**What is it?** A forensic imaging tool that creates bit-for-bit copies of storage media and captures volatile memory (RAM).

**Developer:** AccessData / Exterro
**License:** Free
**Used in:** DFIR

## 🎯 What Problem Does It Solve?
- Create forensically sound disk images with hash verification
- Capture RAM (volatile memory) before it's lost
- Preview evidence without modifying original
- Mount forensic images for analysis

## 🚀 Key Features
1. Create E01 (EnCase) and DD (raw) forensic images
2. MD5 and SHA-1 hash verification
3. RAM/memory capture
4. Preview files without altering evidence
5. Mount images as read-only drives
6. Export individual files from images

## 📝 Basic Workflow
1. **Capture RAM:** File → Capture Memory → Select destination
2. **Create Image:** File → Create Disk Image → Physical Drive
3. **Verify:** Check hash values match (MD5 + SHA-1)
4. **Document:** Fill chain of custody fields
5. **Analyse:** Open image in Autopsy for analysis

## 🔗 Related Notes
- [[Evidence Acquisition & Imaging|Evidence Acquisition]]
- [[Autopsy]] — Analyse the images FTK creates
- [[Forensic Artifacts Reference|Forensic Artifacts]] ⚡
