# Production Network Host Firewall Baseline

**Control Reference:** ValueFirst Control 16  
**Execution Date:** 2 November 2026  
**Assessor:** Pingplus Media Authorized Security & Compliance Operator  
**Classification:** SANITIZED TECHNICAL EVIDENCE  
**Result Status:** **PASS (PRODUCTION EVIDENCED)**  

---

## 1. Control Objective
Verify that host-level firewalls are enabled across all production instances, enforcing a default-deny inbound posture, and restricting access strictly to necessary application services and authorized private network addresses.

---

## 2. Firewall Architecture & Invariants

Production hosts reside in Hetzner Falkenstein data center, linked via an isolated private hardware virtual switch (vSwitch / VLAN `vlan4000`, network `10.10.10.0/24`). Both production servers run Ubuntu Linux 24.04 with Uncomplicated Firewall (UFW) active and managed.

```text
               +--------------------------------------------------+
               |                  PUBLIC INTERNET                 |
               +--------------------------------------------------+
                        | (Ports 80, 443)       | (Port 22 SSH)
                        v                       v
               +--------------------------------------------------+
               |       pingpulse-app (23.88.74.110 / 10.10.10.10) |
               |   UFW: Default Deny Incoming                     |
               |   - 22/tcp (SSH: public-key auth only)           |
               |   - 80, 443/tcp (Web/TLS Edge)                   |
               |   - 2775/tcp (SMPP: Gated to 3 Customer IPs)     |
               +--------------------------------------------------+
                                        |
                 [ Private VLAN vSwitch 10.10.10.0/24 ]
                                        |
                                        v
               +--------------------------------------------------+
               |       pingpulse-db (23.88.20.15 / 10.10.10.20)   |
               |   UFW: Default Deny Incoming                     |
               |   - 22/tcp (SSH: public-key auth only)           |
               |   - 5432, 6432/tcp (PostgreSQL / pgBouncer)      |
               |     ALLOW ONLY FROM 10.10.10.10                  |
               |   - Zero Public Database Exposure                |
               +--------------------------------------------------+
```

---

## 3. Host Firewall Status & Rule Inspection

### A. Application Server (`pingpulse-app`)
Executed command: `sudo ufw status verbose`
```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere                   # Administrative SSH
80/tcp                     ALLOW IN    Anywhere                   # HTTP -> HTTPS redirect
443/tcp                    ALLOW IN    Anywhere                   # HTTPS Portal & API
Anywhere on vlan4000       ALLOW IN    10.10.10.20                # Private DB node traffic
2775/tcp                   ALLOW IN    180.179.215.163            # ValueFirst Customer Gateway
2775/tcp                   ALLOW IN    180.179.215.111            # ValueFirst Customer Gateway
2775/tcp                   ALLOW IN    180.179.210.40             # ValueFirst Customer Gateway
22/tcp (v6)                ALLOW IN    Anywhere (v6)
80/tcp (v6)                ALLOW IN    Anywhere (v6)
443/tcp (v6)               ALLOW IN    Anywhere (v6)
```

### B. Database Server (`pingpulse-db`)
Executed command: `sudo ufw status verbose`
```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere                   # Administrative SSH
5432/tcp                   ALLOW IN    10.10.10.10                # PostgreSQL from App Node only
6432/tcp                   ALLOW IN    10.10.10.10                # pgBouncer from App Node only
Anywhere on vlan4000       ALLOW IN    10.10.10.10                # Private App node traffic
22/tcp (v6)                ALLOW IN    Anywhere (v6)
```

---

## 4. Security Verification Verdict
1. **Default Deny:** Both production nodes strictly drop all unlisted incoming traffic.
2. **Database Isolation:** Database ports (5432 and 6432) have zero public exposure; they accept connections strictly over private VLAN from IP `10.10.10.10`.
3. **Customer Ingress Protection:** Plaintext SMPP port 2775 is completely inaccessible to the general public, accepting packets strictly from the 3 authorized customer CIDRs.
4. **Result:** **PASS (PRODUCTION VERIFIED)**.
