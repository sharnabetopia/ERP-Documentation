# Audit Requirements

Document status: Draft v0.1 (Stage 3 part 2). Proposed. These requirements describe what internal and external auditors need from the ERP. They are not tied to a named audit standard (OQ-018, OQ-010).
Related: [Audit Trail](../06-RBAC-and-Security/Audit-Trail.md) (event catalogue AUD-001 to AUD-100), [Compliance Requirements](Compliance-Requirements.md), FR-030 to FR-032, BR-005, BRL-007, NFR-026, NFR-027.

## 1. Auditor needs

| Need | How the ERP supports it |
|---|---|
| Independent, read-only access | Auditor role with read, export and view only (RBAC Matrix, SEC-018) |
| Complete population for sampling | Exports of documents, ledger entries, approvals and audit records within scope, with row counts (FR-050) |
| Traceability from statements to source documents | Drill-down from reports to ledger to document (FR-066) |
| Evidence of controls | Approval history, SoD reports, access reviews, period close records, reconciliation results |
| Evidence that data was not altered | Immutable postings, append-only audit, hash chain, verification reports (TR-003, Audit Trail section 5) |
| Understanding of the system | Documentation package, data dictionary, API and RBAC documents, change log |
| Time-bound view | Period filters, as-at balances, prior-period comparatives |

## 2. Requirements

| ID | Requirement | Linked | Pri |
|---|---|---|---|
| AUR-001 | The Auditor role can search and export audit records, documents and reports for assigned companies without any ability to change data | FR-031, SEC-018 | P0 |
| AUR-002 | Every query and export by an auditor is itself audited | AUD-072 | P0 |
| AUR-003 | Posted entries, audit records and approvals are immutable and verifiable | FR-064, FR-032, DBC-03 | P0 |
| AUR-004 | Any financial figure can be traced to source documents and their approval history | FR-066, FR-041 | P0 |
| AUR-005 | Standard audit reports exist: access changes, privileged actions, financial overrides, manual journals, stock adjustments, delegations, exports, refused actions | Audit Trail section 7 | P0 |
| AUR-006 | A report of all manual journals shows maker, checker, date, amounts and narration | FR-062, FR-063 | P0 |
| AUR-007 | A SoD report lists conflicts, overrides granted, reasons, approvers and end dates | FR-017, RBAC Matrix | P0 |
| AUR-008 | Reconciliation evidence (AP, AR, stock valuation, summaries to rows) can be produced for any date | NFR-023, INV-03 to INV-06 | P0 |
| AUR-009 | Period close and reopen history with reasons and approvers is available | FR-058 to FR-060, AUD-050 | P0 |
| AUR-010 | Document numbering continuity report (gaps and reuse), subject to the gap policy | FR-025, OQ-031 | P1 |
| AUR-011 | Audit records are retained at least as long as the financial records they cover | NFR-027, RP-09 | P0 |
| AUR-012 | Audit data volume is managed by partitioning and archival without losing searchability | Database Design section 4, ADR-016 | P0 |
| AUR-013 | Hash chain verification runs on a schedule and its results are available to the auditor | AUD-094, MON-021 | P0 |
| AUR-014 | The auditor can see the configuration in force at a date: approval chains, limits, tolerances, tax codes, posting rules, with change history | FR-027, AUD-021 to AUD-023 | P0 |
| AUR-015 | Access reviews (users, roles, scope, temporary grants) are supported by a report and signed off in the system or by attached evidence | Maintenance SOP MNT-11 | P1 |
| AUR-016 | Change and release evidence is available: release notes, test summary, approvals | Release Management, Change Management | P1 |
| AUR-017 | Backup and restore test records are available | Backup and Recovery section 5 | P1 |
| AUR-018 | Exports to auditors are produced in open formats and carry user, time and scope information | FR-050, TR-014 | P0 |
| AUR-019 | The system supports read-only auditor access for external auditors with time limits (temporary role grant, MFA) | FR-016, SEC-006 | P1 |
| AUR-020 | Disposal and anonymisation actions are auditable | AUD-080, RP-19 | P0 |

Requirements: 20.

## 3. Audit reports (proposed list)

| ID | Report | Parameters | Audience |
|---|---|---|---|
| AUR-R1 | Access change report | Period, user, role | Auditor, security officer |
| AUR-R2 | Privileged actions report | Period | Auditor |
| AUR-R3 | Financial overrides (duplicate invoice, variance, credit limit) | Period, company | Auditor, Finance Manager |
| AUR-R4 | Manual journals | Period | Auditor |
| AUR-R5 | Stock adjustments and negative stock overrides | Period, warehouse | Auditor, Inventory Manager |
| AUR-R6 | Delegations and escalations | Period | Auditor |
| AUR-R7 | Exports and bulk reads | Period, user | Auditor, security officer |
| AUR-R8 | Refused actions by user | Period | Security officer |
| AUR-R9 | Vendor master changes including bank changes | Period | Auditor, Finance Manager |
| AUR-R10 | Reconciliation status | Date | Auditor, Finance Manager |

These reports are not yet specified at column level (gap AUG-01). Financial audit reports module M-048 is R2.

## 4. Audit cycle support

| Activity | Support |
|---|---|
| Planning | Provide documentation, roles, system description |
| Walkthroughs | Test environment with realistic data, UAT scenarios as demonstrations |
| Control testing | Reports in section 3, sampling exports |
| Substantive testing | Ledger and document exports, drill-down |
| Reporting | Management responses tracked as change requests or actions |
| Follow-up | Evidence of fixes through release records |

## 5. Gaps

| ID | Gap |
|---|---|
| AUG-01 | Audit reports not specified at column and filter level |
| AUG-02 | Named audit standard or regulator requirements unknown |
| AUG-03 | Sampling tools (random or statistical selection) not specified |
| AUG-04 | External auditor access procedure (identity checks, time limits) not defined |

## 6. Open points

OQ-018 (framework), OQ-010 (jurisdiction), OQ-022 (retention), OQ-031 (numbering), OQ-032 (SoD override reporting).
