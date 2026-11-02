# Data Retention & Privacy Deletion Policy Summary

**Document ID:** `PP-POL-RET-2026-V1`  
**Effective Date:** 11 April 2026  
**Status:** Approved and in effect  
**Applicability:** Customer CDRs, message logs, billing records, and audit events.  

---

## 1. Retention Durations
In alignment with enterprise telecom compliance, regulatory obligations, and customer reconciliation requirements:
- **Call Detail Records (CDRs) & Delivery Logs:** Retained for exactly **180 days** to support billing auditability, carrier reconciliation, and regulatory dispute resolution.
- **Message Content & Body Text:** Scrubbed or pseudonymized per customer tenant privacy configurations, retaining cryptographic hashes for integrity proof.
- **Security & Activity Logs:** Retained for **365 days** across SIEM and system journals for forensic analysis.
- **Automated Expired Data Purge:** Automated background retention workers cycle expired records past their mandatory retention window.

*Full public text available at: https://pingplusmedia.com/data-retention/*
