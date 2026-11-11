# Test Strategy

Document status: Draft v0.1 (Stage 3 part 1). Proposed. Targets marked "proposed" need approval by the QA lead and sponsor.
Related: [Test Plan](Test-Plan.md), [RTM](RTM.md), [Acceptance Criteria](../03-Product-and-System-Requirements/Acceptance-Criteria.md), [MVP Strategy](../00-Project-Management/MVP-Strategy.md) (release gates G1 to G9), [NFR](../03-Product-and-System-Requirements/NFR.md).

## 1. Objectives

1. Show that every MVP requirement behaves as specified, including failure and edge cases.
2. Protect financial integrity: balanced ledgers, exact money, reconciled sub-ledgers and stock.
3. Prove data isolation and authorisation: nobody reaches data outside their scope.
4. Prove resilience: retries, partial failures and concurrency do not corrupt data.
5. Provide evidence for the release gates and for auditors.

## 2. Scope

| In scope (MVP) | Out of scope for now |
|---|---|
| 58 MVP modules, 130 FRs, 80 stories, 16 use cases | R2 to R4 modules (tests written per release) |
| Security requirements SEC-001 to SEC-053 | Penetration test execution (commissioned separately, planned in the [Security Test Plan](Security-Test-Plan.md)) |
| NFRs with measurable targets, once targets are agreed (OQ-015, OQ-028) | Native mobile apps (OQ-025) |
| Data migration rehearsal reconciliation | Legacy system testing |

## 3. Test levels

| Level | Purpose | Who | Environment | Tooling (not chosen, OQ-024) |
|---|---|---|---|---|
| Unit | Business rules, posting logic, calculations, authorisation decisions in isolation | Developers | Local and CI, no database or network | Stack-appropriate unit framework |
| Component and integration | Module with its database, approval engine, outbox, adapters with stubs | Developers and QA | Test | Test containers, partner stubs |
| API | Contract, authorisation matrix, error codes, idempotency | QA | Test | API test framework generated from the API document |
| End-to-end | Full journeys through UI and API | QA | Test and Staging | Browser automation |
| Non-functional | Performance, concurrency, resilience, backup restore | QA and operations | Staging at sized volume | Load tool, fault injection |
| Security | Verification of SEC and THR, plus penetration test | Security tester, independent party | Staging | See Security Test Plan |
| UAT | Business acceptance | Business users | Staging with production-like data | Manual with scripts |

Unit tests touch no database, network or external process. Integration and browser suites run separately.

## 4. Test types and coverage in the written cases

| Type | Where | Cases |
|---|---|---|
| Positive, negative, boundary, validation, edge | Module test case files | In functional files |
| Permission and role-based access | API permission tests (one per permission code) and module files | Generated from the RBAC matrix |
| Data isolation | Module files and cross-module file | Isolation matrix |
| Integration | Cross-module file | Invariants, atomic postings, hand-offs, database constraints |
| End-to-end | E2E file | Journeys for WF-01, 02, 03, 06, 07, 12 |
| API | API file | Contract, permission, error codes |
| Security | Security file | One or more per SEC, abuse cases per THR |
| Performance, concurrency, resilience | Performance plan and cross-module file | NFR-001 to NFR-011 and concurrency |
| Accessibility, cross-browser, UI states | E2E file | NFR-030 to NFR-033 |
| Regression | [Regression Test Plan](Regression-Test-Plan.md) | Selection from the above |
| UAT | UAT file | 20 scenarios |

Counts per file are in the [Test Plan](Test-Plan.md). They were counted by script.

## 5. Risk-based prioritisation

| Priority | Meaning | Examples |
|---|---|---|
| P0 | Failure blocks release. Financial integrity, security, data isolation, core posting, approvals | Posting balance, stock negative, cross-company access, self-approval |
| P1 | Important, release possible with an agreed workaround | Reports, preferences, notifications detail |
| P2 | Low impact | Cosmetic and rare edge cases |

| Severity | Meaning |
|---|---|
| Critical | Data loss or corruption, money or stock wrong, security breach, system unusable |
| High | Major function broken, no workaround |
| Medium | Function impaired, workaround exists |
| Low | Cosmetic or minor |

Test priority was aligned with requirement priority: every P0 FR has at least one P0 test case (checked by script). Priority inflation is a known risk. 361 of 553 cases are P0, which is more than a typical suite. It results from marking all security, financial and authorisation tests P0. The QA lead should re-triage after the first execution cycle.

## 6. Entry and exit criteria

| Stage | Entry | Exit |
|---|---|---|
| Unit | Code complete for a story | Unit suite passes, coverage meets targets below |
| Integration and API | Build deployed to Test, test data loaded | No open Critical or High in scope. All P0 cases executed |
| E2E and non-functional | Integration exit met | P0 and P1 executed, performance targets met (targets TBD) |
| Security | Feature complete | No open High or Critical findings (gate G4) |
| UAT | Staging stable, migration rehearsal done | Business owners sign (gate G8) |
| Release | Gates G1 to G9 in the MVP Strategy | Release approval |

## 7. Coverage targets (proposed)

| Measure | Target | Note |
|---|---|---|
| Requirements with at least one test case | 100 percent of MVP FR, NFR, TR, SEC, INV | Counted in the RTM. Currently achieved on paper only. No test has run |
| P0 requirements executed and passed | 100 percent | Release gate |
| Unit line coverage, general code | 90 percent (proposed) | To be set by the team |
| Unit line and branch coverage, critical modules | 100 percent on posting engine, authorisation, approval engine, stock locking, numbering | Critical modules defined here |
| Mutation or equivalent check on posting and tax rounding | Reference data sets must pass | See [Test Data Management](Test-Data-Management.md) |

No coverage result is claimed. Coverage figures are measured by tools after execution and reported in the [Test Coverage Report](../12-Final-Audit/Test-Coverage-Report.md).

## 8. Environments and data

Test, Staging and the rules for data are in [Test Data Management](Test-Data-Management.md) and [Deployment Architecture](../04-System-Architecture/Deployment-Architecture.md). Production data is never used unmasked.

## 9. Defect management

Defects use the [Defect Report Template](Defect-Report-Template.md). Lifecycle: New, Triaged, In progress, Fixed, Ready for retest, Verified, Closed, Reopened, Rejected, Deferred. Every fixed defect gets a regression test that fails on the unfixed build and passes on the fix. Critical defects stop the test cycle until triaged.

## 10. Automation approach

| Category | Automate | Keep manual |
|---|---|---|
| Unit, API, integration, permission matrix, error codes | Yes | |
| Posting, tax, rounding, valuation reference sets | Yes | |
| Concurrency and fault injection | Yes, scripted | |
| End-to-end happy paths | Yes | Exploratory sessions |
| Accessibility | Automated checks plus manual screen reader pass | Manual pass |
| Security | Automated scans plus scripted cases | Penetration test, tabletop |
| UAT | | Manual by business |

In the written cases, 41 are inspection, accessibility, UI, cross-browser or UAT types that lean manual. The rest are suitable for automation.

## 11. Tools and traceability

Tool names are deliberately not chosen (OQ-024). Test cases will be loaded into a test management tool. IDs (TC-0001 onward) and the RTM stay the source of truth.

## 12. Risks to the test effort

| Risk | Handling |
|---|---|
| NFR targets and volumes unknown (OQ-015, OQ-028) | Performance cases are written with TBD targets. Execution waits for agreed numbers |
| Business rules still Proposed | Cases follow the proposals. Changes after OQ answers need case updates |
| Large permission surface (211 codes) | Matrix tests are generated, so they regenerate when the RBAC data changes |
| UAT staff availability (OQ-029) | Plan early |
| Test data for rounding, currency, stock valuation methods (OQ-009) | Reference sets agreed with Finance |
