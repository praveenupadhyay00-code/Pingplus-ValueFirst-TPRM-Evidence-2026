# Pingplus Media — ValueFirst TPRM Public Evidence Repository (2026)

**Repository Reference:** `praveenupadhyay00-code/Pingplus-ValueFirst-TPRM-Evidence-2026`  
**Initial packaging:** 2 November 2026  
**Owner classification reconciliation:** 5 November 2026  
**Intended external reviewer:** ValueFirst Digital Media Pvt. Ltd. / Tanla Platforms Ltd. (acceptance pending)  
**System Evaluated:** Ping Pulse Messaging Platform  
**System scope:** Application, database, messaging gateways, security controls and documented recovery arrangements (no private host inventory published)  
**Certification Standard:** ISO/IEC 27001:2022 (Certificate `305026090945IS`, QRO Certification LLP)  
**Classification:** PUBLIC, SCOPE-QUALIFIED SELF-ASSESSMENT — see publication safety notice for historical Git risks  

---

## 1. Overview & Purpose

This repository is Pingplus Media's **owner-controlled TPRM submission evidence index** prepared for review by ValueFirst / Tanla. It is not an audit performed or approved by those organizations. 

It indexes documented policies, owner attestations, scoped QA receipts and selected production technical records for the checklist's **50 controls**. Distinguish operator-reported work from independently verified artifacts and from live enforcement.

### Anti-Fabrication & Strict Transparency Policy
In strict compliance with TPRM integrity rules:
- **Zero Inventions:** No external contracts, signed third-party audit reports, or SOC agreements are claimed unless verified artifacts exist.
- **Explicit Scope Boundaries:** Distinctions between production enforcement, isolated QA validation, owner attestation, and operational exceptions are explicitly stated.
- **Public safety:** Do not publish credentials, customer message data, private network details or carrier contracts. Current edits do not clean earlier public Git history; see [publication safety notice](PUBLICATION_SAFETY_NOTICE.md).
- **Restricted Vault Reference:** Original signed documents (e.g. GST registration certificate, full ISO 27001 certificate PDF, commercial carrier agreements) are maintained in an access-controlled private vault and made available to authorized auditors upon request.

---

## 2. Evidence Architecture & Directory Layout

The repository is structured into modular compliance domains:

```text
├── README.md                                          # Master repository overview and security attestations
├── policies/
│   ├── CONTROL_POLICY_INDEX.md                        # Master mapping of all 50 controls to live published policies
│   ├── INFORMATION_SECURITY_POLICY_SUMMARY.md         # ISO/IEC 27001:2022 ISMS policy summary
│   ├── INCIDENT_RESPONSE_POLICY_SUMMARY.md            # Incident response lifecycle, reporting SLA & RACI
│   ├── BUSINESS_CONTINUITY_POLICY_SUMMARY.md          # BCP/DR targets (RTO < 4h, RPO < 15m) & recovery architecture
│   └── DATA_RETENTION_POLICY_SUMMARY.md              # 180-day CDR retention & automated data purging controls
├── network-security/
│   ├── SMPP_PORT_2775_ALLOWLIST_VERIFICATION.md       # Inbound firewall default-deny & 3-IP customer allowlist
│   ├── TRANSPORT_SECURITY_AND_TLS_VERIFICATION.md     # Safe TLS 1.3/1.2 negotiation & peer transport matrix
│   └── NETWORK_FIREWALL_BASELINE.md                   # Host UFW baseline & private VLAN database isolation
├── identity-access/
│   ├── PRODUCTION_ADMIN_MFA_ENFORCEMENT_EVIDENCE.md   # Keycloak OIDC TOTP enforcement & negative test rejection
│   ├── PASSWORD_POLICY_AND_REVOCATION_CONTROLS.md     # Complexity, brute-force lockout & immediate session revocation
│   └── PRIVILEGED_ACCESS_MANAGEMENT_EVALUATION.md     # Formal PAM evaluation (Teleport / Boundary) & sudo exception
├── monitoring-soc/
│   ├── WAZUH_SIEM_AND_AGENT_VERIFICATION.md           # Wazuh SIEM manager/agent verification & live security alerts
│   ├── WAF_QA_QUALIFICATION_REPORT.md                 # Isolated QA WAF qualification report (OWASP Top 10 injection mitigation)
│   ├── FAIL2BAN_INTRUSION_PREVENTION_STATUS.md        # Active host IPS status (16,917 blocked / 1,052 banned)
│   └── NOC_24X7_MONITORING_ATTESTATION.md             # Owner attestation of in-house 24x7x365 rotating three-shift NOC
├── logging-and-audit/
│   ├── DETAILED_DELIVERY_AND_CDR_AUDIT_LOGGING.md     # Message lifecycle, status transitions & routing audit trails
│   ├── CAMPAIGN_AND_SETTLEMENT_DATA_ANALYSIS.md       # Transparent reconciliation of Campaign 16 (279 msgs) & billing
│   └── LOG_RETENTION_AND_TAMPER_PROTECTION.md         # Journald, logrotate & remote streaming WAL protections
├── backup-and-dr/
│   ├── PGBACKREST_ENCRYPTED_BACKUP_STATUS.md          # Stanza status, AES-256-CBC remote encryption & daily diffs
│   ├── LIVE_MESSAGE_ARCHIVE_ENCRYPTION_ASSESSMENT.md  # Comprehensive primary storage, hashing, repo2 & repo1 exception audit
│   └── ISOLATED_RECOVERY_REHEARSAL_AND_RTO_RPO.md     # Out-of-place restore & full app cold-start rehearsal (<4m RTO, <1m RPO)
├── vapt/
│   ├── INTERNAL_VAPT_SUMMARY_AND_FINDINGS_LEDGER.md   # Internal technical VAPT findings (5 closed in QA, 0 high/critical)
│   └── ANNUAL_VAPT_POLICY_AND_SCHEDULE.md             # Policy commit for annual VAPT (next cycle: 31 October 2027)
├── governance/
│   ├── ISO_27001_CERTIFICATE_VERIFICATION.md          # QRO Cert 305026090945IS & IAF CertSearch active verification
│   ├── OWNER_OPERATIONAL_RECORDS_REGISTER.md          # Attested training, access review, log review, drill & NOC rota
│   ├── RISK_MANAGEMENT_AND_ESCALATION_SUMMARY.md      # Technical candidate risk register & CEO escalation framework
│   ├── THIRD_PARTY_DPA_AND_BREACH_NOTICE_TERMS.md     # Bilateral DPA status, 48h breach clause & carrier interconnects
│   └── DEVICE_MANAGEMENT_AND_EMAIL_SECURITY.md        # cPanel email, monitored DMARC, ManageEngine Cloud MDM
└── evidence-index/
    ├── CONTROL_EVIDENCE_REGISTER.md                   # Owner-approved 5 Nov 34E / 15P / 1G matrix
    ├── CONTROL_CLASSIFICATION_RECONCILIATION_20261102.md # Historical 2 Nov 21E / 27P / 2G assessment
    ├── VALUEFIRST_TPRM_SUBMISSION_RECONCILIATION_SYNC.csv # 5 Nov 50-row owner-grade sync
    └── CONTROL_EVIDENCE_SCOPE_20261105.md            # QA vs production scope and evidence provenance
```

---

## 3. Current owner-approved control status (5 November 2026)

- **Owner-evidenced (`E`):** **34** controls. Includes owner-accepted policies, governance records and technical evidence; this is **not** synonymous with live technical enforcement or external approval.
- **Partial / Exception (`P`):** **15** controls. Includes safely isolated QA/design work and remaining deployment/verification exceptions. Control 36 (delegated PAM workflow) is **P**, not G.
- **Contractual Gap (`G`):** **1** control: #30, the bilateral breach notification SLA (48h signed draft vs the questionnaire's 24h reference; ValueFirst countersignature pending). Control #2 remains **P** pending counterpart execution.
- **Total:** **50**, aligned with the live owner-approved submission workbook (restricted owner-held workbook, not published here).

Read the [current register](evidence-index/CONTROL_EVIDENCE_REGISTER.md) and [5 November scope/provenance addendum](evidence-index/CONTROL_EVIDENCE_SCOPE_20261105.md) before relying on any older report. Historical documents remain available for audit trail but do not override the current classifications. The underlying approved policy PDFs have not been changed.


---

## 4. Integrity and distribution safety

The original `checksums.sha256` is a **historical** 2 November manifest and must not be used as a current-pack receipt. The updated artifact set is enumerated separately in `checksums-20261105.sha256` (SHA-256 of each current file except the manifest itself and the historical manifest); verify with `sha256sum -c checksums-20261105.sha256` after fetching the exact repository version. A checksum proves byte integrity, not operational truth or Git-history privacy.

Earlier public Git history may still contain unredacted details. A current HEAD update does **not** purge historical blobs; see [PUBLICATION_SAFETY_NOTICE.md](PUBLICATION_SAFETY_NOTICE.md). New local-only operator receipts must not be represented as independently published or verified until the relevant commit is accessible.

---

## 5. Contact & Inquiries

For questions regarding this evidence pack or to coordinate formal third-party audit walkthroughs:
- **Security & Systems Architecture:** `security@pingplusmedia.com`
- **Legal & Compliance:** `compliance@pingplusmedia.com`
- **Official Website:** `https://pingplusmedia.com/`
