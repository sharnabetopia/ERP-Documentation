# Monitoring and Logging Strategy

Document status: Draft v0.1 (Stage 3 part 2). Proposed. Tools are not chosen (OQ-024). Thresholds are **proposed starting values** to tune against real load (OQ-015).
Related: NFR-028, NFR-029, SEC-046, SEC-047, [Audit Trail](../06-RBAC-and-Security/Audit-Trail.md), [Infrastructure Architecture](../04-System-Architecture/Infrastructure-Architecture.md), [SLA and Support](SLA-and-Support.md).

## 1. Objectives

1. Know before users do when the service degrades.
2. Find the cause quickly using correlated logs, metrics and traces.
3. Detect security events and misuse.
4. Prove service levels (SLA reporting).
5. Keep personal and sensitive data out of logs (SEC-039).

## 2. Three data streams

| Stream | Purpose | Examples | Retention (TBD, OQ-022) |
|---|---|---|---|
| Logs | Events and errors for troubleshooting | Application, worker, gateway, database, security | Short for debug, longer for security |
| Metrics | Numbers over time | Latency, errors, throughput, queue depth, resources | Aggregated long term |
| Traces | Request path across components | Request through policy, module, database, outbox | Sampled, short |
| Audit (separate) | Business evidence | AUD-001 to AUD-100 | Financial retention (Audit Trail) |

Audit records are not logs. They live in the database, are immutable and are never sampled or dropped.

## 3. Log standards

| Rule | Detail |
|---|---|
| Format | Structured, one event per line, UTC time |
| Required fields | Time, level, service, environment, correlation id (TR-018), tenant and company ids, user id, request path, outcome |
| Never log | Passwords, tokens, secrets, full card or bank numbers, personal data values, request bodies of sensitive endpoints |
| Levels | error, warning, info, debug (debug off in production) |
| Security events | Sign-in, lockout, permission refusal, privilege change, export, SoD override (SEC-046) |
| Access | Operations and security staff. Logs are append-only in the log store |

## 4. Metrics and alert catalogue

Severity: S1 page immediately, S2 notify within working hours quickly, S3 ticket. Values are proposals.

| ID | Metric | Alert condition (proposed) | Severity | Linked |
|---|---|---|---|---|
| MON-001 | Availability probe (synthetic sign-in and read) | Fails 3 times in a row | S1 | NFR-007 |
| MON-002 | Error rate (5xx) | Above 1 percent for 5 minutes | S1 | NFR-001 |
| MON-003 | Read latency p95 | Above 2 s for 10 minutes | S2 | NFR-001 |
| MON-004 | Save and posting latency p95 | Above 3 s for 10 minutes | S2 | NFR-001 |
| MON-005 | Database availability and failover events | Any failover or unavailability | S1 | NFR-007 |
| MON-006 | Database replication lag | Above RPO budget (for example 5 minutes) | S1 | NFR-009 |
| MON-007 | Database connections and locks | Above 80 percent, or lock waits above threshold | S2 | NFR-024 |
| MON-008 | Slow queries | New query above threshold | S3 | NFR-004 |
| MON-009 | Disk and storage growth | Above 80 percent, or growth rate forecast | S2 | NFR-042 |
| MON-010 | CPU and memory | Above 85 percent for 15 minutes | S2 | NFR-005 |
| MON-011 | Outbox depth and age | Oldest pending above 5 minutes | S2 | TR-007 |
| MON-012 | Dead-letter count | Any new dead item | S2 | FR-128 |
| MON-013 | Background job failures | Failure of a scheduled job | S2 | TR-008 |
| MON-014 | Notification failure rate | Above 5 percent | S3 | FR-037 |
| MON-015 | Integration partner health | Circuit open, or credential failure | S2 | SEC-047 |
| MON-016 | Failed sign-ins | Spike above baseline | S2 | SEC-047 |
| MON-017 | Account lockouts | Spike above baseline | S3 | SEC-047 |
| MON-018 | Privilege changes and SoD overrides | Any | S3 notify Auditor | SEC-021 |
| MON-019 | Mass export | Above threshold | S2 | SEC-045 |
| MON-020 | Audit write failure | Any | S1 | AUD-093 |
| MON-021 | Audit hash chain verification | Any break | S1 | Audit Trail section 5 |
| MON-022 | Backup success | Missed or failed backup | S1 | NFR-011 |
| MON-023 | Restore test result | Overdue or failed | S2 | NFR-011 |
| MON-024 | Certificate and secret expiry | Under 30 days | S3 | SEC-038 |
| MON-025 | Reconciliation job (AP, AR, stock to ledger, summaries to rows) | Any difference | S1 for ledger, S2 for others | INV-03 to INV-06 |
| MON-026 | Negative stock check (where policy forbids) | Any | S1 | INV-07 |
| MON-027 | Unposted or stuck approvals ageing | Above threshold | S3 | FR-045 |
| MON-028 | Time synchronisation drift | Above tolerance | S3 | TR-016 |
| MON-029 | Dependency and image vulnerabilities (runtime) | New Critical | S2 | SEC-049 |
| MON-030 | Rate limit hits | Sustained high | S3 | SEC-044 |

## 5. Dashboards

| Dashboard | Audience | Content |
|---|---|---|
| Service health | On-call, operations | Availability, error rate, latency, saturation |
| Business operations | Support, finance operations | Postings per hour, approvals pending, failed integrations, queue depth, jobs |
| Data integrity | Finance, QA | Reconciliation results, balances of control accounts versus sub-ledgers, summary checks |
| Security | Security officer | Failed sign-ins, lockouts, refusals, privilege changes, exports |
| SLA | Management | Availability and response targets achieved |

## 6. On-call and response

Alerts go to the on-call channel with a runbook link. S1 acknowledgement target 15 minutes and S2 within 60 minutes (proposed, see SLA and Support). Every alert has an owner and a runbook entry. Noisy alerts are tuned or removed within a week.

## 7. Verification

| Check | When |
|---|---|
| Alert rules tested by simulated failures | Before go-live and each release touching monitoring |
| Log scan for secrets and personal data | Each release candidate (SEC-039) |
| Restore of logs and metrics | Annually |

Written test cases touching monitoring: SEC-046 and SEC-047 cases in the [Security Test Cases](../08-QA-and-Testing/test-cases/Security-Test-Cases.md) and the written cases that reference NFR-029.

## 8. Open points

Tools (OQ-024), retention values (OQ-022), baselines and thresholds (OQ-015), on-call staffing (OQ-014).
