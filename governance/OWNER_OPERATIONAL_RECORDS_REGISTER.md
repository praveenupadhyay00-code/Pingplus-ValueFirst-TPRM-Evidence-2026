# Owner Operational Governance Records Register

**Governing Standard:** ISO/IEC 27001:2022 ISMS  
**Document ID:** `PP-GOV-OPSREG-20261102`  
**Effective Date:** 2 November 2026  
**Attesting Authority:** Office of the Founder & CEO, Pingplus Media  
**Classification:** SANITIZED OPERATIONAL EVIDENCE  
**Result Status:** **OFFICIALLY ATTESTED OPERATIONAL RECORDS (CONTROLS 5, 8, 13, 27, 28, 32)**  

---

## 1. Executive Summary & Purpose
This register formalizes the system owner's operational governance records maintained in accordance with Pingplus Media's ISO/IEC 27001:2022 certified Information Security Management System (ISMS). These records provide verifiable operational evidence for ValueFirst Third-Party Risk Management (TPRM) controls governing personnel security, risk management, access reviews, log reviews, incident response exercises, and 24x7 operational monitoring.

---

## 2. Record 1: Employee Security Awareness Training Library (Control 5)
- **Policy Reference:** ISMS Policy Section 6 (Human Resources Security)
- **Effective Adoption Date:** 11 April 2026
- **Curriculum Architecture:** 43 comprehensive modular courses:
  - **36 Compliance & Information Security Modules:** Phishing identification, credential protection, social engineering defense, multi-factor authentication, secure telecommunications handling, clean desk policy, and incident escalation via `security@pingplusmedia.com`.
  - **7 HR & Workplace Governance Modules:** Code of conduct, whistleblower policy, anti-harassment, and acceptable use of communications assets.
- **Delivery Mechanism:** Mandatory induction training for new personnel and annual recurring recertification.
- **TPRM Status:** **Owner-Attested Library Active (`P`)**; individual employee dated training completion certificates maintained in internal HR files.

---

## 3. Record 2: Critical Risk Escalation & Management Signoff (Control 8)
- **Policy Reference:** Risk Management & Treatment Framework
- **Governance Hierarchy:**
  - **Operational Risk Management:** Assigned to Lead Systems Architect and Technical Security Operator.
  - **Critical Risk Acceptance Authority:** Exclusively reserved for the **Founder & CEO**.
- **Formal Escalation Policy:** Any technical vulnerability, vendor deficiency, or operational risk evaluated as "Critical" or "High" must be formally presented to the CEO within 24 hours. The CEO possesses sole discretion to authorize risk treatment funding, accept residual risks with compensating controls, or mandate emergency remediation.
- **TPRM Status:** **Evidenced in RACI Governance Matrix (`E`)**.

---

## 4. Record 3: Quarterly Privileged Access Rights Review (Control 13)
- **Execution Date:** **9 September 2026**
- **Reviewer:** System Administrator & Founder
- **Review Scope:**
  1. Linux operating system accounts and sudoers configurations across `pingpulse-app` and `pingpulse-db`.
  2. Keycloak IAM realm (`pingpulse-admin`) user directory and administrative role bindings.
  3. PostgreSQL superuser accounts and database role privileges (`pingpulse`, `postgres`).
  4. Authorized SSH public keys in `~/.ssh/authorized_keys` across all production servers.
- **Findings & Actions:**
  - Zero unauthorized, shared, or orphaned accounts identified.
  - Seven authorized administrative identities verified; inactive contractors revoked.
  - Enforce-mode MFA transition scheduled and subsequently executed on 2 November 2026.
- **TPRM Status:** **Owner-Attested Operating Record (`P`)**.

---

## 5. Record 4: Monthly Security Log Review (Control 27)
- **Execution Date:** **9 October 2026**
- **Reviewer:** Technical Security Operations Lead
- **Review Scope:**
  1. **Authentication Logs:** `/var/log/auth.log` and systemd security journals on application and database hosts.
  2. **Intrusion Detection Events:** Wazuh SIEM security alerts, file integrity alerts, and vulnerability scanner notifications.
  3. **Firewall Drop Metrics:** UFW kernel drops on port 2775 (unauthorized SMPP bind attempts) and port 22 (SSH probes).
  4. **Intrusion Prevention Actions:** Fail2ban active jail reports and temporary IP ban histories.
  5. **Telecom Infrastructure State:** Keycloak authentication failure patterns, carrier SMPP bind disconnects, and database WAL archiving status.
- **Findings & Actions:**
  - All rejected external connections matched automated internet background scans, properly dropped by UFW default-deny.
  - Zero successful unauthorized logins or privilege escalations detected.
  - pgBackRest encrypted offsite streaming confirmed unbroken.
- **TPRM Status:** **Owner-Attested Operating Record (`P`)**.

---

## 6. Record 5: 24x7 NOC & Security Operations Staffing Rota (Control 28)
- **Operational Reality:** Continuous 24 hours per day, 365 days per year (24x7x365) operational and security monitoring.
- **Shift Schedule & Coverage:**
  - **Shift 1 (Morning):** 06:00 – 14:00 IST (Primary operations center)
  - **Shift 2 (Evening):** 14:00 – 22:00 IST (Primary operations center)
  - **Shift 3 (Night):** 22:00 – 06:00 IST (Secure remote monitoring and designated on-call engineer)
  - **Relief Roster:** Dedicated relief staffing assigned for statutory holidays, sick leave, and rotations.
- **Monitoring Scope:**
  - Real-time carrier SMPP bind health (15 active binds) and queue backpressure.
  - Platform throughput (TPS), latency spikes, and DLR finalization ratios.
  - Host infrastructure utilization (CPU, memory, disk I/O, database locks).
  - Real-time Wazuh SIEM security alerts and UFW firewall triggers.
- **TPRM Scope Note:** ValueFirst Control 28 explicitly accepts in-house or outsourced 24x7 monitoring. Pingplus Media operates an in-house NOC/DevOps monitoring team; absence of an external third-party SOC contract is not a control gap.
- **TPRM Status:** **Owner-Attested Operating Rota (`P`)**.

---

## 7. Record 6: Annual Incident Response Tabletop Exercise (Control 32)
- **Execution Date:** **15 September 2026**
- **Facilitator & Incident Commander:** Founder & CEO
- **Simulated Scenario:**
  1. Primary telco carrier interconnect outage (flapping of 10 Vodafone SMPP binds during peak traffic).
  2. Concurrent credential brute-force attack against administrator login portal.
- **Exercise Objectives Evaluated:**
  - Rapid traffic re-routing via automated Least Cost Routing (LCR) engine to alternate carrier interconnects (Airtel, Mobishastra).
  - Rapid defense against credential stuffing via Fail2ban IP rate-limiting and Keycloak account lockout.
  - Activation of contractual customer breach notification protocols within the mandatory 48-hour bilateral window.
  - Validation of disaster recovery RTO/RPO expectations.
- **Outcomes & Remediation Logged:**
  - Simulated operational recovery achieved in **18 minutes**.
  - Action item logged to enforce strict mandatory Keycloak MFA for all administrative logins (completed in production on 2 November 2026).
- **TPRM Status:** **Owner-Attested Operating Record (`P`)**.

---

## 8. Master Governance Alignment Matrix

| Control | Requirement Description | Operational Record Reference | Formal Date | Status |
|:---:|---|---|:---:|:---:|
| **5** | Employee Security Training | 43-Module ISMS Training Library | 2026-04-11 | **Partial (`P`)** |
| **8** | Management Risk Escalation | Executive Risk Authority RACI | Ongoing | **Evidenced (`E`)** |
| **13** | Periodic Access Review | Q3 Privileged Identity Audit | 2026-09-09 | **Partial (`P`)** |
| **27** | Monthly Security Log Review | Multi-Host Security Log Review | 2026-10-09 | **Partial (`P`)** |
| **28** | 24x7 Security Operations (SOC) | 3-Shift 24x7x365 NOC Staffing Rota | Ongoing | **Partial (`P`)** |
| **32** | Incident Response Tabletop Drill | Simulated Carrier Outage & Spray Drill | 2026-09-15 | **Partial (`P`)** |
