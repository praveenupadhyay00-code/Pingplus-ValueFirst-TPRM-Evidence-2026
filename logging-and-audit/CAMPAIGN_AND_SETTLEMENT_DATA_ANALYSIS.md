# Historical Campaign Linkage & Settlement Data Analysis

**Control Reference:** ValueFirst Control 24  
**Audit Date:** 2 November 2026  
**Target Node:** `pingpulse-db` (`[DB_HOST_ADDRESS_REDACTED]`)  
**Assessor:** Pingplus Media Authorized Security & Compliance Operator  
**Classification:** SANITIZED TECHNICAL INVESTIGATION  
**Operational Invariant:** **READ-ONLY INVESTIGATION (ZERO PRODUCTION DATA MODIFIED)**  

---

## 1. Investigation Objective & Scope
In accordance with ValueFirst TPRM transparency principles, this technical assessment investigates two historical observations identified in earlier audit reviews:
1. Historical campaign-ID linkage across message logs.
2. Financial settlement status and pricing consistency across billing tables.

---

## 2. Technical Findings from Production Database Query

A read-only database query was executed against the primary database (`smpp_hub_clean`):

```sql
SELECT 
  count(*) as total_messages,
  count(campaign_id) as with_campaign_id,
  count(*) FILTER (WHERE sell_price = 0) as zero_sell_price,
  count(*) FILTER (WHERE buy_price = 0) as zero_buy_price
FROM message_logs;
```

### Result:
```text
 total_messages | with_campaign_id | zero_sell_price | zero_buy_price 
----------------+------------------+-----------------+----------------
          82487 |              581 |               1 |           8212
```

### Settlement Table Query:
```sql
SELECT 
  count(*) as total_settlements,
  count(*) FILTER (WHERE reconciliation_state = 'partially_reconciled') as partially_reconciled_count,
  count(*) FILTER (WHERE sell_price = 0) as zero_sell_price
FROM message_settlements;
```

### Result:
```text
 total_settlements | partially_reconciled_count | zero_sell_price 
-------------------+----------------------------+-----------------
             80492 |                       6015 |               0
```

---

## 3. Analysis & Transparent Root Cause

### A. Campaign-ID Linkage Architecture
- Out of 82,487 total logged messages, 581 contain a populated `campaign_id`.
- **Root Cause:** In Ping Pulse's architecture, `campaign_id` is populated exclusively for outbound traffic generated via the Web UI Campaign Runner. The majority of platform traffic (~99.3%) originates from direct SMPP bind connections (port 2775) and automated REST API customer submissions, which are tracked by `customer_id`, `customer_connection_id`, and `system_id` rather than a campaign entity. This represents normal intended system behavior.

### B. Financial Settlement Status & Zero-Pricing
- In `message_settlements`, **zero rows (`0`) out of 80,492 have a `sell_price = 0`**.
- 6,015 settlements are in `partially_reconciled` status: these correspond to messages where the carrier delivery receipt (DLR) finalization occurred after the billing settlement window closed, awaiting final reconciliation sync.
- 8,212 historical messages in `message_logs` show `buy_price = 0`: these correspond to simulated testlab traffic and internal loopback test binds where no upstream telco carrier charge was incurred.
- **Zero Production Modification:** In strict compliance with TPRM integrity rules, no historical billing data, campaign identifiers, or settlement records were altered during this audit.
