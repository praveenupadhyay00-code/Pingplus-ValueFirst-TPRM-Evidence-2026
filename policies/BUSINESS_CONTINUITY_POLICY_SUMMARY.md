# Business Continuity & Disaster Recovery Policy Summary

**Document ID:** `PP-POL-BCP-2026-V1`  
**Effective Date:** 11 April 2026  
**Status:** Approved and in effect  
**Applicability:** Core telecom routing, customer API, message processing, and billing databases.  

---

## 1. Objectives & Recovery Metrics
Pingplus Media designs its core messaging infrastructure for high availability and resilient disaster recovery:
- **Recovery Time Objective (RTO):** Target of less than **4 hours** for full database and routing service reconstitution following a major catastrophic facility outage.
- **Recovery Point Objective (RPO):** Target of less than **15 minutes** for data durability, enabled by continuous PostgreSQL Write-Ahead Log (WAL) archiving.

## 2. Backup Architecture & Cadence
- **Local Snapshots (repo1):** High-speed local operational backup repository for rapid crash recovery.
- **Remote Encrypted Snapshots (repo2):** Independent remote backup archive stored on Hetzner Storage Box in a separate physical zone, encrypted with AES-256-CBC cipher.
- **Cadence:** Daily differential backups (executed daily at ~03:45 UTC) anchored against base full backups, with continuous streaming WAL pushes via pgBackRest.
- **Verification:** Regular out-of-place database restoration rehearsals conducted in isolated environments to validate cryptographic decryption, catalog integrity, and transaction replay.

*Full public text available at: https://pingplusmedia.com/business-continuity/*
