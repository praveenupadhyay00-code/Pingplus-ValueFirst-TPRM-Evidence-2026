# Pingplus Media — ValueFirst TPRM Public Evidence Repository (2026)

**Repository Reference:** `praveenupadhyay00-code/Pingplus-ValueFirst-TPRM-Evidence-2026`  
**Execution & Packaging Date:** 2 November 2026  
**Auditor / Client:** ValueFirst Digital Media Pvt. Ltd. / Tanla Platforms Ltd.  
**System Evaluated:** Ping Pulse Messaging Platform  
**Target Hosts:** `pingpulse-app` (`[APP_HOST_ADDRESS_REDACTED]`) & `pingpulse-db` (`[DB_HOST_ADDRESS_REDACTED]`)  
**Certification Standard:** ISO/IEC 27001:2022 (Certificate `305026090945IS`, QRO Certification LLP)  
**Classification:** SANITIZED PUBLIC COMPLIANCE EVIDENCE  

---

## 1. Overview & Purpose

This repository serves as the official sanitized public evidence repository for Pingplus Media's **Third-Party Risk Management (TPRM) Security Assessment** conducted for ValueFirst / Tanla. 

It organizes and presents verified technical test outputs, configuration extracts, policy links, and governance documentation corresponding to all **50 controls** of the ValueFirst Aggregator Security Checklist.

### Anti-Fabrication & Strict Transparency Policy
In strict compliance with TPRM integrity rules:
- **Zero Inventions:** No external contracts, signed third-party audit reports, or SOC agreements are claimed unless verified artifacts exist.
- **Explicit Scope Boundaries:** Distinctions between production enforcement, isolated QA validation, owner attestation, and operational exceptions are explicitly stated.
- **Zero Secrets / Zero PII:** Proprietary source code, customer message payloads, subscriber telephone numbers, cryptographic private keys, passwords, and non-public network topologies are omitted or redacted.
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
│   └── DEVICE_MANAGEMENT_AND_EMAIL_SECURITY.md        # Live DNS SPF, 2048-bit RSA DKIM, DMARC & Google Workspace MDM
└── evidence-index/
    ├── CONTROL_EVIDENCE_REGISTER.md                   # Authoritative master 50-control compliance matrix
    ├── CONTROL_CLASSIFICATION_RECONCILIATION_20261102.md # Final post-operator 21E / 27P / 2G reconciliation audit
    └── VALUEFIRST_TPRM_SUBMISSION_RECONCILIATION_SYNC.csv # 1-to-1 sync matrix for submission spreadsheet
```

---

## 3. High-Level Control Status Summary (reconciled 2 November 2026)

- **Evidenced (`E`):** 21 controls (enforced in live production, backed by technical command output or active ISO 27001 certification)
- **Partial / Exception / QA-Qualified (`P`):** 27 controls (scoped QA qualified, operating under documented operational exception, or owner-attested)
- **Genuine Gap (`G`):** 2 controls (Control 30 bilateral DPA 24h countersignature term; Control 36 formal approval-based PAM workflow)
- **Total:** 50 controls fully mapped and accounted for.


---

## 4. Verification Checksums

To verify document integrity and ensure authentic evidence provenance, SHA-256 checksums are generated for all published technical artifacts:

```bash
# Verify checksums across repository files:
find . -type f -name "*.md" -not -name "README.md" -exec sha256sum {} +
```

---

## 5. Contact & Inquiries

For questions regarding this evidence pack or to coordinate formal third-party audit walkthroughs:
- **Security & Systems Architecture:** `security@pingplusmedia.com`
- **Legal & Compliance:** `compliance@pingplusmedia.com`
- **Official Website:** `https://pingplusmedia.com/`
