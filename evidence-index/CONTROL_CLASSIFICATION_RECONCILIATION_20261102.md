# ValueFirst TPRM: Final Post-Operator Classification Reconciliation — 2 November 2026

> **Historical assessment only — superseded 5 November 2026.** This document records the 2 November `21 E / 27 P / 2 G` assessment and must not be used as the current ValueFirst submission disposition. The [owner-approved current register](CONTROL_EVIDENCE_REGISTER.md) and [5 November scope addendum](CONTROL_EVIDENCE_SCOPE_20261105.md) govern the current `34 E / 15 P / 1 G` classification. Old platform references (including Google Workspace) and scope claims are retained solely for historical traceability, not current operational assertions. ValueFirst approval remains pending.

---


**Document ID:** `PP-RECON-50C-FINAL-20261102`  
**Execution Date:** 2 November 2026  
**Governing Standard:** ISO/IEC 27001:2022 ISMS  
**Assessor:** Pingplus Media Authorized Security & Compliance Operator  
**Classification:** SANITIZED MASTER RECONCILIATION AUDIT  
**Final Master Tally:** **21 Evidenced (`E`) / 27 Partial (`P`) / 2 Genuine Gaps (`G`) = 50 Controls**  

---

## 1. Classification Accounting & Tally Reconciliation

An earlier operator's initial report summary of 19 E / 23 P / 8 G did not reconcile with its 50-row matrix (21 E / 22 P / 7 G). Following systematic empirical verification and isolated QA remediation of Control 17, the master register is finalized at **21 E / 27 P / 2 G**:
- **Evidenced (`E`) — 21 Controls:** 1, 3, 4, 8, 10, 11, 12, 14, 16, 18, 19, 20, 24, 26, 29, 31, 34, 38, 39, 40, 41.
- **Partial / Exception / Owner-Attested / QA-Qualified (`P`) — 27 Controls:** 2, 5, 6, 7, 9, 13, 15, 17, 21, 22, 23, 25, 27, 28, 32, 33, 35, 37, 42, 43, 44, 45, 46, 47, 48, 49, 50.
- **Genuine Gap (`G`) — 2 Controls:** 
  - **Control 30:** Customer breach notification contractual SLA (Pingplus owner-signed DPA specifies 48 hours; awaiting ValueFirst countersignature and reconciliation with the requested 24h illustrative clause).
  - **Control 36:** Privileged Access Management (formal approval-based PAM software not deployed; broad Linux sudo and authorized SSH key access retained by explicit owner direction).

---

## 2. Core Operational & Technical Qualifications

### A. WAF Gap Remediation in Isolated QA (Control 17: `G` -> `P`)
- **Remediation:** Added application-layer WAF inspection rules to `config/nginx/pingpulse-qa-edge.conf` targeting OWASP Top 10 web injection vectors (SQLi, XSS, Path Traversal, RCE, and malicious scanner user-agents).
- **Validation:** Go regression test suite `TestQAEdgeWAFQualification` (`internal/api/waf_qa_qualification_test.go`) passed 14/14 test cases. All 11 malicious attack probes were intercepted with HTTP 403 Forbidden, while legitimate health checks, API queries, and Keycloak OIDC paths passed with HTTP 200 OK.
- **Protection of Production:** Production SMPP binds, Go API daemon, and Keycloak endpoints remain untouched. See [`monitoring-soc/WAF_QA_QUALIFICATION_REPORT.md`](../monitoring-soc/WAF_QA_QUALIFICATION_REPORT.md).

### B. Message Archive & Storage Encryption (Control 22: `P`)
- **Primary Live Storage:** 2x NVMe SSDs in Software RAID 1 (`/dev/md2` ext4) are unencrypted at the block device/LUKS layer (documented operational exception). Sensitive message text is protected via mandatory SHA-256 hashes (`content_sha256`) and PII redaction (`redaction_status = 'full'`).
- **Backup Repositories:** Remote Storage Box `repo2` is AES-256-CBC encrypted with unbroken streaming WAL archiving. Local disk `repo1` plaintext cipher is a documented operational exception for rapid crash recovery. See [`backup-and-dr/LIVE_MESSAGE_ARCHIVE_ENCRYPTION_ASSESSMENT.md`](../backup-and-dr/LIVE_MESSAGE_ARCHIVE_ENCRYPTION_ASSESSMENT.md).

### C. End-to-End Service Recovery & Attainment (Control 46: `P`)
- **Database Restoration:** Decryption and extraction of 4.5 GB database from encrypted offsite `repo2` completed in **2 minutes, 48 seconds**; PostgreSQL daemon startup and catalog check took **34 seconds**.
- **Application Runtime Recovery:** Cold-start boot of Go server binary, GORM connection pool, Redis cache, NATS JetStream, ratecards, Fiber HTTP routes, and SMPP socket listeners completed in **1.85 seconds**.
- **Total Service Recovery Time (RTO):** **3 minutes, 23.85 seconds** (exceeding policy target of < 4 hours).
- **Maximum Data Loss Window (RPO):** **< 60 seconds** via continuous WAL streaming (exceeding policy target of < 15 minutes). See [`backup-and-dr/ISOLATED_RECOVERY_REHEARSAL_AND_RTO_RPO.md`](../backup-and-dr/ISOLATED_RECOVERY_REHEARSAL_AND_RTO_RPO.md).

### D. Campaign 16 & the 279 Settlement Discrepancy (Control 24: `E`)
- **Truthful Reconciliation:** The 279 discrepancy has not been inferred as disappeared. All 279 recipients in Campaign 16 (`00000_TEXT_XX16`) were successfully delivered in `campaign_messages`.
- **Root Cause:** Executed under synthetic test tenant `00000_TEST_XX` with unconfigured rate cards ($0.00 default). The Phase 80D.2-S1 billing safety engine barred zero-sell financial intent from revenue settlement, keeping them in `partially_reconciled` status. The aggregate scan showing 0 zero-sell rows in `message_settlements` reflects finalized commercial data. See [`logging-and-audit/CAMPAIGN_AND_SETTLEMENT_DATA_ANALYSIS.md`](../logging-and-audit/CAMPAIGN_AND_SETTLEMENT_DATA_ANALYSIS.md).

### E. Email Authentication & Device Security (Controls 47–50)
- **Public DNS Verification:** SPF (`v=spf1 ... ~all`), active 2048-bit RSA DKIM key published under selector `default` at `default._domainkey.pingplusmedia.com`, and DMARC (`v=DMARC1; p=none;`) verified live.
- **Google Workspace MDM:** 2-Step Verification and Endpoint Management active with remote account wipe and session revocation; full hardware wipe available on enterprise-enrolled mobile endpoints. Owner console screenshots documented for formal fleet validation. See [`governance/DEVICE_MANAGEMENT_AND_EMAIL_SECURITY.md`](../governance/DEVICE_MANAGEMENT_AND_EMAIL_SECURITY.md).

### F. Owner Governance Operating Records (Controls 5, 8, 13, 27, 28, 32)
- Formally recorded and linked to the master register:
  - 43-module employee training library effective 11 April 2026.
  - Quarterly privileged access review completed 9 September 2026.
  - Monthly security log review completed 9 October 2026.
  - Annual incident response tabletop drill completed 15 September 2026.
  - 3-shift 24x7x365 in-house NOC & security monitoring staffing rota.
  - See [`governance/OWNER_OPERATIONAL_RECORDS_REGISTER.md`](../governance/OWNER_OPERATIONAL_RECORDS_REGISTER.md).

---

## 3. Publication Safety & History Notice

- **HEAD Sanitization:** All markdown documents in HEAD are completely redacted and sanitized of internal IP addresses and sensitive infrastructure details.
- **History Preservation:** In strict accordance with owner instructions, no forced history rewrite (`git push --force`) or remote repository deletion was executed. A clean-history candidate branch is prepared locally for future owner authorization.
- **Integrity Checksums:** Master SHA-256 manifest (`checksums.sha256`) is regenerated against the final frozen file set.
- **Production Protection:** Zero production SMS, service restart, carrier disconnection, firewall change, SSH/sudo modification, or database write occurred.
