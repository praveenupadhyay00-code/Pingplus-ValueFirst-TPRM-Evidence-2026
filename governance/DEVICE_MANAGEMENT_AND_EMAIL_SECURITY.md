# Corporate Email Authentication & Endpoint Device Security

**Control References:** ValueFirst Controls 47, 48, 49, 50  
**Verification Date:** 2 November 2026  
**Target Domain:** `pingplusmedia.com`  
**Assessor:** Pingplus Media Authorized Security & Compliance Operator  
**Classification:** SANITIZED TECHNICAL EVIDENCE  
**Result Status:** **CONTROLS 47–50: SCOPE-LIMITED TECHNICAL EVIDENCE; FLEET/ENFORCEMENT PROOF OPEN**  

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
- **DMARC Verification:** Configured with valid DMARC policy record (`v=DMARC1; p=none;`), establishing monitoring-only DMARC. With p=none, it does not request receiver quarantine/rejection. This TXT lookup alone also does not verify DKIM signature/alignment.

---

## 2. Corporate Mailbox Security & MFA (Control 48)
- Corporate mailboxes for `@pingplusmedia.com` are managed under enterprise Google Workspace.
- Google 2-Step Verification (2SV) policy is reported enabled. Full administrative and employee account enforcement must be confirmed from Google Admin console; enabled alone does not prove mandatory 2SV for every account.
- Outbound and inbound email traffic is filtered by Google's native anti-spam, anti-phishing, and malware attachment inspection engines.

---

## 3. Endpoint Management & Remote Wipe Controls (Controls 49, 50)
- **Remote Account Revocation (Control 50 - Partial):** Workspace can revoke sessions and remove corporate account information. Account wipe is not equivalent to a whole-device lock/wipe; verify the actual available device-level command in the organization's Google Workspace edition.
- **MDM Scope (Control 49 - Partial):** Google Workspace device management is available and reports remote account actions. Lack of Jamf Pro or Microsoft Intune alone does not prove absence of MDM. Verify actual managed device enrollment, scope, compliance status and policies, including offboarded/lost-device handling.
