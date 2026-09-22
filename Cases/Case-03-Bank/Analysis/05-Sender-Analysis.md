# Sender Analysis

**Case:** Case-03-Bank  
**Evidence:** `sample-220.eml`  
**Analysis Phase:** Sender & Email Infrastructure Analysis  
**Status:** Completed  
**Analyst:** Abdull Ashthaf CK  

---

## 1. Objective

The objective of this analysis was to examine the sender identity, email headers, DNS infrastructure, authentication results, and registration information associated with the suspicious email contained in:

```text
sample-220.eml
````

The analysis focused on:

* Sender identity
* From and Return-Path comparison
* Message-ID and Reply-To fields
* Received headers
* SPF authentication results
* DNS records
* WHOIS information
* Identification of indicators requiring further investigation

---

## 2. Email Evidence

| Field         | Observed Value                                |
| ------------- | --------------------------------------------- |
| Evidence File | `sample-220.eml`                              |
| From          | `"santander" <compras@oftalmozonasul.com.br>` |
| Return-Path   | `compras@oftalmozonasul.com.br`               |
| To            | `phishing@pot`                                |
| Subject       | `=?ISO-8859-1?Q?Atualiza=E7=E3o_Pendente.`    |
| Date          | `Fri, 6 Jan 2023 19:15:08 -0300`              |
| Sender Domain | `oftalmozonasul.com.br`                       |

---

# 3. Sender Identity Analysis

## 3.1 From Header

### Command

```bash
grep -in '^From:' sample-220.eml
```

### Result

```text
From: "santander" <compras@oftalmozonasul.com.br>
```

### Screenshot

![From Header](../Evidence/email/screenshots/05-from-header.png)

### Observation

The email uses the display name:

```text
santander
```

while the actual email address is:

```text
compras@oftalmozonasul.com.br
```

The displayed identity and the sender domain do not represent the same organization.

This discrepancy is recorded as an indicator for further investigation.

---

# 4. Return-Path Analysis

### Command

```bash
grep -in '^Return-Path:' sample-220.eml
```

### Result

```text
Return-Path: compras@oftalmozonasul.com.br
```

### Screenshot

![Return-Path](../Evidence/email/screenshots/08-return-path.png)

### Observation

The Return-Path uses the same address as the sender address:

```text
compras@oftalmozonasul.com.br
```

No separate Return-Path domain was identified.

---

# 5. Message-ID Analysis

### Command

```bash
grep -in '^Message-ID:' sample-220.eml
```

### Result

The command returned the following header without a populated value:

```text
Message-ID:
```

### Screenshot

![Message-ID Analysis](../Evidence/email/screenshots/06-message-id.png)

### Observation

A populated Message-ID value was not observed in the examined email.

---

# 6. Reply-To Analysis

### Command

```bash
grep -in '^Reply-To:' sample-220.eml
```

### Result

No `Reply-To` header was returned.

### Screenshot

![Reply-To Analysis](../Evidence/email/screenshots/07-reply-to.png)

### Observation

No separate reply address was identified from the available headers.

---

# 7. Sender Header Analysis

### Command

```bash
grep -in '^Sender:' sample-220.eml
```

### Result

No `Sender:` header was returned.

### Screenshot

![Sender Header](../Evidence/email/screenshots/10-sender-header.png)

### Observation

No separate Sender header was present in the examined email.

---

# 8. Header Comparison

The principal sender-related headers were extracted using:

```bash
grep -inE '^(From:|Return-Path:|Sender:|Reply-To:|To:|Delivered-To:)' sample-220.eml
```

### Result

```text
From: "santander" <compras@oftalmozonasul.com.br>
To: phishing@pot
Return-Path: compras@oftalmozonasul.com.br
```

### Screenshot

![Header Comparison](../Evidence/email/screenshots/09-header-overview.png)

### Summary

| Header       | Value                                         |
| ------------ | --------------------------------------------- |
| From         | `"santander" <compras@oftalmozonasul.com.br>` |
| To           | `phishing@pot`                                |
| Return-Path  | `compras@oftalmozonasul.com.br`               |
| Sender       | Not observed                                  |
| Reply-To     | Not observed                                  |
| Delivered-To | Not observed                                  |

---

# 9. Full Header Analysis

A broader extraction of relevant email headers was performed:

```bash
grep -inE '^(From:|To:|Cc:|Bcc:|Reply-To:|Sender:|Return-Path:|Message-ID:|Date:|Subject:|Received:|Authentication-Results:|DKIM-Signature:|ARC|X-|MIME-Version|Content-Type):' sample-220.eml
```

### Relevant Results

```text
Received: from PH7PR19MB6484.namprd19.prod.outlook.com (::1) by
Received: from ZR0P278CA0127.CHEP278.PROD.OUTLOOK.COM
Received: from VI1EUR06FT063.eop-eur06.prod.protection.outlook.com
Authentication-Results: spf=permerror (sender IP is 187.85.67.134)
Received: from dbmailb02.dbmail.porta80.com.br (187.85.67.134)
Received: from localhost (localhost [127.0.0.1])
Received: from dbmailb02.dbmail.porta80.com.br ([127.0.0.1])
Received: from DESKTOP-PUC3EA (unknown [185.54.230.174])
From: "santander" <compras@oftalmozonasul.com.br>
To: phishing@pot
Date: Fri, 6 Jan 2023 19:15:08 -0300
Return-Path: compras@oftalmozonasul.com.br
MIME-Version: 1.0
Content-Type: multipart/alternative
Content-Type: text/plain
Content-Type: text/html
```

### Screenshot

![Full Header Analysis](../Evidence/email/screenshots/09-header-overview.png)

---

# 10. Authentication Analysis

The email contains the following authentication result:

```text
Authentication-Results: spf=permerror (sender IP is 187.85.67.134)
```

### Screenshot

![Authentication Results](../Evidence/email/screenshots/09-header-overview.png)

### Observed SPF Result

```text
SPF: permerror
Sender IP: 187.85.67.134
```

### Observation

The SPF evaluation returned:

```text
spf=permerror
```

for the sender IP:

```text
187.85.67.134
```

This authentication result should be correlated with the DNS and infrastructure findings.

No `DKIM-Signature` header was observed in the extracted header output.

---

# 11. Received Header Analysis

Multiple `Received:` headers were identified.

Important entries include:

```text
Received: from dbmailb02.dbmail.porta80.com.br (187.85.67.134)
```

and:

```text
Received: from DESKTOP-PUC3EA (unknown [185.54.230.174])
```

### Screenshot

![Received Headers](../Evidence/email/screenshots/09-header-overview.png)

### Observed Infrastructure Indicators

| Indicator                         | Type     | Context                                           |
| --------------------------------- | -------- | ------------------------------------------------- |
| `187.85.67.134`                   | IPv4     | Associated with `dbmailb02.dbmail.porta80.com.br` |
| `185.54.230.174`                  | IPv4     | Associated with `DESKTOP-PUC3EA`                  |
| `dbmailb02.dbmail.porta80.com.br` | Hostname | Appears in Received header                        |
| `DESKTOP-PUC3EA`                  | Hostname | Appears in Received header                        |
| `127.0.0.1`                       | IPv4     | Localhost entry                                   |

These indicators are preserved for further infrastructure and threat-intelligence analysis.

---

# 12. DNS Analysis

The sender domain identified from the email was:

```text
oftalmozonasul.com.br
```

The following DNS record types were queried:

* A
* MX
* NS
* TXT

---

## 12.1 A Record

### Command

```bash
dig A oftalmozonasul.com.br
```

### Result

```text
status: NXDOMAIN
ANSWER: 0
```

### Screenshot

![DNS A Record](../Evidence/email/screenshots/01-dns-a.png)

### Observation

No A record was returned for:

```text
oftalmozonasul.com.br
```

The DNS response returned `NXDOMAIN`.

---

## 12.2 MX Record

### Command

```bash
dig MX oftalmozonasul.com.br
```

### Result

```text
status: NXDOMAIN
ANSWER: 0
```

### Screenshot

![DNS MX Record](../Evidence/email/screenshots/02-dns-mx.png)

### Observation

No MX record was returned for the queried domain.

---

## 12.3 NS Record

### Command

```bash
dig NS oftalmozonasul.com.br
```

### Result

```text
status: NXDOMAIN
ANSWER: 0
```

### Screenshot

![DNS NS Record](../Evidence/email/screenshots/03-dns-ns.png)

### Observation

No NS record was returned for the queried domain.

---

## 12.4 TXT Record

### Command

```bash
dig TXT oftalmozonasul.com.br
```

### Result

```text
status: NXDOMAIN
ANSWER: 0
```

### Screenshot

![DNS TXT Record](../Evidence/email/screenshots/04-dns-txt.png)

### Observation

No TXT record was returned for the queried domain.

As a result, no TXT-based SPF or other domain information was obtained from this DNS query.

---

# 13. DNS Summary

A combined DNS query was also performed:

```bash
for type in A MX NS TXT; do
    echo "===== $type ====="
    dig +short $type oftalmozonasul.com.br
done
```

### Results

| Record Type | Result    |
| ----------- | --------- |
| A           | No result |
| MX          | No result |
| NS          | No result |
| TXT         | No result |

The individual `dig` queries returned `NXDOMAIN`.

### Screenshot

![Combined DNS Analysis](../Evidence/email/screenshots/04-dns-txt.png)

---

# 14. WHOIS Analysis

### Command

```bash
whois oftalmozonasul.com.br
```

### Result

```text
No match for oftalmozonasul.com.br
```

The response also stated that:

```text
whois.registro.br only accepts exact match queries for domains,
registrants, contacts, tickets, providers, IPs, and ASNs.
```

### Screenshot

![WHOIS Lookup](../Evidence/email/screenshots/11-whois.png)

### Observation

No WHOIS registration match was returned for:

```text
oftalmozonasul.com.br
```

This finding is documented separately from the DNS results because WHOIS and DNS provide different types of information.

---

# 15. Sender Analysis Summary

The investigation produced the following observations:

| Category       | Observation                             |
| -------------- | --------------------------------------- |
| Display Name   | `santander`                             |
| Sender Address | `compras@oftalmozonasul.com.br`         |
| Return-Path    | `compras@oftalmozonasul.com.br`         |
| Reply-To       | Not observed                            |
| Sender Header  | Not observed                            |
| Message-ID     | Header observed without populated value |
| SPF            | `permerror`                             |
| SPF IP         | `187.85.67.134`                         |
| Additional IP  | `185.54.230.174`                        |
| Mail Host      | `dbmailb02.dbmail.porta80.com.br`       |
| Hostname       | `DESKTOP-PUC3EA`                        |
| A Record       | `NXDOMAIN`                              |
| MX Record      | `NXDOMAIN`                              |
| NS Record      | `NXDOMAIN`                              |
| TXT Record     | `NXDOMAIN`                              |
| WHOIS          | No match                                |

---

# 16. Indicators Requiring Further Investigation

The following indicators were extracted during this phase:

| Indicator                         | Type          | Next Action                             |
| --------------------------------- | ------------- | --------------------------------------- |
| `oftalmozonasul.com.br`           | Domain        | Infrastructure / Threat Intelligence    |
| `compras@oftalmozonasul.com.br`   | Email Address | Sender correlation                      |
| `187.85.67.134`                   | IPv4          | IP reputation / infrastructure analysis |
| `185.54.230.174`                  | IPv4          | IP reputation / infrastructure analysis |
| `dbmailb02.dbmail.porta80.com.br` | Hostname      | Infrastructure analysis                 |
| `DESKTOP-PUC3EA`                  | Hostname      | Correlation with email routing          |
| `santander`                       | Display Name  | Sender identity analysis                |

---

# 17. Current Assessment

The evidence examined in this phase shows several points requiring additional investigation:

1. The email uses the display name **`santander`**, while the actual sender address belongs to **`oftalmozonasul.com.br`**.
2. The sender domain returned **`NXDOMAIN`** for A, MX, NS, and TXT queries during the investigation.
3. The email authentication result contains an **SPF `permerror`** associated with `187.85.67.134`.
4. The Received headers expose infrastructure including:

   * `187.85.67.134`
   * `185.54.230.174`
   * `dbmailb02.dbmail.porta80.com.br`
   * `DESKTOP-PUC3EA`
5. The WHOIS lookup returned **`No match`** for the queried domain.

These findings are indicators for continued investigation. They are not, by themselves, sufficient to establish the complete origin or intent of the email.

---

# 18. Investigation Limitations

This analysis is based on the evidence available in:

```text
sample-220.eml
```

and the DNS/WHOIS queries performed during this investigation.

This phase does not yet establish:

* The actual identity of the sender
* Ownership of the observed infrastructure
* Whether the observed IP addresses are malicious
* Whether URLs contained in the email are malicious
* Whether attachments are malicious
* Whether the email resulted in a compromise
* The complete attack chain

Additional evidence is required before making those determinations.

---

# 19. Next Investigation Steps

The next phase of the investigation should proceed with:

### 1. Email Body Analysis

* Extract the plain-text body.
* Extract the HTML body.
* Identify social-engineering indicators.
* Identify suspicious wording and requested actions.

### 2. URL Extraction

* Extract every URL from the email.
* Record destination domains.
* Compare visible link text with actual URLs.
* Investigate suspicious redirects or domains.

### 3. Attachment Analysis

* Identify all MIME attachments.
* Calculate file hashes.
* Determine actual file types.
* Perform static analysis where applicable.

### 4. IOC Enrichment

Investigate:

```text
187.85.67.134
185.54.230.174
dbmailb02.dbmail.porta80.com.br
oftalmozonasul.com.br
```

### 5. Threat Intelligence Correlation

Correlate extracted indicators with available threat-intelligence sources.

### 6. MITRE ATT&CK Mapping

Map confirmed behaviors to MITRE ATT&CK techniques after sufficient evidence has been collected.

### 7. Final Incident Assessment

Combine:

* Header analysis
* DNS analysis
* URL analysis
* Attachment analysis
* Infrastructure analysis
* Threat intelligence

into the final incident assessment.

---

# 20. Investigation Status

| Investigation Area              | Status      |
| ------------------------------- | ----------- |
| Sender identification           | ✅ Completed |
| From / Return-Path analysis     | ✅ Completed |
| Message-ID check                | ✅ Completed |
| Reply-To check                  | ✅ Completed |
| Sender header check             | ✅ Completed |
| Received header review          | ✅ Completed |
| SPF result identification       | ✅ Completed |
| A record lookup                 | ✅ Completed |
| MX record lookup                | ✅ Completed |
| NS record lookup                | ✅ Completed |
| TXT record lookup               | ✅ Completed |
| WHOIS lookup                    | ✅ Completed |
| Email body analysis             | ⏳ Pending   |
| URL extraction                  | ⏳ Pending   |
| URL analysis                    | ⏳ Pending   |
| Attachment analysis             | ⏳ Pending   |
| IOC enrichment                  | ⏳ Pending   |
| Threat intelligence correlation | ⏳ Pending   |
| MITRE ATT&CK mapping            | ⏳ Pending   |
| Final incident assessment       | ⏳ Pending   |

---

## Evidence Screenshots

The screenshots referenced in this document should be stored under:

```text
Evidence/email/screenshots/
```

Expected evidence:

* `01-dns-a.png`
* `02-dns-mx.png`
* `03-dns-ns.png`
* `04-dns-txt.png`
* `05-from-header.png`
* `06-message-id.png`
* `07-reply-to.png`
* `08-return-path.png`
* `09-header-overview.png`
* `10-sender-header.png`
* `11-whois.png`

---

**Analysis Phase:** Sender & Email Infrastructure Analysis
**Evidence File:** `sample-220.eml`
**Status:** Completed
**Next Phase:** Email Body, URL & Attachment Analysis

````

### GitHub folder structure

Your case should now look roughly like:

```text
Case-03-Bank/
├── Evidence/
│   └── email/
│       ├── sample-220.eml
│       └── screenshots/
│           ├── 01-dns-a.png
│           ├── 02-dns-mx.png
│           ├── 03-dns-ns.png
│           ├── 04-dns-txt.png
│           ├── 05-from-header.png
│           ├── 06-message-id.png
│           ├── 07-reply-to.png
│           ├── 08-return-path.png
│           ├── 09-header-overview.png
│           ├── 10-sender-header.png
│           └── 11-whois.png
│
└── Analysis/
    └── sender-analysis.md
````


