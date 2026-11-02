# Information Security & Trust Policy Summary

**Document ID:** `PP-POL-ISMS-2026-V1`  
**Effective Date:** 11 April 2026  
**Status:** Approved and in effect  
**Governing Standard:** ISO/IEC 27001:2022 (Certificate `305026090945IS`, QRO Certification LLP)  
**Applicability:** All systems, personnel, contractors, and computing facilities of Pingplus Media.  

---

## 1. Objectives & Scope
The Information Security Management System (ISMS) of Pingplus Media establishes administrative, technical, and operational controls to safeguard the confidentiality, integrity, and availability of customer messaging data, routing metadata, administrative infrastructure, and corporate records.

## 2. Core Security Principles
1. **Access Control & Least Privilege:** Administrative privileges are restricted based on business necessity and role-based access control (RBAC). Multi-factor authentication (MFA) is mandatory for production management interfaces.
2. **Perimeter Defense & Default-Deny:** Production network interfaces adhere to default-deny firewall policies. Unrestricted public access to backend ports (including PostgreSQL, SMPP, Redis, and NATS) is strictly prohibited.
3. **Data Protection in Transit:** All public web endpoints, APIs, and administrative consoles enforce modern TLS protocols (TLS 1.2 and TLS 1.3) with AEAD cipher suites.
4. **Vulnerability & Patch Management:** Continuous monitoring, automated intrusion detection, and periodic internal vulnerability assessments are mandated to identify and remediate security risks.
5. **Operational Resiliency:** Continuous encrypted backups, point-in-time recovery mechanisms, and business continuity protocols ensure data durability and rapid service restoration.

## 3. Policy Governance & Review
This policy is reviewed annually and upon significant infrastructure changes. The Office of the Founder & CEO serves as the executive sponsor and ultimate approval authority.

*Full public text available at: https://pingplusmedia.com/information-security/*
