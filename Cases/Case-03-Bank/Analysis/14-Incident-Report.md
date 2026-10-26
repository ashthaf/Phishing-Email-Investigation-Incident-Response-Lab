# Incident Report — Case-03-Bank

## 1. Incident Overview

| Field | Details |
|---|---|
| Case ID | Case-03-Bank |
| Incident Type | Suspected Phishing / Financial Institution Impersonation |
| Evidence File | `sample-220.eml` |
| Email Date | 6 January 2023, 19:15:08 -0300 |
| Investigation Status | Analysis completed; recipient impact unconfirmed |
| Severity | High potential risk |
| Primary Targeted Information | Payment-card and potentially personal information |
| Primary Suspicious Domain | `conecte-way.online` |
| Sender Domain | `oftalmozonasul.com.br` |

**Classification note:** The severity reflects the potential risk indicated by the email. It does not mean a successful compromise has been confirmed.

## 2. Executive Summary

This investigation examined a suspicious email that presents itself as a Santander card-related notification and requests that the recipient update payment-card information.

The email uses urgent wording and includes a call to action directing the recipient to a URL associated with `conecte-way.online`. The sender address uses `oftalmozonasul.com.br`, a domain different from the financial institution named in the message.

Technical analysis identified email-authentication concerns, suspicious link characteristics, and routing information that warranted further investigation. Threat intelligence checks using VirusTotal and Cisco Talos did not provide a positive malicious verdict for the investigated domains or IP addresses in the captured results.

The evidence is consistent with a suspected phishing attempt involving financial institution impersonation and a link intended to persuade the recipient to take action.

The investigation did not establish whether a recipient clicked the link, submitted card details or credentials, executed malware, or experienced financial loss.

**Final assessment:** Treat the email as suspected phishing. Preserve the original evidence, investigate recipient interaction where records are available, and apply appropriate containment controls.

## 3. Incident Description

The investigated message has the decoded subject:

`Atualização Pendente.`

The sender is displayed as:

`santander <compras@oftalmozonasul.com.br>`

The message claims to concern Banco Santander Cartões S.A. and asks the recipient to update card information.

The HTML body contains a call to action:

`CLIQUE AQUI PARA ATUALIZAR OS DADOS!`

The associated suspicious URL in the message is:

`https://conecte-way.online/painel_=3Dstone_esfera_way_nubank_/painel_stone_esfera_way_nubank_2.2/esfera/`

The `=3D` sequence is present in the quoted-printable email representation and corresponds to an encoded equals sign. The original evidence should be preserved without modifying the raw message.

## 4. Investigation Scope and Methodology

The investigation was conducted through the following phases:

1. Initial triage
2. Header analysis
3. Routing analysis
4. Authentication analysis
5. Sender analysis
6. Content analysis
7. URL analysis
8. Attachment analysis
9. IOC extraction
10. Threat intelligence
11. MITRE ATT&CK mapping
12. Risk assessment
13. Containment recommendations

The investigation examined the email's structure, sender information, routing headers, authentication results, message content, URL, MIME parts, extracted indicators, and available reputation results.

No claim is made that the suspicious destination was opened or that the investigation included a live interaction with the phishing website.

## 5. Technical Findings

### 5.1 Email Metadata

| Attribute | Finding |
|---|---|
| Evidence filename | `sample-220.eml` |
| File type | RFC 822 email message |
| File size | Approximately 18 KB |
| SHA-256 | `b4e6cce6749f1fc6f8eee7b5f21b7da0e668e29a853ab6f4bb647f34b75d17fb` |
| Decoded subject | `Atualização Pendente.` |
| Sender address | `compras@oftalmozonasul.com.br` |
| Return-Path | `compras@oftalmozonasul.com.br` |
| Message date | 6 January 2023 |
| Message-ID | Header was present, but the displayed value was empty |

The SHA-256 hash provides a reference for identifying the examined evidence file. It should be recalculated against the preserved original if evidence integrity needs to be independently verified.

### 5.2 Routing Analysis

The email's `Received` headers included the following addresses:

- `187.85.67.134`
- `185.54.230.174`
- `127.0.0.1`
- `10.13.7.4`

The first two are the observed public-looking routing IP addresses investigated during the threat intelligence phase. `127.0.0.1` is a loopback address, and `10.13.7.4` is a private IPv4 address.

The presence of an address in a routing header does not, by itself, prove that it was the attacker's originating IP or that the infrastructure was malicious.

### 5.3 Email Authentication

The earlier authentication analysis recorded:

- SPF: `permerror`
- DKIM: No result observed in the examined message
- DMARC: No result observed in the examined message

These results do not provide positive assurance of sender legitimacy. The SPF error is not equivalent to a definitive SPF failure, and the absence of a recorded DKIM or DMARC result does not independently prove spoofing.

### 5.4 Content and Social Engineering

The message contains several suspicious characteristics:

- Use of Santander branding and identity.
- A request to update payment-card information.
- Urgent wording designed to encourage action.
- A prominent call to action.
- A link hosted on a domain separate from the sender domain.

Together, these characteristics support classifying the message as suspected phishing.

### 5.5 URL Analysis

The principal suspicious domain identified in the email is:

`conecte-way.online`

The URL path contains references to payment and financial-service brands, including Stone, Esfera, and Nubank. This path is part of the observed URL; it does not establish a relationship with those brands.

The investigation did not establish what the destination served to a visitor or whether it collected information.

### 5.6 Attachment Analysis

The examined message used a `multipart/alternative` MIME structure with:

- A `text/plain` part using quoted-printable encoding.
- A `text/html` part using quoted-printable encoding.

No named attachment candidates were identified. The HTML part referenced an externally hosted Avast signature image.

No attachment-based malware delivery was established from the examined MIME structure. The embedded link remained a separate concern.

## 6. Indicators of Compromise

The following table lists the indicators observed during the investigation. An indicator's inclusion does not automatically mean that it is confirmed malicious infrastructure.

| Indicator | Type | Context |
|---|---|---|
| `conecte-way.online` | Domain | Host associated with the suspicious link |
| `oftalmozonasul.com.br` | Domain | Sender and Return-Path domain |
| `187.85.67.134` | IPv4 address | Observed mail-routing address |
| `185.54.230.174` | IPv4 address | Observed routing address |
| `sample-220.eml` | Evidence file | Original email analyzed |
| `b4e6cce6749f1fc6f8eee7b5f21b7da0e668e29a853ab6f4bb647f34b75d17fb` | SHA-256 | Hash recorded for the email evidence |

### Indicators Not Classified as Public Malicious IOCs

- `127.0.0.1` is a loopback address.
- `10.13.7.4` is a private IPv4 address.

These addresses are retained as contextual evidence but should not be treated as public malicious indicators.

## 7. Threat Intelligence Findings

### VirusTotal

The captured VirusTotal results showed:

| Indicator | Displayed result |
|---|---|
| `conecte-way.online` | 0/92 detections |
| `oftalmozonasul.com.br` | 0/92 detections |
| `187.85.67.134` | 0/92 detections |
| `185.54.230.174` | 0/92 detections |

The displayed analysis dates differed by indicator. The sender-domain analysis was particularly old, and the results should not be treated as current guarantees of safety.

### Cisco Talos

| Indicator | Displayed reputation |
|---|---|
| `conecte-way.online` | Unknown |
| `oftalmozonasul.com.br` | Unknown |
| `187.85.67.134` | Neutral |
| `185.54.230.174` | Neutral |

Talos displayed the first IP's hostname as `dbmailb02.dbmail.porta80.com.br` and identified the second IP's network owner as `AVAST SLOVAKIA S.R.O.`, with a Personal VPN content category.

### Interpretation

The captured results did not provide a positive malicious verdict for these indicators. Unknown or neutral reputation does not establish that an indicator is safe, and reputation results may change over time.

The email's content and suspicious URL remain the primary evidence supporting the phishing assessment.

### Screenshot Evidence

The detailed screenshots are documented in:

- `Artifacts/Threat-Intelligence/screenshots/01-virustotal-phishing-domain.png`
- `Artifacts/Threat-Intelligence/screenshots/02-talos-phishing-domain.png`
- `Artifacts/Threat-Intelligence/screenshots/03-virustotal-sender-domain.png`
- `Artifacts/Threat-Intelligence/screenshots/04-talos-sender-domain.png`
- `Artifacts/Threat-Intelligence/screenshots/05-virustotal-ip-187.85.67.134.png`
- `Artifacts/Threat-Intelligence/screenshots/06-virustotal-ip-185.54.230.174.png`
- `Artifacts/Threat-Intelligence/screenshots/07-talos-ip-187.85.67.134.png`
- `Artifacts/Threat-Intelligence/screenshots/08-talos-ip-185.54.230.174.png`

## 8. MITRE ATT&CK Mapping

The observed email behavior was mapped to the following techniques.

| Technique ID | Technique | Tactic | Justification |
|---|---|---|---|
| T1566.002 | Phishing: Spearphishing Link | Initial Access | The email contains a suspicious external link intended to encourage recipient interaction. |
| T1684.001 | Social Engineering: Impersonation | Stealth | The email presents itself as Santander and requests sensitive financial information. |

These mappings describe behavior consistent with the observed evidence. They do not prove successful access, data theft, or any subsequent stage of an attack.

### Screenshot Evidence

- `Artifacts/MITRE-Mapping/screenshots/01-mitre-spearphishing-link-T1566.002.png`
- `Artifacts/MITRE-Mapping/screenshots/02-mitre-impersonation-T1684.001.png`

## 9. Risk Assessment

**Overall rating: High potential risk**

The risk rating reflects the email's apparent financial-phishing objective and the potential impact of disclosing payment-card information.

| Risk scenario | Potential impact | Status |
|---|---|---|
| Payment-card information theft | High | Not confirmed |
| Credential or personal-information theft | High | Not confirmed |
| Exposure to a fraudulent webpage | Medium to High | Recipient interaction unknown |
| Malware execution | High potential impact | Not established |
| Account compromise | High potential impact | Not established |
| Financial loss | High potential impact | Not established |

These are qualitative assessments, not measured probabilities.

### Impact Areas

- **Confidentiality:** Payment-card, credential, or personal information could be exposed if a recipient submits it.
- **Integrity:** Unauthorized account changes could occur if stolen credentials were subsequently misused.
- **Availability:** No service disruption was established.
- **Financial:** Fraudulent transactions could occur if payment details were obtained and misused.

## 10. Containment and Remediation Recommendations

### Immediate Containment

1. Search email gateway records for matching messages using the sender, subject, suspicious URL, and other message characteristics.
2. Quarantine confirmed phishing messages according to organizational policy.
3. Preserve the original email and its hash.
4. Consider targeted blocking of `conecte-way.online` through approved DNS, proxy, web-filtering, and email-security controls.
5. Record and verify any blocking rules actually deployed.

### Recipient and Endpoint Investigation

1. Review available browser, proxy, DNS, endpoint, and mail gateway logs.
2. Establish whether recipients clicked the link or submitted sensitive information.
3. Investigate downloads or endpoint alerts if recipient interaction is identified.
4. Reset exposed credentials and revoke active sessions when appropriate.
5. If payment-card details were disclosed, advise the affected person to contact the card issuer.

### Infrastructure Caution

Do not automatically block `oftalmozonasul.com.br`, `187.85.67.134`, or `185.54.230.174` solely because they appeared in the email. Review their context and operational impact before deploying broader blocks.

These are recommendations. The investigation has not established that the actions above were performed.

## 11. Incident Status and Outstanding Questions

| Question | Status |
|---|---|
| Is the email suspicious? | Yes; evidence supports suspected phishing |
| Was the link clicked? | Unknown |
| Were card details or credentials submitted? | Unknown |
| Was the email reported or blocked? | Unknown |
| Was malware executed? | Not established |
| Was an account compromised? | Not established |
| Was financial loss incurred? | Not established |
| Were containment controls deployed? | Not established |

The investigation should be updated if mail gateway, endpoint, browser, recipient, or account activity evidence becomes available.

## 12. Evidence Preservation

The original email should be preserved as evidence. Its recorded SHA-256 is:

`b4e6cce6749f1fc6f8eee7b5f21b7da0e668e29a853ab6f4bb647f34b75d17fb`

Keep the original evidence unchanged and conduct further analysis on working copies where practical.

Document subsequent evidence collection and containment actions with timestamps, analyst details, source information, and verification results.

## 13. Final Conclusion

The investigated email exhibits characteristics consistent with a financial phishing attempt. It impersonates Santander, requests an update to payment-card information, uses urgent wording, and includes a suspicious link hosted on a separate domain.

Email authentication concerns and the mismatch between the claimed organization and sender domain add context to the assessment. Threat intelligence results were inconclusive or neutral and do not establish that the indicators are safe.

**Final classification: Suspected phishing — high potential risk; successful compromise not confirmed.**

The recommended next steps are to preserve evidence, identify and contain matching emails, investigate recipient interaction, and perform account or endpoint response if additional evidence supports it.

## 14. Evidence Report Index

The following case reports provide the detailed analysis supporting this incident report:

- `Analysis/Initial-Triage.md`
- `Analysis/Header-Analysis.md`
- `Analysis/Routing-Analysis.md`
- `Analysis/Authentication-Analysis.md`
- `Analysis/Sender-Analysis.md`
- `Analysis/Content-Analysis.md`
- `Analysis/URL-Analysis.md`
- `Analysis/Attachment-Analysis.md`
- `Analysis/IOC-Extraction.md`
- `Analysis/Threat-Intelligence.md`
- `Analysis/MITRE-Mapping.md`
- `Analysis/Risk-Assessment.md`
- `Analysis/Containment-Recommendations.md`

**Note:** Confirm that each report filename matches the actual file in the repository. Update this index if your filenames differ.

