# Third-Party DPA, Carrier Chain & Breach Notification Terms

**Control References:** ValueFirst Controls 2, 30, 41, 42, 43  
**Review Date:** 2 November 2026  
**Governing Authority:** Legal & Compliance Office, Pingplus Media  
**Classification:** SANITIZED CONTRACTUAL GOVERNANCE SUMMARY  
**Result Status:** **CONTROLS 41, 43 (PASS) / CONTROLS 2, 30, 42 (PARTIAL)**  

---

## 1. Bilateral Data Processing Agreement (DPA) Status (Control 2, 30)
- **Document Status:** Pingplus Media has executed the formal Data Processing Agreement and Security Schedule, and delivered the agreement for **bilateral countersignature by ValueFirst / Tanla**.
- **Breach Notification SLA Clause:** The current drafted DPA specifies customer notification **within 48 hours** following confirmation of a security incident affecting customer data. 
- *TPRM Transparency Note:* In adherence to anti-fabrication guidelines, Pingplus Media explicitly clarifies that the contractual notification clause currently specifies 48 hours rather than a 24-hour obligation, pending final bilateral execution.

---

## 2. Upstream Telecom Carrier & Aggregator Chain (Control 41)
Pingplus Media maintains direct, contractually bound interconnects with accredited telecommunication operators and aggregators:

| Upstream Partner | Connection Type | Active Binds | Interconnect Endpoint | Protocol |
|---|---|:---:|---|---|
| **Vodafone Idea Ltd** | Direct Telecom Carrier Gateway | 10 Binds | Dedicated Carrier Port 2777 | SMPP v3.4 over Private Network |
| **Bharti Airtel Ltd** | Direct Telecom Carrier Gateway | 2 Binds | Dedicated Carrier Port 2778 | SMPP v3.4 over Private Network |
| **Mobishastra Technologies** | Direct SMS Aggregator Gateway | 2 Binds | Dedicated Host Port 18849 | SMPP v3.4 over Private Network |

---

## 3. Subprocessor Security & Data Protection Clauses (Control 43)
- **Subprocessor Register:** Publicly disclosed at `https://pingplusmedia.com/subprocessors/`, listing infrastructure hosts (Hetzner Online GmbH) and telecommunication partners.
- **Mandatory Security Clauses:** All subprocessors and carriers operate under enterprise service agreements incorporating strict confidentiality, data protection, and operational security requirements.
