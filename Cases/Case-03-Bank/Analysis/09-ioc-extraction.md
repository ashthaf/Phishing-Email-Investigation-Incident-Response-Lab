# Phase 9 — IOC Extraction

**Case ID:** CASE-03-BANK  
**Evidence File:** `sample-220.eml`  
**Analysis Type:** Indicators of Compromise (IOC) Extraction  
**Environment:** Kali Linux  
**Status:** Completed

---

## 1. Objective

The objective of this phase was to extract and document potential Indicators of Compromise (IOCs) from the suspicious email `sample-220.eml`.

The examination focused on:

- URLs and destination domains present in the email.
- Sender email address and sender domain.
- IPv4 addresses observed in the email source and routing headers.
- Email metadata, including subject, date, and Message-ID.
- Contextual resources that should not automatically be classified as malicious.

Extracted values were assessed in context. Their presence in the email does not, by itself, establish that an IP address or domain is malicious.

## 2. Evidence Source

| Property | Value |
|---|---|
| Evidence file | `sample-220.eml` |
| File format | RFC 822 email |
| Subject | `Atualização Pendente.` |
| Date | `Fri, 6 Jan 2023 19:15:08 -0300` |
| Message-ID | Header present, value empty in the examined output |
| Sender | `compras@oftalmozonasul.com.br` |
| Return-Path | `compras@oftalmozonasul.com.br` |

The original email was examined as text. Suspicious links were not opened during this phase.

### Screenshot — Email metadata

![Email metadata extraction](../Artifacts/IOC-Extraction/screenshots/05-message-metadata.png)

## 3. URL Extraction

Candidate URLs were extracted using the following command:

```bash
grep -Eio 'https?://[^[:space:]"<>]+' sample-220.eml
```

The command identified URL candidates associated with the phishing destination and email-signature resources.

### Screenshot — URL extraction output

![URL extraction results](../Artifacts/IOC-Extraction/screenshots/01-url-extraction.png)

### 3.1 Primary suspicious URL

The HTML context was examined with:

```bash
grep -in -C 3 'conecte-way.online' sample-220.eml
```

The email's HTML contains a link behind the call to action:

`CLIQUE AQUI PARA ATUALIZAR OS DADOS!`

The destination shown in the raw source is:

```text
https://conecte-way.online/painel_=3Dstone_esfera_way_nubank_/painel_stone_esfera_way_nubank_2.2/esfera/
```

The path contains quoted-printable encoding. In this encoding, `=3D` represents the `=` character. The raw representation is retained above as observed in the source; a decoded representation should be documented separately if produced and verified.

| Field | Assessment |
|---|---|
| Indicator type | URL |
| Hostname | `conecte-way.online` |
| Source | HTML hyperlink in `sample-220.eml` |
| Context | Link prompts the recipient to update card data |
| Assessment | Suspicious phishing URL candidate |
| Confidence | High that the URL appears in the email; maliciousness is inferred from the surrounding context, not independently confirmed here |

The URL's wording and surrounding message content are consistent with a credential or payment-data phishing attempt. The URL was not opened.

### Screenshot — Phishing URL in HTML context

![Phishing URL context](../Artifacts/IOC-Extraction/screenshots/03-phishing-url-context.png)

## 4. Sender Indicators

The sender and routing fields were examined using:

```bash
grep -inE '^(From|Reply-To|Return-Path|Received):' sample-220.eml
```

The output showed:

- `From`: `"santander" <compras@oftalmozonasul.com.br>`
- `Return-Path`: `compras@oftalmozonasul.com.br`

The sender address uses the domain `oftalmozonasul.com.br`, rather than an apparent Santander-controlled domain. Earlier sender-analysis checks reported NXDOMAIN for the domain's DNS queries. These findings support treating the sender identity as suspicious in this message's context, but do not independently prove who operated the sending infrastructure.

| Indicator | Type | Source | Assessment |
|---|---|---|---|
| `compras@oftalmozonasul.com.br` | Email address | `From`, `Return-Path` | Suspicious sender identity |
| `oftalmozonasul.com.br` | Domain | Sender address | Suspicious in the context of the claimed bank identity |
| `conecte-way.online` | Domain | HTML hyperlink | Suspicious destination domain |

### Screenshot — Sender and routing fields

![Sender and routing indicators](../Artifacts/IOC-Extraction/screenshots/04-sender-and-routing-iocs.png)

## 5. IPv4 Address Extraction

Candidate IPv4 strings were extracted using:

```bash
grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}' sample-220.eml | sort -u
```

The command returned four unique values.

### Screenshot — Extracted IPv4 addresses

![IP extraction results](../Artifacts/IOC-Extraction/screenshots/02-ip-extraction.png)

### 5.1 IP address classification

| IP address | Observed context | Classification | Assessment |
|---|---|---|---|
| `187.85.67.134` | `Received` header associated with `dbmailB02.dbmail.porta80.com.br` | Routing infrastructure | Relevant infrastructure indicator; malicious reputation not established in this phase |
| `185.54.230.174` | `Received` header associated with `DESKTOP-PUCJ3EA` | Source/routing context | Relevant infrastructure indicator; ownership and malicious reputation not established in this phase |
| `127.0.0.1` | `localhost` in a `Received` header | Loopback address | Contextual value; exclude from external IOC list |
| `10.13.7.4` | Found by the raw-text extraction command | Private IPv4 address | Contextual value; exact operational meaning not established by extraction alone |

The IP extraction command identifies IPv4-looking strings wherever they occur in the file. Therefore, results must be checked against their surrounding headers or message content before being treated as external indicators.

Neither `127.0.0.1` nor `10.13.7.4` should be represented as a confirmed public malicious IP based on this extraction.

## 6. Contextual URLs and Resources

The URL extraction command also returned Avast-related resources.

| URL or hostname | Context | Treatment |
|---|---|---|
| `www.avast.com` | Email signature links | Contextual resource; not classified as malicious based on this evidence |
| `s-install.avcdn.net` | Externally hosted image referenced by the HTML email | External resource; not an identified email attachment |

The Avast-related URLs appear associated with email-signature content. They are retained as contextual resources rather than included in the primary suspicious IOC list.

The URL extraction command truncated some values because it processes the raw source text without fully decoding the email's quoted-printable content. Consequently, the command output should be treated as candidate extraction, not a complete normalized URL inventory.

## 7. Email Metadata

The following command was used:

```bash
grep -inE '^(Message-ID|Date|Subject):' sample-220.eml
```

Recorded values:

| Header | Observed value |
|---|---|
| `Subject` | `=?ISO-8859-1?Q?Atualiza=E7=E3o?= Pendente.` |
| `Date` | `Fri, 6 Jan 2023 19:15:08 -0300` |
| `Message-ID` | Header present, value empty in the displayed output |

The encoded subject corresponds to **“Atualização Pendente.”** The Message-ID field should remain documented as empty in the examined output; no identifier has been inferred or fabricated.

### Screenshot — Message metadata

![Message metadata](../Artifacts/IOC-Extraction/screenshots/05-message-metadata.png)

## 8. Consolidated IOC Table

The table below separates suspicious candidates from contextual values and records what is supported by the current evidence.

| Indicator | Type | Priority | Evidence / Rationale |
|---|---|---|---|
| `conecte-way.online` | Domain | High | Destination domain of the link requesting card-data updates |
| Full URL under `conecte-way.online` | URL | High | Observed in the HTML hyperlink behind the update call to action |
| `compras@oftalmozonasul.com.br` | Email address | High | Sender address presented under the claimed Santander identity |
| `oftalmozonasul.com.br` | Domain | High | Sender domain; earlier DNS checks returned NXDOMAIN |
| `187.85.67.134` | IPv4 address | Medium | Observed in a `Received` header; reputation not independently verified here |
| `185.54.230.174` | IPv4 address | Medium | Observed in a `Received` header; reputation not independently verified here |
| `www.avast.com` | Domain | Contextual | Email-signature link |
| `s-install.avcdn.net` | Domain | Contextual | Externally hosted signature image |
| `127.0.0.1` | IPv4 address | Excluded from external IOC list | Loopback address |
| `10.13.7.4` | IPv4 address | Contextual | Private IPv4 address; meaning not confirmed |

**Priority is an investigative triage assessment, not a verified threat-intelligence reputation rating.** No independent reputation verdict for the listed IP addresses or domains is claimed in this phase.

## 9. Limitations

- The URL extraction command may truncate or retain quoted-printable encoding.
- IPv4 extraction can include loopback, private, or otherwise contextual addresses.
- An email's sender fields do not independently authenticate the sender's identity.
- DNS findings reflect the earlier checks and the time they were performed.
- This phase did not establish an independently verified threat-intelligence reputation for every indicator.
- Suspicious URLs were not opened.

## 10. Conclusion

IOC extraction identified a suspicious destination domain, a phishing-related URL in the HTML body, and a sender address inconsistent with the claimed Santander identity. Two IP addresses were observed in routing-related headers and retained as infrastructure indicators pending further validation.

The extracted data also included an Avast email-signature resource, a loopback address, and a private IPv4 address. These were separated from the primary suspicious IOC candidates to avoid misclassification.

**Phase 9 Status:** Completed — candidate indicators extracted and categorized for further investigation.

---

## Evidence Screenshot Directory

Screenshots referenced in this report are intended to be stored under:

`Artifacts/IOC-Extraction/screenshots/`

Confirm that each referenced filename exists in the repository and update the Markdown paths if your actual screenshot names differ.
