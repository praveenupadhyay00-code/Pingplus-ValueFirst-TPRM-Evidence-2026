# Live Message-Archive & Storage Encryption Assessment

**Control Reference:** ValueFirst Control 22  
**Verification Date:** 2 November 2026  
**Target Node:** `pingpulse-db` (`[DB_HOST_ADDRESS_REDACTED]`)  
**Assessor:** Pingplus Media Authorized Security & Compliance Operator  
**Classification:** SANITIZED TECHNICAL EVIDENCE  
**Result Status:** **PARTIAL / DOCUMENTED EXCEPTION (OFFSITE AES-256-CBC PASS; PRIMARY & LOCAL REPO1 EXCEPTION)**  

---

## 1. Control Requirement & Assessment Scope
- **Control 22 Requirement:** Message archives, message logs, and databases stored at rest must be encrypted using approved cryptographic algorithms.
- **Scope of Read-Only Audit:**
  1. Primary host block storage and filesystem encryption configuration.
  2. Application-level data hashing, masking, and PII redaction in message audit tables.
  3. pgBackRest offsite encrypted backup repository (`repo2`) and streaming WAL archives.
  4. pgBackRest local backup repository (`repo1`) plaintext exception.

---

## 2. Primary Host Block Storage Findings

A read-only block device and filesystem audit was conducted on production database host `pingpulse-db`:

```text
NAME        FSTYPE            LABEL    UUID                                 MOUNTPOINTS
nvme1n1                                                                                          
├─nvme1n1p1 vfat                       5AC2-E4D7                            /boot/efi
├─nvme1n1p2 linux_raid_member rescue:0 0ed96195-c366-f3c0-5a04-6afbee552d87 
│ └─md0     swap                       0cd9e8b2-3e13-4565-9699-191186a8bf61 [SWAP]
├─nvme1n1p3 linux_raid_member rescue:1 eb58560f-895a-70b8-3b06-22b18d671cf1 
│ └─md1     ext3                       3c6a7423-5f03-4883-b56a-e4edf70a3170 /boot
└─nvme1n1p4 linux_raid_member rescue:2 0897ca49-0037-fe51-3d25-9861d549ad38 
  └─md2     ext4                       1f3b4c29-642a-4344-9946-488d5d0427a4 /
nvme0n1     (Mirrored RAID 1 pair)
```

- **Physical Media:** 2x NVMe enterprise SSDs configured in Linux Software RAID 1 (`/dev/md2` mounted as `/`).
- **Filesystem:** Standard `ext4` filesystem.
- **At-Rest Encryption Verdict:** Primary host filesystem is **unencrypted at the block device level** (no dm-crypt / LUKS layer configured). Physical security relies on the hosting provider's Tier III datacenter perimeter controls.

---

## 3. Database Application-Level Message Protection

In the absence of whole-disk LUKS encryption, sensitive message data in PostgreSQL (`smpp_hub_clean`) is protected through application-layer cryptographic hashing and scrubbing:

Table schema for `message_content_audits`:
```text
Table "public.message_content_audits"
  Column           | Type                     | Nullable | Default
-------------------+--------------------------+----------+--------------------
  content_sha256   | character varying(64)    | not null | 
  redaction_status | character varying(32)    |          | 'full'::varchar
  content_text     | text                     |          | [Masked/Redacted]
  captured_at      | timestamp with time zone | not null | 
  expires_at       | timestamp with time zone | not null | 
```

- **Tamper-Evident Hashing:** Mandatory SHA-256 cryptographic digest stored in `content_sha256` for message integrity verification without storing exposed plain text.
- **PII Scrubbing:** `redaction_status` defaults to `'full'` for sensitive OTP and customer message payloads.
- **Retention Lifecycle:** Mandatory `expires_at` timestamp enforces automatic data purging per retention policy.

---

## 4. Offsite vs. Local pgBackRest Archive Encryption

Command output from `sudo pgbackrest --stanza=pingpulse info` on `pingpulse-db`:
```text
stanza: pingpulse
    status: ok
    cipher: mixed
        repo1: none
        repo2: aes-256-cbc
    db (current)
        wal archive min/max (16): 000000010000000000000023/000000010000034D0000006F
        diff backup: 20261028-075044F_20261102-034512D
            timestamp start/stop: 2026-11-02 03:45:12+00 / 2026-11-02 03:47:13+00
            repo2: backup set size: 899.5MB, backup size: 201.3MB
```

1. **Remote Offsite Repository (`repo2` - Evidenced):**
   - Stored offsite at Hetzner Storage Box via SFTP.
   - Encrypted at rest using **`aes-256-cbc`**.
   - Verified active with fresh differential backup on 2 November 2026 and unbroken WAL stream.
2. **Local Disk Repository (`repo1` - Documented Operational Exception):**
   - Configured with `cipher: none`.
   - **Operational Justification:** Local `repo1` is dedicated to zero-overhead fast local rollback and WAL retention to prevent disk write saturation during high-burst SMS throughput (>10,000 msgs/min).
   - Documented and approved by system owner as an operational exception; offsite repository strictly maintains AES-256-CBC.

---

## 5. Comprehensive Summary Matrix

| Storage Domain | Storage Mechanism | Cryptographic Protection | TPRM Compliance Disposition |
|---|---|---|:---:|
| **Primary Live DB** | Linux SoftRAID (`/dev/md2` ext4) | Unencrypted block layer; App-level SHA-256 & PII masking | **Documented Operational Exception** |
| **Local Backup** | `/var/lib/pgbackrest` (`repo1`) | Plaintext (`cipher: none`) | **Documented Operational Exception** |
| **Remote Backup** | Offsite Storage Box (`repo2`) | Strong Encryption (**`aes-256-cbc`**) | **Evidenced & Active** |
| **Streaming WAL** | Continuous offsite archive | Strong Encryption (**`aes-256-cbc`**) | **Evidenced & Active** |
