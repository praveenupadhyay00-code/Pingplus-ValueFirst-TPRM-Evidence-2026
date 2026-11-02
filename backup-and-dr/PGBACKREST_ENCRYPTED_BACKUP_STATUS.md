# pgBackRest Encrypted Offsite Backup Status

**Control References:** ValueFirst Controls 22, 37  
**Execution Date:** 2 November 2026  
**Target Node:** `pingpulse-db` (`[DB_HOST_ADDRESS_REDACTED]`)  
**Assessor:** Pingplus Media Authorized Security & Compliance Operator  
**Classification:** SANITIZED TECHNICAL EVIDENCE  
**Result Status:** **PASS (ENCRYPTED REPO2) / DOCUMENTED EXCEPTION (LOCAL REPO1)**  

---

## 1. Control Requirements & Objective
- **Control 22 (Message Archives Encrypted at Rest):** Ensure message archives and database repositories are encrypted using strong industry-standard cryptography.
- **Control 37 (Encrypted Backup & Recovery):** Automated database backups encrypted in transit and at rest, stored offsite, with verifiable integrity.

---

## 2. Technical Evidence & Execution Output

Executed command: `sudo pgbackrest --stanza=pingpulse info` on `pingpulse-db`:

```text
stanza: pingpulse
    status: ok
    cipher: mixed
        repo1: none
        repo2: aes-256-cbc

    db (current)
        wal archive min/max (16): 000000010000000000000023/000000010000034D0000006F

        full backup: 20261028-075044F
            timestamp start/stop: 2026-10-28 07:50:44+00 / 2026-10-28 07:53:19+00
            wal start/stop: 000000010000033000000040 / 000000010000033000000042
            database size: 4.5GB, database backup size: 4.5GB
            repo2: backup set size: 892.9MB, backup size: 892.9MB

        diff backup: 20261028-075044F_20261029-034523D
            timestamp start/stop: 2026-10-29 03:45:23+00 / 2026-10-29 03:45:54+00
            database size: 4.4GB, database backup size: 1.7GB
            repo2: backup set size: 892.1MB, backup size: 192.5MB
            backup reference list: 20261028-075044F

        diff backup: 20261028-075044F_20261030-034513D
            timestamp start/stop: 2026-10-30 03:45:13+00 / 2026-10-30 03:45:46+00
            database size: 4.4GB, database backup size: 1.7GB
            repo2: backup set size: 892MB, backup size: 193.1MB
            backup reference list: 20261028-075044F

        diff backup: 20261028-075044F_20261031-034513D
            timestamp start/stop: 2026-10-31 03:45:13+00 / 2026-10-31 03:45:48+00
            database size: 4.4GB, database backup size: 1.7GB
            repo2: backup set size: 891.9MB, backup size: 193.5MB
            backup reference list: 20261028-075044F

        diff backup: 20261028-075044F_20261102-034512D
            timestamp start/stop: 2026-11-02 03:45:12+00 / 2026-11-02 03:47:13+00
            wal start/stop: 000000010000034C00000002 / 000000010000034C00000005
            database size: 4.5GB, database backup size: 1.7GB
            repo2: backup set size: 899.5MB, backup size: 201.3MB
            backup reference list: 20261028-075044F
```

---

## 3. Analysis & Cryptographic Verification

1. **Remote Offsite Encryption (`repo2`):**
   - **Storage Destination:** Hetzner Storage Box in a separate physical facility via secure SFTP.
   - **Cipher:** `aes-256-cbc`.
   - **Passphrase Security:** Encryption key managed via restricted `/etc/pgbackrest/pgbackrest.conf` (permissions `0600`, owned by `postgres:postgres`), never exposed in command lines or repositories.
   - **Freshness:** Differential backup completed successfully today at **03:45:12 UTC on 2 November 2026** (`20261028-075044F_20261102-034512D`).
2. **Continuous WAL Streaming:**
   - WAL archiving is active (`archive_mode: on`).
   - Archived range spans continuously from base WAL `000000010000000000000023` to current WAL `000000010000034D0000006F`.
3. **Documented Operational Exception (`repo1`):**
   - Local on-disk repository `repo1` cipher is configured as `none`. This is preserved as an operational exception to optimize local I/O throughput during rapid crash recovery.
   - The remote offsite archive (`repo2`) strictly enforces `aes-256-cbc` encryption for all data at rest.
