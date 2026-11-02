# Risk Management Governance & Escalation Summary

**Control References:** ValueFirst Controls 6, 7, 8  
**Effective Date:** 11 April 2026  
**Governing Standard:** ISO/IEC 27001:2022 (Clause 6.1 & 8.2)  
**Governing Authority:** Office of the Founder & CEO, Pingplus Media  
**Classification:** SANITIZED GOVERNANCE SUMMARY  
**Result Status:** **CONTROLS 6, 7 (PARTIAL) / CONTROL 8 (EVIDENCED)**  

---

## 1. Risk Management Framework
Pingplus Media operates an ongoing Information Security Risk Assessment methodology aligned with ISO/IEC 27001:2022:
1. **Threat & Vulnerability Identification:** Regular reviews of infrastructure assets, software dependencies, and supplier integrations.
2. **Impact & Likelihood Scoring:** Qualitative and quantitative scoring of candidate risks across operational, financial, legal, and reputational domains.
3. **Risk Treatment Plans:** Defined options for risk mitigation, transfer, avoidance, or formal management acceptance.
4. **Periodic Review:** Quarterly risk treatment review cycle committed under ISMS policy.

---

## 2. Technical Candidate Risk Register (Seeded 2026)

| Risk ID | Observed Technical Condition | Potential Impact | Treatment Strategy | Status |
|---|---|---|---|:---:|
| **RISK-001** | Primary PostgreSQL storage shares underlying software RAID volume | Local disk failure requires remote restore | Continuous remote streaming WAL and daily diff backups to Hetzner Storage Box | **MITIGATED** |
| **RISK-002** | Remote backup restore tested in isolated lab, not full alternate DC | Failover duration in catastrophic DC loss dependent on provisioning | Documented BCP runbooks; staged alternate DC planning | **ACCEPTED EXCEPTION** |
| **RISK-003** | Local pgBackRest repo1 cipher configured as `none` | Local backups depend on host filesystem security | Restricted permissions (`0600`); remote repo2 encrypted with `aes-256-cbc` | **MITIGATED** |
| **RISK-004** | Plaintext SMPP v3.4 on port 2775 required for carrier compatibility | Eavesdropping if exposed publicly | Gated behind UFW default-deny and 3 specific customer IP allowlist rules | **MITIGATED** |
| **RISK-005** | Internal VAPT completed; formal external certificate pending | ValueFirst contractual assurance requirement | Annual VAPT scheduled; internal findings resolved | **MANAGED** |
| **RISK-006** | In-house 24x7 NOC operational without third-party external SOC | Third-party 24x7 SOC contractual requirement | 24x7x365 three-shift rota active; external SOC proposal evaluated | **MANAGED** |

---

## 3. Management Escalation Matrix (Control 8)
- **Low to Medium Risks:** Reviewed and approved by the Technical Systems Architect.
- **High to Critical Risks:** Escalated within 4 hours to the **Founder & CEO**, who serves as the sole authorized executive for formal risk acceptance and emergency resource allocation.
