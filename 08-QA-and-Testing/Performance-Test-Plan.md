# Performance Test Plan

Document status: Draft v0.1 (Stage 3 part 1). Proposed. **Targets and volumes are not agreed.** They come from NFR defaults (NFR-001 to NFR-011) that depend on OQ-015 (volumes) and OQ-028 (service levels). No performance test has run.
Related: [NFR](../03-Product-and-System-Requirements/NFR.md), [Test Strategy](Test-Strategy.md), [Deployment Architecture](../04-System-Architecture/Deployment-Architecture.md), [Test Data Management](Test-Data-Management.md).

## 1. Objectives

1. Verify the response times, throughput and capacity targets once agreed.
2. Find limits and bottlenecks (database, locks, background jobs).
3. Verify concurrency safety: no duplicate numbers, lost updates or negative stock under load.
4. Verify availability, failover, degraded mode and backup restore against RPO and RTO.

## 2. Environment

Staging with the same topology as production (see Deployment Architecture), sized data set generated to the agreed volume, partner stubs with controllable latency, monitoring enabled. Results from smaller environments are not accepted as evidence for the gate.

## 3. Workload model (placeholder until OQ-015)

| Item | Working assumption (from NFR-005) | Status |
|---|---|---|
| Companies | 100 per deployment | Design assumption |
| Concurrent users | 2,000 | Design assumption |
| Ledger and stock rows per year | 20 million | Design assumption |
| Mix | 60 percent reads, 25 percent document creation and edits, 10 percent postings and approvals, 5 percent reports | Proposed |
| Peak posting rate | TBD | OQ-015 |
| Think time | 5 to 30 seconds | Proposed |

## 4. Test types

| Type | Purpose | Method |
|---|---|---|
| Baseline | Single user times | One user, empty then loaded system |
| Load | Targets under expected peak | Ramp to expected peak and hold 30 minutes |
| Stress | Find breaking point | Increase until errors or targets fail |
| Soak | Leaks and drift | 8 hours at 70 percent of peak |
| Spike | Sudden surge | Step to 2x peak for 5 minutes |
| Concurrency | Locks, numbering, stock, approvals | Many actors on one scarce resource |
| Resilience | Failover, instance loss, provider outage | Kill instances, stop provider stubs |
| Recovery | Backup restore, point in time | Restore and measure data loss and time |
| Scalability | Add instances | Compare throughput with 1, 2, 3 application instances |

## 5. Key scenarios

Sign-in, list and search on large lists, create PR and PO, post GRN, post and match invoice, release payment, confirm order with reservation, post delivery, post invoice, trial balance and ageing reports over a full year, stock valuation run, import of 100,000 rows, export of large lists, approval inbox for 10,000 pending items.

## 6. Measures and targets

| Measure | Target (proposed, NFR) | Source |
|---|---|---|
| Interactive read 95th percentile | under 2 s | NFR-001 |
| Interactive save 95th percentile | under 3 s | NFR-001 |
| Standard reports 95th percentile | under 10 s, longer as background jobs | NFR-002 |
| List search 95th percentile on 1 million rows | under 2 s | NFR-004 |
| Error rate under load | under 0.1 percent (proposed) | |
| Duplicate numbers, lost updates, negative stock | zero | NFR-024, INV-07 |
| Availability | 99.5 percent monthly | NFR-007 |
| Data loss after failure (RPO) | 15 minutes | NFR-009 |
| Recovery time (RTO) | 4 hours | NFR-010 |
| Resource headroom at peak | CPU under 70 percent, database connections under 80 percent (proposed) | |

## 7. Monitoring during tests

Response times by endpoint, error rates, database locks and wait times, slow queries, connection pools, queue depth for outbox and jobs, memory and CPU for application, workers and database, replication lag.

## 8. Entry and exit

| Entry | Exit |
|---|---|
| Feature complete, sized data loaded, targets agreed, monitoring in place | Targets met or exceptions accepted by the sponsor (gate G6), restore test passed (gate G7), bottleneck list handed to development |

## 9. Test cases

The performance and non-functional cases are listed below with the same fields as other test cases (12 cases, IDs TC-0522 to TC-0533). Several depend on agreed numbers and cannot run until OQ-015 and OQ-028 are answered. Every Actual Result is blank and every status is Not Run.

| TC ID | Requirement ID | Story ID | Module | Submodule | Scenario | Title | Description | Preconditions | Test Data | Execution Steps | Expected Result | Actual Result | Priority | Severity | Test Type | Execution Status |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| TC-0522 | NFR-001 | n/a | Non-functional | NFR-001 | Performance | Interactive response under sized load | Non-functional check for NFR-001. | Staging with sized data (OQ-015 TBD) | sized user count and 20 million ledger rows per year equivalent | 1. Run mixed workload of reads and saves for 30 minutes | 95th percentile reads under 2 s and saves under 3 s |  | P0 | High | Performance | Not Run |
| TC-0523 | NFR-002 | n/a | Non-functional | NFR-002 | Performance | Report response over a full fiscal year | Non-functional check for NFR-002. | Full year of data | trial balance, ageing, stock valuation | 1. Run each 20 times<br>2. measure | 95th percentile under 10 s; longer reports run as jobs |  | P1 | Medium | Performance | Not Run |
| TC-0524 | NFR-003 | n/a | Non-functional | NFR-003 | Performance | Posting throughput at peak rate | Non-functional check for NFR-003. | Peak rate from OQ-015 (TBD) | GRN, delivery, invoice, payment postings | 1. Ramp to peak<br>2. hold 30 min | Meets interactive target; no errors; no lock timeouts |  | P0 | High | Performance | Not Run |
| TC-0525 | NFR-004 | n/a | Non-functional | NFR-004 | Performance | List search on indexed fields | Non-functional check for NFR-004. | 1 million rows in a list | search and filter | 1. Run 100 queries | 95th percentile under 2 s |  | P1 | Medium | Performance | Not Run |
| TC-0526 | NFR-005 | n/a | Non-functional | NFR-005 | Performance | Volume scalability | Non-functional check for NFR-005. | Sized data set | 100 companies, 2,000 concurrent users (design assumption) | 1. Step load to full volume | No degradation beyond targets; horizontal scaling adds capacity |  | P0 | High | Performance | Not Run |
| TC-0527 | NFR-007, NFR-008 | n/a | Non-functional | NFR-007 | Performance | Availability and degraded mode with partner outage | Non-functional check for NFR-007, NFR-008. | Staging with HA topology | kill one app instance; stop mail provider | 1. Kill instance under load<br>2. stop provider | No user-visible outage beyond failover time; posting continues |  | P0 | High | Performance | Not Run |
| TC-0528 | NFR-009, NFR-010, NFR-011 | n/a | Non-functional | NFR-009 | Performance | Backup restore meets RPO and RTO | Non-functional check for NFR-009, NFR-010, NFR-011. | Backup chain | simulated disaster | 1. Restore to point in time<br>2. measure data loss and time | Within RPO and RTO targets (OQ-028 TBD) |  | P0 | Critical | Performance | Not Run |
| TC-0529 | NFR-024, FR-025, FR-113 | n/a | Non-functional | NFR-024 | Performance | Concurrency soak: numbering, stock and approvals | Non-functional check for NFR-024, FR-025, FR-113. | Load tool | 50 parallel users for 1 hour | 1. Create documents, issue stock, approve concurrently | Zero duplicate numbers, zero lost updates, zero negative stock |  | P0 | Critical | Concurrency | Not Run |
| TC-0530 | NFR-025 | n/a | Non-functional | NFR-025 | Performance | Retry safety under failure injection | Non-functional check for NFR-025. | Fault injection | network drops between request and response | 1. Retry with same keys | Single effect per key |  | P0 | High | Performance | Not Run |
| TC-0531 | NFR-021 | n/a | Non-functional | NFR-021 | Performance | Atomic posting under injected faults at every step | Non-functional check for NFR-021. | Fault injection per step | each posting type | 1. Inject fault at each step | No partial data in any case |  | P0 | Critical | Integration | Not Run |
| TC-0532 | NFR-041 | n/a | Non-functional | NFR-041 | Performance | Time handling in UTC and zones | Non-functional check for NFR-041. | Two zones | date boundaries and DST | 1. Create and view documents | Correct display; posting date unaffected |  | P1 | Medium | Functional | Not Run |
| TC-0533 | NFR-040 | n/a | Non-functional | NFR-040 | Performance | Archive keeps reports and drill-down working | Non-functional check for NFR-040. | Archived closed year | report and drill-down | 1. Archive then run report and drill down | Same results |  | P2 | Medium | Functional | Not Run |

## 10. Risks and open points

Unknown volumes and service levels (OQ-015, OQ-028). Staging cost at sized volume. Lock contention on numbering and stock rows (ADR-012, OQ-031). Reporting load on the primary database (ADR for read replica).
