# Maintenance and Operations SOP

Document status: Draft v0.1 (Stage 3 part 2). Proposed. Outline of recurring tasks. Task details become runbooks once the stack and hosting are chosen (OQ-016, OQ-024). Frequencies are proposals.
Related: [Monitoring and Logging](Monitoring-and-Logging.md), [Backup and Recovery](Backup-and-Recovery.md), [SLA and Support](SLA-and-Support.md), [Change Management](../09-Development-and-Delivery/Change-Management.md).

## 1. Principles

1. Planned maintenance happens in agreed windows with prior notice.
2. Routine tasks follow a checklist and are recorded.
3. Anything that changes production goes through change management (standard, normal or emergency).
4. No manual edits of ledger, stock or audit data. Corrections are documents.
5. Privileged access is by exception, with MFA, and is logged (AUD-092).

## 2. Routine task schedule

| ID | Task | Frequency | Owner | Record |
|---|---|---|---|---|
| MNT-01 | Review alerts, dashboards and failed jobs | Daily | Operations | Shift log |
| MNT-02 | Check backups succeeded, review backup size trends | Daily | Operations | Backup report |
| MNT-03 | Dead-letter and integration queue review | Daily | Support Tier 2 | Ticket per item |
| MNT-04 | Reconciliation job results: AP, AR, stock to ledger, summaries to rows | Daily | Finance operations | Report, S1 on ledger differences |
| MNT-05 | Review security events and privilege changes | Daily, weekly summary | Security officer | Security log |
| MNT-06 | Patch operating systems, containers, dependencies (security updates first) | Monthly, critical within days | Operations with QA | Change record |
| MNT-07 | Certificate and secret expiry review, rotation | Monthly | Operations, security | Rotation log |
| MNT-08 | Database maintenance: statistics, index health, partitions for ledger, stock and audit tables | Weekly and monthly | Database engineer | Maintenance log |
| MNT-09 | Archive closed-year data per retention policy | Annually and as policy says | Database engineer, Finance | Archive report |
| MNT-10 | Purge sessions, tokens, idempotency records, outbox, old logs | Daily jobs, monthly check | Operations | Job report |
| MNT-11 | User access review: roles, scope, temporary grants, inactive users, SoD overrides | Quarterly | Organization Admin, Auditor | Signed review |
| MNT-12 | Review approval chains and delegations for leavers and changes | Quarterly | Finance Manager | Review note |
| MNT-13 | Restore test and DR component test | Quarterly | Operations | Test report |
| MNT-14 | Capacity review: storage, database size, performance trend | Quarterly | Operations, architect | Capacity report |
| MNT-15 | Vulnerability scan and review of findings | Weekly scan, monthly review | Security officer | Findings log |
| MNT-16 | Review SLA performance and incident trends | Monthly | Support manager | Service report |
| MNT-17 | Exchange rate source check, tax rate updates, calendars (fiscal periods created ahead) | Monthly and annually | Finance operations | Checklist |
| MNT-18 | Review feature flags and remove obsolete ones | Quarterly | Product owner | Flag list |
| MNT-19 | Documentation review: runbooks, contact lists, DR plan | Twice a year | Operations lead | Review log |
| MNT-20 | Audit hash chain verification report | Daily job, monthly review | Security officer | AUD-094 record |

## 3. Period-end operations support

Before month end: confirm periods exist, rates loaded, close checklist guidance sent, no planned maintenance in the close window. During close: increased monitoring, support on call for Finance. After close: confirm statements generated, backups taken after close. Before year end: fiscal year created, year-end procedure rehearsed on Staging (UC-005, FR-061).

## 4. Standard operational procedures (to be written as runbooks)

| ID | Procedure | Status |
|---|---|---|
| RB-07 | Create tenant, company, first administrator | Not written |
| RB-08 | Rotate a secret or key | Not written |
| RB-09 | Add or remove a user at request (standard change) | Not written |
| RB-10 | Unlock an account, revoke sessions | Not written |
| RB-11 | Retry or cancel an integration queue item | Not written |
| RB-12 | Re-run a failed background job safely | Not written |
| RB-13 | Rebuild summary tables from ledgers | Not written |
| RB-14 | Reopen a period (business process with approval, not an operations action) | Not written |
| RB-15 | Respond to a suspected security incident | Not written |
| RB-16 | Onboard a new branch or company | Not written |

Backup and DR runbooks RB-01 to RB-06 are listed in Backup and Recovery. All sixteen runbooks are outstanding (OPS-GAP-01).

## 5. Maintenance windows and notices

Planned windows outside business hours of the main time zones (TBD). Notice at least 5 working days for planned work with downtime. Emergency maintenance announced as soon as decided. Rolling deployments avoid downtime for application changes.

## 6. Access and separation of duties in operations

| Rule | Detail |
|---|---|
| Engineers cannot approve their own production changes | Two-person rule |
| Production data access for support is read-only and masked where possible | Unmask by approval |
| Database operators cannot edit append-only tables (DBC-03) | THR-044 control |
| Break-glass accounts sealed, tested, alerted on use | Reviewed after each use |

## 7. Documentation of operations

Environment log, change log, incident log, problem log, maintenance logs, review records. Kept with retention per policy and available to auditors.

## 8. Open points

Tool-specific procedures (OQ-024), windows and staffing (OQ-014, OQ-028), retention for archive (OQ-022).
