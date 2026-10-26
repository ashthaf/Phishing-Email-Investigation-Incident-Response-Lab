# Lessons Learned — Case-03-Bank

## 1. Objective

The purpose of this phase is to document the lessons learned from investigating `sample-220.eml`, identify opportunities to improve phishing detection and incident response, and recommend preventive measures for future incidents.

The findings are based on the investigation phases completed so far, including email analysis, IOC extraction, threat intelligence, MITRE ATT&CK mapping, risk assessment, containment recommendations, and incident reporting.

## 2. Incident Summary

The investigated email presented itself as a Santander card-related notification and requested that the recipient update payment-card information.

The message used urgent wording and included a suspicious link associated with `conecte-way.online`. The sender address used `oftalmozonasul.com.br`, which differed from the financial institution named in the message.

The investigation identified authentication concerns and suspicious email content. VirusTotal and Cisco Talos did not provide positive malicious verdicts for the investigated domains and IP addresses in the captured results.

The evidence supports classifying the message as suspected phishing. However, recipient interaction, information submission, account compromise, malware execution, and financial loss were not established.

## 3. Key Lessons Learned

### 3.1 Email Content Must Be Examined Alongside Technical Indicators

The message's financial-institution impersonation, urgent request for card-data updates, and suspicious external link were important indicators.

**Lesson:** Email classification should consider the sender, authentication results, message content, link destinations, and business context together rather than relying on one indicator.

**Recommended improvement:** Establish an email-triage checklist that includes sender validation, authentication analysis, URL inspection, and social-engineering indicators.

### 3.2 A Clean Reputation Result Does Not Guarantee Safety

The captured VirusTotal results showed 0/92 detections for the investigated domains and IP addresses. Cisco Talos returned unknown or neutral reputations.

**Lesson:** Reputation services provide useful context but cannot independently prove that an email or domain is safe.

**Recommended improvement:** Combine reputation checks with email content analysis, historical evidence, URL context, and relevant security logs. Record the lookup date and displayed result because reputation data can change.

### 3.3 Sender Identity Should Be Verified

The email used Santander's name while the sender address used `oftalmozonasul.com.br`.

**Lesson:** A familiar display name does not establish that a message originated from the organization it claims to represent.

**Recommended improvement:** Examine sender addresses, authentication results, and trusted communication channels. Verify sensitive financial requests using independently obtained official contact details.

### 3.4 Email Authentication Results Require Careful Interpretation

The examined message showed an SPF `permerror`, with no DKIM/DMARC results recorded in the earlier analysis.

**Lesson:** Authentication anomalies warrant investigation but should not be treated as automatic proof of spoofing or malicious intent.

**Recommended improvement:** Review authentication results alongside the sending infrastructure, message headers, and organizational email-security policies.

### 3.5 Preserve Evidence Before Taking Action

The original email was analyzed as an evidence file, and its SHA-256 hash was recorded.

**Lesson:** Preserving the original message supports reproducibility and helps maintain evidence integrity.

**Recommended improvement:** Keep original evidence unchanged, analyze working copies where practical, record hashes, and document timestamps and evidence sources.

### 3.6 Separate Observed Evidence from Assumptions

The suspicious link and financial-information request were observed, but no recipient click or data submission was established.

**Lesson:** A suspicious email does not prove that a compromise occurred.

**Recommended improvement:** Record findings using clear categories such as observed, inferred, unconfirmed, and not investigated. Update the incident assessment when additional logs or recipient information become available.

### 3.7 IOC Context Matters

The investigation identified domains and IP addresses in the sender and routing information. The reputation services returned inconclusive or neutral results for these indicators.

**Lesson:** An IP address appearing in an email header is not automatically malicious, and shared infrastructure can support legitimate activity.

**Recommended improvement:** Record the role of each indicator, its source, its reputation at lookup time, and the confidence of the assessment before recommending a block.

### 3.8 Incident Response Requires Recipient and Mail-System Evidence

The email investigation did not establish whether the link was clicked, sensitive information was submitted, or the message was reported or blocked.

**Lesson:** Email-file analysis alone cannot determine the full impact of a suspected phishing incident.

**Recommended improvement:** Where authorized records are available, correlate email gateway, browser, DNS, endpoint, and account activity to establish delivery, recipient interaction, and potential impact.

## 4. Detection and Prevention Improvements

The following improvements are recommended for an organization handling similar emails.

| Area | Recommended improvement | Expected benefit |
|---|---|---|
| Email filtering | Improve detection of suspicious financial requests and external links | Reduce delivery of phishing messages |
| Authentication | Monitor and investigate SPF, DKIM, and DMARC anomalies | Improve sender-validation visibility |
| URL protection | Use approved DNS, proxy, and email URL-filtering controls | Reduce access to suspected phishing destinations |
| User awareness | Train users to verify unexpected financial requests | Reduce social-engineering success |
| Incident response | Define procedures for quarantine, investigation, escalation, and recovery | Improve consistency of response |
| Evidence handling | Preserve original messages, hashes, and investigation records | Support repeatable analysis |
| Threat intelligence | Record reputation results with timestamps and limitations | Avoid overreliance on stale or incomplete data |
| Logging | Retain relevant mail gateway, browser, DNS, and endpoint logs | Improve investigation of recipient interaction |

These are recommendations. The investigation did not verify that these controls were deployed or tested in a production environment.

## 5. Improvements to the Investigation Workflow

For future phishing investigations, the following workflow is recommended:

1. Preserve the original email and calculate its hash.
2. Record initial metadata and classify the message for further investigation.
3. Examine headers, routing, and authentication results.
4. Analyze message content and extract URLs and other indicators.
5. Review MIME structure and attachment candidates without opening suspicious content.
6. Check relevant domains and IPs with threat intelligence services.
7. Map observed behavior to applicable MITRE ATT&CK techniques.
8. Assess potential risk and identify unknown impact.
9. Recommend containment and investigate recipient interaction when evidence is available.
10. Produce the incident report and record lessons learned.

## 6. Limitations and Outstanding Questions

The investigation did not establish:

- Whether a recipient clicked the suspicious link.
- Whether payment-card details, credentials, or personal information were submitted.
- Whether the email was reported or blocked by a security control.
- Whether malware was downloaded or executed.
- Whether an account was compromised.
- Whether financial loss occurred.
- Whether any recommended containment action was implemented.

These questions require additional evidence, such as recipient confirmation or relevant mail gateway, browser, endpoint, and account logs.

## 7. Final Lessons Learned

This case demonstrates the value of combining email-header analysis, authentication checks, content analysis, URL inspection, threat intelligence, and ATT&CK mapping when investigating suspected phishing.

The most important lesson is that no single signal determines whether an email is malicious. Suspicious content and behavior must be considered alongside technical evidence, and inconclusive reputation results must not be mistaken for proof of safety.

A disciplined investigation should preserve evidence, clearly separate observations from assumptions, assess potential impact, recommend targeted containment, and identify the additional records needed to determine whether a compromise actually occurred.

**Final assessment:** The email remains classified as suspected phishing with high potential risk. Successful compromise has not been confirmed.

## 8. Related Case Reports

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
- `Analysis/Incident-Report.md`

Verify that each filename matches the actual report in the repository before committing.

