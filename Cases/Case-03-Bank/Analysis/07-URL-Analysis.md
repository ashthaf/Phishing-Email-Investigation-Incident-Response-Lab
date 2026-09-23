# Case 03 — Bank Phishing Email
## URL Analysis

**Case:** Case-03-Bank  
**Evidence:** `sample-220.eml`  
**Analysis Phase:** URL Analysis  
**Analyst:** Abdull Ashthaf  
**Environment:** Kali Linux — CyberLab Analysis VM  
**Analysis Date:** 23 September 2026  

---

# 1. Objective

The objective of this phase is to identify, extract, and analyze URLs contained within the suspicious email.

The analysis focuses on:

- Extracting URLs from the email.
- Identifying suspicious and legitimate URLs.
- Examining the HTML source surrounding suspicious links.
- Identifying suspicious URL paths and parameters.
- Performing DNS resolution analysis.
- Performing WHOIS/RDAP registration analysis.
- Comparing the apparent purpose of the URL with the context of the email.
- Preserving findings for subsequent Threat Intelligence analysis.

External threat-intelligence reputation checks are intentionally reserved for the **Threat Intelligence phase**.

---

# 2. Evidence Examined

Primary evidence:

```text
sample-220.eml
````

Relevant artifact:

```text
Artifacts/URL-Analysis/extracted_urls.txt
```

The URLs were extracted directly from the email evidence without visiting the suspicious destination.

---

# 3. URL Extraction

The following command was used to extract HTTP/HTTPS URLs from the email:

```bash
grep -Eo 'https?://[^"]+' sample-220.eml | sort -u \
> ~/CyberLab/Cases/Case-03-Bank/Artifacts/URL-Analysis/extracted_urls.txt
```

The resulting artifact was reviewed using:

```bash
cat ~/CyberLab/Cases/Case-03-Bank/Artifacts/URL-Analysis/extracted_urls.txt
```

### Extracted URLs

```text
https://conecte-way.online/painel_=
https://s-install.avcdn.net/ipm/preview/icons/i=
https://www.avast.com/sig-email?utm_medium=3Dema=
https://www.avast.com/sig-email?utm_medium=3Demail&utm_source=3Dlink&u=
```

The extracted URLs include both:

* A suspicious external domain.
* URLs associated with Avast email-security/signature infrastructure.

The presence of Avast-related URLs is consistent with the email containing an antivirus/security signature and does not by itself indicate malicious activity.

---

## Evidence Screenshot — URL Extraction

![URL Extraction](screenshots/01-url-extraction.png)

**Evidence:** The terminal output shows the URLs extracted from `sample-220.eml`.

---

# 4. Identification of the Suspicious URL

The URL of primary investigative interest is:

```text
https://conecte-way.online/painel_=
```

The domain is:

```text
conecte-way.online
```

During source inspection, the suspicious URL was found embedded within an HTML hyperlink.

The relevant HTML showed:

```text
href=3D"https://conecte-way.online/painel_=3D
stone_esfera_way_nubank_/painel_stone_esfera_way_nubank_2.2/esfera/"
```

The link was associated with the visible text:

```text
CLIQUE AQUI PARA
ATUALIZAR OS DADOS!
```

This is significant because the link text presents an account/data-update action while the destination domain does not correspond to the apparent banking identity referenced by the email.

---

# 5. Suspicious URL Source Inspection

The surrounding HTML was inspected using:

```bash
grep -in -C 5 "conecte-way.online" sample-220.eml
```

The output showed the suspicious domain being used as the destination of an HTML `<a href>` element.

The relevant source contained:

```text
href="https://conecte-way.online/painel_=
stone_esfera_way_nubank_/painel_stone_esfera_way_nubank_2.2/esfera/"
```

The hyperlink used:

```text
target="_blank"
```

and displayed a call-to-action requesting the recipient to update information.

---

## Evidence Screenshot — Suspicious URL in Email Source

![Suspicious URL Source](screenshots/02-suspicious-url-source.png)

**Evidence:** The screenshot shows the suspicious domain and the surrounding HTML hyperlink.

---

# 6. URL Path Analysis

The source contains a longer path than the initially extracted URL.

Observed suspicious path:

```text
/painel_=stone_esfera_way_nubank_/painel_stone_esfera_way_nubank_2.2/esfera/
```

The path contains banking-related terminology, including:

```text
stone
esfera
way
nubank
painel
```

The visible call-to-action associated with the link was:

```text
CLIQUE AQUI PARA ATUALIZAR OS DADOS!
```

This combination is noteworthy because the URL attempts to present a banking/account-management context through the URL path while using a separate domain:

```text
conecte-way.online
```

### Important observation

The banking-related words occur in the **path**, not in the registered domain.

The registered domain is:

```text
conecte-way.online
```

Therefore, the presence of terms such as `nubank` in the URL path does not establish that the destination belongs to the referenced financial institution.

---

# 7. URL Context Analysis

The URL was assessed in the context of the email rather than by opening it.

Observed characteristics:

| Observation                                            | Finding                                            |
| ------------------------------------------------------ | -------------------------------------------------- |
| Destination domain                                     | `conecte-way.online`                               |
| Apparent banking context                               | Yes                                                |
| Banking-related terms in path                          | Yes                                                |
| Account/data-update CTA                                | Yes                                                |
| Domain directly associated with apparent bank identity | Not established                                    |
| URL requires external destination                      | Yes                                                |
| Destination was opened during analysis                 | No                                                 |
| DNS resolution                                         | NXDOMAIN                                           |
| WHOIS/RDAP status                                      | Domain available for registration at time of query |

The combination of an account-update request, banking-related URL path, and a destination domain unrelated to the apparent banking identity is suspicious.

---

# 8. DNS Analysis

The suspicious domain was queried using `dig`:

```bash
dig conecte-way.online
```

The response returned:

```text
status: NXDOMAIN
```

The response contained:

```text
ANSWER: 0
```

and an SOA record in the authority section:

```text
online. 300 IN SOA ns.trs-dns.com. trs-ops.tucows.com.
```

The DNS server used for the query was:

```text
103.199.160.80
```

The query therefore did not produce an A record for:

```text
conecte-way.online
```

at the time of analysis.

---

## Evidence Screenshot — DNS Analysis

![DNS Analysis](screenshots/03-dns-analysis.png)

**Evidence:** `dig` returned `NXDOMAIN`, indicating that the queried domain did not resolve through DNS at the time of testing.

---

# 9. Host Resolution Check

A second resolution check was performed using:

```bash
host conecte-way.online
```

The result was:

```text
Host conecte-way.online not found: 3(NXDOMAIN)
```

This independently confirms that the domain was not resolving through the resolver at the time of analysis.

---

## Evidence Screenshot — Host Resolution

![Host Resolution](screenshots/04-host-resolution.png)

**Evidence:** The `host` command returned `NXDOMAIN`.

---

# 10. WHOIS / Domain Registration Analysis

A WHOIS query was performed:

```bash
whois conecte-way.online
```

The response indicated:

```text
Domain conecte-way.online is available for registration
```

The WHOIS service also displayed:

```text
Last update of WHOIS database:
2026-09-23T16:53:34.940Z
```

The response identified Tucows Registry as the registry providing the WHOIS information.

The result indicates that, at the time of the query, the domain was reported as available for registration.

---

## Evidence Screenshot — WHOIS Analysis

![WHOIS Analysis](screenshots/05-whois-analysis.png)

**Evidence:** The WHOIS response indicated that `conecte-way.online` was available for registration at the time of analysis.

---

# 11. Additional Source Inspection

The email source was also searched for terms associated with the suspicious URL.

For example:

```bash
grep -Ein -C 10 "stone_esfera_way_nubank" sample-220.eml
```

and:

```bash
grep -Ein -C 10 "esfera" sample-220.eml
```

These searches confirmed that the banking-related terminology occurs within the HTML content surrounding the suspicious hyperlink.

The relevant source includes:

```text
stone_esfera_way_nubank
```

and:

```text
painel_stone_esfera_way_nubank_2.2/esfera/
```

The link is presented as an action for updating information.

---

## Evidence Screenshot — Banking-Themed URL Path

![Banking URL Path](screenshots/06-banking-url-path.png)

**Evidence:** The screenshot demonstrates the banking-related terminology embedded within the suspicious URL path.

---

# 12. Avast URL Analysis

The email also contained URLs associated with Avast infrastructure:

```text
https://www.avast.com/sig-email
```

and:

```text
https://s-install.avcdn.net/ipm/preview/icons/
```

The email source also contained the following antivirus-related indicators:

```text
X-Antivirus: Avast
```

and:

```text
This email has been checked for viruses by Avast antivirus software.
```

These URLs appear to be associated with the email's Avast security/signature content.

They are therefore separated from the primary suspicious URL during analysis.

---

## Evidence Screenshot — Avast References

![Avast References](screenshots/07-avast-references.png)

**Evidence:** The screenshot shows Avast-related URLs and antivirus metadata present in the email.

---

# 13. URL Analysis Findings

The investigation established the following:

### Finding 1 — Suspicious destination domain

The email contains a hyperlink to:

```text
conecte-way.online
```

The domain does not directly correspond to the apparent financial institution referenced by the banking-themed URL path.

### Finding 2 — Banking-themed URL path

The hyperlink contains banking-related terminology:

```text
stone_esfera_way_nubank
```

and:

```text
painel_stone_esfera_way_nubank_2.2/esfera/
```

These terms are located in the URL path rather than establishing ownership of the domain.

### Finding 3 — Account-update call to action

The link is presented with:

```text
CLIQUE AQUI PARA ATUALIZAR OS DADOS!
```

This indicates that the recipient is being directed toward an account/data-update action.

### Finding 4 — DNS failure

At the time of investigation:

```text
conecte-way.online
```

returned:

```text
NXDOMAIN
```

from both:

```bash
dig
```

and:

```bash
host
```

### Finding 5 — WHOIS availability

The WHOIS query indicated:

```text
Domain conecte-way.online is available for registration
```

at the time of analysis.

### Finding 6 — Avast infrastructure is separate

Avast-related URLs were identified in the email, but these appear to be associated with the email security/signature content rather than the suspicious destination.

---

# 14. Evidence Summary

| Evidence                   | Result                                 | Significance                                           |
| -------------------------- | -------------------------------------- | ------------------------------------------------------ |
| Suspicious domain          | `conecte-way.online`                   | Primary URL requiring investigation                    |
| URL path                   | Banking-themed terminology             | Attempts to associate destination with banking context |
| CTA                        | `CLIQUE AQUI PARA ATUALIZAR OS DADOS!` | Requests recipient action                              |
| DNS                        | `NXDOMAIN`                             | Domain did not resolve during analysis                 |
| Host lookup                | `NXDOMAIN`                             | Confirms DNS resolution failure at analysis time       |
| WHOIS                      | Available for registration             | Domain reported as unregistered at query time          |
| Avast URLs                 | Present                                | Likely email security/signature infrastructure         |
| Destination visited        | No                                     | Analysis remained non-interactive                      |
| External reputation checks | Not performed                          | Reserved for Threat Intelligence phase                 |

---

# 15. Analyst Assessment

The URL evidence is **suspicious** based on the combination of:

1. A destination domain unrelated to the apparent banking identity.
2. Banking-related terminology embedded within the URL path.
3. An account/data-update call to action.
4. DNS resolution returning `NXDOMAIN`.
5. WHOIS reporting the queried domain as available for registration at the time of analysis.

However, URL analysis alone does not establish the complete infrastructure history, reputation, or previous malicious use of the URL.

Those questions are reserved for the next phase.

---

# 16. Limitations

The following limitations apply to this phase:

* The suspicious destination was **not opened** during analysis.
* No credentials or personal information were submitted.
* No active interaction with the destination was performed.
* DNS results represent the state observed at the time of investigation.
* WHOIS availability represents the registry state at the time of the query.
* URL reputation and historical threat information have not yet been evaluated.
* The extracted URL artifact may not preserve the complete HTML URL because of encoding and extraction behavior.
* The complete URL was therefore validated against the original email source.

---

# 17. Chain of Evidence

The following artifact was generated during analysis:

```text
Artifacts/URL-Analysis/extracted_urls.txt
```

The primary source evidence remains:

```text
Evidence/email/sample-220.eml
```

The screenshots documenting the analysis are stored under:

```text
Artifacts/URL-Analysis/screenshots/
```

Recommended screenshot set:

```text
01-url-extraction.png
02-suspicious-url-source.png
03-dns-analysis.png
04-host-resolution.png
05-whois-analysis.png
06-banking-url-path.png
07-avast-references.png
```

---

# 18. Phase Conclusion

The URL analysis identified a suspicious hyperlink associated with:

```text
conecte-way.online
```

The URL uses banking-related terminology in its path and presents an account/data-update call to action.

At the time of investigation, the domain returned:

```text
NXDOMAIN
```

and the WHOIS query indicated that the domain was available for registration.

The URL has therefore been preserved as a primary IOC for further investigation.

No active access to the suspicious destination was performed during this phase.

---

## Primary IOC

```text
conecte-way.online
```

## Suspicious URL observed in source

```text
https://conecte-way.online/painel_=stone_esfera_way_nubank_/painel_stone_esfera_way_nubank_2.2/esfera/
```

## Next Phase

**Threat Intelligence Analysis**

The next phase should investigate the identified IOC using external intelligence and reputation sources while preserving the current URL-analysis evidence as the baseline.

````

### Screenshot folder

Based on the screenshots you sent, keep them in:

```text
~/CyberLab/Cases/Case-03-Bank/Artifacts/URL-Analysis/screenshots/
````

and rename them to:

```text
01-url-extraction.png
02-suspicious-url-source.png
03-dns-analysis.png
04-host-resolution.png
05-whois-analysis.png
06-banking-url-path.png
07-avast-references.png
```

**Important:** I kept the `NXDOMAIN` and WHOIS findings exactly as your screenshots show them, and I separated the **URL Analysis** findings from the upcoming **Threat Intelligence** work. That way the next phase can build directly on `conecte-way.online` as the primary IOC without mixing the two stages.

