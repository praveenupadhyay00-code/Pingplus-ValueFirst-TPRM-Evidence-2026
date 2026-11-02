# Privileged Access Management (PAM) Architecture Evaluation

**Control References:** ValueFirst Controls 35, 36  
**Evaluation Date:** 2 November 2026  
**Assessor:** Pingplus Media Authorized Security & Systems Architect  
**Classification:** SANITIZED TECHNICAL ARCHITECTURE ASSESSMENT  
**Current Operational Status:** **OPERATIONAL EXCEPTION PRESERVED (NO PRODUCTION DISRUPTION)**  

---

## 1. Executive Context & Current Baseline

### A. Current Production Operational Exception (Control 36)
In production, administrative access to `pingpulse-app` and `pingpulse-db` is managed via hardened OpenSSH:
- **Authentication:** Public-key only (`PasswordAuthentication no`, `PermitRootLogin no`, `AllowUsers pingpulse tprm_auditor`).
- **Privilege Escalation:** Sudoers configuration grants user `pingpulse` administrative execution (`NOPASSWD: ALL`).
- **Operational Justification:** To ensure platform stability and prevent emergency lockout during high-throughput messaging operations and the Keycloak MFA cutover, this operational exception was formally approved by the system owner. No modifications to SSH or sudo were made during the MFA deployment.

---

## 2. Formal PAM Architecture Evaluation (Teleport vs. HashiCorp Boundary)

To address ValueFirst Control 36 (Formal Privileged Access Management approval workflows, Just-In-Time access elevation, and session recording), Pingplus Media evaluated two leading enterprise PAM solutions: **Gravitational Teleport Community/Enterprise** and **HashiCorp Boundary**.

### Comparative Architectural Matrix

| Evaluation Dimension | Gravitational Teleport | HashiCorp Boundary | Native Bastion + SSH Certificates |
|---|---|---|---|
| **1. Approval & Time-Bound Access (JIT)** | Native Access Requests via CLI/Web; dual-authorization approval; auto-expiring certificates (1-8 hours). | Target-based sessions with time-bound credentials via Vault integration. | Manual administrative review; static key expiration. |
| **2. Session & Command Auditing** | Complete terminal session recording (replayable in web UI); structured JSON audit logs for every command. | Connection logging; session recording requires external proxy/eBPF integration. | Shell history (`/var/log/sudo.log`); syslog text logs; no terminal replay. |
| **3. Emergency Break-Glass Administration** | Local emergency break-glass credentials bypassing SSO/OIDC gateway. | Static recovery credentials stored in HashiCorp Vault. | Direct physical/serial console access via Hetzner Robot. |
| **4. Owner-Controlled Administrative Recovery** | Root CA certificate authority controlled by system owner; air-gapped recovery keys. | KMS-based unseal keys held exclusively by system owner. | Owner root SSH keys stored in offline hardware vault. |
| **5. Host Compatibility & Overhead** | Single static Go binary (`teleport`); native Ubuntu 24.04 support; lightweight eBPF kernel support. | Go binary with PostgreSQL backend requirement for controller state. | Zero additional software required (native OpenSSH). |
| **6. Cost & Maintenance Burden** | Community Edition is open-source (zero software licensing cost); requires ~200MB RAM per host. | Boundary Community Edition is open-source; requires dedicated controller instance. | Zero software licensing or maintenance cost. |
| **7. Migration & Rollback Strategy** | Non-disruptive parallel deployment on port 3022; native SSH port 22 remains fallback until cutover. | Requires client desktop application or CLI proxy for connections. | Standard OpenSSH configuration rollback. |

---

## 3. Recommended Implementation Roadmap (Staged in Isolated QA)

### Phase 1: Proof-of-Concept in Isolated QA (Target: Q1 2027)
1. Deploy Teleport Community Edition inside the isolated QA environment (`qa-edge` and `qa-app`).
2. Integrate Teleport with existing Keycloak OIDC identity provider (`auth.pingplusmedia.com`) to enforce MFA upon gateway login.
3. Configure Role-Based Access Control (RBAC) with 1-hour time-bound access limits for routine engineering tasks.
4. Verify session recording, command audit log capture, and tamper-resistance.

### Phase 2: Pilot Deployment on Staging Node
1. Test approval workflows where administrative privilege escalation requires secondary approval from the system owner.
2. Validate emergency break-glass procedure to guarantee zero administrative lockout in disaster recovery scenarios.

### Phase 3: Production Migration Gate
- The transition from OpenSSH sudoers to formal Teleport PAM will only occur after 30 consecutive days of fault-free QA operation, with explicit owner approval and verified rollback mechanisms.
- **Production Status Today:** Preserved existing hardened OpenSSH and sudo configuration. Zero disruptive changes.
