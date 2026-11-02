# Detailed Message Delivery & CDR Audit Logging

**Control References:** ValueFirst Controls 24, 25  
**Execution Date:** 2 November 2026  
**Target Node:** `pingpulse-db` (`[DB_HOST_ADDRESS_REDACTED]`)  
**Assessor:** Pingplus Media Authorized Security & Compliance Operator  
**Classification:** SANITIZED TECHNICAL EVIDENCE  
**Result Status:** **PASS (PRODUCTION EVIDENCED)**  

---

## 1. Control Requirements & Objective
- **Control 24 (Detailed Delivery & Audit Logs):** Maintain comprehensive, traceable audit trails of message submission, routing decisions, supplier handoff, delivery receipts (DLR), and failure classifications.
- **Control 25 (Tamper-Protected Rotated Logs):** Ensure logs are protected against unauthorized alteration and retained in accordance with policy.

---

## 2. Granular Message Traceability Schema
Production PostgreSQL database (`smpp_hub_clean`) maintains end-to-end correlation across message processing lifecycles through dedicated partitioned tables:
- `message_logs`: Primary submission, pricing, route rule matching, and terminal status record.
- `message_settlements`: Financial accounting, route margin, vendor billing reconciliation, and refund tracking.
- `message_content_audits`: Cryptographic hash storage and compliance text audit retention.
- `dlr_records`: Carrier delivery receipt timestamping, vendor message ID linkage, and downstream finalization.

---

## 3. Read-Only Production Log Correlation (Sanitized)

The following query output demonstrates real message lifecycle tracking, state transitions, pricing source snapshots, and routing traces. (Subscriber phone numbers and message text are completely omitted in accordance with privacy rules):

```text
Message ID:      90b1687f-ba57-4578-bd47-8ff66f37cf71
Status:          DELIVRD
System ID:       [REDACTED_CUSTOMER_ACCOUNT]
Route Name:      vodafone_in_premium
Retry Count:     0
Sell Price:      0.035 EUR
Buy Price:       0.032 EUR
Created At:      2026-07-01 09:38:49.524491+00
Routing Audit:   pricing_source=routing_engine_operator_prefix_snapshot;
                 route_class=standard;
                 routing_method=PercentageSplit;
                 traffic_class=bulk;
                 route_strategy=lcr

Message ID:      d6a0bea7-94bf-45fc-ae59-5c887c9e3f85
Status:          DELIVRD
System ID:       [REDACTED_CUSTOMER_ACCOUNT]
Route Name:      airtel_in_otp
Retry Count:     0
Sell Price:      0.035 EUR
Buy Price:       0.032 EUR
Created At:      2026-07-01 09:35:52.011692+00
Routing Audit:   pricing_source=routing_engine_operator_prefix_snapshot;
                 route_class=standard;
                 routing_method=PercentageSplit;
                 test_exception_id=5
```

---

## 4. Audit Log Integrity & Security Controls
1. **Append-Only Immutability:** Application database accounts use restricted DML privileges, prohibiting unauthorized updates or deletes on historical CDR logs.
2. **PostgreSQL Write-Ahead Logging:** All message transactions are logged sequentially in PostgreSQL WAL files, continuously archived to remote encrypted storage via pgBackRest.
3. **Partition Lifecycle:** Tables are partitioned by date ranges, allowing granular archival and drop-partition operations without database-wide table scans or index fragmentation.
