---
type: concept
subject: dfir
tags: [cybersecurity, concept, dfir, concepts, evidence-acquisition, imaging, hashing, memory-forensics, file-recovery]
status: learning
created: 2026-07-02
---

# Evidence Acquisition & Imaging

> 📝 **Concept Note** — DFIR Unit II | Source: MCA Sem III Syllabus

---

## Volatile vs Non-Volatile Data

### Order of Volatility
Collect **most volatile first** — it disappears fastest:

| Priority | Data Type | Volatility | Examples |
|----------|-----------|-----------|---------|
| 1 | **CPU registers, cache** | Milliseconds | Current process state |
| 2 | **RAM / Memory** | Power off = gone | Running processes, encryption keys, network connections |
| 3 | **Network state** | Seconds-minutes | ARP cache, routing table, active connections |
| 4 | **Running processes** | Active only while running | Process list, open files |
| 5 | **Disk (temporary)** | Until overwritten | Temp files, swap/pagefile |
| 6 | **Disk (persistent)** | Survives reboot | Files, logs, registry |
| 7 | **Backup media** | Long-term | Tapes, cloud backups |

> 💡 **RAM first, disk second.** Once you power off, RAM is gone forever.

### Live vs Dead Acquisition

| Aspect | Live Acquisition | Dead Acquisition |
|--------|-----------------|-----------------|
| **System state** | Running | Powered off |
| **Captures** | RAM + disk | Disk only |
| **Risk** | May alter evidence slightly | No alteration risk |
| **When to use** | Encrypted drives (keys in RAM), malware analysis | Safe evidence preservation |

---

## Write Blockers

### What are they?
Devices that allow **read-only access** to storage media — prevents any modification to evidence during acquisition.

### Types

| Type | How It Works | Cost |
|------|-------------|------|
| **Hardware write blocker** | Physical device between evidence and forensic workstation | ₹10,000–₹1,00,000 |
| **Software write blocker** | OS-level blocking (Linux `blockdev --setro`, Windows registry) | Free |

> ⚠️ **Without a write blocker, simply connecting a drive can modify timestamps and metadata** — potentially invalidating evidence.

---

## Forensic Imaging

### What is a Forensic Image?
A **bit-for-bit copy** (bit-stream copy) of the entire storage medium — including deleted files, slack space, unallocated space, and file system metadata. NOT just a file copy.

### Imaging Tools

| Tool | Type | Format |
|------|------|--------|
| **FTK Imager** | GUI (Windows) | E01 (EnCase format), DD (raw) |
| **dd** | CLI (Linux) | Raw image |
| **EnCase** | Commercial GUI | E01 |
| **Autopsy** | Open-source GUI | Can create and analyse images |

### FTK Imager Workflow
1. Connect evidence drive (through write blocker)
2. File → Create Disk Image
3. Select source (Physical Drive)
4. Choose image type (E01 recommended)
5. Set destination path
6. FTK calculates MD5 and SHA-1 hashes
7. Verify hashes match → Image is verified

### dd Command (Linux)
```bash
# Create raw image of /dev/sdb
dd if=/dev/sdb of=/forensics/evidence.dd bs=4096 status=progress

# Verify with hash
md5sum /dev/sdb
md5sum /forensics/evidence.dd
# Both should match exactly
```

---

## Hash Verification

### Why Multiple Hashes?

| Algorithm | Purpose | Status |
|-----------|---------|--------|
| **MD5** | Quick verification, widely used | ⚠️ Collision attacks exist |
| **SHA-1** | Better than MD5 | ⚠️ Deprecated |
| **SHA-256** | Primary integrity verification | ✅ Secure |

> 💡 Record **both MD5 and SHA-256** — MD5 for compatibility with older tools, SHA-256 for court-grade integrity proof.

### When to Hash
1. **Before imaging** — Hash the original evidence
2. **After imaging** — Hash the image file
3. **Before analysis** — Verify image hasn't changed
4. **After analysis** — Prove analysis didn't modify evidence

---

## Deleted File Recovery

### How Deletion Works
When you delete a file, the OS doesn't erase the data — it simply marks the space as "available" and removes the directory entry. The data remains until **overwritten**.

### Key Concepts

| Concept | Definition |
|---------|-----------|
| **Allocated space** | Currently in use by files |
| **Unallocated space** | Marked as free — may contain deleted files |
| **Slack space** | Unused portion of the last cluster allocated to a file |
| **File carving** | Recovering files based on header/footer signatures (not file system metadata) |

### File System Signatures

| File Type | Header (Magic Bytes) |
|-----------|---------------------|
| JPEG | `FF D8 FF` |
| PNG | `89 50 4E 47` |
| PDF | `25 50 44 46` |
| ZIP | `50 4B 03 04` |
| EXE | `4D 5A` (MZ) |

---

## RAM / Memory Acquisition

### Why Memory Matters
- **Encryption keys** — Full disk encryption keys exist in RAM
- **Running malware** — Fileless malware lives only in memory
- **Network connections** — Active C2 connections visible
- **User credentials** — Passwords, tokens in process memory
- **Process information** — What was running at capture time

### Memory Acquisition Tools

| Tool | Platform | Output |
|------|----------|--------|
| **FTK Imager** | Windows | .mem file |
| **Belkasoft RAM Capturer** | Windows | .mem file |
| **LiME** | Linux | .lime file |
| **WinPmem** | Windows | Raw dump |

### FTK Imager Memory Capture
File → Capture Memory → Select destination → Capture

> ⚠️ Memory capture **will** alter some memory contents (the capture tool itself runs in memory). This is accepted and documented.

---

## Evidence Storage and Handling
- Store in **tamper-evident bags** for physical media
- Use **encrypted storage** for digital images
- Maintain **access logs** for evidence storage room
- **Backup evidence images** to separate location
- Label everything with case number, date, handler

---

## Related Concepts

- [[Digital Forensics Fundamentals]] — Investigation process and legal context
- [[Windows & Linux Forensics]] — Analysing the acquired evidence
- [[Hash & Encryption Reference]] ⚡
- [[Forensic Artifacts Reference]] ⚡
- [[DFIR MOC]] — Subject navigation
