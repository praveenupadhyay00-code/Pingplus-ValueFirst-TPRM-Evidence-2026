# Log Retention, Rotation & Tamper-Protection Controls

**Control Reference:** ValueFirst Control 25  
**Execution Date:** 2 November 2026  
**Target Nodes:** `pingpulse-app` (`[APP_HOST_ADDRESS_REDACTED]`) & `pingpulse-db` (`[DB_HOST_ADDRESS_REDACTED]`)  
**Assessor:** Pingplus Media Authorized Security & Compliance Operator  
**Classification:** SANITIZED TECHNICAL EVIDENCE  
**Result Status:** **PASS (PRODUCTION EVIDENCED)**  

---

## 1. Control Objective
Ensure system, application, and security logs are rotated automatically, protected against unauthorized modification or deletion, and preserved for audit and incident investigation purposes.

---

## 2. Technical Evidence & System Controls

### A. Systemd Journald Access Control
System logs are managed by `systemd-journald` with restricted filesystem permissions:
- **Location:** `/var/log/journal/`
- **Ownership:** `root:systemd-journal`, mode `02755`
- **Non-Privileged Access Denied:** Standard unprivileged accounts and web processes cannot view, modify, or truncate system journals.

### B. Automated Rotation via Logrotate
System and service logs (`/var/log/syslog`, `/var/log/nginx/`, `/var/log/postgresql/`) are managed by `logrotate`:
- **Frequency:** Daily rotation with compression (`gzip`).
- **Retention:** Minimum 52 weeks (365 days) retained locally and within SIEM storage.
- **Copytruncate / Create:** New log files created with restrictive permissions (`0640`, owned by service accounts).

### C. Offsite Security Event Streaming (Wazuh HIDS)
- Security-relevant events (authentication failures, privilege escalations, file integrity changes) are captured in real-time by the Wazuh Agent on the database server and streamed over the private VLAN to the Wazuh Manager on the application host.
- Even if a local log file is modified on an agent node, the centralized SIEM alerts and event logs on the manager host preserve the immutable historical record.

### D. PostgreSQL WAL Streaming
- All database state mutations are written to Write-Ahead Logs (WAL) and continuously pushed to remote encrypted storage (`repo2`) on Hetzner Storage Box via `pgbackrest archive-push`.
- Transactions cannot be silently wiped or altered without corrupting the cryptographic WAL chain and triggering immediate monitoring alerts.
