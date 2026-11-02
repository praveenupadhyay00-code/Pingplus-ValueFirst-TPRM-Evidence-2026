# Fail2ban Host Intrusion Prevention (IPS) Status

**Control Reference:** ValueFirst Control 17  
**Execution Date:** 2 November 2026  
**Target Node:** `pingpulse-app` (`[APP_HOST_ADDRESS_REDACTED]`)  
**Assessor:** Pingplus Media Authorized Security & Compliance Operator  
**Classification:** SANITIZED TECHNICAL EVIDENCE  
**Result Status:** **PASS (PRODUCTION EVIDENCED)**  

---

## 1. Control Objective
Verify that automated host-based intrusion prevention mechanisms are actively intercepting brute-force and unauthorized access attempts on network services.

---

## 2. Technical Evidence & Execution Output

Executed command: `sudo fail2ban-client status sshd` on `pingpulse-app`:
```text
Status for the jail: sshd
|- Filter
|  |- Currently failed: 7
|  |- Total failed:     16917
|  `- Journal matches:  _SYSTEMD_UNIT=sshd.service + _COMM=sshd
`- Actions
   |- Currently banned: 0
   |- Total banned:     1052
   `- Banned IP list:   [DYNAMICALLY ROTATED - ATTACKER IPS OMITTED]
```

### Key Security Metrics:
- **Total Failed Attempts Intercepted:** `16,917` brute-force login attempts detected and filtered.
- **Total Attacking IPs Automatically Banned:** `1,052` unique malicious source IP addresses banned via Linux iptables/netfilter chains.
- **Journal Integration:** Native systemd journald real-time monitoring (`_SYSTEMD_UNIT=sshd.service`).

---

## 3. IPS Integration Conclusion
Fail2ban functions as an active Host Intrusion Prevention System (HIPS). In combination with UFW default-deny and public-key-only SSH authentication, unauthorized SSH intrusion attempts are automatically dropped at the firewall layer before reaching application authentication routines.
