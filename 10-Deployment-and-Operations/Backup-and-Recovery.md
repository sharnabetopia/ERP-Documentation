# Backup and Recovery Plan

Document status: Draft v0.1 (Stage 3 part 2). Proposed. RPO and RTO are **proposed** (15 minutes and 4 hours, NFR-009, NFR-010) and not agreed (OQ-028).
Related: NFR-009 to NFR-011, [Disaster Recovery](Disaster-Recovery.md), [Infrastructure Architecture](../04-System-Architecture/Infrastructure-Architecture.md), [Data Protection](../06-RBAC-and-Security/Data-Protection.md), [Performance Test Plan](../08-QA-and-Testing/Performance-Test-Plan.md) (restore test case).

## 1. Scope of protection

| Asset | Backed up | Method |
|---|---|---|
| Primary database (ledgers, documents, master data, audit) | Yes | Continuous log archiving plus scheduled full backups |
| Object storage (attachments, exports) | Yes | Versioning and cross-zone or cross-region replication |
| Configuration and infrastructure code | Yes | Version control, separate backup of the repository |
| Secrets and keys | Yes | Secret and key store backups, with recovery procedure and separate access (key loss makes encrypted backups useless) |
| Container images and artifacts | Yes | Registry replication |
| Audit hash anchors (outside the primary database) | Yes | Write-once storage |
| Monitoring and log data | By policy | Lower priority |

## 2. Backup schedule (proposed)

| Type | Frequency | Retention | Purpose |
|---|---|---|---|
| Continuous log archiving | Continuous | 14 days | Point-in-time recovery, RPO 15 minutes or better |
| Full database backup | Daily | 35 days | Base for restore |
| Long-term full backup | Monthly | Per legal retention (OQ-022), for example 7 years or more | Legal and audit |
| Object storage versions | Continuous | 35 days for deleted or overwritten versions | Recover from deletion |
| Configuration snapshots | On change | Per release | Rebuild |

Retention values are placeholders pending OQ-022. Backups must also respect erasure obligations: data erased from the live system is removed from backups within a defined period (Data Protection section 6), or kept encrypted and excluded from restores of personal data.

## 3. Backup rules

1. Backups are encrypted at rest and in transit with keys separate from the data keys (SEC-036).
2. Stored in a different failure domain from the primary (another zone, and another region if the DR level requires it).
3. Access is restricted and logged. No developer access to production backups.
4. Immutability or write protection for a copy so ransomware or an insider cannot delete all copies.
5. Backup success is monitored (MON-022). A missed backup is an S1 alert.
6. Backups never include data from other tenants in a way that prevents per-tenant restore. A tenant-level restore procedure exists (ADR-002 consequence) or is a stated gap.

## 4. Restore scenarios

| Scenario | Procedure | Target |
|---|---|---|
| Accidental deletion of a file | Restore previous object version | Minutes |
| Corrupted or wrongly changed rows (logical error) | Point-in-time restore into a side database, extract rows, apply corrective documents (never overwrite posted ledgers) | Hours |
| Database failure in one zone | Automatic failover to standby | Minutes |
| Full database loss | Restore latest full plus log archive to the required point | Within RTO |
| Tenant-level restore | Restore to a side environment, export the tenant, reload through controlled procedure | Hours, procedure TBD |
| Ransomware or malicious change | Isolate, restore from immutable copy, rotate secrets, forensics | Per DR plan |

Posted financial data is not edited in place during recovery. Differences found after restore are resolved by corrective postings so the audit trail stays continuous.

## 5. Restore testing

| Test | Frequency | Pass condition |
|---|---|---|
| Restore of latest full backup to an isolated environment and reconciliation checks (INV-01 to INV-06) | Quarterly (NFR-011) | Data complete, reconciliations pass |
| Point-in-time restore drill | Twice a year | Recovery to chosen time, measured data loss within RPO |
| Full recovery timing test | Annually and before go-live | Within RTO |
| Object version restore | Twice a year | Files restored |
| Key recovery drill | Annually | Backups decryptable using recovery procedure |

Results are recorded with date, duration, data loss, issues, and follow-up actions. A failed restore test is an S2 issue until fixed.

## 6. Roles

| Role | Duty |
|---|---|
| Operations lead | Owns schedule, monitoring and tests |
| Database engineer | Executes restores |
| Security officer | Reviews access and encryption |
| Finance Manager | Verifies business data after a restore |
| Incident commander | Decides on restore actions during incidents |

## 7. Runbooks to be written

RB-01 file restore, RB-02 point-in-time restore, RB-03 full database restore, RB-04 tenant restore, RB-05 key recovery, RB-06 post-restore verification. They are not written yet (gap OPS-GAP-01).

## 8. Open points

OQ-028 (RPO, RTO), OQ-022 (retention), OQ-016 (backup tooling and storage), OQ-019 (tenant-level restore need).
