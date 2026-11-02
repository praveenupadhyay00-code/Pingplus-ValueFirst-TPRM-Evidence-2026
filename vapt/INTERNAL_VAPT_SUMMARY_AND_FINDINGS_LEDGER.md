# Internal Technical VAPT Summary & Findings Ledger

**Control References:** ValueFirst Controls 38, 40  
**Assessment Date:** 31 October 2026  
**Assessment Scope:** Web Portal, Admin APIs, Keycloak Identity Service, SMPP Listeners, Host OS Network Ports  
**Classification:** SANITIZED TECHNICAL SECURITY REPORT  
**Result Status:** **INTERNAL ASSESSMENT COMPLETED (ZERO HIGH/CRITICAL FINDINGS REMAIN)**  

---

## 1. Executive Summary & Control Requirements
- **Control 38 (Independent or Internal VAPT):** Conduct regular internal or independent Vulnerability Assessment and Penetration Testing. (The questionnaire explicitly permits an internal assessment).
- **Control 40 (VAPT Closure & Reporting):** Track identified vulnerabilities through remediation, retesting, and formal closure.

On 31 October 2026, Pingplus Media executed a comprehensive internal technical security assessment across isolated QA and staging infrastructure. The assessment evaluated network attack surfaces, TLS configurations, web application security headers, identity protocols, and SMPP messaging listeners using automated scanners (Nmap, Nuclei, testssl.sh) and manual inspection.

---

## 2. Findings & Retest Ledger

| Finding ID | Title & Vulnerability Summary | Severity | Remediation Action | Retest Result | Status |
|---|---|:---:|---|---|:---:|
| **VF-VAPT-001** | Missing required HTTP security headers (HSTS, X-Content-Type-Options, X-Frame-Options) | Low | Configured `always` header directives in Nginx reverse proxy configuration. | Retested with curl and Nuclei; headers confirmed present on all responses. | **REMEDIATED & CLOSED** |
| **VF-VAPT-002** | Legacy CBC cipher suites offered by web TLS configuration | Medium | Hardened Nginx TLS cipher string to restrict TLS 1.2 to modern ECDHE AES-GCM and ChaCha20 suites; TLS 1.3 enabled. | Retested via openssl s_client and testssl; legacy CBC suites rejected. | **REMEDIATED & CLOSED** |
| **VF-VAPT-003** | Content Security Policy (CSP) & cross-origin isolation headers | Informational | Resource origins and frame requirements inventoried. Policy adoption pending workflow impact review. | Pending formal policy rollout to avoid breaking Keycloak redirects. | **OPEN (POLICY DECISION)** |
| **VF-VAPT-004** | Historical SMPP TLS listener port documentation mapping | Evidence Gap | Corrected test target documentation to reflect maintained port `2778`. | Retested on port `2778`; authenticated TLS 1.3 bind verified. | **CLOSED** |
| **VF-VAPT-005** | Private QA CA certificate trust path on testing workstations | Operational | Trusted verified root CA on assessor workstations without disabling browser security warnings. | Clean browser verification across Chrome, Safari, and Brave. | **CLOSED** |
| **VF-VAPT-006** | TestLab synthetic data retention and OTP first-use atomicity | Medium | Added conditional first-use consumption and atomic verifier scrubbing in repository layer; clamped retention window. | Disposable PostgreSQL concurrency test suite passed 100%. | **REMEDIATED & CLOSED** |
| **VF-VAPT-007** | Independent third-party / CERT-In accredited audit certification | Governance | Internal assessment completed and documented. Independent external auditor proposal prepared. | Awaiting ValueFirst confirmation on whether external report is mandatory. | **OPEN (EXTERNAL ASSURANCE)** |

---

## 3. Vulnerability Closure Accounting
- **Remediated & Verified Closed:** 5 findings (VF-VAPT-001, VF-VAPT-002, VF-VAPT-004, VF-VAPT-005, VF-VAPT-006).
- **Open Informational Policy:** 1 item (VF-VAPT-003, Content Security Policy).
- **Open External Assurance Dependency:** 1 item (VF-VAPT-007, Third-party certification).
- **Remaining Critical / High Severity Vulnerabilities:** **ZERO (0)**.
