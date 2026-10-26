---
type: concept
subject: dfir
tags: [cybersecurity, concept, dfir, csf-ns, concepts, law, it-act, dpdp, ipc, crpc, evidence, legal-framework, indian-law]
status: learning
created: 2026-07-12
---

# Cyber Law & Legal Framework for Digital Forensics

> 📝 **Concept Note** — DFIR Unit I (Legal Aspects) | Source: MCA Sem III Syllabus

---

## Why Law Matters in Cybersecurity

Digital forensics without legal grounding is useless — evidence collected without following proper legal procedure can be **thrown out of court**, prosecutions can fail, and investigators can themselves face liability. Every forensic examiner and cybersecurity professional must understand the legal framework before touching any evidence.

```
LAW
├── IPC (Indian Penal Code)           ← What acts are criminal
├── CrPC (Code of Criminal Procedure) ← How investigation & search happen
│   └── Sections 91, 93
├── IEA (Indian Evidence Act)         ← How evidence is proved in court
│   └── Section 65B
└── Special Acts
    ├── IT Act 2000 (+ 2008 Amendment)  ← Cyber-specific offences
    └── DPDP Act 2023                   ← Personal data protection
```

---

## Part 1 — IPC (Indian Penal Code, 1860)

The IPC is India's main criminal code. While it predates computers, several sections are applied to cybercrimes in conjunction with the IT Act.

### Relevant IPC Sections in Cybercrime

| Section | Offence | Application in Cybercrime |
|---------|---------|--------------------------|
| **S. 420** | Cheating and dishonestly inducing delivery of property | Online fraud, phishing scams |
| **S. 463–465** | Forgery | Forging digital documents, fake websites |
| **S. 468** | Forgery for purpose of cheating | Identity theft using forged digital credentials |
| **S. 499–500** | Defamation | Online defamation, fake social media posts |
| **S. 503** | Criminal intimidation | Cyberstalking, threatening emails/messages |
| **S. 354C** | Voyeurism | Secretly recording/distributing intimate images |
| **S. 499** | Defamation | Defamatory content posted online |

### Key Point
> The IPC and IT Act are often invoked **together** in cybercrime FIRs. For example, a phishing-based bank fraud would attract both **IT Act S. 66C** (identity theft) and **IPC S. 420** (cheating).

---

## Part 2 — CrPC (Code of Criminal Procedure, 1973)

The CrPC lays down the **procedure** for investigating and prosecuting crimes. Two sections are critical for digital evidence collection.

### Section 91 — Summons to Produce Document or Thing

**What it says:** A court or officer-in-charge of a police station may issue a written order (summons) requiring any person to **produce any document or other thing** that is considered necessary for an investigation or trial.

**In digital forensics context:**
- Police can issue S. 91 summons to a company to produce server logs, CCTV footage, email records, or any stored digital data
- Social media platforms (Meta, Google, X/Twitter) receive S. 91 notices to produce account data, IP addresses, and login records
- ISPs receive S. 91 notices to produce subscriber information and traffic logs

**Example:**
```
A company's employee is suspected of leaking trade secrets via email.
Police issue S. 91 summons to the IT department:
→ Produce all emails sent by accused from [date range]
→ Produce server access logs
→ Produce VPN connection records
```

**Key Distinction:** S. 91 is a **summons** (a legal demand) — the person must comply or face contempt of court.

---

### Section 93 — When Search Warrant May Be Issued

**What it says:** A search warrant is issued when:
1. A person refuses to comply with a S. 91 summons
2. It is not known who possesses the document or thing
3. Summoning would cause delay that might defeat the purpose of justice

**In digital forensics context:**
- Police obtain a S. 93 search warrant to physically enter premises and seize computers, hard drives, mobile phones, and other digital devices
- Authorises forensic investigators to conduct **on-site or off-site examination** of seized devices
- The warrant must specify: **what to search for** and **where to search** — a general warrant is invalid

**Search vs. Summons:**

| Feature | S. 91 Summons | S. 93 Search Warrant |
|---------|--------------|---------------------|
| Issued by | Court / Police Officer | Magistrate |
| Target | Specific person asked to produce | Physical premises |
| Compliance | Voluntary (but mandatory) | Enforced physically |
| Use when | Person is cooperative or known | Uncooperative, or location unknown |

**Important for Forensics:**
When executing a S. 93 warrant on digital devices:
- A **panchanama** (seizure memo) must be prepared listing every device seized
- Devices must be sealed and signed by witnesses
- Hashes must be calculated **at the scene** and recorded in the panchanama
- This becomes the beginning of the **chain of custody**

---

## Part 3 — IEA (Indian Evidence Act, 1872) — Section 65B

### The Problem — Why Old Evidence Rules Failed

The IEA was written in 1872 — long before computers. Courts faced a dilemma: how do you admit a **printout** of a WhatsApp conversation or a **screenshot** of a bank transaction as evidence, when the original "document" exists only as bits on a server?

### Section 65B — Admissibility of Electronic Records

Section 65B (inserted by IT Act 2000 amendment) makes electronic records **admissible as evidence** subject to strict conditions.

**What it covers:**
- Any information stored/processed in electronic form (emails, logs, messages, website data, CCTV footage, audio recordings)
- The output can be a printout, CD, or any stored form

### The 65B Certificate — Most Critical Requirement

To use any electronic record as evidence, the person **responsible for the computer system** must provide a certificate under S. 65B(4) stating:

| Certificate Must Confirm | Why It Matters |
|--------------------------|---------------|
| The computer was in regular use during the relevant period | Proves the output came from a functioning system |
| Information was fed into the computer in the ordinary course of activities | Proves it's not fabricated |
| The computer was operating properly during the period | Ensures reliability of output |
| The electronic record is a reproduction of the original | Ensures authenticity |

**The certificate must be signed by a responsible official** (the system owner, IT administrator, or a person in charge of the computer).

### Landmark Cases

**Anvar P.V. vs P.K. Basheer (2014) — Supreme Court:**
> A 65B certificate is **mandatory** for admitting electronic evidence. Without it, even the original pen drive or hard drive cannot be admitted as evidence. The certificate is not just procedural — it is substantive.

**Arjun Panditrao Khotkar vs Kailash Kushanrao Gorantyal (2020) — Supreme Court:**
> Reaffirmed that a 65B certificate is **compulsory**. The person who issues the certificate need not be technically expert but must be in a **position of responsibility** over the computer system.

### Practical Implication for Investigators

```
❌ WRONG (Evidence will be rejected):
Forensic examiner prints WhatsApp screenshot → presents to court → No 65B certificate

✅ CORRECT (Evidence will be admitted):
Forensic examiner gets 65B certificate from:
  → WhatsApp/Meta (for server-side data)
  → Phone owner's IT admin (for device data)
→ Attaches certificate to evidence → Court admits it
```

> ⚠️ **This is why chain of custody and proper documentation matters.** A brilliant forensic analysis is worthless if the evidence is inadmissible.

---

## Part 4 — IT Act 2000 (Information Technology Act)

India's primary legislation for cybercrime and electronic commerce. The **IT (Amendment) Act 2008** significantly expanded criminal provisions.

### Section 65 — Tampering with Computer Source Code

**What it says:** Knowingly or intentionally concealing, destroying, altering, or causing another to conceal/destroy/alter any computer source code when it is required to be kept or maintained by law.

**Punishment:** Up to **3 years** imprisonment OR fine up to ₹2 lakhs OR both.

**In practice:**
- A software developer deletes the original source code of a billing system to hide fraudulent modifications
- An IT admin alters audit logs that are legally mandated to be maintained
- A company destroys its firewall logs that are required to be kept under SEBI regulations

> 💡 This section protects the **integrity of code and mandated records** — not just from hackers, but from insiders who tamper with systems.

---

### Section 66 — Computer-Related Offences

The core provision for hacking and unauthorised computer access. This section creates criminal liability for acts done **dishonestly or fraudulently** under S. 43.

**Section 43 (Civil) → Section 66 (Criminal) relationship:**

| S. 43 Civil Liability | S. 66 Criminal (same act, criminal intent) | Punishment |
|----------------------|-------------------------------------------|-----------|
| Unauthorised access to a computer | With dishonest/fraudulent intent | Up to **3 years** + fine up to ₹5 lakhs |
| Introducing virus/malware | With dishonest/fraudulent intent | Same |
| Damaging a computer/data | With dishonest/fraudulent intent | Same |
| Downloading/copying data without authorisation | With dishonest/fraudulent intent | Same |
| Denying authorised access (DoS) | With dishonest/fraudulent intent | Same |

**Key sub-sections added by 2008 amendment:**

| Sub-section | Offence | Punishment |
|-------------|---------|-----------|
| **66A** | Sending offensive messages (struck down by SC in 2015 — *Shreya Singhal case*) | Struck down |
| **66B** | Dishonestly receiving stolen computer resource | Up to **3 years** + fine ₹1 lakh |
| **66C** | Identity theft — fraudulently using another's electronic signature/password/biometric | Up to **3 years** + fine ₹1 lakh |
| **66D** | Cheating by personation using computer | Up to **3 years** + fine ₹1 lakh |
| **66E** | Publishing private images of a person without consent (voyeurism) | Up to **3 years** + fine ₹2 lakhs |
| **66F** | **Cyber terrorism** — attacks on critical infrastructure threatening unity/security of India | Up to **life imprisonment** |

**Example — 66C (Identity Theft):**
```
Attacker steals OTP by SIM swapping → logs into victim's bank account → transfers ₹5 lakh
Charges: S. 66C (identity theft) + S. 66D (cheating by personation) + IPC S. 420 (cheating)
```

---

### Section 79 — Exemption of Intermediary from Liability

**What it says:** An intermediary (ISP, social media platform, cloud provider, online marketplace) is **not liable** for third-party content or actions if:

1. Its role is limited to **providing access** (passive conduit)
2. It does not initiate the transmission or modify the content
3. It **observes due diligence** (follows IT Rules 2021 guidelines)
4. Upon receiving actual knowledge of unlawful content, it **acts expeditiously** to remove it

**Who is an "Intermediary"?**

| Type | Example |
|------|---------|
| ISP | Jio, Airtel, BSNL |
| Social media platform | Meta (WhatsApp, Instagram), X (Twitter) |
| Search engine | Google, Bing |
| Cloud provider | AWS, Google Cloud, Azure |
| E-commerce | Amazon, Flipkart |
| App stores | Google Play Store, Apple App Store |

**The "Safe Harbour" Protection:**
- S. 79 is India's equivalent of the US "Safe Harbor" under DMCA
- Platforms are protected from liability for user-generated content **as long as** they act on takedown notices

**When Safe Harbour is LOST:**
- Platform **knowingly** hosts illegal content
- Platform does not act within the prescribed time upon receiving valid notice
- Platform modifies or initiates the illegal content itself

**IT Rules 2021 Obligations (for significant social media intermediaries with >50 lakh users):**
- Appoint a Chief Compliance Officer (resident in India)
- Appoint a Nodal Contact Person for law enforcement
- Appoint a Grievance Officer (resident in India)
- Monthly compliance reports
- Traceability — ability to identify first originator of a message (for messaging platforms)
- Take down content within 24 hours for sexual content; 36 hours for other notices

**Relevance for forensics/investigations:**
```
Police investigating a cybercrime involving WhatsApp messages:
→ Issue S. 91 notice to Meta/WhatsApp (intermediary)
→ Under S. 79 + IT Rules, Meta must cooperate
→ Provide account information, IP logs, message metadata
→ (Encrypted content may not be available — platform cannot decrypt E2E)
```

---

## Part 5 — DPDP Act 2023 (Digital Personal Data Protection Act)

India's first comprehensive data privacy law, passed in August 2023. Modelled on concepts from GDPR but adapted for India.

### Core Philosophy

> **"Personal data of individuals shall be processed only for lawful purposes and in a responsible manner that respects the right to privacy."**

### Key Definitions

| Term | Meaning |
|------|---------|
| **Personal Data** | Any data about an identifiable individual (name, email, phone, location, biometric, health data, etc.) |
| **Data Fiduciary** | An entity that determines the **purpose and means** of processing personal data (like a company collecting customer data) |
| **Data Principal** | The **individual** whose personal data is being processed (the customer/user) |
| **Data Processor** | An entity that processes data **on behalf of** a Data Fiduciary (e.g., a cloud provider) |
| **Consent Manager** | A new entity introduced by DPDP — helps individuals manage their consents across multiple fiduciaries |
| **Significant Data Fiduciary (SDF)** | Entities processing large volumes of sensitive data — subject to additional obligations |

### Core Principles

1. **Lawful Processing** — Data can only be collected for a legitimate purpose
2. **Consent** — Must be free, informed, specific, and unconditional (can be withdrawn anytime)
3. **Purpose Limitation** — Data collected for one purpose cannot be used for another
4. **Data Minimisation** — Collect only what is necessary for the stated purpose
5. **Accuracy** — Data must be kept accurate and updated
6. **Storage Limitation** — Delete data when the purpose is fulfilled
7. **Security** — Implement appropriate security safeguards

### Rights of Data Principals (Individuals)

| Right | What it Means |
|-------|--------------|
| **Right to Access** | Know what personal data is held and how it's being used |
| **Right to Correction** | Request correction of inaccurate or incomplete data |
| **Right to Erasure** | Request deletion of personal data when no longer needed |
| **Right to Grievance Redressal** | Raise complaints with the Data Fiduciary |
| **Right to Nominate** | Nominate another person to exercise rights on death/incapacity |

### Obligations of Data Fiduciaries

```
Data Collection → Obtain free & informed consent → Specify purpose
        ↓
Data Processing → Use only for stated purpose → Keep accurate
        ↓
Data Breach → Notify Data Protection Board + affected individuals
        ↓
Purpose Fulfilled → Delete data → Notify erasure
```

**For Security Incidents:**
- A data fiduciary **must notify** the Data Protection Board of India (DPBI) **and** affected data principals in case of a personal data breach
- Failure to notify = significant penalty

### Penalties Under DPDP Act 2023

| Violation | Maximum Penalty |
|-----------|----------------|
| Failure to take security safeguards → data breach | ₹250 crore |
| Failure to notify breach to Board/individuals | ₹200 crore |
| Non-fulfilment of obligations for children's data | ₹200 crore |
| Non-compliance with additional SDF obligations | ₹150 crore |
| Breach of any other provision | ₹50 crore |

> Penalties are **per incident**, not per affected person.

### DPDP Act + Digital Forensics

The DPDP Act creates a tension for investigators:

| Situation | DPDP Consideration |
|-----------|-------------------|
| Forensic examiner discovers PII (Aadhaar, health data) during investigation | Must handle carefully; only use for investigation purpose; cannot share beyond need |
| Company requests forensic investigation of employee devices | Must have lawful basis; ideally covered in employee contract/policy |
| Law enforcement requesting personal data from fiduciary | DPDP Act carves out **exceptions for law enforcement** — lawful order under S. 91/93 CrPC overrides consent requirements |
| Storing forensic images containing PII | Must implement appropriate security; delete when investigation is complete |

### Exceptions Under DPDP Act

The following are **exempt** from most DPDP provisions:
- Processing for **national security and law enforcement** (subject to government notification)
- Processing for **judicial/regulatory proceedings**
- Processing for **prevention, detection, investigation of offences**
- Processing of publicly available personal data

---

## Comparison Summary — All Laws at a Glance

| Law | What It Governs | Who It Applies To | Key Provisions |
|-----|----------------|-------------------|----------------|
| **IPC** | General criminal acts | Everyone | S. 420, 463, 503 — cybercrimes charged alongside IT Act |
| **CrPC S. 91** | Summons to produce digital evidence | Law enforcement → custodians | Compel production of logs, records, data |
| **CrPC S. 93** | Search warrants for digital evidence | Magistrates → police | Physical search and seizure of devices |
| **IEA S. 65B** | Admissibility of electronic records | Courts, prosecutors | Certificate mandatory for e-evidence |
| **IT Act S. 65** | Tampering with source code | Anyone handling code | 3 years + ₹2 lakh fine |
| **IT Act S. 66** | Hacking, identity theft, cyber terror | Attackers | 3 years to life imprisonment |
| **IT Act S. 79** | Intermediary liability & safe harbour | ISPs, social media, cloud | Due diligence + takedown obligations |
| **DPDP Act 2023** | Personal data protection | All data fiduciaries | Consent, breach notification, up to ₹250 crore penalty |

---

## Common Exam Scenarios

**Scenario 1:** Police receive a tip that a hacker accessed a bank's servers from a café's WiFi. What sections apply?
```
→ CrPC S. 93: Search warrant for café's router logs and computers
→ CrPC S. 91: Summons to ISP for subscriber information
→ IT Act S. 66: Hacking charges against accused
→ IEA S. 65B: Certificate needed for all digital evidence in court
```

**Scenario 2:** A forensic investigator extracts WhatsApp messages from a seized phone. How are they admissible?
```
→ Must obtain 65B certificate from phone's owner/IT admin
→ Certificate confirms: phone was in regular use, messages were sent/received in ordinary course
→ Hash the extracted data to prove integrity
→ Document in chain of custody
```

**Scenario 3:** A company suffers a ransomware attack that exposes 10,000 customers' personal data. What are the legal obligations?
```
→ IT Act S. 43A: Company liable for inadequate security (civil)
→ IT Act S. 66: File FIR against attackers
→ DPDP Act: Must notify Data Protection Board + affected customers
→ Penalty for failing to notify: up to ₹200 crore
```

---

## Related Concepts

- [[Digital Forensics Fundamentals]] — Investigation process where these laws apply
- [[Evidence Acquisition & Imaging]] — CrPC S. 93 and chain of custody
- [[Ethical Hacking Fundamentals]] — IT Act legal framework for pentesters
- [[VAPT Reporting]] — Legal obligations in professional engagements
- [[Incident Response & Malware Analysis]] — DPDP breach notification requirements
- [[DFIR MOC]] — Subject navigation
