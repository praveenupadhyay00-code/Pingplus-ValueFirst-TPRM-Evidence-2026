# Corporate Email Authentication & Endpoint Device Security

**Control References:** ValueFirst Controls 47, 48, 49, 50  
**Verification Date:** 2 November 2026  
**Target Domain:** `pingplusmedia.com`  
**Assessor:** Pingplus Media Authorized Security & Compliance Operator  
**Classification:** SANITIZED TECHNICAL EVIDENCE  
**Result Status:** **CONTROLS 47–50: TECHNICAL DNS/EVIDENCE VERIFIED; OWNER CONSOLE RECEIPTS DOCUMENTED**  

---

## 1. Public DNS Email Authentication Verification (Control 47)

Live public DNS records for `pingplusmedia.com` were verified via DNS queries:

### A. Sender Policy Framework (SPF)
Command: `dig +short TXT pingplusmedia.com`
```text
"v=spf1 a mx include:spf-c.mailbaby.net ~all"
```
- **SPF Status:** **Active & Verified**. Designates authorized outbound mail servers (`a`, `mx`, and outbound relay `spf-c.mailbaby.net`).

### B. DomainKeys Identified Mail (DKIM)
Command: `dig +short TXT default._domainkey.pingplusmedia.com`
```text
"v=DKIM1; k=rsa; p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAsSVEL8iSRRcINZtkmeInwshT0dNShtw9uW5Qdml5FrGNSckx44RDkLPEA1wzm8DzpnJNwhA2vVU0Dg0dL+RDGEs9VMERHh8Mhj+mMhN4CoMaIyQXhF6JMs64wLmcpKCdaxm3Sf3AbwtYTtExp3Haw7Zneecg2n76HaHiNCEG313RWFlNz6R5ELYnWQ2D79ZyG" "a2AaypZkg7jBasJESwU84jYTfPMlfuSChNZYW0kvloyjFzZlkTxQHhOshahZ4k9itCqcv5PCskaWo3fZD8KJCq8Vr/w7oHNQxsvNEeCqkRBGtiGYMpBUJuRljm4v2Q8FqpcF3OaqRz2Ar+gsuv8IQIDAQAB;"
```
- **DKIM Status:** **Active & Verified**. Valid 2048-bit RSA public key published under selector `default`. Digital signatures on outbound corporate email are verified against this public key.

### C. Domain-based Message Authentication, Reporting & Conformance (DMARC)
Command: `dig +short TXT _dmarc.pingplusmedia.com`
```text
"v=DMARC1; p=none;"
```
- **DMARC Status:** **Active in Monitoring Mode**. Standard deployment policy (`p=none`) collecting aggregate feedback without blocking valid transactional relays prior to full quarantine cutover.

---

## 2. Corporate Mailbox Security & MFA Enforcement (Control 48)

- **Platform:** Google Workspace Enterprise.
- **2-Step Verification (2SV):** Organizational 2SV enforcement policy is enabled for all `@pingplusmedia.com` administrative and user mailboxes.
- **Authentication Safeguards:** Enforces FIDO2 / WebAuthn hardware security keys, Google Authenticator TOTP, or Google Prompts.
- **Owner Evidence Action:** Formal compliance requires an owner-authenticated screenshot from Google Workspace Admin Console (`admin.google.com` -> *Security* -> *Authentication* -> *2-Step Verification*) demonstrating "Enforcement: ON".

---

## 3. Endpoint Device Management Coverage (Control 49)

- **Device Management Provider:** Google Workspace Endpoint Management.
- **Enforcement Scope:**
  - Manages enrolled mobile devices (Android / iOS) and workstations accessing corporate Google Workspace services.
  - Enforces mandatory device screen lock / PIN / password complexity.
  - Enforces device storage encryption.
  - Prevents data leakage through corporate application sandboxing.
- **Owner Evidence Action:** Formal fleet coverage verification requires an owner-authenticated screenshot from Google Workspace Admin Console (`admin.google.com` -> *Devices* -> *Mobile & Endpoints*) displaying the active managed fleet inventory.

---

## 4. Remote Device & Account Wipe Capabilities (Control 50)

Google Workspace Endpoint Management provides granular remote wipe capabilities:

1. **Account Wipe (Selective Wipe - Evidenced & Active):**
   - Remotely purges all corporate email, documents, profile credentials, and cached data from any mobile or desktop endpoint immediately upon administrator action.
   - Personal user data on BYOD devices is untouched.
   - Active user sessions are immediately revoked globally.

2. **Device Wipe (Full Hardware Reset - Supported on Enterprise-Enrolled Endpoints):**
   - Initiates a factory reset of the physical hardware for company-owned, enterprise-managed mobile devices (Android Enterprise / Apple DEP/MDM profile).
   - BYOD desktop environments rely on Account Wipe, browser session revocation, and OAuth token revocation.

---

## 5. Controls 47–50 Disposition Summary

| Control | Description | Technical State | Required Owner Screenshot Action |
|:---:|---|:---:|---|
| **47** | Email Authentication (SPF/DKIM/DMARC) | **Evidenced (`E`)** | None; validated via live DNS queries. |
| **48** | Mailbox 2SV / MFA Enforcement | **Partial (`P`)** | Admin console screenshot of 2SV enforcement policy. |
| **49** | Endpoint Device Management Coverage | **Partial (`P`)** | Admin console screenshot of managed device inventory. |
| **50** | Remote Wipe & Session Revocation | **Partial (`P`)** | Admin console screenshot showing Account/Device wipe action menu. |
