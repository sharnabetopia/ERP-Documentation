# Non-Functional and Technical Requirements

Document status: v0.2 Phase 2 remediation, 2026-11-11: the 21 NFRs that had no measurable target now have one (section 1, column "Proposed target", and section 1A). Proposed values need stakeholder approval and are not final business targets.
Previous status: Draft v0.1 (Stage 2). Proposed.
All numeric targets are **proposed engineering defaults, not agreed service levels**.
They need confirmation through OQ-015 (volumes), OQ-016 (hosting) and OQ-028 (service levels).
A target marked "TBD" has no sensible default without business input.

Related: [FRS](FRS.md), [BRD](../02-Business-Analysis/BRD.md) (BR-062), [ARD](../04-System-Architecture/ARD.md).

## 1. Non-functional requirements

| ID | Category | Title | Requirement | Proposed target | BR / BRL | Pri | Verif. |
|---|---|---|---|---|---|---|---|
| NFR-001 | Performance | Interactive response | Typical screen loads and saves respond quickly under normal load. | 95th percentile under 2 s for reads and under 3 s for saves at the sized load | BR-062 | P0 | T |
| NFR-002 | Performance | Report response | Standard reports return in acceptable time for a full fiscal year of data. | 95th percentile under 10 s. Longer reports run in the background | BR-058, BR-062 | P1 | T |
| NFR-003 | Performance | Posting throughput | Posting documents (invoice, receipt, payment) completes within the interactive target and sustains the peak rate. | Proposed: sustain 20 postings per second (invoice, receipt, payment mix) for 30 minutes with 95th percentile save under 3 s, 0 failed and 0 duplicate postings | BR-062 | P0 | T |
| NFR-004 | Performance | Search | List search with filters returns on indexed fields. | 95th percentile under 2 s for 1 million rows per list | BR-062 | P1 | T |
| NFR-005 | Scalability | Volume | The design supports the sized numbers of companies, branches, users and annual transactions. | Proposed: 100 companies, 2,000 concurrent users, 20 million ledger and stock rows per year, 5 years retained (100 million rows). NFR-001, NFR-002 and NFR-004 targets still hold at this volume | BR-062 | P0 | A, T |
| NFR-006 | Scalability | Growth | Adding companies, branches or modules needs configuration or deployment of the new module, not redesign of existing ones. | Adding a company or branch needs 0 code deployments and at most 1 hour of administrator work. Adding a module changes 0 files in the public interface of existing modules | BR-001, BR-061 | P0 | A |
| NFR-007 | Availability | Uptime | The system is available during business hours with planned maintenance windows. | 99.5 percent monthly for MVP (proposed). Final per OQ-028 | BR-062 | P0 | T |
| NFR-008 | Availability | Degraded mode | Failure of an external provider (email, bank, gateway) does not stop core posting. | With email, bank adapter and gateway simulated down for 4 hours: 100 percent of postings commit, outbox backlog returns to 0 within 30 minutes of recovery, 0 events lost | BR-060 | P0 | T |
| NFR-009 | Recovery | RPO | The maximum data loss after a failure. | 15 minutes proposed. Final per OQ-028 | BR-062 | P0 | T |
| NFR-010 | Recovery | RTO | The maximum time to restore service after a disaster. | 4 hours proposed. Final per OQ-028 | BR-062 | P0 | T |
| NFR-011 | Backup | Backups | Backups are automatic, encrypted, stored apart from primary, and restore-tested. | Daily full plus continuous log. Restore test each quarter | BR-062 | P0 | T |
| NFR-012 | Security | Transport encryption | All network traffic is encrypted in transit. | Current TLS version, no plain HTTP | BR-004 | P0 | T |
| NFR-013 | Security | Encryption at rest | Databases, file stores and backups are encrypted at rest. Sensitive fields (bank details, tax ids, personal data) get additional field-level protection. | 256-bit security strength encryption at rest for databases, object storage and backups. Bank and tax identifiers also protected at field level. Keys in a managed key store, rotated at least every 12 months. 0 of 1,000 sampled sensitive fields readable as plain text in raw storage | BR-004, BR-059 | P0 | I |
| NFR-014 | Security | Secrets | Secrets are never in source control and are rotated. | 0 secrets in source control history at release (full-history scan). Secrets rotated at least every 90 days. A quarterly rotation drill completes with 0 outage. Service tokens expire within 24 hours | BR-004 | P0 | I |
| NFR-015 | Security | Web vulnerabilities | The application resists the OWASP Top 10 including XSS, CSRF, SQL injection and IDOR. | No open High findings at release | BR-003, BR-004 | P0 | T |
| NFR-016 | Security | Authorisation on every call | Every request is authorised server-side regardless of what the client hides. | 100 percent of endpoints | BR-003 | P0 | T, I |
| NFR-017 | Security | Rate limiting | Sign-in, reset, export and API calls are rate limited per user and per address. | Sign-in 5 failures per account per 15 minutes then lockout (SEC-003). 20 attempts per address per minute then 429. Password reset 3 per account per hour. Exports 5 per user per hour. API 600 requests per minute per token | BR-004 | P0 | T |
| NFR-018 | Security | Tenant isolation | Data isolation between companies and tenants is enforced in the data layer, not only in the application code. | No cross-tenant access in isolation tests | BR-001 | P0 | T |
| NFR-019 | Security | Dependency hygiene | Third-party components are scanned and updated. | No known Critical vulnerability at release | BR-004 | P0 | T |
| NFR-020 | Data integrity | Money precision | Monetary values use exact decimal types with currency-specific scale and a configured rounding rule. | No floating-point money | BRL-020, CON-01 | P0 | I, T |
| NFR-021 | Data integrity | Transactions | A business document and its postings commit atomically or not at all. | 100 percent | BRL-016, BRL-017 | P0 | T |
| NFR-022 | Data integrity | Referential integrity | Relationships are enforced by the database. | Foreign keys on all references | BR-014 | P0 | I |
| NFR-023 | Data integrity | Reconciliation | Sub-ledgers reconcile to control accounts and stock valuation reconciles to the ledger, checkable on demand. | Zero unexplained difference | BR-019, BR-039 | P0 | T |
| NFR-024 | Concurrency | Safe concurrent use | Concurrent edits and postings do not corrupt data or issue duplicate numbers. | Zero duplicate numbers, zero lost updates in load tests | BRL-014, BRL-065 | P0 | T |
| NFR-025 | Idempotency | Retry safety | Create and post requests that change money or stock can be repeated safely. | Idempotency key supported on those endpoints | BRL-029 | P0 | T |
| NFR-026 | Auditability | Audit completeness | Every audited event is recorded and cannot be altered. | 100 percent | BR-005, BRL-007 | P0 | T |
| NFR-027 | Auditability | Audit retention | Audit records are kept for at least the longest applicable legal period. | Audit records kept at least 7 years or the longest applicable legal period if longer. Deletion before expiry refused in 100 percent of attempts | BR-059 | P0 | I |
| NFR-028 | Observability | Logging | Application, security and integration events are logged with correlation ids, without secrets or personal data in clear. | 100 percent of log lines carry a timestamp and correlation id. 0 occurrences of passwords, tokens or full bank numbers in a 7 day log scan. Logs kept 90 days searchable and 1 year archived | BR-062 | P0 | I |
| NFR-029 | Observability | Monitoring and alerts | Health, error rate, latency, queue depth and job failures are monitored with alerts. | Alerts fire for: error rate above 2 percent over 5 minutes, 95th percentile latency above twice the NFR-001 target over 10 minutes, outbox backlog older than 15 minutes, any job failure, 3 failed health checks. Alert delivered within 5 minutes | BR-062 | P0 | D |
| NFR-030 | Usability | Task efficiency | Frequent tasks (create PR, receive goods, post invoice) are completable with few steps and keyboard support. | With at least 5 users per role, median create purchase requisition at most 8 steps and 3 minutes, receive goods at most 6 steps, post invoice at most 10 steps. At least 90 percent complete each task without help | BR-013 | P1 | T |
| NFR-031 | Usability | Clear errors | Validation and business errors say what is wrong and how to fix it. | 100 percent of error responses carry a code, a message naming the field or rule, and an action. 0 messages with only a generic text or a stack trace. At least 8 of 10 testers can state the fix from the message | All | P0 | T |
| NFR-032 | Accessibility | Standard | The web client meets WCAG 2.2 level AA. | Level AA | BR-013 | P1 | T |
| NFR-033 | Compatibility | Browsers and devices | Supports current versions of major desktop browsers and responsive layouts for tablet and phone. | Current and previous major versions of Chrome, Edge, Firefox and Safari on desktop. Layouts usable at widths 360, 768, 1024 and 1440 pixels. 0 blocking defects across the matrix | BR-011 | P1 | T |
| NFR-034 | Localisation | Internationalisation | All user-facing text, dates, numbers and currencies are locale aware and translatable. | 100 percent of user-facing strings in resource files (scan finds 0 hard-coded strings). Launch in English. At least one non-Latin and one right-to-left test locale render without defects. Number, date and currency format follows the locale on 100 percent of sampled screens. Further languages per OQ-011 | BR-011 | P1 | I |
| NFR-035 | Maintainability | Modularity | Modules have defined boundaries and public interfaces so that a module can change without breaking others. | 0 cyclic dependencies between modules. 0 cross-module access outside public interfaces. Both checked in the build and failing the build on violation | BR-006, BR-061 | P0 | A, I |
| NFR-036 | Maintainability | Test automation | Business rules and posting logic are covered by automated tests. | Coverage targets in Test Strategy (Stage 3) | BR-062 | P0 | I |
| NFR-037 | Maintainability | Configuration over code | Approval chains, numbering, tolerances and tax are configurable without deployment. | Approval chains, numbering formats, tolerances and tax codes change with 0 deployments. A change is effective within 5 minutes of approval | BR-006, BR-061 | P0 | D |
| NFR-038 | Portability | Environment parity | The system runs identically in development, test, staging and production through containers and infrastructure as code. | Staging and production are built from the same definitions. An automated comparison shows 0 differences outside a declared list (size, secrets, URLs) | BR-062 | P1 | I |
| NFR-039 | Compliance | Privacy | Personal data is minimised, access-limited, exportable and erasable or anonymisable where law allows. | Personal data inventory covers 100 percent of personal fields. A subject export is produced within 24 hours of the request (the legal period depends on OQ-010). After anonymisation 0 direct identifiers remain and ledger totals are unchanged (difference 0.00) | BR-059 | P0 | I |
| NFR-040 | Compliance | Retention | Records are retained and then archived or deleted per configured policy. | Retention period configurable per record type. Deletion inside retention refused in 100 percent of attempts. After expiry the archive job handles the record within 24 hours. Default financial retention 7 years as a placeholder | BR-059 | P0 | T |
| NFR-041 | Data | Time handling | All timestamps are stored in UTC with the user's zone applied at display. Business dates (posting date) are dates, not instants. | 0 timestamp columns stored in a non-UTC zone. Display is correct for 5 time zones including a daylight saving change day. Posting dates are date-only values (0 posting date columns with a time part) | BR-011 | P0 | T |
| NFR-042 | Data | Archival | Old closed-period data can be archived without breaking reports or drill-down. | Closed-period data older than N years is archived with reports and drill-down returning identical totals (difference 0.00). N is 7 as a placeholder | BR-059 | P1 | A |

## 1A. Measurable acceptance criteria for all 42 NFRs (v0.2)

Every NFR has a unique ID, a measurable criterion (column 3, the same text as the "Proposed target" column of section 1), a verification method, and at least one test case. Test cases are found by exact NFR ID in the test case files.
None of the numbers is a confirmed business target. Values marked PENDING APPROVAL need the sponsor or the named owner (OQ-015, OQ-022, OQ-028, OQ-010, OQ-011).
NFR test cases written in v0.2 are in `08-QA-and-Testing/test-cases/NFR-Measurable-Test-Cases.md` (TC-0649 to TC-0679). Earlier cases are the performance plan cases TC-0522 to TC-0533 and the security, inspection and integration cases.

| Title | NFR | Measurable criterion | Verification method | Value status | Test cases |
|---|---|---|---|---|---|
| Interactive response | NFR-001 | 95th percentile under 2 s for reads and under 3 s for saves at the sized load | Test | Proposed in v0.1, PENDING APPROVAL (not a final business target) | TC-0522 |
| Report response | NFR-002 | 95th percentile under 10 s. Longer reports run in the background | Test | Proposed in v0.1, PENDING APPROVAL (not a final business target) | TC-0523, TC-0744 |
| Posting throughput | NFR-003 | Proposed: sustain 20 postings per second (invoice, receipt, payment mix) for 30 minutes with 95th percentile save under 3 s, 0 failed and 0 duplicate postings | Test | Value changed in v0.2, proposed, PENDING APPROVAL | TC-0524, TC-0649, TC-0650, TC-0972 |
| Search | NFR-004 | 95th percentile under 2 s for 1 million rows per list | Test | Proposed in v0.1, PENDING APPROVAL (not a final business target) | TC-0525 |
| Volume | NFR-005 | Proposed: 100 companies, 2,000 concurrent users, 20 million ledger and stock rows per year, 5 years retained (100 million rows). NFR-001, NFR-002 and NFR-004 targets still hold at this volume | Analysis, Test | Value changed in v0.2, proposed, PENDING APPROVAL | TC-0526, TC-0651 |
| Growth | NFR-006 | Adding a company or branch needs 0 code deployments and at most 1 hour of administrator work. Adding a module changes 0 files in the public interface of existing modules | Analysis | Value changed in v0.2, proposed, PENDING APPROVAL | TC-0498, TC-0652 |
| Uptime | NFR-007 | 99.5 percent monthly for MVP (proposed). Final per OQ-028 | Test | Proposed in v0.1, PENDING APPROVAL (not a final business target) | TC-0527 |
| Degraded mode | NFR-008 | With email, bank adapter and gateway simulated down for 4 hours: 100 percent of postings commit, outbox backlog returns to 0 within 30 minutes of recovery, 0 events lost | Test | Value changed in v0.2, proposed, PENDING APPROVAL | TC-0527, TC-0653, TC-0745, TC-0824, TC-0926 |
| RPO | NFR-009 | 15 minutes proposed. Final per OQ-028 | Test | Proposed in v0.1, PENDING APPROVAL (not a final business target) | TC-0528 |
| RTO | NFR-010 | 4 hours proposed. Final per OQ-028 | Test | Proposed in v0.1, PENDING APPROVAL (not a final business target) | TC-0528 |
| Backups | NFR-011 | Daily full plus continuous log. Restore test each quarter | Test | Proposed in v0.1, PENDING APPROVAL (not a final business target) | TC-0528 |
| Transport encryption | NFR-012 | Current TLS version, no plain HTTP | Test | Fixed by standard or design rule | TC-0219, TC-0227 |
| Encryption at rest | NFR-013 | 256-bit security strength encryption at rest for databases, object storage and backups. Bank and tax identifiers also protected at field level. Keys in a managed key store, rotated at least every 12 months. 0 of 1,000 sampled sensitive fields readable as plain text in raw storage | Inspection | Value changed in v0.2, proposed, PENDING APPROVAL | TC-0228, TC-0229, TC-0654, TC-0655 |
| Secrets | NFR-014 | 0 secrets in source control history at release (full-history scan). Secrets rotated at least every 90 days. A quarterly rotation drill completes with 0 outage. Service tokens expire within 24 hours | Inspection | Value changed in v0.2, proposed, PENDING APPROVAL | TC-0230, TC-0656, TC-0657 |
| Web vulnerabilities | NFR-015 | No open High findings at release | Test | Proposed in v0.1, PENDING APPROVAL (not a final business target) | TC-0216, TC-0217, TC-0218, TC-0219, TC-0220, TC-0221, TC-0245 |
| Authorisation on every call | NFR-016 | 100 percent of endpoints | Test, Inspection | Fixed by standard or design rule | TC-0205, TC-0243, TC-0518, TC-0899, TC-0910 |
| Rate limiting | NFR-017 | Sign-in 5 failures per account per 15 minutes then lockout (SEC-003). 20 attempts per address per minute then 429. Password reset 3 per account per hour. Exports 5 per user per hour. API 600 requests per minute per token | Test | Value changed in v0.2, proposed, PENDING APPROVAL | TC-0236, TC-0658, TC-0659, TC-0660 |
| Tenant isolation | NFR-018 | No cross-tenant access in isolation tests | Test | Fixed by standard or design rule | TC-0499 |
| Dependency hygiene | NFR-019 | No known Critical vulnerability at release | Test | Proposed in v0.1, PENDING APPROVAL (not a final business target) | TC-0241 |
| Money precision | NFR-020 | No floating-point money | Inspection, Test | Fixed by standard or design rule | TC-0500, TC-0889, TC-0918, TC-0928 |
| Transactions | NFR-021 | 100 percent | Test | Fixed by standard or design rule | TC-0531, TC-0868, TC-0870, TC-0921, TC-0922 |
| Referential integrity | NFR-022 | Foreign keys on all references | Inspection | Fixed by standard or design rule | TC-0266 |
| Reconciliation | NFR-023 | Zero unexplained difference | Test | Proposed in v0.1, PENDING APPROVAL (not a final business target) | TC-0501 |
| Safe concurrent use | NFR-024 | Zero duplicate numbers, zero lost updates in load tests | Test | Proposed in v0.1, PENDING APPROVAL (not a final business target) | TC-0529, TC-0867 |
| Retry safety | NFR-025 | Idempotency key supported on those endpoints | Test | Fixed by standard or design rule | TC-0232, TC-0530, TC-0869, TC-0922, TC-0925 |
| Audit completeness | NFR-026 | 100 percent | Test | Fixed by standard or design rule | TC-0502 |
| Audit retention | NFR-027 | Audit records kept at least 7 years or the longest applicable legal period if longer. Deletion before expiry refused in 100 percent of attempts | Inspection | Value changed in v0.2, proposed, PENDING APPROVAL | TC-0502, TC-0661, TC-0662 |
| Logging | NFR-028 | 100 percent of log lines carry a timestamp and correlation id. 0 occurrences of passwords, tokens or full bank numbers in a 7 day log scan. Logs kept 90 days searchable and 1 year archived | Inspection | Value changed in v0.2, proposed, PENDING APPROVAL | TC-0231, TC-0238, TC-0521, TC-0663 |
| Monitoring and alerts | NFR-029 | Alerts fire for: error rate above 2 percent over 5 minutes, 95th percentile latency above twice the NFR-001 target over 10 minutes, outbox backlog older than 15 minutes, any job failure, 3 failed health checks. Alert delivered within 5 minutes | Demonstration | Value changed in v0.2, proposed, PENDING APPROVAL | TC-0239, TC-0240, TC-0664, TC-0665 |
| Task efficiency | NFR-030 | With at least 5 users per role, median create purchase requisition at most 8 steps and 3 minutes, receive goods at most 6 steps, post invoice at most 10 steps. At least 90 percent complete each task without help | Test | Value changed in v0.2, proposed, PENDING APPROVAL | TC-0497, TC-0551, TC-0666 |
| Clear errors | NFR-031 | 100 percent of error responses carry a code, a message naming the field or rule, and an action. 0 messages with only a generic text or a stack trace. At least 8 of 10 testers can state the fix from the message | Test | Value changed in v0.2, proposed, PENDING APPROVAL | TC-0215, TC-0222, TC-0496, TC-0551, TC-0667 |
| Standard | NFR-032 | Level AA | Test | Fixed by standard or design rule | TC-0494 |
| Browsers and devices | NFR-033 | Current and previous major versions of Chrome, Edge, Firefox and Safari on desktop. Layouts usable at widths 360, 768, 1024 and 1440 pixels. 0 blocking defects across the matrix | Test | Value changed in v0.2, proposed, PENDING APPROVAL | TC-0495, TC-0668, TC-0669 |
| Internationalisation | NFR-034 | 100 percent of user-facing strings in resource files (scan finds 0 hard-coded strings). Launch in English. At least one non-Latin and one right-to-left test locale render without defects. Number, date and currency format follows the locale on 100 percent of sampled screens. Further languages per OQ-011 | Inspection | Value changed in v0.2, proposed, PENDING APPROVAL | TC-0503, TC-0670, TC-0671 |
| Modularity | NFR-035 | 0 cyclic dependencies between modules. 0 cross-module access outside public interfaces. Both checked in the build and failing the build on violation | Analysis, Inspection | Value changed in v0.2, proposed, PENDING APPROVAL | TC-0504, TC-0672 |
| Test automation | NFR-036 | Coverage targets in Test Strategy (Stage 3) | Inspection | Proposed in v0.1, PENDING APPROVAL (not a final business target) | TC-0505 |
| Configuration over code | NFR-037 | Approval chains, numbering formats, tolerances and tax codes change with 0 deployments. A change is effective within 5 minutes of approval | Demonstration | Value changed in v0.2, proposed, PENDING APPROVAL | TC-0506, TC-0673 |
| Environment parity | NFR-038 | Staging and production are built from the same definitions. An automated comparison shows 0 differences outside a declared list (size, secrets, URLs) | Inspection | Value changed in v0.2, proposed, PENDING APPROVAL | TC-0242, TC-0244, TC-0520, TC-0674 |
| Privacy | NFR-039 | Personal data inventory covers 100 percent of personal fields. A subject export is produced within 24 hours of the request (the legal period depends on OQ-010). After anonymisation 0 direct identifiers remain and ledger totals are unchanged (difference 0.00) | Inspection | Value changed in v0.2, proposed, PENDING APPROVAL | TC-0231, TC-0675 |
| Retention | NFR-040 | Retention period configurable per record type. Deletion inside retention refused in 100 percent of attempts. After expiry the archive job handles the record within 24 hours. Default financial retention 7 years as a placeholder | Test | Value changed in v0.2, proposed, PENDING APPROVAL | TC-0533, TC-0676 |
| Time handling | NFR-041 | 0 timestamp columns stored in a non-UTC zone. Display is correct for 5 time zones including a daylight saving change day. Posting dates are date-only values (0 posting date columns with a time part) | Test | Value changed in v0.2, proposed, PENDING APPROVAL | TC-0519, TC-0532, TC-0677, TC-0678, TC-0889 |
| Archival | NFR-042 | Closed-period data older than N years is archived with reports and drill-down returning identical totals (difference 0.00). N is 7 as a placeholder | Analysis | Value changed in v0.2, proposed, PENDING APPROVAL | TC-0507, TC-0679 |

## 2. Technical requirements

TRs state technical needs that follow from the BRs and NFRs. They do not choose products. Product choice is in the ADRs and is provisional.

| ID | Title | Requirement | Linked | Pri | Verif. |
|---|---|---|---|---|---|
| TR-001 | Relational store | The system of record shall be a relational database with ACID transactions, foreign keys and exact decimal types. | NFR-020, NFR-021, NFR-022 | P0 | I |
| TR-002 | Tenant key | Every business table shall carry a company key (and a tenant key if OQ-019 requires), enforced in queries and by database policy. | FR-019, NFR-018 | P0 | I, T |
| TR-003 | Append-only ledgers | Ledger, stock ledger and audit tables shall be append-only. Corrections are new rows. | FR-064, FR-104, FR-030 | P0 | T |
| TR-004 | Optimistic locking | Mutable records shall carry a version used to detect conflicting updates. | FR-125 | P0 | T |
| TR-005 | Atomic numbering | Document numbers shall come from a transaction-safe sequence per company, type and year. | FR-025 | P0 | T |
| TR-006 | Stock locking | Stock updates shall use row-level locking or equivalent serialisation per item and warehouse. | FR-113 | P0 | T |
| TR-007 | Event outbox | Domain events (document posted, approval decided) shall be written in the same transaction as the change and published reliably afterwards. | FR-030, FR-037, FR-128 | P0 | T |
| TR-008 | Background jobs | Notifications, imports, report generation and retries shall run as background jobs with retry and dead-letter handling. | FR-037, FR-049, FR-128 | P0 | T |
| TR-009 | Policy engine | A single authorisation component shall evaluate role, scope and attribute rules for every request. | FR-015, NFR-016 | P0 | T |
| TR-010 | Approval engine | One workflow component shall serve every approvable document type through configuration. | FR-039 to FR-047 | P0 | D |
| TR-011 | Idempotency store | Money- and stock-changing endpoints shall accept an idempotency key and store its outcome for a defined time. | FR-076, NFR-025 | P0 | T |
| TR-012 | File storage | Attachments shall be stored in encrypted object storage with access through authorised, short-lived links. | FR-033, FR-034 | P1 | I |
| TR-013 | API versioning | Public APIs shall be versioned and changes shall be backward compatible within a version. | API requirements | P0 | I |
| TR-014 | Standard formats | Import and export shall use CSV and a spreadsheet format at minimum. | FR-048 to FR-050 | P0 | T |
| TR-015 | Cache | Caching shall never serve data outside the requester's scope and shall not cache financial balances without invalidation on posting. | NFR-016, NFR-023 | P1 | T |
| TR-016 | Time source | Servers shall use synchronised time. | NFR-041 | P0 | I |
| TR-017 | Configuration management | Environment differences shall be only configuration, and configuration changes shall be versioned. | NFR-038 | P1 | I |
| TR-018 | Correlation | Each request shall carry a correlation id through logs, events and audit records. | NFR-028 | P0 | I |

## 3. Counts

42 NFRs and 18 TRs, measured when this file was written.
API requirements (API-xxx) are in [API Documentation](../04-System-Architecture/API-Documentation.md).
