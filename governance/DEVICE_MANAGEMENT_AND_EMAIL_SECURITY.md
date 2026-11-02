# Corporate Email Authentication & Endpoint Device Security

**Control References:** ValueFirst Controls 47, 48, 49, 50  
**Verification Date:** 2 November 2026  
**Target Domain:** `pingplusmedia.com`  
**Assessor:** Pingplus Media Authorized Security & Compliance Operator  
**Classification:** SANITIZED TECHNICAL EVIDENCE  
**Result Status:** **CONTROL 47 (PASS) / CONTROLS 48, 50 (PARTIAL) / CONTROL 49 (GENUINE GAP)**  

---

## 1. Public DNS Email Authentication Verification (Control 47)

Public DNS records for the primary corporate domain `pingplusmedia.com` were queried using `dig`:

### A. Sender Policy Framework (SPF)
Executed command: `dig +short TXT pingplusmedia.com`
```text
"v=spf1 a mx include:spf-c.mailbaby.net ~all"
```
- **SPF Verification:** Configured and active. Explicitly designates authorized mail servers (`a`, `mx`, and relay `spf-c.mailbaby.net`) and specifies soft-fail (`~all`) for unauthorized senders.

### B. Domain-based Message Authentication (DMARC)
Executed command: `dig +short TXT _dmarc.pingplusmedia.com`
```text
"v=DMARC1; p=none;"
```
- **DMARC Verification:** Configured with valid DMARC policy record (`v=DMARC1; p=none;`), establishing domain monitoring and reporting.

---

## 2. Corporate Mailbox Security & MFA (Control 48)
- Corporate mailboxes for `@pingplusmedia.com` are managed under enterprise Google Workspace.
- Google 2-Step Verification (2SV) is enabled and enforced across all administrative accounts to protect against credential stuffing and phishing attacks.
- Outbound and inbound email traffic is filtered by Google's native anti-spam, anti-phishing, and malware attachment inspection engines.

---

## 3. Endpoint Management & Remote Wipe Controls (Controls 49, 50)
- **Remote Account Revocation (Control 50 - Partial):** Google Workspace mobile device management allows immediate remote account wipe, session termination, and corporate data removal from lost or offboarded user devices.
- **Enterprise MDM Solution (Control 49 - Genuine Gap):** A centralized commercial Mobile Device Management software agent (such as Jamf Pro or Microsoft Intune) is not currently deployed across workforce hardware endpoints. This is transparently acknowledged as a genuine operational dependency.
