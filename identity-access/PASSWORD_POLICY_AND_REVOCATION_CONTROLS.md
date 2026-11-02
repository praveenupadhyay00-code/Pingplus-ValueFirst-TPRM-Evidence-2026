# Password Policy, Credential Hardening & Access Revocation

**Control References:** ValueFirst Controls 9, 10, 14  
**Execution Date:** 2 November 2026  
**Assessor:** Pingplus Media Authorized Security & Compliance Operator  
**Classification:** SANITIZED TECHNICAL EVIDENCE  
**Result Status:** **PASS (CONTROLS 10, 14) / PARTIAL (CONTROL 9 ROTATION)**  

---

## 1. Control Objectives & Scope
- **Control 9 (Password Policy / 90-day Changes):** Enforce strong password complexity and rotation across administrative and customer accounts.
- **Control 10 (Default Credentials Eliminated):** Confirm elimination of default vendor and operating system passwords.
- **Control 14 (Immediate Access Revocation):** Verify that terminated accounts or revoked privileges take effect immediately, invalidating active sessions.

---

## 2. Technical Evidence & Implementation

### A. Password Hardening & Brute Force Protection (Control 9)
- **IAM Complexity:** Keycloak password policies enforce minimum length (12 characters), mixed-case, digits, and special characters for all administrative accounts.
- **Brute Force Lockout:** Keycloak attack detection is active. Excessive failed authentication attempts (`numFailures >= 8`) automatically place the user account into `user_temporarily_disabled` status, mitigating automated credential-stuffing attacks.
- **Scope Note on 90-Day Rotation:** The system owner enforces 90-day password rotation for administrative staff. However, because automated global forced expiry across all historical SMPP binds is not implemented (as uncoordinated bind password changes break carrier traffic), Control 9 is classified as **PARTIALLY EVIDENCED**.

### B. Elimination of Default Credentials (Control 10)
- **Operating System Accounts:** Standard default Linux accounts (`guest`, `test`, `ubuntu`, `admin`) are removed or disabled with `/sbin/nologin`. Direct root SSH login is blocked (`PermitRootLogin no`).
- **PostgreSQL Database:** Accepts connections exclusively over the private virtual switch (`10.10.10.20`), requiring scram-sha-256 password authentication. Peer authentication is enforced for local system accounts.
- **Keycloak IAM:** The master realm administrative account (`kcadmin`) uses a unique generated 32-character credential stored in `/opt/keycloak/conf/.admin_password` (mode `0600`).
- **Internal Messaging (Redis/NATS):** Bound strictly to localhost loopback interfaces (`127.0.0.1` and `[::1]`) with authenticated credentials.

### C. Immediate Session & Credential Revocation (Control 14)
- **Token Invalidation:** Application JWT middleware (`internal/api/auth.go`) performs database-backed status validation. When an administrator or user is flagged inactive (`is_active = false`), subsequent API calls immediately return `401 Unauthorized` without waiting for token expiry.
- **Keycloak SSO Session Termination:** Invoking session logout via Keycloak OIDC immediately terminates the user's active session across all connected applications.
- **Verification:** Unit and integration test suites (`TestAdminRevocation_DisabledUser`, `TestSessionTermination`) confirm that disabled credentials cannot access protected routes.
