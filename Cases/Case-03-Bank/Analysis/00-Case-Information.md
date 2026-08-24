# Case 03 – Bank Phishing Email Investigation

## Case Overview

This case involves the investigation of a phishing email impersonating **Banco Santander**, a major banking institution. The email attempts to create urgency by claiming that account information requires updating and directs the recipient to click a malicious link.

The objective of this investigation is to analyze the email, identify phishing indicators, extract Indicators of Compromise (IOCs), validate sender authenticity, inspect embedded URLs, and document findings according to the incident response playbook used throughout this project.

---

## Case Details

| Field | Value |
|---------|---------|
| Case ID | Case-03-Bank |
| Category | Phishing Email |
| Threat Type | Bank Impersonation |
| Impersonated Organization | Banco Santander |
| Evidence File | sample-220.eml |
| File Type | RFC 822 Email Message |
| Investigator | Abdull Ashthaf |
| Investigation Date | 24 August 2026 |
| Status | In Progress |

---

## Evidence Acquisition

The suspicious email was obtained from the phishing email corpus and copied into the case evidence directory for investigation.

### Evidence Location

```bash
/home/analyst/CyberLab/Cases/Case-03-Bank/Evidence/email/
```

### Evidence File

```text
sample-220.eml
```

---

## Evidence Verification

### SHA256 Hash

```text
b4e6cce6749f1fc6f8eee7b5f21b7da0e668e29a853ab6f4bb647f34b75d17fb
```

Hash verification was performed to ensure evidence integrity before analysis.

### File Identification

```bash
file sample-220.eml
```

Output:

```text
RFC 822 mail, ASCII text, with very long lines (355), with CRLF line terminators
```

The file is a valid RFC 822 formatted email message suitable for forensic analysis.

---

## Chain of Custody

| Date | Action | Performed By |
|--------|---------|---------|
| 24-Aug-2026 | Email sample identified from phishing corpus | Analyst |
| 24-Aug-2026 | Email copied into case evidence directory | Analyst |
| 24-Aug-2026 | SHA256 hash calculated and recorded | Analyst |
| 24-Aug-2026 | Evidence preserved for analysis | Analyst |

---

## Investigation Scope

The investigation will include:

- Initial triage and visual inspection
- Email header analysis
- Message routing analysis
- SPF validation
- Sender verification
- Content analysis
- URL extraction and analysis
- IOC extraction
- Threat intelligence enrichment
- MITRE ATT&CK mapping
- Risk assessment
- Incident report generation

---

## Evidence Screenshots

The following screenshots were collected during evidence acquisition:

1. Evidence file present in case directory
2. SHA256 hash verification
3. File type verification
4. Working directory confirmation

Screenshots stored in:

```text
Cases/Case-03-Bank/Screenshots/
```

---

## Notes

The email appears to impersonate Banco Santander and attempts to persuade the recipient to update account information through an embedded hyperlink. Further analysis will determine sender legitimacy, infrastructure used, authentication results, and associated indicators of compromise.

---

**Next Phase:** 01-Initial-Triage.md
