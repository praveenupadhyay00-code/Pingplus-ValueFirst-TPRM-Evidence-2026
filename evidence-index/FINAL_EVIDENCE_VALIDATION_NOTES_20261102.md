# Evidence validation notes — 2 November 2026

> **Superseded for current grades (5 November 2026).** This is a dated 2 November validation note, not the live submission authority. For current owner-approved **34 E / 15 P / 1 G**, see [the master register](CONTROL_EVIDENCE_REGISTER.md) and [evidence scope/provenance addendum](CONTROL_EVIDENCE_SCOPE_20261105.md). Earlier Google Workspace, PAM, DR, technical-completion and checksum statements are historical snapshots only and must not override the revised scope; ValueFirst acceptance is still pending.

---


**Audience:** ValueFirst/Tanla security reviewers and Pingplus Media compliance owners.
**Scope:** Internal operator evidence pack published on 2 November 2026, not an independent security audit or ValueFirst acceptance. 
**Source:** Public evidence commit `54b955ec047eb0a00dabd5cb8f8b827097fba61d`; these notes qualify the evidence, not the service.

## Technical evidence and accepted limits

| Control | Positive evidence | Scope not yet established |
|---|---|---|
| 11 & 20 | Inbound plaintext SMPP 2775 restricted to approved source addresses under default-deny UFW | This is IP access restriction, **not** encryption of SMPP PDUs |
| 17 | Wazuh and Fail2ban are operational; 14/14 Go/Fiber middleware qualification cases passed | These tests execute a **Go mirror** of the QA Nginx regexes. They do not demonstrate actual Nginx request processing or a production WAF installation. No production change is claimed |
| 22 | Offsite repo2 backup/WAL is encrypted with AES-256-CBC | Live message database volume and local repo1 are unencrypted in the tested storage layers. SHA-256 content hashes and redaction are **not encryption at rest**. The live/local gap requires a risk treatment or remediation plan |
| 24 | Detailed production logs and DLR records exist; Campaign 16 was read-only inspected | Aggregate `message_settlements` returned zero rows with `sell_price=0`, while the report describes 279 synthetic records with zero-value partial settlements. A **same-message-ID join with actual settlement rows** is needed to reconcile these two statements before presenting a confirmed root cause |
| 45 & 46 | Isolated encrypted DB restoration 2m48s, PostgreSQL startup/check 34s, separate application health cold-start 1.85s, reported WAL lag under 60s | This does **not** prove end-to-end carrier rebind, queue/worker replay, customer routing, DNS or alternate-DC failover. RTO/RPO achievement should be described as **isolated recovery scope** |
| 47 | SPF, DKIM public-key TXT and DMARC TXT queried | A published DKIM key does **not** verify sent messages are signed/aligned. DMARC `p=none` is monitor-only, not reject/quarantine |
| 48–50 | Google Workspace 2SV and device-management functions described; owner confirms deployment | User-by-user MFA enforcement, enrolled device counts, supported remote lock/full-device wipe and action logs require actual admin-console evidence |

## Owner-confirmed governance records, not reproduced historical source receipts

Pingplus Media's owner states that access review occurred 9 September 2026, log review occurred 9 October 2026, incident drill occurred 15 September 2026, and 24x7 in-house security/NOC monitoring uses rotating shifts. The 11 April 2026 awareness-policy/training library is available. These are documented **owner attestations**. The November 2 compiled register is **not**, by itself, the original signed September/October review, attendance roster, drill minutes or staffed on-duty alert acknowledgment. Do not describe its generated scenario details, elapsed times or conclusions as independently contemporaneously evidenced without an underlying dated record.

## Contractual/security exceptions

- Bilateral DPA is signed by Pingplus, awaiting ValueFirst countersignature; its present clause describes a 48-hour breach notice and requires agreed final treatment for the questionnaire's 24-hour example.
- Full privileged-access approval PAM/JIT workflow is not deployed; retained SSH/sudo permissions are an owner-approved operating exception.
- Primary and local message/archive encryption limitations and plaintext SMPP interconnect exceptions must not disappear under a blanket `P` tally.
- Internal VAPT findings are verified within their stated QA scopes; the absence of high/critical items in that ledger does not guarantee the absence of untested production vulnerabilities.

## Public distribution safety

The original public Git commit contains infrastructure/peer IP addresses, even though later HEAD redacted them. Git history was **not** force-rewritten. This repository must not be represented as historically scrubbed until a clean-root republication is completed and verified. The current `checksums.sha256` belongs to the 2 November evidence snapshot at its publication state and must be regenerated after additional document edits.

**No production system, carrier connection, server control, SMS message, billing record or SSH/sudo setting was changed in producing these validation notes.**
