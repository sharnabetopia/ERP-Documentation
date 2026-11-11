# Data Retention Policy

Document status: Draft v0.1 (Stage 3 part 2). Proposed. **Retention periods are TBD** because the legal requirements are unknown (OQ-022, OQ-010). The structure, rules and defaults below are for approval by the compliance officer after legal input. Placeholders marked "TBD" must not be used as real values.
Related: BR-059, FR-127, FR-129, NFR-027, NFR-040, BRL-063, [Data Protection](../06-RBAC-and-Security/Data-Protection.md) section 6, [Audit Trail](../06-RBAC-and-Security/Audit-Trail.md) section 8, [Backup and Recovery](../10-Deployment-and-Operations/Backup-and-Recovery.md).

## 1. Principles

1. Keep records for the longest applicable legal or business period, no longer than needed.
2. Retention is configuration per record type and company (FR-127), not code.
3. Deletion inside a retention period is blocked (RET-001 error).
4. A legal hold suspends deletion and anonymisation for the held records.
5. Posted financial records are never edited. Disposal means archival, then deletion or anonymisation after the period, with approval.
6. Ledger integrity is preserved when personal data is anonymised (Data Protection section 5).
7. Backups follow the same obligations (Backup and Recovery section 2).

## 2. Retention schedule

Retention clock starts at the "trigger" event. Period is TBD until OQ-022 is answered. The column "Design default" is a placeholder for the system, not a recommendation of law.

| ID | Record type | Trigger | Retention period | Design default | Disposal after period |
|---|---|---|---|---|---|
| RP-01 | General ledger entries, journals | End of fiscal year | TBD (legal accounting records) | Keep, archive after closed years | Delete after approval |
| RP-02 | Sales and purchase invoices, credit notes | End of fiscal year | TBD (tax law) | As RP-01 | As RP-01 |
| RP-03 | Payments, bank records, reconciliations | End of fiscal year | TBD | As RP-01 | As RP-01 |
| RP-04 | Tax records and returns | Filing date | TBD (tax law) | As RP-01 | As RP-01 |
| RP-05 | Purchase orders, GRNs, quotations, orders, deliveries | End of fiscal year | TBD | As RP-01 | Delete or archive |
| RP-06 | Stock ledger and valuation | End of fiscal year | TBD | As RP-01 | As RP-01 |
| RP-07 | Master data: vendors, customers, items (inactive) | Deactivation | TBD | Keep while referenced by retained documents | Anonymise personal fields, keep keys |
| RP-08 | Employee core records | End of employment | TBD (labour law) | Keep per law | Anonymise or delete |
| RP-09 | Audit log | Event date | At least as long as the records it covers (NFR-027) | Same as RP-01 | Archive, then delete after period |
| RP-10 | Approval history | Document closure | Same as the document | Same as document | With document |
| RP-11 | User accounts of leavers | Deactivation | TBD | Keep for history, minimise personal data | Anonymise |
| RP-12 | Attachments | Parent record retention | Same as the parent record | Same as parent | With parent |
| RP-13 | Sessions, tokens, idempotency records | Expiry | Days (technical) | Purge by job | Delete |
| RP-14 | Outbox events, notification logs | Publication or send | Days to weeks | Purge by job | Delete |
| RP-15 | Application and security logs | Event time | TBD, short for debug, longer for security | Per Monitoring and Logging | Delete |
| RP-16 | Import batches and error reports | Commit | TBD (suggest months) | Keep for support | Delete |
| RP-17 | Exports | Creation | Short (days) | Expire on server | Delete |
| RP-18 | Backups | Backup date | Per Backup and Recovery schedule and erasure rules | See plan | Expire |
| RP-19 | Personal data subject requests and anonymisation records | Request date | TBD (evidence of compliance) | Keep | Delete after period |
| RP-20 | Test and migration data | Environment rebuild | Until rebuild | Masked only | Delete |

Count: 20 record types. All periods marked TBD or technical need confirmation.

## 3. Mechanism

| Capability | Specification |
|---|---|
| Rule store | `retention_policy` per record type: years, legal hold flag (Database Design) |
| Enforcement | Delete and anonymise operations check the rule. Inside retention or under hold, refuse with RET-001 |
| Archival | Closed-year data moves to archive storage and stays searchable and reportable (NFR-042, ADR-016) |
| Disposal job | Scheduled job lists eligible records, a person approves, the job disposes and writes an audit record |
| Legal hold | Set by authorised role with reason, lists affected records, audited, blocks disposal until lifted |
| Evidence | Disposal log kept per RP-19 |

## 4. Disposal methods

| Method | Used for |
|---|---|
| Delete | Records with no further obligation and no references |
| Anonymise | Master and employee records still referenced by retained documents (replace personal fields, keep keys) |
| Archive then delete | Large historic data after period, with approval |
| Secure erase of backups | By expiry of the backup, not selective deletion, with documented timing |

## 5. Roles

| Role | Duty |
|---|---|
| Compliance officer | Approves schedule and periods |
| Data owners | Confirm periods for their domain and approve disposal lists |
| Organization Admin | Operates the retention rules and legal holds in the system |
| IT and database engineers | Operate archive and disposal jobs |
| Auditor | Reviews disposal records |

## 6. Review

Schedule reviewed yearly and when laws or business activity change. Changes go through change management and are audited (AUD-024).

## 7. Open points

OQ-022 (periods), OQ-010 (laws), OQ-016 (archive storage), OQ-019 (tenant-specific schedules). Legal hold ownership and procedure need approval.
