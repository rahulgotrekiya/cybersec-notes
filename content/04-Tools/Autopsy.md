---
type: tool
subject: shared
tags: [cybersecurity, tool, autopsy, disk-forensics, file-recovery, dfir]
status: learning
created: 2026-07-02
---

# Autopsy

> 🔧 **Tool Note** — Category: Digital Forensic Analysis | Open Source

---

## 📌 Quick Overview
**What is it?** An open-source digital forensics platform for analysing disk images — recovering deleted files, extracting artifacts, and building timelines.

**Official Website:** https://www.autopsy.com/
**License:** Apache 2.0 (Free, Open Source)
**Used in:** DFIR

## 🎯 What Problem Does It Solve?
- Analyse forensic disk images without expensive commercial tools
- Recover deleted files from NTFS/FAT/ext4
- Extract browser history, email, registry artifacts automatically
- Build activity timelines from multiple artifact sources
- Keyword search across entire disk images

## 🚀 Key Features
1. Ingest E01 and DD forensic images
2. Automated artifact extraction (browser, email, USB, registry)
3. Deleted file recovery from unallocated space
4. Timeline analysis module
5. Keyword search and hash lookup
6. Registry viewer module
7. File type categorisation and thumbnail preview

## 📝 Basic Workflow
1. Create New Case → Enter case details
2. Add Data Source → Select forensic image (.E01)
3. Configure Ingest Modules (enable all relevant modules)
4. Wait for processing
5. Browse results in tree view (Web History, Recent Documents, Deleted Files, etc.)
6. Export findings and generate reports

## 🔗 Related Notes
- [[Windows & Linux Forensics]]
- [[Evidence Acquisition & Imaging|Evidence Acquisition]]
- [[FTK Imager]] — Creates images for Autopsy to analyse
- [[Volatility]] — Memory forensics complement
