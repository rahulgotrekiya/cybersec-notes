---
type: reference
subject: shared
tags: [cybersecurity, reference, cheatsheet, volatility, memory-forensics, dfir, quickref]
status: learning
created: 2026-07-02
---

# Volatility Commands Cheatsheet ⚡

> ⚡ **Quick Reference** — Volatility 3 memory forensics commands

---

## Basic Usage
```bash
# All commands follow this pattern:
python3 vol.py -f <memory_dump> <plugin>
```

## System Information
```bash
# OS information
vol.py -f memory.raw windows.info

# Environment variables
vol.py -f memory.raw windows.envars
```

## Process Analysis
```bash
# List processes
vol.py -f memory.raw windows.pslist

# Process tree (parent-child relationships)
vol.py -f memory.raw windows.pstree

# Scan for hidden processes
vol.py -f memory.raw windows.psscan

# Process command line arguments
vol.py -f memory.raw windows.cmdline
```

## Network Connections
```bash
# Active network connections
vol.py -f memory.raw windows.netscan

# Network statistics
vol.py -f memory.raw windows.netstat
```

## File Analysis
```bash
# List open file handles
vol.py -f memory.raw windows.handles

# Scan for files in memory
vol.py -f memory.raw windows.filescan

# Dump a specific file
vol.py -f memory.raw windows.dumpfiles --pid <PID>
```

## Registry
```bash
# List registry hives
vol.py -f memory.raw windows.registry.hivelist

# Print registry key
vol.py -f memory.raw windows.registry.printkey --key "Software\Microsoft\Windows\CurrentVersion\Run"
```

## Malware Detection
```bash
# Detect injected code
vol.py -f memory.raw windows.malfind

# List loaded DLLs
vol.py -f memory.raw windows.dlllist --pid <PID>

# Detect API hooks
vol.py -f memory.raw windows.ssdt
```

## String Search
```bash
# Extract strings from dump
strings memory.raw | grep -i "http://"
strings memory.raw | grep -i "password"
strings memory.raw | grep -i "cmd.exe"
```

## What to Look For
- `cmd.exe` spawned from `svchost.exe` → **Suspicious!**
- Processes with no parent → **Hidden process**
- Network connections to unknown IPs → **C2 traffic**
- Injected code (malfind) → **Malware**
- Unusual DLLs loaded → **DLL injection**
