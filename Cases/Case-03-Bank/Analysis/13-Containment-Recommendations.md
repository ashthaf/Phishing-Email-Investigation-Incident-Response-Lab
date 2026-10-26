# Containment Recommendations — Case-03-Bank

## 1. Objective

This phase recommends actions to contain the suspected phishing email `sample-220.eml`, reduce the likelihood of further recipient exposure, and limit potential damage.

The recommendations are based on evidence collected during the preceding investigation phases. They describe proposed actions and do not claim that containment has already been performed.

## 2. Incident Summary

The investigated email impersonates Santander, requests an update to payment-card information, and contains a suspicious link associated with `conecte-way.online`.

The sender address uses `oftalmozonasul.com.br`. Previous analysis identified an SPF `permerror` and no DKIM/DMARC results in the examined message.

Threat intelligence results were inconclusive or neutral. They do not establish that the suspicious email or its destination is safe.

The reviewed evidence does not establish whether a recipient clicked the link, submitted sensitive information, or suffered a compromise.

## 3. Priority 1 — Quarantine the Email

### Recommended Actions

1. Search mailboxes and mail gateway records for matching messages.
2. Use the suspicious URL, sender address, subject, and message characteristics to identify related emails.
3. Quarantine or remove confirmed matching phishing messages according to organizational policy.
4. Preserve the original email and record its SHA-256 hash.
5. Record the number of messages found, recipients affected, and actions taken.

### Expected Outcome

Reduce the likelihood that additional recipients interact with the suspected phishing email while preserving evidence for investigation.

### Verification

Review mail gateway or email administration records to confirm the number of messages located, quarantined, or removed.

## 4. Priority 2 — Apply Targeted Indicator Blocks

### Primary Indicator

`conecte-way.online`

### Recommended Actions

1. Validate the domain and full URL against the evidence preserved from the original email.
2. Consider blocking the domain through approved DNS filtering, web proxy, endpoint web protection, or email-security controls.
3. Where supported, block the confirmed phishing URL in email and web-filtering systems.
4. Record the rule, implementation time, scope, and owner.
5. Test the control through an approved method to confirm that it works.

### Additional Indicators

- Sender domain: `oftalmozonasul.com.br`
- IP address: `187.85.67.134`
- IP address: `185.54.230.174`

These indicators require context. Do not automatically block the sender domain or routing IP addresses solely because they appeared in the email. Consider their observed role, shared infrastructure, business impact, and the available reputation evidence before implementing broader blocks.

### Expected Outcome

Reduce the chance of recipients accessing the suspected phishing destination and receiving matching messages.

### Verification

Review security-control logs and test the deployed rule using an approved method. Do not assume that a block is active until it has been verified.

## 5. Priority 3 — Identify Additional Recipients

### Recommended Actions

1. Search mail gateway records for the sender, subject, URL, and similar message content.
2. Identify the number of matching messages and affected recipients.
3. Determine whether the messages were delivered, quarantined, or reported.
4. Remove or quarantine confirmed malicious copies according to policy.
5. Notify the security team and affected users through approved channels.

### Expected Outcome

Establish the scope of distribution and reduce further exposure.

## 6. Priority 4 — Investigate Recipient Interaction

No click or data submission has been confirmed in the reviewed evidence.

### Recommended Actions

1. Review available browser, proxy, DNS, endpoint, and mail gateway logs.
2. Establish whether a recipient accessed the suspicious destination.
3. Check for downloads, unexpected processes, or related endpoint alerts.
4. Determine whether credentials, payment-card details, or other information were submitted.
5. Preserve relevant logs and document the investigation timeline.

### Conditional Response

- **If a recipient clicked the link:** investigate browser and endpoint activity.
- **If a file was downloaded or executed:** follow the organization's endpoint incident-response procedure and isolate the device when justified.
- **If credentials were disclosed:** reset affected credentials through the legitimate service, revoke sessions where appropriate, and review account activity.
- **If payment-card information was disclosed:** contact the card issuer and follow its fraud-response procedures.
- **If unauthorized account activity is discovered:** escalate to incident response and preserve the relevant evidence.

### Expected Outcome

Determine whether the event remained an attempted phishing incident or resulted in additional compromise.

## 7. Priority 5 — User Awareness and Reporting

### Recommended Actions

1. Remind recipients not to follow links in unexpected financial-account update emails.
2. Encourage users to report suspicious messages using the organization's approved reporting process.
3. Explain that display names and branding can be impersonated.
4. Remind users to verify financial requests through independently obtained official contact information.

### Expected Outcome

Reduce the likelihood of successful social engineering and improve future reporting.

## 8. Evidence Preservation

Preserve the original email and its SHA-256 hash:

`b4e6cce6749f1fc6f8eee7b5f21b7da0e668e29a853ab6f4bb647f34b75d17fb`

Maintain the original file as evidence and perform analysis on working copies where possible.

Record any containment action with the date and time, responsible analyst, affected indicator or message, control applied, and verification result.

## 9. Recommended Verification Checklist

- [ ] Matching emails searched and disposition recorded.
- [ ] Confirmed phishing messages quarantined or removed, if authorized.
- [ ] URL/domain blocking decision documented.
- [ ] Any deployed blocking rule verified.
- [ ] Additional recipients identified, where records are available.
- [ ] Recipient click and data-submission status investigated.
- [ ] Account or endpoint response performed if supported by evidence.
- [ ] Original evidence and relevant logs preserved.
- [ ] Final containment status documented.

## 10. Conclusion

The email should be treated as suspected phishing because it impersonates a financial institution, requests payment-card information, and includes a suspicious external link.

The recommended containment approach is to preserve evidence, identify and quarantine matching emails, apply targeted controls to validated indicators, investigate recipient interaction, and respond to any confirmed exposure.

No containment action or successful compromise is claimed to have occurred as part of this analysis. Actual status must be established through authorized security controls and their logs.

