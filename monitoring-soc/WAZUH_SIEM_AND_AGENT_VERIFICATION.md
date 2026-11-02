# Wazuh SIEM, Host EDR & Antivirus Verification

**Control References:** ValueFirst Controls 17, 26, 34  
**Execution Date:** 2 November 2026  
**Target Nodes:** `pingpulse-app` (`23.88.74.110`) & `pingpulse-db` (`23.88.20.15`)  
**Assessor:** Pingplus Media Authorized Security & Compliance Operator  
**Classification:** SANITIZED TECHNICAL EVIDENCE  
**Result Status:** **PASS (PRODUCTION EVIDENCED)**  

---

## 1. Control Requirements & Objectives
- **Control 17 (IDS/IPS and Host Security):** Deploy host-based intrusion detection and prevention across production instances.
- **Control 26 (Log Anomaly Detection):** Automate log monitoring, anomaly detection, and security alerting.
- **Control 34 (EDR/Antivirus):** Deploy host endpoint security, rootkit detection, and antivirus scanning agents.

---

## 2. SIEM & Agent Topology

Pingplus Media deploys an enterprise Wazuh SIEM and Host Intrusion Detection System (HIDS) architecture:
- **Application Server (`pingpulse-app`):** Hosts the centralized Wazuh Manager, Indexer, and Dashboard (`wazuh-manager.service`, `wazuh-indexer.service`, `wazuh-dashboard.service` all active).
- **Database Server (`pingpulse-db`):** Hosts the Wazuh Agent (`wazuh-agent.service` active), forwarding system call logs, file integrity monitoring (FIM) events, and authentication activity to the manager over the private VLAN.
- **Antivirus Coverage:** ClamAV scheduled scanner timers (`pingpulse-clamav-scan.timer` on APP and `pingpulse-db-clamav-scan.timer` on DB) run daily automated filesystem scans with automatic virus database signature updates via `clamav-freshclam`.

---

## 3. Live Systemd Unit Verification

### A. Application Server (`pingpulse-app`)
```text
wazuh-manager.service                 active (running)
wazuh-indexer.service                 active (running)
wazuh-dashboard.service               active (running)
pingpulse-clamav-scan.timer           active (waiting)
fail2ban.service                      active (running)
```

### B. Database Server (`pingpulse-db`)
```text
wazuh-agent.service                   active (running)
pingpulse-db-clamav-scan.timer        active (waiting)
auditd.service                        active (running)
fail2ban.service                      active (running)
```

---

## 4. Live Sanitized Wazuh SIEM Security Alerts

The following sanitized alert transcript from `/var/ossec/logs/alerts/alerts.log` demonstrates live real-time log ingestion, rule matching, and compliance cross-referencing across both production hosts:

```text
** Alert 1793635141.3052618: - syslog,sshd,gdpr_IV_35.7.d,gdpr_IV_32.2,gpg13_7.1,hipaa_164.312.b,invalid_login,nist_800_53_AU.14,nist_800_53_AC.7,pci_dss_10.2.4,pci_dss_10.2.5,tsc_CC6.1,tsc_CC6.8,tsc_CC7.2,tsc_CC7.3
2026 Nov 02 15:59:01 (pingpulse-db) any->journald
Rule: 5718 (level 5) -> 'sshd: Attempt to login using a denied user.'
Src IP: [EXTERNAL_ATTACKER_IP_REDACTED]
User: root
Nov 02 15:58:59 pingpulse-db sshd[267428]: User root from [REDACTED] not allowed because not listed in AllowUsers

** Alert 1793635141.3053106: - syslog,sshd,authentication_failed,invalid_login,nist_800_53_AU.14,pci_dss_10.2.4,tsc_CC6.1
2026 Nov 02 15:59:01 (pingpulse-db) any->journald
Rule: 5710 (level 5) -> 'sshd: Attempt to login using a non-existent user'
Src IP: [EXTERNAL_ATTACKER_IP_REDACTED]
User: root
Nov 02 15:58:59 pingpulse-db sshd[267428]: Connection reset by invalid user root [REDACTED] port 23028 [preauth]
```

### Automated Regulatory Compliance Tagging
Wazuh automatically correlates detected host events against major industry security frameworks:
- **PCI DSS v4.0:** Requirements 10.2.4, 10.2.5 (Invalid logical access attempts)
- **SOC 2 Trust Services Criteria:** TSC CC6.1, CC6.8, CC7.2, CC7.3 (Logical access controls and intrusion monitoring)
- **NIST SP 800-53:** AC.7 (Unsuccessful logon attempts), AU.14 (Session audit)

---

## 5. Scope Distinction & Boundaries
- **Host IDS / EDR (Evidenced):** Wazuh provides real-time file integrity monitoring, anomaly detection, rootkit detection, and audit correlation across both nodes.
- **Dedicated Cloud WAF (Partial):** Nginx edge enforces rate-limiting, TLS ciphers, and security headers. A dedicated third-party cloud WAF (such as Cloudflare Enterprise or AWS WAF) is not deployed.
