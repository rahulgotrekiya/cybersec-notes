---
type: reference
subject: shared
tags: [cybersecurity, reference, cheatsheet, forensics, artifacts, windows, linux, quickref]
status: learning
created: 2026-07-02
---

# Forensic Artifacts Reference ⚡

> ⚡ **Quick Reference** — Where to find forensic evidence on Windows and Linux

---

## Windows Artifacts

### User Activity
| Artifact | Location | What It Reveals |
|----------|----------|----------------|
| Recent Documents | `NTUSER.DAT\...\RecentDocs` | Files recently opened |
| UserAssist | `NTUSER.DAT\...\UserAssist` | Programs executed (ROT13) |
| Shellbags | `NTUSER.DAT\...\BagMRU` | Folders browsed |
| Prefetch | `C:\Windows\Prefetch\` | Programs executed + count |
| LNK files | `%AppData%\...\Recent\` | Shortcut targets + timestamps |
| Jump Lists | `%AppData%\...\AutomaticDestinations\` | Recent files per app |

### USB / External Devices
| Artifact | Location | What It Reveals |
|----------|----------|----------------|
| USBSTOR | `SYSTEM\...\Enum\USBSTOR` | USB vendor, product, serial |
| MountedDevices | `SYSTEM\MountedDevices` | Drive letter assignments |
| setupapi.dev.log | `C:\Windows\INF\setupapi.dev.log` | First connection timestamp |

### Persistence Mechanisms
| Artifact | Location | What It Reveals |
|----------|----------|----------------|
| Run keys | `SOFTWARE\...\Run` | Auto-start programs |
| RunOnce | `SOFTWARE\...\RunOnce` | One-time auto-start |
| Services | `SYSTEM\...\Services` | Installed services |
| Scheduled Tasks | `C:\Windows\System32\Tasks\` | Scheduled programs |
| Startup folder | `%AppData%\...\Start Menu\Programs\Startup\` | Startup items |

### Browser History
| Browser | Path |
|---------|------|
| Chrome | `%LocalAppData%\Google\Chrome\User Data\Default\History` |
| Firefox | `%AppData%\Mozilla\Firefox\Profiles\*.default\places.sqlite` |
| Edge | `%LocalAppData%\Microsoft\Edge\User Data\Default\History` |

---

## Linux Artifacts

| Artifact | Location | What It Reveals |
|----------|----------|----------------|
| Auth logs | `/var/log/auth.log` | SSH, sudo, logins |
| Syslog | `/var/log/syslog` | System events |
| Bash history | `~/.bash_history` | Commands executed |
| Crontabs | `/etc/crontab`, `/var/spool/cron/` | Scheduled tasks |
| SSH keys | `~/.ssh/authorized_keys` | Who can SSH in |
| Known hosts | `~/.ssh/known_hosts` | SSH connections made |
| Passwd/Shadow | `/etc/passwd`, `/etc/shadow` | User accounts |
| Tmp directory | `/tmp/` | Attacker staging area |

---

## File System Timestamps (MACB)

| Letter | Meaning | NTFS | ext4 |
|--------|---------|------|------|
| **M** | Modified | ✅ | ✅ |
| **A** | Accessed | ✅ | ✅ |
| **C** | Changed (metadata) | ✅ MFT modified | ✅ inode change |
| **B** | Born (created) | ✅ | ✅ (ext4 only) |
