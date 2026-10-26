# MITRE ATT&CK Mapping

## 1. Objective

The objective of this phase is to map the observed behavior in the suspicious email `sample-220.eml` to relevant MITRE ATT&CK techniques.

The mapping is based on evidence collected during email triage, header analysis, authentication analysis, content analysis, URL analysis, IOC extraction, and threat intelligence analysis.

The presence of a technique in this report indicates that the available evidence is consistent with the technique. It does not prove that every stage of an attack occurred.

## 2. Framework and Tools

- **Framework:** MITRE ATT&CK Enterprise
- **Reference:** https://attack.mitre.org/
- **Purpose:** Classify observed phishing and social-engineering behavior using standardized technique IDs.

---

## 3. Technique 1 — Spearphishing Link

**Technique ID:** T1566.002  
**Technique Name:** Phishing: Spearphishing Link  
**Tactic:** Initial Access  
**Official Reference:** https://attack.mitre.org/techniques/T1566/002/

### Observed Evidence

The email contains a call to action asking the recipient to update card information. The HTML body includes a link to the suspicious domain:

`conecte-way.online`

The message presents itself as a Santander card-related notification and uses urgent language to encourage the recipient to act.

### Mapping Justification

The suspicious link and persuasive content are consistent with link-based phishing behavior described by MITRE ATT&CK technique T1566.002.

The investigation did not establish that a recipient clicked the link or that the destination successfully delivered malware or collected information.

### Screenshot Evidence

The following screenshot shows the official MITRE ATT&CK page for T1566.002, including the technique ID, tactic, and description.

![MITRE ATT&CK T1566.002 — Spearphishing Link](../Artifacts/MITRE-Mapping/screenshots/01-mitre-spearphishing-link-T1566.002.png)

**Evidence file:** `Artifacts/MITRE-Mapping/screenshots/01-mitre-spearphishing-link-T1566.002.png`

---

## 4. Technique 2 — Impersonation

**Technique ID:** T1684.001  
**Technique Name:** Social Engineering: Impersonation  
**Tactic:** Stealth  
**Official Reference:** https://attack.mitre.org/techniques/T1684/001/

### Observed Evidence

The email uses the display name `santander` and claims to concern Banco Santander Cartões S.A. The message requests an update to card information and uses urgent wording.

The sender address uses the domain `oftalmozonasul.com.br`, which differs from the bank's name presented in the message.

### Mapping Justification

The use of a trusted financial institution's identity in a message requesting sensitive financial information is consistent with impersonation behavior described by MITRE ATT&CK technique T1684.001.

The mismatch between the claimed organization and the sender domain supports further suspicion but does not, by itself, prove the sender's identity or intent.

### Screenshot Evidence

The following screenshot shows the official MITRE ATT&CK page for T1684.001, including the technique ID, tactic, and description.

![MITRE ATT&CK T1684.001 — Impersonation](../Artifacts/MITRE-Mapping/screenshots/02-mitre-impersonation-T1684.001.png)

**Evidence file:** `Artifacts/MITRE-Mapping/screenshots/02-mitre-impersonation-T1684.001.png`

---

## 5. Technique Mapping Summary

| Technique ID | Technique | Tactic | Evidence-based assessment |
|---|---|---|---|
| T1566.002 | Phishing: Spearphishing Link | Initial Access | Supported by the suspicious link embedded in the email |
| T1684.001 | Social Engineering: Impersonation | Stealth | Supported by the use of Santander's identity and the request for card-data updates |

---

## 6. Techniques Not Confirmed

The following activities were not established by the available email evidence:

- Successful credential or financial-information theft.
- Execution of malware on a recipient's device.
- Successful exploitation of a browser or operating system.
- Persistence or lateral movement within a victim environment.
- Successful account compromise.

These activities should not be included as confirmed techniques without additional evidence.

---

## 7. Conclusion

The email exhibits behavior consistent with link-based phishing and impersonation. The suspicious URL, urgent request for card-data updates, Santander branding, and sender-domain mismatch support mapping the email to MITRE ATT&CK techniques T1566.002 and T1684.001.

This mapping documents the observed phishing indicators and suspected social-engineering objective. It does not establish that the recipient interacted with the link or that the attack succeeded.

---

## 8. Evidence Index

All screenshots are stored in `Artifacts/MITRE-Mapping/screenshots/` relative to the case directory.

| Screenshot | Description |
|---|---|
| `01-mitre-spearphishing-link-T1566.002.png` | Official MITRE ATT&CK page for Spearphishing Link |
| `02-mitre-impersonation-T1684.001.png` | Official MITRE ATT&CK page for Impersonation |


