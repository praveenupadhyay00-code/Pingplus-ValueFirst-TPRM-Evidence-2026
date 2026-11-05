# Corporate Email Authentication and Managed Device Evidence

**Controls:** ValueFirst 47–50  
**Owner assessment cutoff:** 5 November 2026  
**Classification:** Sanitized owner-controlled technical and policy evidence  
**Current owner grades:** **47 P · 48 P · 49 E · 50 E**  
**Scope warning:** Owner-submitted records and operator-reported checks are not a current independent DNS/MDM audit or ValueFirst sign-off.

## 47. SPF, DKIM and DMARC — Partial (P)

Owner and operator records report SPF alignment and a 2048-bit RSA DKIM key for the corporate domain. Previously recorded DMARC policy is `v=DMARC1; p=none;`, meaning **monitoring, not rejection/quarantine enforcement**. A proposed staged quarantine/reject rollout is a **plan only**; no live DNS changes were authorized by this TPRM review.

A current DNS query, sender inventory, aggregate report analysis, and separately approved rollback process are required before increasing DMARC enforcement. DNS evidence from 2 November is a dated record, not a claim of continuous live verification on 5 November.

## 48. Corporate email MFA and transport — Partial (P)

- **Actual corporate mailbox platform:** **cPanel/WHM**, with Dovecot/IMAP and SMTP. Do not describe these mailboxes as Google Workspace Enterprise.
- **Webmail:** TOTP 2FA reported as enabled for Webmail access.
- **Outlook desktop:** Standard IMAP/SMTP TLS clients still use mailbox passwords. **Webmail TOTP does not extend to Outlook IMAP password authentication**, and direct IMAP MFA must not be claimed.
- **Transport safeguards:** Operator report describes modern TLS for mail transport and cPHulk brute-force protection. This does not replace per-protocol MFA.
- **Continuity:** No mailbox hosting migration, password rotation, Outlook disruption or DNS/MX cutover was undertaken.

## 49. Mobile Device Management — Owner-evidenced (E)

The current owner-approved evidence identifies **ManageEngine Cloud MDM**, not Google Workspace Endpoint Management. It records one managed, compliant Android work-profile enrollment with Play Protect. Classification E represents owner acceptance of the documented deployed MDM capability. It does **not** attest universal management of every company or BYOD device. Restricted device inventory and enrollment screenshots remain owner-controlled.

## 50. Remote management, lock and work-data removal — Owner-evidenced (E)

Owner records state that a **ManageEngine Remote Lock** operation was performed and audited. Selective work-profile removal and corporate account/session revocation are available. No destructive wipe of a personal device was performed or is claimed. The restricted management-console receipt is available for authorized review, not public publication.

## Boundary and submission position

| Control | Owner grade | Evidence scope | Open limitation |
|---|:---:|---|---|
| 47 | **P** | SPF/DKIM records and DMARC staged policy design | DMARC remains `p=none`; no production enforcement |
| 48 | **P** | cPanel Webmail MFA and mail TLS safeguards | Outlook IMAP password authentication does not enforce MFA |
| 49 | **E** | Owner-evidenced ManageEngine managed Android profile | No universal fleet coverage claim |
| 50 | **E** | Owner-evidenced remote lock and selective corporate data removal | No personal-device full wipe claim |

**Authority:** 5 November owner-approved submission matrix (restricted owner-held workbook, not published here). This is the owner self-assessment scope, subject to ValueFirst/Tanla's independent review. Previous 2 November Google Workspace claims are superseded and should not be reused.
