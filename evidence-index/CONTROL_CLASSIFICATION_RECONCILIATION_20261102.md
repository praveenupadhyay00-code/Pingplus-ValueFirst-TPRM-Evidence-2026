# ValueFirst TPRM: post-operator classification reconciliation — 2 November 2026

An operator's initial report summary of 19 E / 23 P / 8 G did not reconcile with its 50-row matrix (21 E / 22 P / 7 G). The corrected register totals 21 E / 26 P / 3 G.

## Owner-attested operating records
- Employee training library effective 11 April 2026 exists (36 CP + 7 HR modules); dated staff completion receipts are outstanding.
- Access review last completed 9 September 2026; log review last completed 9 October 2026; breach drill last held 15 September 2026: these dates are owner attestations, with original review and drill records to attach.
- Risk register approved; quarterly treatment and executive/CTO escalation owner-attested.
- In-house NOC/security monitoring staffed 24x7/365 in three 8-hour shifts. The question allows in-house or outsourced; an external SOC vendor contract is not inherently required. Confirm analyst duty rota and actual security alert handling.
- Multi-server failover plan exists per owner; a functioning active standby is not yet demonstrated.
- Workspace mobile management is active; whether all in-scope devices have enforceable management and remote lock/full-wipe needs proof. A specific Jamf/Intune vendor is not mandatory.

## Technical qualifications
- SMPP 2775: production default-deny firewall and three allowed peers satisfy inbound IP restriction; TLS encryption across every connection is NOT proven.
- Recovery: ~3.5-minute isolated database restore and reported WAL delay <60 seconds are evidence for the database tier, NOT end-to-end platform RTO/RPO.
- DMARC TXT p=none is monitoring, not anti-spoofing enforcement. SPF record alone does not validate DKIM signatures/alignment.
- 80,492 aggregate settlement rows with zero sell_price=0 do not independently disprove the earlier Campaign 16 zero-value/partially reconciled settlement observation. Inspect the same campaign and amount/reconciliation fields.
- CSP hardening remains an informational open finding; independent VAPT is a governance dependency. No open high/critical finding was identified in the recorded internal QA scope.
- Nginx rate limiting, Wazuh IDS and Fail2ban do not alone demonstrate a general WAF or a full PAM approval workflow.
- Owner-signed DPA is awaiting ValueFirst countersignature; current breach notification text is 48h rather than the requested illustrative 24h.

## Publication safety
The original public Git commit contained production and partner IPs. Current HEAD masks these, but historical Git objects remain accessible; a clean-history republication is required before calling the repository historically sanitized. The original SHA-256 manifest refers to the initial publication and should be regenerated against the final frozen files.

No production service, SMPP bind, message, SSH/sudo, firewall, or database was changed by this documentation update.
