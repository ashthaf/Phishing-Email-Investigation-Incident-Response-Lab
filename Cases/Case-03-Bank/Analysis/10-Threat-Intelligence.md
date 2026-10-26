# Threat Intelligence Analysis

## 1. Objective

The objective of this phase was to evaluate the reputation of the suspicious phishing domain, sender domain, and IP addresses observed during the previous email-analysis phases.

The investigation used VirusTotal and Cisco Talos Reputation Center. Findings reflect the results displayed in the captured screenshots and should not be interpreted as permanent or definitive classifications.

## 2. Tools Used

- **VirusTotal:** Domain and IP reputation analysis.
- **Cisco Talos Reputation Center:** Web reputation, sender IP reputation, network ownership, and block-list status.

## 3. Phishing Domain: `conecte-way.online`

### VirusTotal
- Detection ratio: 0/92.
- No security vendors flagged the domain as malicious.
- The displayed last analysis date was approximately two months before the lookup.
- The domain was labelled "not recommended site."

### Cisco Talos
- Web reputation: Unknown.
- Content category: No established content categories.
- Talos Security Intelligence Block List: Not added.

### Assessment
Neither service returned a positive malicious verdict in the captured results. However, the domain remains suspicious in the context of the email's impersonation of Santander, urgent request to update card information, and suspicious link.

The results do not establish that the domain is safe.

### Evidence
- `Artifacts/Threat-Intelligence/screenshots/01-virustotal-phishing-domain.png`
- `Artifacts/Threat-Intelligence/screenshots/02-talos-phishing-domain.png`

## 4. Sender Domain: `oftalmozonasul.com.br`

### VirusTotal
- Detection ratio: 0/92.
- No security vendors flagged the domain as malicious.
- The displayed last analysis date was approximately two years before the lookup.
- Multiple vendors were marked as unrated.

### Cisco Talos
- Web reputation: Unknown.
- Content category: No established content categories.
- Talos Security Intelligence Block List: Not added.

### Assessment
The captured results do not establish that the sender domain is malicious or safe. The old VirusTotal analysis and unknown Talos reputation limit the conclusions that can be drawn.

The domain's appearance in the sender address of an email impersonating Santander warrants further investigation.

### Evidence
- `Artifacts/Threat-Intelligence/screenshots/03-virustotal-sender-domain.png`
- `Artifacts/Threat-Intelligence/screenshots/04-talos-sender-domain.png`

## 5. IP Address: `187.85.67.134`

### VirusTotal
- Detection ratio: 0/92.
- One detected file was reported as embedding this IP address.
- Network range: `187.85.64.0/20`.
- ASN: AS53060.
- Network owner: Porta 80 - Serviços em Internet Ltda.
- Country shown: Brazil.
- Displayed last analysis: approximately one month before the lookup.

### Cisco Talos
- Sender IP reputation: Neutral.
- Web reputation: Neutral.
- Location: São Paulo, Brazil.
- Hostname: `dbmailb02.dbmail.porta80.com.br`.
- Network owner: PORTA 80 - SERVICOS EM INTERNET LTDA.
- The displayed BL.SPAMCOP.NET and CBL.ABUSEAT.ORG block-list entries were both marked "Not Listed."

### Assessment
The results show neutral Talos reputation and no VirusTotal malicious detections. The IP also matches mail-server infrastructure referenced in the email's routing headers. This does not establish that the IP itself was responsible for malicious activity.

### Evidence
- `Artifacts/Threat-Intelligence/screenshots/05-virustotal-ip-187.85.67.134.png`
- `Artifacts/Threat-Intelligence/screenshots/07-talos-ip-187.85.67.134.png`

## 6. IP Address: `185.54.230.174`

### VirusTotal
- Detection ratio: 0/92.
- No security vendors flagged the IP address as malicious.
- Country shown: Slovakia.

### Cisco Talos
- Sender IP reputation: Neutral.
- Web reputation: Neutral.
- Location: Slovakia.
- Hostname and domain: No data shown.
- Network owner: AVAST SLOVAKIA S.R.O.
- Content category: Personal VPN.
- The displayed BL.SPAMCOP.NET and CBL.ABUSEAT.ORG block-list entries were both marked "Not Listed."
- Email volume shown: 0.0 for both the last day and last month.

### Assessment
The displayed reputation results do not identify this IP as malicious. The Personal VPN category and network-owner information provide context, but do not establish the identity of the user or explain the IP's presence in the email headers.

### Evidence
- `Artifacts/Threat-Intelligence/screenshots/06-virustotal-ip-185.54.230.174.png`
- `Artifacts/Threat-Intelligence/screenshots/08-talos-ip-185.54.230.174.png`

## 7. Overall Findings

| Indicator | VirusTotal result | Cisco Talos result |
|---|---|---|
| `conecte-way.online` | 0/92 detections | Unknown |
| `oftalmozonasul.com.br` | 0/92 detections | Unknown |
| `187.85.67.134` | 0/92 detections | Neutral |
| `185.54.230.174` | 0/92 detections | Neutral |

## 8. Final Assessment

The threat intelligence lookups did not produce a positive malicious verdict for the investigated domains or IP addresses in the captured results. These findings must be interpreted cautiously because reputation databases can lack coverage, analyses may be outdated, and infrastructure may be shared.

The email remains suspicious based on the evidence from previous phases, including Santander impersonation, an urgent request for payment-card data updates, and a link hosted on a separate domain.

**Conclusion:** Treat the email as suspected phishing. The reputation checks provide contextual evidence but neither confirm nor disprove malicious intent on their own.

## 9. Limitations

- Results represent the reputation data displayed at the time of the screenshots.
- VirusTotal detection ratios are not guarantees of safety.
- Unknown or neutral reputation does not mean benign.
- IP geolocation and network ownership do not identify the person who sent the email.
- The presence of an IP in a routing header does not independently prove malicious activity.
- No phishing URL was opened during this phase.

