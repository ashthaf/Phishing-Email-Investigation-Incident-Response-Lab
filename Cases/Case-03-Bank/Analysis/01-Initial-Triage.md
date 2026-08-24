# Initial Triage

## Objective

The objective of the initial triage phase is to perform a preliminary, non-invasive examination of the suspicious email and identify visible indicators that may suggest phishing or impersonation activity.

The email was first inspected from the perspective of a potential recipient using Thunderbird before proceeding to detailed technical analysis.

---

# Evidence Examined

| **Item**             | **Details**       |
| -------------------- | ----------------- |
| Case ID              | Case-03-Bank      |
| Evidence File        | `sample-220.eml`  |
| Evidence Type        | Phishing Email    |
| Analysis Method      | Visual Inspection |
| Email Client         | Thunderbird       |
| Interaction          | Non-invasive      |
| Links Clicked        | No                |
| Attachments Executed | No                |

---

# Thunderbird Visual Inspection

The original email was opened in Thunderbird for visual inspection.

The message presents itself as being associated with **Santander / Banco Santander** and attempts to persuade the recipient to update account-related information.

The visible sender information is:

```text
"santander" <compras@oftalmozonasul.com.br>
```

The subject is:

```text
Atualização Pendente.
```

The email contains Santander-related branding and a prominent call-to-action requesting the recipient to update their information.

No links were clicked during the visual inspection.

### Screenshot — Email Visual Inspection

![Email Visual Inspection](../Screenshots/01-Initial-Triage/01-email-visual-inspection.png)

---

# Initial Observations

## 1. Bank Brand Impersonation

The email uses the name and branding associated with **Santander / Banco Santander**.

The use of a financial institution's branding is intended to make the message appear legitimate and increase the likelihood of recipient interaction.

---

## 2. Suspicious Sender Identity

The displayed sender is:

```text
"santander" <compras@oftalmozonasul.com.br>
```

The display name references Santander, while the email address uses:

```text
oftalmozonasul.com.br
```

This domain does not correspond to the Santander brand represented in the message.

This observation will be validated further during the **Header Analysis** and **Sender Analysis** phases.

---

## 3. Pending Update Theme

The subject line:

```text
Atualização Pendente.
```

creates a sense that an account-related action is pending.

This type of messaging can encourage recipients to act quickly without independently verifying the request.

---

## 4. Account Information Update Request

The email instructs the recipient to update account or related information.

The message contains a prominent call-to-action:

```text
CLIQUE AQUI PARA ATUALIZAR OS DADOS!
```

This indicates that the recipient is being encouraged to interact with an embedded hyperlink.

---

## 5. Social Engineering

The message combines:

* Trusted banking branding
* An apparent pending update
* A request to update information
* A prominent call-to-action
* An embedded hyperlink

These elements are consistent with social engineering techniques commonly used to persuade recipients to interact with phishing emails.

---

# Suspicious Link Inspection

The suspicious hyperlink was inspected visually by hovering over the link in Thunderbird.

The link was **not clicked** during the investigation.

No credentials or personal information were entered.

The actual URL and destination infrastructure will be extracted and investigated during the dedicated **URL Analysis** phase.

### Screenshot — Suspicious Link Hover

![Suspicious Link Hover](../Screenshots/01-Initial-Triage/02-suspicious-link-hover.png)

---

# Safety Precautions

The following precautions were followed during the initial triage phase:

* The original email evidence was preserved.
* The email was opened only for visual inspection.
* No suspicious links were clicked.
* No credentials were entered.
* No personal information was submitted.
* No attachments were executed.
* No external website was intentionally opened from the email.
* The suspicious hyperlink will be analyzed separately using controlled investigation methods.

---

# Initial Phishing Indicators

| **Indicator**          | **Observation**                            | **Assessment**     |
| ---------------------- | ------------------------------------------ | ------------------ |
| Brand Impersonation    | Santander / Banco Santander branding       | Suspicious         |
| Sender Identity        | Display name claims to be Santander        | Suspicious         |
| Sender Domain          | `oftalmozonasul.com.br`                    | Suspicious         |
| Subject                | `Atualização Pendente.`                    | Social Engineering |
| Account Update Request | Recipient instructed to update information | Suspicious         |
| Call-to-Action         | `CLIQUE AQUI PARA ATUALIZAR OS DADOS!`     | Suspicious         |
| Embedded Hyperlink     | User interaction requested                 | Suspicious         |

These observations are preliminary and will be validated through technical analysis during the subsequent investigation phases.

---

# Analyst Assessment

The initial visual inspection identified multiple characteristics consistent with a bank impersonation phishing email.

The message uses Santander-related branding, presents a pending account-update scenario, uses a sender identity that does not visibly correspond to the claimed organization, and encourages the recipient to interact with an embedded hyperlink.

At this stage, the findings are considered **initial observations** rather than final conclusions.

Further investigation is required to validate:

* Email authentication results
* Mail-routing infrastructure
* Sender and Return-Path relationships
* Originating IP addresses
* Embedded URLs
* Domain infrastructure
* Threat intelligence reputation
* Potential Indicators of Compromise (IOCs)

---

# Screenshots

The following screenshots were captured during the initial triage phase.

## Screenshot 1 — Email Visual Inspection

The screenshot shows the suspicious email as displayed to the recipient, including sender information, subject, Santander branding, message content, and the account-update request.

![Email Visual Inspection](../Screenshots/01-Initial-Triage/01-email-visual-inspection.png)

---

## Screenshot 2 — Suspicious Link Hover

The screenshot shows the suspicious hyperlink being inspected by hovering over it without clicking.

![Suspicious Link Hover](../Screenshots/01-Initial-Triage/02-suspicious-link-hover.png)

---

# Phase Status

**Phase 01 — Initial Triage: COMPLETE**

The email has been visually inspected in Thunderbird and the initial phishing indicators have been documented.

No potentially malicious links were clicked and no credentials or personal information were submitted during the triage process.

The investigation will now proceed to **Phase 02 — Header Analysis**.
