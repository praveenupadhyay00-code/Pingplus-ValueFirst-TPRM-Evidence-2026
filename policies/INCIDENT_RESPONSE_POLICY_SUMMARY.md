# Security Incident Response & Breach Notification Policy Summary

**Document ID:** `PP-POL-IR-2026-V1`  
**Effective Date:** 11 April 2026  
**Status:** Approved and in effect  
**Dedicated Reporting Channel:** `security@pingplusmedia.com`  
**Applicability:** All security incidents affecting Pingplus Media infrastructure, customer communications, or corporate data.  

---

## 1. Incident Management Lifecycle
Pingplus Media maintains a structured 5-phase incident response methodology:
1. **Identification & Triage:** Security alerts from SIEM (Wazuh), Fail2ban, system monitors, or external reports are triaged 24x7x365 by on-call engineering.
2. **Containment:** Rapid isolation of affected services, revoking compromised sessions/keys, and implementing network-level firewall blocks without disrupting healthy traffic.
3. **Eradication:** Root cause analysis, patching exploited vulnerabilities, and hardening affected configurations.
4. **Recovery & Validation:** Controlled verification of restored systems, data integrity validation, and heightened telemetry monitoring.
5. **Post-Incident Review:** Documenting timelines, lessons learned, and updating defensive controls.

## 2. Customer Notification & SLA
- **High-Risk Data Breaches:** Pingplus Media commits to notifying affected enterprise customers without undue delay following confirmation of an unauthorized access or security incident affecting customer data.
- **Contractual SLA:** Where bilaterally agreed under executed DPAs (such as the pending ValueFirst agreement with a 48-hour notification clause), notifications are delivered via encrypted electronic mail to designated customer security contacts.

## 3. Incident RACI
- **Incident Commander:** Founder & CEO (Ultimate operational decision-maker)
- **Technical & Security Lead:** Lead DevOps & Systems Architect
- **Legal & Compliance Lead:** Grievance Officer & Corporate Counsel
- **Communications Lead:** Customer Operations Lead

*Full public text available at: https://pingplusmedia.com/incident-reporting/*
