# Test Plan (MVP)

Document status: Draft v0.1 (Stage 3 part 1). Proposed. No dates, team or budget exist (OQ-014), so the plan is expressed in cycles and gates, not calendar time.
Related: [Test Strategy](Test-Strategy.md), [RTM](RTM.md), [MVP Strategy](../00-Project-Management/MVP-Strategy.md), [Test Data Management](Test-Data-Management.md).

## 1. Items under test

The MVP release: 58 modules grouped into four milestones (MVP-A Platform, MVP-B Finance core, MVP-C Inventory and P2P, MVP-D O2C and release).

## 2. Test case inventory (counted by script)

| File | Cases | Notes |
|---|---|---|
| [Authentication](test-cases/Authentication-Test-Cases.md) | 18 | FR-001 to FR-007 |
| [Administration](test-cases/Administration-Test-Cases.md) | 67 | Users, roles, organisation, configuration, audit, approvals, import and export, cross-cutting |
| [HR](test-cases/HR-Test-Cases.md) | 3 | Employee core only |
| [Finance](test-cases/Finance-Test-Cases.md) | 46 | FR-053 to FR-085 |
| [Procurement](test-cases/Procurement-Test-Cases.md) | 22 | FR-086 to FR-100 |
| [Inventory](test-cases/Inventory-Test-Cases.md) | 20 | FR-101 to FR-113 |
| [Sales](test-cases/Sales-Test-Cases.md) | 16 | FR-114 to FR-124 |
| [Project](test-cases/Project-Test-Cases.md) | 0 | Not an MVP module group |
| [Manufacturing](test-cases/Manufacturing-Test-Cases.md) | 0 | Not an MVP module group |
| [Asset](test-cases/Asset-Test-Cases.md) | 0 | Not an MVP module group |
| [Security](test-cases/Security-Test-Cases.md) | 66 | 53 SEC cases plus 13 abuse cases |
| [Cross-module integration](test-cases/Cross-Module-Integration-Test-Cases.md) | 61 | Invariants, atomic postings, hand-offs, 12 database constraints, 24 technical and non-functional |
| [API](API-Test-Cases.md) | 186 | 16 contract, 127 permission, 43 error code |
| [E2E](E2E-Test-Cases.md) | 16 | 12 journeys, 4 UI and accessibility |
| [Performance and non-functional](Performance-Test-Plan.md) | 12 | Targets TBD |
| [UAT](UAT-Test-Cases.md) | 20 | |
| **Total** | **553** | P0 361, P1 186, P2 6 |

The counts of permission tests (127 of 186 API cases) and error-code tests (43) follow the API document and RBAC matrix, so these numbers change if those documents change.
No test has been executed. Actual Result is blank and status is Not Run in every case.

## 3. Features not tested in this plan

R2 to R4 modules. Non-MVP workflows (WF-04, 05, 08, 09, 10, 11, 13, 14 have outline status). Native mobile. Third-party partner internals (only the adapter behaviour with stubs).

## 4. Approach by cycle

| Cycle | Scope | Entry | Exit |
|---|---|---|---|
| C0 Test readiness | Environments, baselines, stubs, reference sets signed, automation skeleton | Architecture decisions that affect tooling (OQ-024) | Baselines load, smoke passes |
| C1 MVP-A Platform | Authentication, Administration, HR core, Security cases for auth and authorisation, API contract and permission tests for platform | MVP-A build | All P0 pass |
| C2 MVP-B Finance | Finance cases, invariants INV-01 to INV-04, INV-10, INV-15, finance security | MVP-B build | All P0 pass |
| C3 MVP-C Inventory and P2P | Inventory, Procurement, INV-05 to INV-08, concurrency | MVP-C build | All P0 pass |
| C4 MVP-D O2C | Sales, E2E, remaining cross-module | MVP-D build | All P0 pass |
| C5 Hardening | Full regression, performance at sized load, security tests, resilience, backup restore | Feature complete | Gates G2 to G7 |
| C6 UAT and release | UAT, migration rehearsal reconciliation | C5 exit | Gates G8, G9, sign-off |

## 5. Roles

| Role | Responsibility |
|---|---|
| QA lead | Plan, triage, reporting, gate evidence |
| Test engineers | Write automation, execute manual, log defects |
| Developers | Unit and integration tests, fix defects, add regression tests |
| Security tester | Security cases and coordination of the penetration test |
| Finance, procurement, warehouse, sales owners | Reference data, UAT |
| Sponsor | Gate decisions |

## 6. Schedule and effort

Not estimated. Needs team size and dates (OQ-014). Effort can be sized once the automation split in the Test Strategy is agreed.

## 7. Reporting

| Report | Frequency | Content |
|---|---|---|
| Execution summary | Daily in a cycle | Executed, passed, failed, blocked, by priority and area |
| Defect summary | Daily | Open by severity, ageing |
| Coverage summary | Per cycle | Requirement coverage from the RTM, code coverage from tools |
| Gate report | At each gate | Evidence against G1 to G9 |

## 8. Suspension and resumption

Testing in a cycle pauses when the build does not start, a Critical defect blocks more than 20 percent of the cycle, or the environment is unavailable. It resumes when the blocker is fixed and the smoke suite passes.

## 9. Risks

See [Test Strategy](Test-Strategy.md) section 12 and the [Risk Register](../00-Project-Management/Risk-Register.md) (R-004, R-005, R-009, R-012).

## 10. Deliverables

Test cases and RTM (this stage), automation code, execution reports, defect log, coverage reports, gate evidence, UAT sign-off.
