---
type: reference
subject: shared
tags: [cybersecurity, reference, cheatsheet, ports, protocols, network-security, quickref]
status: learning
created: 2026-07-02
---

# Common Ports & Protocols Reference ⚡

> ⚡ **Quick Reference** — Security-critical ports and protocols

---

## Must-Know Ports

| Port | Protocol | Service | Security Notes |
|------|----------|---------|---------------|
| 20 | TCP | FTP Data | Cleartext — use SFTP |
| 21 | TCP | FTP Control | Cleartext credentials ❌ |
| 22 | TCP | SSH | Secure remote access ✅ |
| 23 | TCP | Telnet | Cleartext — NEVER use ❌ |
| 25 | TCP | SMTP | Email sending — target for spam |
| 53 | TCP/UDP | DNS | DNS attacks, tunnelling |
| 67/68 | UDP | DHCP | Rogue DHCP server attacks |
| 69 | UDP | TFTP | No authentication ❌ |
| 80 | TCP | HTTP | Unencrypted web ❌ |
| 110 | TCP | POP3 | Email retrieval — cleartext |
| 111 | TCP/UDP | RPCbind | Common target on Linux |
| 135 | TCP | MS RPC | Windows — often exploited |
| 137-139 | TCP/UDP | NetBIOS | Windows shares — enumeration target |
| 143 | TCP | IMAP | Email — cleartext |
| 161/162 | UDP | SNMP | Default community string "public" ❌ |
| 389 | TCP | LDAP | Directory services |
| 443 | TCP | HTTPS | Encrypted web ✅ |
| 445 | TCP | SMB | Windows file sharing — WannaCry target |
| 993 | TCP | IMAPS | Encrypted IMAP ✅ |
| 995 | TCP | POP3S | Encrypted POP3 ✅ |
| 1433 | TCP | MS SQL | Database — often exposed |
| 1521 | TCP | Oracle DB | Database |
| 3306 | TCP | MySQL | Database — often exposed |
| 3389 | TCP | RDP | Remote Desktop — brute force target |
| 5432 | TCP | PostgreSQL | Database |
| 5900 | TCP | VNC | Remote display — often weak auth |
| 6379 | TCP | Redis | Often no authentication |
| 8080 | TCP | HTTP Alt | Alternative HTTP / proxy |
| 8443 | TCP | HTTPS Alt | Alternative HTTPS |
| 27017 | TCP | MongoDB | Often no authentication |

## Secure vs Insecure Protocols

| Insecure | Port | Secure Alternative | Port |
|----------|------|--------------------|------|
| HTTP | 80 | HTTPS | 443 |
| FTP | 21 | SFTP/SCP | 22 |
| Telnet | 23 | SSH | 22 |
| POP3 | 110 | POP3S | 995 |
| IMAP | 143 | IMAPS | 993 |
| SMTP | 25 | SMTPS | 465/587 |
| SNMP v1/v2 | 161 | SNMP v3 | 161 |
| LDAP | 389 | LDAPS | 636 |
