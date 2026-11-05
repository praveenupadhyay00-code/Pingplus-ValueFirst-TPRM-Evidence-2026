# ValueFirst TPRM — 5 November 2026 Owner Classification and Evidence-Scope Addendum

**Document:** PP-TPRM-SCOPE-20261105  
**Assessment date:** 5 November 2026  
**Entity:** Pingplus Media  
**Basis:** Owner-approved 50-control submission workbook, prior public evidence register, and operator-supplied corrective QA handoff  
**Authoritative grades:** **34 owner-evidenced (E) / 15 partial (P) / 1 contractual gap (G)**  
**External status:** This is an owner-controlled self-assessment. ValueFirst/Tanla acceptance has **not** been received.

## 1. Source-of-truth reconciliation

The [owner's live submission workbook](https://docs.google.com/spreadsheets/d/1_So-Qp9-_K5gDrytB2bsZcn-Z3qtmZSM-uXokVFmW-A/edit) is the classification authority as of 5 November. The earlier public 2 November matrix (21 E / 27 P / 2 G) is a historical snapshot; it must not be presented as the current submission grade.

The fourteen grade changes versus that historical snapshot are:
- P to E, on owner-accepted policy, paperwork or operational evidence: **6, 7, 13, 27, 28, 32, 33, 35, 42, 43, 44, 49, 50**.
- G to P, on documented delegated-access design and isolated application-path tests, while production PAM coverage remains incomplete: **36**.
- **30 remains G**: the owner-signed DPA is not countersigned by the counterparty and its 48-hour notice wording differs from the questionnaire's 24-hour reference. **2 remains P** pending the contractual signature and schedule.

E is an **owner evidence disposition**, not a claim that every supporting technical activity was implemented on the production platform. Previously approved PDFs and restricted governing documents are unchanged.

## 2. New operator-reported QA work: actual scope

The following statements summarize the 5 November corrective QA handoff. They have not been independently reproduced from an accessible, published source commit or its native execution artifacts. They are **supplemental technical context**, not new evidence that silently upgrades a partial control.

| Control | Operator-reported work | Qualification boundary |
|---|---|---|
| 21, 22, 37 | Isolated encrypted-block storage, PostgreSQL/WAL and random-write benchmark; synthetic restore design | No live storage migration or change to existing local/remote backup recovery paths. Local at-rest exceptions remain. |
| 23 | Content-filter regression cases and a synthetic Wazuh rule match | A rule match is not proof that a production alert reached the on-call team or an incident ticket. |
| 25 | Read-only journal integrity check and append-only/immutable test-file denial | Not proof of immutable all-source audit-log archival; offsite WAL backups are not security-log WORM storage. |
| 36 | Delegated approval checked in application-route/middleware test, including rejected replay | Real production PAM/JIT enforcement remains unproved. Owner emergency access must remain available. |
| 45, 46 | Isolated database, identity/session, queue and synthetic SMPP bind/submit/DLR exercises | Unit-test duration is **not** real secondary-datacenter RTO or RPO. No real carrier bind or live failover. |
| 47 | SPF/DKIM assessment and staged DMARC rollout design | Current DMARC is monitoring-only (`p=none`). No DNS change or enforcement claim. |
| 48 | cPanel/Webmail MFA and mail transport protections described | Desktop IMAP passwords do not inherit Webmail TOTP; corporate email remains cPanel-hosted. |

Additional owner-evidenced statuses (44, 49 and 50): the owner attests alternate-site capacity and an existing recovery plan; device management is documented as **ManageEngine Cloud MDM** with an enrolled work profile and remote lock. These statements do not establish an automated live failover, universal fleet coverage, or destructive personal-device wipe.

## 3. Repository and test provenance

The operator handoff names a local corrected commit `d68eb9c49db67ab6a4998229d3d97c497b638863`, but GitHub did not expose that object at reconciliation time. Accordingly, **do not imply its new reports were fetched, independently test-run, merged, or published**. The current remote updates are a forward-only documentation reconciliation sourced from the approved workbook, not a fabricated reconstruction of inaccessible local test receipts.

The old public Git history may still contain obsolete or sensitive records. An edited current HEAD does not sanitize historical commits. Restricted details, counterparty identities, traffic routes, binding credentials, private source logs, and signed contracts must never be included in a public evidence file. Use exact immutable remote commit hashes to identify the current file versions; the historical `checksums.sha256` manifest is not a valid current-pack integrity receipt.

## 4. Permanent operational restrictions

- **Never** reset, rotate, expire, disable, or change existing SMPP supplier or customer bind credentials as part of TPRM.
- Preserve the IP-restricted **legacy plaintext SMPP port 2775** and its active customers; maintain separate **TLS port 2778** for compatible clients.
- Never interrupt live SMS campaigns, queues, billing, carrier connections, or existing email synchronization during evidence work.
- Preserve owner-approved emergency administration; do not remove access in the name of delegated-PAM testing.
- No unapproved production LUKS conversion, archive encryption change, WAF cutover, DNS enforcement, Helsinki failover, or new infrastructure purchase.
- No internal/independent VAPT equivalence: the 31 October VAPT is an **internal** technical assessment, not a third-party/CERT-In attestation.

## 5. Submission state

The master register and companion CSV carry the owner-approved **34/15/1** classifications. Existing approved PDFs stay in their owner-controlled file locations. Final external acceptance still depends on the reviewer; the outstanding contractual item #30 requires bilateral legal resolution. Nothing in this documentation update constitutes production deployment, real disaster recovery execution, or ValueFirst sign-off.
