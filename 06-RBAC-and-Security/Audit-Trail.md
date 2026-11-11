# Audit Trail Specification

Document status: Draft v0.1 (Stage 2 part 2). Proposed.
Related: FR-030 to FR-032, BR-005, BRL-007, NFR-026, NFR-027, [ADR-011](../04-System-Architecture/Architecture-Decision-Records.md), [Database Design](../04-System-Architecture/Database-Design.md) (`audit_log`), [Security Requirements](Security-Requirements.md).

## 1. Principles

1. Complete: every in-scope event has a record.
2. Atomic: the audit record and the business change commit together or not at all (FR-032).
3. Immutable: no role, including Super Admin and database operators using the application role, can edit or delete audit records (BRL-007, DBC-03).
4. Attributable: every record names the actor, and the delegator when delegation applies.
5. Useful: a user with permission can answer who, what, when, where and why for any record.
6. Protected: audit data is Restricted ([Data Protection](Data-Protection.md)).

## 2. Record structure

| Field | Description | Required |
|---|---|---|
| id | Unique id | Yes |
| occurred_at | Event time, UTC, system generated | Yes |
| tenant_id, company_id | Isolation keys | Yes (company where applicable) |
| actor_user_id | User who acted | Yes. System jobs use a system actor id |
| on_behalf_of_user_id | Delegator, if any | If delegation |
| actor_role_context | Role assignment used for the action | Yes |
| action | From the audit action code list | Yes |
| entity_type, entity_id | Target record | Yes |
| before, after | Changed fields only, masked where required | For changes |
| reason | Required for reject, reopen, override, adjustment, reversal | Where the rule requires |
| source | Channel: UI, API, import, job, integration | Yes |
| client_ip, user_agent | Source network data | Yes for interactive actions |
| trace_id | Correlation id, same as in logs | Yes |
| outcome | Success or refused, with refusal code | Yes |
| prev_hash, record_hash | Hash chain fields for tamper evidence | Yes |

## 3. Event catalogue

Event IDs `AUD-nnn`. All are MVP unless marked.

| ID | Category | Event | Detail captured |
|---|---|---|---|
| AUD-001 | Authentication | Sign-in success | Method, MFA used |
| AUD-002 | Authentication | Sign-in failure | Reason class, source |
| AUD-003 | Authentication | Lockout and unlock | Trigger, actor |
| AUD-004 | Authentication | Password change, reset request and completion | Actor, source |
| AUD-005 | Authentication | MFA enrol, disable, failure | |
| AUD-006 | Authentication | Session revoked, expired by policy | Actor |
| AUD-007 | Authentication | Step-up success and failure | Action that required it |
| AUD-010 | Access | User created, invited, activated, suspended, deactivated | Before and after |
| AUD-011 | Access | Role created, changed, retired | Permissions added and removed |
| AUD-012 | Access | Role assignment granted, changed, expired, removed | Scope, validity, reason |
| AUD-013 | Access | SoD conflict blocked | Pair, actor |
| AUD-014 | Access | SoD override granted | Approver, reason, end date |
| AUD-015 | Access | Authorisation refusal | Permission or scope, endpoint |
| AUD-016 | Access | Delegation created, revoked, ended | Delegator, delegate, types, dates |
| AUD-020 | Configuration | Company, branch, department, cost centre changes | Before and after |
| AUD-021 | Configuration | Settings, tolerances, numbering changes | Before and after |
| AUD-022 | Configuration | Approval chain created or changed | Steps and limits |
| AUD-023 | Configuration | Tax code, currency rate, posting rule changes | Before and after |
| AUD-024 | Configuration | Retention rule or legal hold change | |
| AUD-030 | Master data | Vendor created, changed, activated, blocked | Before and after |
| AUD-031 | Master data | Vendor bank detail change proposed and approved | Masked values, both users |
| AUD-032 | Master data | Customer, item, category, warehouse changes | Before and after |
| AUD-033 | Master data | Credit limit change | Old and new |
| AUD-034 | Master data | Chart of accounts changes | Before and after |
| AUD-040 | Documents | Create, update (draft), delete (draft) | Changed fields |
| AUD-041 | Documents | Submit, approve, reject, return, cancel | Decision, reason, round |
| AUD-042 | Documents | Posting | Document, ledger and stock effect references |
| AUD-043 | Documents | Reversal | Link to original, reason |
| AUD-044 | Documents | Duplicate invoice override | Reason, approver |
| AUD-045 | Documents | Match variance accepted | Variance, approver |
| AUD-046 | Documents | Credit limit breach approved | Exposure, limit, approver |
| AUD-050 | Finance | Period soft-close, close, reopen | Reason, step-up |
| AUD-051 | Finance | Year-end close | Results transferred |
| AUD-052 | Finance | Payment created, approved, released, failed, reversed | Idempotency key reference |
| AUD-053 | Finance | Manual journal posted | Maker and checker |
| AUD-054 | Finance | Exchange rate manual entry or correction | Old and new |
| AUD-060 | Inventory | Stock adjustment posted | Reason, value impact |
| AUD-061 | Inventory | Transfer dispatched and received | Quantities |
| AUD-062 | Inventory | Valuation run | Method, totals |
| AUD-063 | Inventory | Negative stock override (if policy allows) | Item, quantity |
| AUD-070 | Data movement | Import validated and committed | Batch, counts, mode |
| AUD-071 | Data movement | Export requested and delivered | Scope, row count, fields |
| AUD-072 | Data movement | Audit log viewed or exported | Filters |
| AUD-073 | Data movement | Attachment uploaded, downloaded, removed | File id, scan result |
| AUD-074 | Data movement | Restricted field unmasked | Field, record |
| AUD-080 | Privacy | Anonymisation request and execution | Subject reference, scope |
| AUD-081 | Privacy | Personal data export for a subject | |
| AUD-090 | System | Integration call failed, retried, cancelled by user | Partner, attempts |
| AUD-091 | System | Background job failure or dead-letter | Job, error class |
| AUD-092 | System | Configuration change by operator, deployment marker | Operator, version |
| AUD-093 | System | Audit write failure detected | Alert |
| AUD-094 | System | Integrity verification run and result | See section 5 |
| AUD-100 | Reserved (R2) | Payroll, salary change, bank reconciliation events | Added with those modules |

### 3A. Authorisation hardening events (v0.2)

| ID | Category | Event | Detail |
|---|---|---|---|
| AUD-101 | Access control | Role grant requested | Grantor, grantee, role, scope, tier |
| AUD-102 | Access control | Role grant approved | Approver, request id |
| AUD-103 | Access control | Role grant rejected or expired | Reason |
| AUD-104 | Access control | Role grant refused: tier or scope violation | Grantor, role, rule |
| AUD-105 | Access control | Self-grant attempt refused | Actor, attempted action |
| AUD-106 | Finance | Payment released | Preparer, approver, releaser, step-up time |
| AUD-107 | Finance | Payment release or reversal refused | Rule (duty conflict, step-up, limit) |
| AUD-108 | Finance | Payment reversal requested and decided | Requester, approver, reason |
| AUD-109 | Master data | Vendor bank change requested | Requester, vendor, old and new masked |
| AUD-110 | Master data | Vendor bank change verified | Verifier, method, outcome |
| AUD-111 | Master data | Vendor bank change approved or rejected | Approver, reason |
| AUD-112 | Finance | First payment held to new account, hold released | Hold end, releaser |
| AUD-113 | Master data | Credit limit change requested and decided | Old and new value, requester, approver |
| AUD-114 | Finance | Period reopen requested | Period, reason, requester |
| AUD-115 | Finance | Period reopen approved or rejected | Approver, window |
| AUD-116 | Finance | Period closed automatically after window | Period, time |
| AUD-117 | Finance | Posting made in a reopened period | Document, user |
| AUD-118 | Configuration | Controlled change requested and decided | Key, old and new value, requester, approver |
| AUD-119 | Access control | SoD override requested, approved, expired | Rule, user, period, requester, approver |
| AUD-120 | Access control | Record-level SoD refusal | User, record, rule |
| AUD-121 | Workflow | Delegation created, used, refused | Delegator, delegate, types, dates |
| AUD-122 | Emergency | Break-glass released | Two releasers, reason, expiry |
| AUD-123 | Emergency | Break-glass action | Every action in the session |
| AUD-124 | Emergency | Break-glass closed and reviewed | Reviewer, finding |
| AUD-125 | Access control | Service account created, approved, token issued or rotated | Owner, scopes, expiry |
| AUD-126 | Documents | Attachment voided | Document, reason, actor |
| AUD-127 | Data movement | Export run | Module, row count, step-up, permission used |

## 4. Capture rules

| Rule | Detail |
|---|---|
| What is changed | Only changed fields in before and after, to limit volume and exposure |
| Sensitive values | Masked or hashed for Restricted fields where law or policy requires. The fact of change is still recorded |
| Reads | Reads are not audited in general. Sensitive reads (AUD-072, AUD-074, bulk export) are |
| Failure | If the audit insert fails the business transaction fails (FR-032). A fallback buffer is not used for financial events |
| Time | Server time, UTC. Client time is never trusted |
| Bulk operations | Import and mass update write one batch record plus per-row records for key entities |
| System actors | Jobs and integrations use named system actors, never a human id |
| Volume | Partitioned monthly (Database Design section 4) |

## 5. Tamper evidence

| Item | Proposal |
|---|---|
| Hash chain | Each record stores the hash of the previous record in its partition and its own hash over content and previous hash |
| Verification | A scheduled job recomputes chains and reports breaks (AUD-094). A break raises a Critical alert |
| Anchoring | Periodic digest of the latest hash is stored outside the primary database, in write-once storage, so deletion of a whole range is detectable |
| Database rights | The application role has insert and select only (DBC-03). Administrative access to bypass is logged and alerted (THR-044) |
| Status | Proposed. The hash design is not yet reviewed (open for the architect) |

## 6. Access to audit data

| Role | Access |
|---|---|
| Auditor | Read and export business and security events within assigned companies (`platform.audit.read.business`, `platform.audit.read.security`, `platform.audit.export`) |
| Super Admin | Read and export tenant-wide security events only (`platform.audit.read.security`, `platform.audit.export`) |
| Organization Admin | Business events within company (`platform.audit.read.business`), no export of Restricted values |
| Branch Admin, Finance Manager | Business events for their scope and entity types only (`platform.audit.read.business`). No security events |
| Others | Own sign-in history only (`platform.audit.read.own`) |

Auditors can filter by actor, entity, action, date, company, branch and outcome (FR-031). Every audit query and export is itself audited (AUD-072).

## 7. Standard audit reports (proposed)

| Report | Purpose |
|---|---|
| Access changes | Who granted what to whom, SoD overrides, temporary access |
| Privileged actions | Period reopen, chain changes, settings changes |
| Financial overrides | Duplicate invoice overrides, variance acceptances, credit limit breaches |
| Manual journals | All manual journals with maker and checker |
| Stock adjustments | Reasons and value impact |
| Delegations | Active and used delegations |
| Exports | Volume and recipients of exports |
| Failed and refused actions | Pattern of refusals by user |

## 8. Retention and archival

Retention is at least as long as the financial records (NFR-027, OQ-022). Old partitions move to archive storage and remain searchable by an Auditor on request. Legal hold suspends deletion.

## 9. Open points

OQ-022 (retention), OQ-010 (residency of audit data), hash chain design review, volume estimates (OQ-015).
