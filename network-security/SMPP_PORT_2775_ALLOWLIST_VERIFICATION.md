# SMPP Port 2775 Inbound Source IP Allowlist Verification

**Control References:** ValueFirst Controls 11, 16, 20  
**Test Node:** `pingpulse-app` (`[APP_HOST_ADDRESS_REDACTED]`)  
**Execution Date:** 2 November 2026  
**Assessor:** Pingplus Media Authorized Security & Compliance Operator  
**Classification:** SANITIZED TECHNICAL EVIDENCE  
**Result Status:** **PASS (WITH DOCUMENTED OPERATIONAL EXCEPTION)**  

---

## 1. Control Requirement & Verification Objective
- **Control 11 (SMPP IP Allowlisting):** Ensure all SMPP client binds are restricted to explicitly approved customer source IP addresses.
- **Control 16 (Host Firewall/ACL):** Verify production host-based firewall enforces default-deny incoming.
- **Control 20 (Plaintext SMPP Disabled or Restricted):** Ensure plaintext SMPP is either disabled or strictly restricted to authorized, non-public connections.

---

## 2. Technical Evidence & Output

### A. Host Firewall (UFW) Default Policy
Inspected on `pingpulse-app`:
```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip
```

### B. Numbered Rules for Port 2775
Executed command: `sudo ufw status numbered | grep 2775`
```text
[ 5] 2775/tcp                   ALLOW IN    [PARTNER_ADDRESS_A_REDACTED]
[ 6] 2775/tcp                   ALLOW IN    [PARTNER_ADDRESS_B_REDACTED]
[ 7] 2775/tcp                   ALLOW IN    [PARTNER_ADDRESS_C_REDACTED]
```
- **Rule Count:** Exactly 3 inbound rules exist for port 2775.
- **Allowlisted IPs:** Restricted to approved ValueFirst/Tanla customer gateway IPs (`[PARTNER_ADDRESS_A_REDACTED]`, `[PARTNER_ADDRESS_B_REDACTED]`, `[PARTNER_ADDRESS_C_REDACTED]`).
- **Default Deny:** All other public IPv4 and IPv6 addresses are dropped by the default-deny policy. Zero unrestricted (`0.0.0.0/0` or `::/0`) rules exist for port 2775.

### C. Active Established Connection Observation
Executed command: `ss -tan state established '( dport = :2775 or sport = :2775 )'`
```text
Netid  Recv-Q  Send-Q     Local Address:Port             Peer Address:Port
tcp    0       0          [APP_HOST_ADDRESS_REDACTED]:2775              [PARTNER_ADDRESS_A_REDACTED]:34002
```
Observed established session originates strictly from allowlisted customer IP `[PARTNER_ADDRESS_A_REDACTED]`, exactly matching UFW rule [5].

### D. Application-Level CIDR Guard
In addition to host-level UFW packet filtering, Ping Pulse enforces dual-layer application allowlists:
- Database table `customer_ip_allowlists` validates incoming peer IP upon `bind_transceiver` / `bind_transmitter`.
- Non-allowlisted connection attempts that bypass network filters are immediately terminated at the PDU handler with `ESME_RBINDFAIL` (0x0000000D).

---

## 3. Operational Exception & Scope Limitations
- **Plaintext Exception:** Native traffic on port 2775 uses standard unencrypted SMPP v3.4 over TCP. This is preserved as an approved commercial exception to support legacy aggregator and telecom downstream client compatibility.
- **Compensating Controls:** Inbound exposure is strictly gated by host firewall IP allowlists, application-level bind ACLs, and isolated virtual network routing. Modern encrypted SMPP over TLS 1.3 is supported on port 2778 for capable clients.
