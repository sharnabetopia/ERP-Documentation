# Test Coverage Report

Document status: Draft v0.1 (final audit, 2026-11-11). Measured by script from the written test case files. **Written is not executed.** Execution, pass rates, defect counts and code coverage cannot be reported because no test has run and no code exists.
Related: [RTM](../08-QA-and-Testing/RTM.md), [Test Plan](../08-QA-and-Testing/Test-Plan.md), [Test Strategy](../08-QA-and-Testing/Test-Strategy.md).

## 1. Execution status

| Measure | Value |
|---|---|
| Test cases written | 553 |
| Status Not Run | 553 |
| Executed | 0 |
| Passed | 0 |
| Failed | 0 |
| Defects logged | 0 |
| Automated scripts | 0 |
| Code coverage | Not measurable |

## 2. Cases by file

| File | Cases |
|---|---|
| API | 186 |
| Administration | 67 |
| Security | 66 |
| Cross-Module | 61 |
| Finance | 46 |
| Procurement | 22 |
| Inventory | 20 |
| UAT | 20 |
| Authentication | 18 |
| Sales | 16 |
| E2E | 16 |
| Performance | 12 |
| HR | 3 |
| **Total** | **553** |

## 3. Cases by priority, severity and type

| Priority | Cases |
|---|---|
| P0 | 361 |
| P1 | 186 |
| P2 | 6 |

| Severity | Cases |
|---|---|
| Critical | 169 |
| High | 304 |
| Medium | 76 |
| Low | 4 |

| Test type | Cases |
|---|---|
| Permission | 131 |
| Functional | 87 |
| Security | 76 |
| API | 58 |
| Negative | 54 |
| Integration | 41 |
| UAT | 20 |
| Inspection | 17 |
| Boundary | 14 |
| End-to-end | 12 |
| Edge case | 11 |
| Concurrency | 10 |
| Performance | 8 |
| Data isolation | 7 |
| Validation | 3 |
| UI | 2 |
| Accessibility | 1 |
| Cross-browser | 1 |

P0 is 65.3 percent of cases. That is high (see gap TG-07).

## 4. Required test categories

| Category (from the brief) | Written cases | Where |
|---|---|---|
| Positive | 77 | Module files |
| Negative and validation | 105 | Module and API files |
| Boundary and equivalence | 14 | Module files |
| Permission and role-based access | 133 | API file (generated) and module files |
| Data isolation | 7 | Module and cross-module files |
| Integration | 41 | Cross-module file |
| End-to-end | 12 | E2E file |
| API | 186 | API file |
| Security | 66 | Security file |
| Performance | 8 | Performance plan |
| Concurrency | 10 | Module, cross-module, performance |
| UI, accessibility, cross-browser | 4 | E2E file (thin) |
| UAT | 20 | UAT file |
| Regression | Suites defined from the above | Regression Test Plan (no separate cases) |

## 5. Coverage of the specified edge cases

| Edge case from the brief | Covered by written case |
|---|---|
| Duplicate transactions | Yes |
| Partial payments | Yes |
| Advance payments | Yes |
| Refunds and reversals | Yes |
| Purchase order cancellation | Yes |
| Partial goods receipt | Yes |
| Duplicate invoices | Yes |
| Negative stock prevention | Yes |
| Stock reservation conflicts | Yes |
| Concurrent updates | Yes |
| Financial year closing | Yes |
| Bank reconciliation discrepancies | No: R2 modules (M-037, M-038), out of MVP |
| Budget overruns | No: R2 modules (M-037, M-038), out of MVP |
| Approval delegation | Yes |
| Unauthorized approvals | Yes |
| Failed external integrations | Yes |
| Retry and recovery behavior | Yes |

Matching is by keyword in test titles, so "Yes" means a case exists whose title addresses the topic. It does not prove adequate depth.

## 6. Gaps

| ID | Gap |
|---|---|
| TG-01 | No test executed |
| TG-02 | NFR targets unknown, performance cases cannot pass or fail |
| TG-03 | No cases for R2 to R4 modules or outline workflows |
| TG-04 | Few UI, accessibility and cross-browser cases (4) |
| TG-05 | No automation |
| TG-06 | Cases follow Proposed rules |
| TG-07 | Priority inflation |
| TG-08 | Generated permission and error-code cases prove consistency with the documents, not that the documents are right |
| TG-09 | Test data and reference sets not built or signed |
| TG-10 | No regression case set separate from the functional cases (suites are selections) |
