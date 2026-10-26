---
type: reference
subject: shared
tags: [cybersecurity, reference, cheatsheet, splunk, spl, siem, soc, quickref]
status: learning
created: 2026-07-02
---

# Splunk SPL Cheatsheet ⚡

> ⚡ **Quick Reference** — Splunk Search Processing Language for SOC analysts

---

## Basic Searches
```spl
index=main                                    # All events in main index
index=main "failed login"                     # Keyword search
index=main sourcetype="WinEventLog:Security"  # Filter by source type
index=main host="webserver01"                 # Filter by host
index=main source="/var/log/auth.log"         # Filter by source file
```

## Windows Event Log Queries
```spl
# Failed logins
index=main EventCode=4625

# Successful logins
index=main EventCode=4624

# Account created
index=main EventCode=4720

# Service installed
index=main EventCode=7045

# Audit log cleared (anti-forensics!)
index=main EventCode=1102

# Failed logins count by source IP
index=main EventCode=4625 | stats count by src_ip | sort -count

# Brute force detection (>10 failures in 5 min)
index=main EventCode=4625 | bucket _time span=5m | stats count by _time, src_ip | where count > 10

# Login timeline
index=main EventCode=4625 | timechart count by src_ip

# Top targeted accounts
index=main EventCode=4625 | top TargetUserName

# New accounts this week
index=main EventCode=4720 earliest=-7d | table _time, TargetUserName, SubjectUserName
```

## Statistical Commands
```spl
| stats count                          # Total count
| stats count by src_ip                # Count per source IP
| stats count by EventCode             # Count per event type
| stats values(user) by src_ip         # Unique users per IP
| stats dc(user) as unique_users       # Distinct count
| top 10 src_ip                        # Top 10 values
| rare user                            # Least common values
```

## Time Commands
```spl
| timechart count                      # Events over time
| timechart count by src_ip            # Events by IP over time
| timechart span=1h count              # Hourly buckets
earliest=-24h                          # Last 24 hours
earliest=-7d latest=now                # Last 7 days
```

## Filtering & Formatting
```spl
| where count > 100                    # Filter results
| search src_ip="10.0.0.*"            # Wildcard search
| table _time, src_ip, user, action   # Select columns
| sort -count                          # Sort descending
| sort +_time                          # Sort ascending
| rename src_ip as "Source IP"         # Rename fields
| dedup src_ip                         # Remove duplicates
```

## Useful SOC Queries
```spl
# Firewall blocks
index=firewall action=blocked | stats count by src_ip | sort -count

# Web server errors
index=webserver status>=400 | stats count by status, uri

# Data exfiltration (large outbound)
index=firewall direction=outbound bytes_out>1000000 | table src_ip, dst_ip, bytes_out

# Account lockouts
index=main EventCode=4740 | table _time, TargetUserName, src_ip
```
