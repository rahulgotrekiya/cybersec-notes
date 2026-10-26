---
type: reference
subject: shared
tags: [cybersecurity, reference, cheatsheet, wireshark, network-security, quickref]
status: learning
created: 2026-07-02
---

# Wireshark Commands Cheatsheet ⚡

> ⚡ **Quick Reference** — Wireshark display filters and shortcuts

---

## Display Filters

### By Protocol
```
http              # HTTP traffic only
dns               # DNS queries/responses
tcp               # All TCP traffic
udp               # All UDP traffic
arp               # ARP requests/replies
icmp              # Ping traffic
ssl || tls        # Encrypted traffic
ftp               # FTP traffic
smtp              # Email sending
ssh               # SSH connections
```

### By IP Address
```
ip.addr == 192.168.1.10         # Traffic to/from IP
ip.src == 192.168.1.10          # Traffic FROM IP
ip.dst == 10.0.0.5              # Traffic TO IP
ip.addr == 192.168.1.0/24       # Entire subnet
!(ip.addr == 192.168.1.1)       # Exclude specific IP
```

### By Port
```
tcp.port == 80                  # HTTP port
tcp.port == 443                 # HTTPS port
tcp.port == 22                  # SSH
tcp.port == 3389                # RDP
udp.port == 53                  # DNS
tcp.dstport == 4444             # Common reverse shell port
tcp.srcport == 80               # Response from web server
```

### By TCP Flags
```
tcp.flags.syn == 1              # SYN packets (connection attempts)
tcp.flags.syn == 1 && tcp.flags.ack == 0   # SYN only (new connections)
tcp.flags.rst == 1              # Reset packets
tcp.flags.fin == 1              # Connection teardown
```

### By HTTP Content
```
http.request.method == "GET"    # GET requests
http.request.method == "POST"   # POST requests (form submissions)
http.request.uri contains "login"  # URLs containing "login"
http.response.code == 200       # Successful responses
http.response.code == 401       # Unauthorized
http.response.code == 404       # Not found
http.host == "example.com"      # Specific host
```

### Security-Focused Filters
```
# Find cleartext credentials
http.request.method == "POST" && http contains "password"

# DNS anomalies (long queries = possible tunnelling)
dns.qry.name.len > 50

# Beaconing detection (connections to single IP)
ip.dst == <suspicious_ip>

# SQL injection attempts
http contains "UNION" || http contains "SELECT"

# Find specific User-Agent
http.user_agent contains "sqlmap"
```

### Combining Filters
```
# HTTP POST from specific IP
ip.src == 192.168.1.10 && http.request.method == "POST"

# DNS queries excluding local DNS
dns && !(ip.dst == 192.168.1.1)

# All traffic except ARP and DNS
!(arp) && !(dns)

# TCP traffic on non-standard ports
tcp && !(tcp.port == 80) && !(tcp.port == 443) && !(tcp.port == 22)
```

---

## Key Operations

| Action | How |
|--------|-----|
| Follow TCP Stream | Right-click packet → Follow → TCP Stream |
| Follow HTTP Stream | Right-click → Follow → HTTP Stream |
| Protocol Hierarchy | Statistics → Protocol Hierarchy |
| Conversations | Statistics → Conversations |
| IO Graphs | Statistics → IO Graphs |
| Export Objects | File → Export Objects → HTTP |
| Apply as Filter | Right-click field → Apply as Filter → Selected |
| Capture Filter | Set BEFORE capture (different syntax) |

## Capture Filters (set before capture starts)

```
host 192.168.1.10               # Specific host
port 80                         # Specific port
net 192.168.1.0/24              # Specific subnet
tcp port 443                    # HTTPS only
not arp                         # Exclude ARP
```

> 💡 **Display filters** (above) filter after capture. **Capture filters** filter during capture — use for performance.

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| Ctrl+E | Start/stop capture |
| Ctrl+F | Find packet |
| Ctrl+G | Go to packet number |
| Ctrl+Shift+P | Preferences |
| / | Display filter bar |
