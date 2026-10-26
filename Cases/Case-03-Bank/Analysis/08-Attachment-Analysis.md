# Phase 8 — Attachment Analysis

**Case ID:** CASE-03-BANK  
**Evidence File:** `sample-220.eml`  
**Analysis Type:** Email MIME and Attachment Inspection  
**Environment:** Kali Linux  
**Status:** Completed

---

## 1. Objective

The objective of this phase was to inspect the MIME structure of the suspicious email `sample-220.eml` to identify conventional attachments, filenames, inline resources, and other potentially embedded content.

The analysis was performed using Linux command-line tools and a Python MIME inventory script. The original email was inspected as text; no suspicious attachment was executed.

## 2. Evidence Information

| Property | Value |
|---|---|
| Evidence file | `sample-220.eml` |
| File format | RFC 822 email |
| File size | Approximately 18 KB |
| MIME type | `multipart/alternative` |
| Subject | `Atualização Pendente.` |
| SHA-256 | `b4e6cce6749f1fc6f8eee7b5f21b7da0e668e29a853ab6f4bb647f34b75d17fb` |

The SHA-256 hash was calculated using `sha256sum` to record the identity of the evidence file examined.

### Evidence integrity commands

```bash
ls -lh sample-220.eml
file sample-220.eml
sha256sum sample-220.eml
```

## 3. MIME Structure and Inventory

The raw email headers and MIME boundaries were inspected to identify the message structure.

The MIME inventory identified two body parts:

| MIME Part | Content-Type | Transfer Encoding | Filename | Content-ID | Attachment Candidate |
|---|---|---|---|---|---|
| 1 | `text/plain` | `quoted-printable` | None | None | No |
| 2 | `text/html` | `quoted-printable` | None | None | No |

The inventory reported **zero named or explicitly identified attachment candidates**.

The MIME structure was consistent with a multipart email containing a plain-text alternative and an HTML alternative. Both identified parts had unspecified content disposition and no filename.

### MIME header inspection

The following command was used to locate MIME-related headers:

```bash
grep -inE '^(Content-Type|Content-Disposition|Content-Transfer-Encoding|Content-ID|MIME-Version):|filename\*?=' sample-220.eml
```

The MIME boundary markers were reviewed using:

```bash
grep -nE '^--[^[:space:]]+' sample-220.eml
```

The boundary output showed the two body-part boundaries and the closing boundary.

**Screenshot evidence:**

- `Artifacts/Attachment-Analysis/screenshots/03-mime-structure-review.png`
- `Artifacts/Attachment-Analysis/screenshots/06-mime-boundaries.png`

## 4. Python MIME Inventory Script

The following Python script was used to parse the email and enumerate its MIME parts. It checks each non-multipart part for its content type, disposition, filename, Content-ID, decoded payload size, and whether it qualifies as a named or explicitly declared attachment candidate.

```python
from email import policy
from email.parser import BytesParser
from pathlib import Path

path = Path("sample-220.eml")

with path.open("rb") as f:
    msg = BytesParser(policy=policy.default).parse(f)

print("CASE-03-BANK | MIME INVENTORY")
print("=" * 55)
print("Evidence:", path)
print("Subject:", msg.get("Subject", "(not present)"))

attachments = 0

for index, part in enumerate(msg.walk()):
    if part.is_multipart():
        continue

    disposition = part.get_content_disposition()
    filename = part.get_filename()
    payload = part.get_payload(decode=True)
    size = len(payload) if payload is not None else 0

    is_attachment = (
        disposition == "attachment"
        or filename is not None
    )

    if is_attachment:
        attachments += 1

    print(f"\nMIME part: {index}")
    print("Content-Type:", part.get_content_type())
    print("Disposition:", disposition or "(unspecified)")
    print("Filename:", filename or "(none)")
    print("Content-ID:", part.get("Content-ID", "(none)"))
    print("Decoded size:", size, "bytes")
    print("Attachment candidate:", "YES" if is_attachment else "NO")

print("\n" + "=" * 55)
print("Named/explicit attachment candidates:", attachments)
print("Review unnamed and inline parts before concluding.")
```

### Script output summary

The recorded inventory output reported:

- **Part 1:** `text/plain`, 885 decoded bytes.
- **Part 2:** `text/html`, 6,693 decoded bytes.
- **Named/explicit attachment candidates:** `0`.

The script does not automatically prove that every possible embedded resource is absent. Its results were therefore considered alongside the raw MIME headers, boundary markers, and HTML resource checks.

**Screenshot evidence:**

- `Artifacts/Attachment-Analysis/screenshots/04-embedded-content-review.png`

## 5. Attachment Indicator Checks

The raw email was searched for attachment-related headers and filenames.

```bash
grep -inE 'Content-Disposition:|filename\*?=|Content-ID:|Content-Location:' sample-220.eml
```

The final execution returned no matching lines.

A separate check was performed for Base64 transfer encoding:

```bash
grep -in 'Content-Transfer-Encoding: base64' sample-220.eml
```

No matching header was returned. The two MIME body parts identified by the inventory used `quoted-printable` encoding.

These results support the finding that no conventional attachment was identified in the examined MIME structure. The absence of a Base64 header alone is not proof that a message contains no embedded content.

**Screenshot evidence:**

- `Artifacts/Attachment-Analysis/screenshots/02-attachment-indicators.png`
- `Artifacts/Attachment-Analysis/screenshots/07-base64-check.png`
- `Artifacts/Attachment-Analysis/screenshots/10-final-attachment-header-check.png`

## 6. Inline and Externally Hosted Resources

The HTML body was searched for image tags, source references, inline Content-IDs, and embedded image data.

```bash
grep -inE '<img|src=|cid:|data:image|Content-ID|Content-Location' sample-220.eml
```

The search identified an externally hosted image reference associated with an Avast email signature:

```text
https://s-install.avcdn.net/ipm/preview/icons/...
```

The displayed URL was truncated in the terminal output, so the full resource URL is not reproduced here.

This reference indicates that the HTML body requests an external image resource. It does **not**, by itself, indicate that the image is attached to the email.

The external resource was not downloaded as part of this phase.

**Screenshot evidence:**

- `Artifacts/Attachment-Analysis/screenshots/04-embedded-content-review.png`

## 7. Findings Summary

| Check | Result |
|---|---|
| MIME structure | `multipart/alternative` |
| Plain-text body | Present |
| HTML body | Present |
| Named attachment candidates | 0 |
| Explicit attachment headers | No matches in the final header search |
| Base64 transfer-encoding header | Not found |
| External image reference | Identified in HTML |
| Attachment extraction | Not performed |

## 8. Conclusion

The MIME inventory and manual header checks identified two ordinary email body parts: a plain-text version and an HTML version. Neither part had a filename or was identified as an attachment candidate by the inventory script.

No explicit attachment-related headers or Base64 transfer-encoding headers were identified in the recorded checks. An externally hosted Avast signature image reference was found in the HTML body; this was treated as an external resource rather than a confirmed attachment.

**Conclusion:** No conventional email attachments were identified in the examined MIME structure of `sample-220.eml`. Attachment extraction was not required based on the evidence collected during this phase.

This conclusion is limited to the checks documented above and does not establish that all possible embedded or externally referenced content is harmless.

## 9. Evidence Screenshots

Screenshots are stored under:

`Artifacts/Attachment-Analysis/screenshots/`

The report references the following evidence images:

1. `03-mime-structure-review.png`
2. `06-mime-boundaries.png`
3. `04-embedded-content-review.png`
4. `02-attachment-indicators.png`
5. `07-base64-check.png`
6. `10-final-attachment-header-check.png`

Confirm the filenames against the actual screenshot directory before committing the report.

## 10. Analysis Safety

- The original email was examined as text.
- No suspicious attachment was executed.
- The externally hosted image was not downloaded.
- No attachment was extracted because no conventional attachment was identified.
- Findings were based on the observed MIME structure and the checks recorded in this report.

---

**Phase 8 Status:** Completed — no conventional attachments identified in the examined MIME structure.
