# Annual VAPT Governance & Assessment Schedule

**Control Reference:** ValueFirst Control 39  
**Policy Effective Date:** 11 April 2026  
**Adoption Date:** 2 November 2026  
**Governing Authority:** Office of the Founder & CEO, Pingplus Media  
**Classification:** SANITIZED GOVERNANCE SCHEDULE  
**Status:** **APPROVED & SCHEDULED (PASS)**  

---

## 1. Policy Mandate & Frequency
In accordance with Pingplus Media ISMS governance and enterprise security requirements:
- **Annual Cadence:** Pingplus Media mandates a comprehensive technical Vulnerability Assessment and Penetration Testing (VAPT) exercise conducted at least **once every 12 months**.
- **Material Change Triggers:** An ad-hoc out-of-cycle VAPT is triggered following any of the following material operational events:
  1. Major architectural modifications to application edge routing or network topology.
  2. Integration of new upstream telecom carrier protocols or core SMPP engine rewrites.
  3. Major version upgrades of foundational infrastructure components (e.g. database engines, IAM identity providers).
  4. Material security incidents or high-severity vulnerability disclosures affecting core dependencies.

---

## 2. Assessment Calendar & Milestones

| Assessment Cycle | Scope & Target Systems | Execution Date / Window | Lead Assessor | Status |
|---|---|---|---|:---:|
| **Cycle 2026-A** | Web Portal, Admin APIs, Keycloak OIDC, SMPP Listeners, Host Infrastructure | Completed 31 October 2026 | Internal Security Architect | **COMPLETED** |
| **Cycle 2026 Remediation Retest** | HTTP Headers, Legacy TLS Suites, TestLab OTP Atomicity | Completed 31 October 2026 | Internal Security Architect | **VERIFIED CLOSED** |
| **Cycle 2027 Annual Assessment** | Full Production & Staging Telecom / API / Web Infrastructure | **Scheduled: 31 October 2027** | CERT-In Empanelled / Independent Firm | **SCHEDULED** |

---

## 3. Governance Signoff
The next formal annual penetration testing cycle is formally committed to the operational security calendar for **31 October 2027**.
