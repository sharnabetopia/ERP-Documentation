# Data Protection and Privacy Requirements

Document status: Draft v0.1 (Stage 2 part 2). Proposed. **No privacy law is named because the jurisdictions are unknown (OQ-010).** Nothing here is legal advice. A privacy or legal specialist must confirm which laws apply.
Related: [Security Requirements](Security-Requirements.md), [Audit Trail](Audit-Trail.md), BR-059, NFR-013, NFR-039, NFR-040, FR-127, FR-129.

## 1. Data classification

| Class | Definition | Examples | Minimum handling |
|---|---|---|---|
| Public | Approved for anyone | Published product names | None |
| Internal | Business data not for outsiders | Item catalogue, org structure, PR and PO content | Access control, TLS, encrypted at rest |
| Confidential | Commercially sensitive | Prices, stock valuation, customer lists, sales, margins | As Internal plus scope-limited access, export audited |
| Restricted | Harm if exposed: financial, personal, secret | Ledger, bank accounts, tax ids, employee personal data, credentials, audit log, backups | As Confidential plus field-level protection, masking, strict roles, step-up for bulk access |

## 2. Data inventory (MVP)

| Data category | Where | Class | Personal data | Controls |
|---|---|---|---|---|
| User account (name, email, password hash, MFA secret) | user_account, session | Restricted | Yes | Hashing, secret encryption, no export of hashes |
| Employee core (name, job, manager, branch) | employee | Internal to Restricted | Yes | Directory shows limited fields (FR-022) |
| Employee sensitive (national id, salary, bank) | R2 modules | Restricted | Yes | Field-level protection, HR roles only |
| Vendor and customer contacts | vendor, customer, customer_contact | Confidential | Often | Masked in exports unless entitled |
| Vendor bank account numbers | vendor_bank_account | Restricted | Sometimes | Field-level encryption, masked display, approval for change |
| Tax identifiers | company, vendor, customer | Confidential or Restricted | Sometimes | Masked display where entitled-only |
| Financial transactions | ledger_entry, payments, invoices | Restricted | Indirect | Immutable, scoped, audited |
| Stock and valuation | stock_movement, cost_layer | Confidential | No | Scoped |
| Audit records | audit_log | Restricted | Yes (actor, IP) | Append-only, restricted read, masking of values |
| Attachments | object storage | Confidential or Restricted | Possible | Scan, private storage, access follows the record |
| Logs and traces | log store | Internal | Possible | Masking, retention limit (SEC-039) |
| Backups | backup storage | Restricted | Yes | Encrypted, separate keys, restore tested |

## 3. Protection requirements

| ID | Requirement | Linked |
|---|---|---|
| DP-001 | Encrypt data in transit and at rest with current algorithms (SEC-035, SEC-036). | NFR-012, NFR-013 |
| DP-002 | Encrypt Restricted fields at field level. Keys are managed in a key store, rotated, and separated per tenant where tenancy needs it (OQ-019). | SEC-037 |
| DP-003 | Mask Restricted fields in lists, logs, notifications and default exports. Unmask only for entitled roles, and audit each unmask or export. | SEC-037, SEC-045 |
| DP-004 | Collect only the personal data the module needs (data minimisation). New fields need a stated purpose. | NFR-039 |
| DP-005 | Use production personal data in non-production environments only when masked or synthetic. | SEC-039 |
| DP-006 | Backups are encrypted, access restricted, and cover the same retention and erasure obligations as live data (see section 6). | NFR-011 |
| DP-007 | Data is stored and processed in the regions the sponsor and law permit. Region is TBD. | OQ-010 |
| DP-008 | Notifications and emails carry no Restricted data. They link to the system instead. | SEC-039 |
| DP-009 | Exports to files are classified, watermarked with user and time where feasible, and expire on the server. | FR-050 |
| DP-010 | Data processing agreements exist with every provider that handles personal data. | OQ-016, OQ-017 |

## 4. Privacy rights support (to be confirmed per law)

| Right (generic) | System support | Linked |
|---|---|---|
| Access and portability | Export of a person's data from master records, documents and audit references | FR-129 |
| Correction | Standard update with audit | FR-008 |
| Erasure or restriction | Anonymisation of master records where law permits. Records needed by law or for ledger integrity are kept, with personal fields masked | FR-129, BRL-063 |
| Objection and consent management | TBD, depends on lawful basis and jurisdiction | OQ-010 |
| Breach notification | Procedure and timelines depend on jurisdiction. Incident process in Stage 3 | SEC-048 |

## 5. Anonymisation approach

1. Ledger, stock and document rows keep party keys, not names (see [Database Design](../04-System-Architecture/Database-Design.md) section 6).
2. Anonymisation replaces personal fields of the master record (name, contact, address, identifiers) with a fixed token and keeps the record id, so reports and totals stay unchanged.
3. Attachments and free-text notes that may hold personal data are listed for manual review or deletion.
4. Audit records keep the actor id but mask personal values stored in before and after fields where law requires.
5. Anonymisation is blocked while retention rules or a legal hold apply (RET-001).
6. Each anonymisation is itself audited and irreversible.

## 6. Retention and disposal

| Data | Rule (values TBD, OQ-022) |
|---|---|
| Financial records | Longest applicable legal period. Archive after closed-year age. Delete only after the period and approval |
| Audit records | At least as long as the financial records |
| Employee records | Employment plus legally required period |
| User accounts of leavers | Deactivated and retained for history. Personal data minimised after the retention window |
| Sessions, tokens, idempotency keys, outbox events | Short, purged by job |
| Logs | Short retention, longer for security logs. Values TBD |
| Backups | Retention set so that erasure is effective within a defined time after the live deletion |
| Legal hold | A flag that suspends deletion and anonymisation (retention_policy.legal_hold) |

Data retention policy detail will be in [Data Retention Policy](../11-Compliance-and-Governance/Data-Retention-Policy.md).

## 7. Cross-border and provider considerations

| Topic | Note |
|---|---|
| Data residency | Region of primary, replica, backups and DR must be allowed (OQ-010) |
| Transfer of personal data across borders | Needs a legal basis if it occurs (support, monitoring tools, providers). Decision TBD |
| Provider access | Provider staff access to customer data is limited and logged where the provider offers it |
| Subprocessors | List kept and reviewed |

## 8. Privacy by design checklist for new features

| Question | Required answer |
|---|---|
| What personal data and why? | Documented purpose |
| Can it be avoided or reduced? | Yes or no with reason |
| Who can see it? | Roles and scope |
| How long is it kept? | Retention rule |
| How is it protected, exported, anonymised? | Controls named |
| Does it leave the system? | Recipient and legal basis |

## 9. Open points

OQ-010 (laws and residency), OQ-022 (retention periods), OQ-016 (provider terms), OQ-017 (partners that receive data). A data protection impact assessment is recommended before go-live if employee or customer personal data at scale is processed. The need is a legal determination.
