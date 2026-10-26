---
type: reference
subject: shared
tags: [cybersecurity, reference, cheatsheet, hashing, encryption, password-cracking, quickref]
status: learning
created: 2026-07-02
---

# Hash & Encryption Reference ⚡

> ⚡ **Quick Reference** — Common hash algorithms, encryption types, and identification

---

## Hash Identification

| Example | Algorithm | Length |
|---------|-----------|--------|
| `5d41402abc4b2a76b9719d911017c592` | MD5 | 32 hex |
| `aaf4c61ddcc5e8a2dabede0f3b482cd9aea9434d` | SHA-1 | 40 hex |
| `2cf24dba5fb0a30e26e83b2ac5b9e29e...` | SHA-256 | 64 hex |
| `$1$salt$hash` | MD5crypt (Linux) | Variable |
| `$5$salt$hash` | SHA-256crypt | Variable |
| `$6$salt$hash` | SHA-512crypt | Variable |
| `$2a$10$salt...hash` | bcrypt | 60 chars |
| `aad3b435b51404eeaad3b435b51404ee:hash` | NTLM (Windows) | LM:NT format |

## Hashcat Modes

| Mode | Algorithm |
|------|-----------|
| 0 | MD5 |
| 100 | SHA-1 |
| 1400 | SHA-256 |
| 1000 | NTLM |
| 1800 | SHA-512crypt ($6$) |
| 3200 | bcrypt |
| 500 | MD5crypt ($1$) |

## Common Commands
```bash
# Generate hashes
echo -n "password" | md5sum
echo -n "password" | sha256sum

# Identify hash type
hashid 'hash_value'
hash-identifier

# John the Ripper
john --wordlist=rockyou.txt hashes.txt
john --show hashes.txt

# Hashcat
hashcat -m 0 hashes.txt rockyou.txt
```

## Encryption Types

| Type | Key | Examples | Use |
|------|-----|---------|-----|
| **Symmetric** | Same key encrypt/decrypt | AES, DES, 3DES, Blowfish | Data encryption (fast) |
| **Asymmetric** | Public/Private key pair | RSA, ECC, Diffie-Hellman | Key exchange, digital signatures |
| **Hashing** | One-way (no key) | MD5, SHA-256, bcrypt | Integrity verification, passwords |

## Key Sizes

| Algorithm | Key Size | Status |
|-----------|----------|--------|
| DES | 56-bit | ❌ Broken |
| 3DES | 168-bit | ⚠️ Deprecated |
| AES-128 | 128-bit | ✅ Secure |
| AES-256 | 256-bit | ✅ Very secure |
| RSA-2048 | 2048-bit | ✅ Current standard |
| RSA-4096 | 4096-bit | ✅ High security |
