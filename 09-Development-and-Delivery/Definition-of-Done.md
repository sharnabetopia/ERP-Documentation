# Definition of Done (DoD)

Document status: Draft v0.1 (Stage 3 part 2). Proposed. A story is done only when every applicable item is true. Done means releasable, not "coded".
Related: [Definition of Ready](Definition-of-Ready.md), [Coding Standards](Coding-Standards.md), [Test Strategy](../08-QA-and-Testing/Test-Strategy.md), [CI/CD Strategy](CICD-Strategy.md).

## 1. Story-level checklist

| # | Criterion | Check |
|---|---|---|
| 1 | Acceptance criteria met and demonstrated to the business owner | Demo |
| 2 | Code reviewed by a second person (and a second reviewer for sensitive areas) | Pull request |
| 3 | Build, lint, static analysis and unit tests pass in CI | Pipeline |
| 4 | Unit tests cover the new business rules, including negative and boundary cases. Coverage meets the Test Strategy targets | Coverage report |
| 5 | Integration and API tests for the story's FRs are written and pass. Written test cases (TC IDs) are automated or have an agreed manual plan | RTM, test run |
| 6 | Permission and scope checks implemented and tested for each new endpoint (permission code present in the catalogue, denied-role test passes) | RBAC tests |
| 7 | Audit events written for every audited action in the story (Audit Trail catalogue) | Audit test |
| 8 | Errors use catalogue codes and messages. No stack traces or internal detail leak | API tests |
| 9 | No new Critical or High findings from security scans and dependency scans | Pipeline |
| 10 | Posting, tax, rounding, stock or numbering changes pass the reference data sets and invariant checks | Finance reference tests |
| 11 | Database migrations reviewed, tested on sized data, backward compatible, with rollback or restore plan | Migration review |
| 12 | Performance impact checked for hot paths. No query without index support on large tables | Review, query plan |
| 13 | Logging and monitoring added: correlation id, key events, alerts if new failure mode | Review |
| 14 | UI: states (empty, loading, error), validation messages, keyboard use and accessibility checks pass | UI test, a11y checks |
| 15 | Documentation updated in the same change: requirements affected, API documentation, data dictionary, RBAC matrix, screen specs. The RTM is regenerated | Docs diff |
| 16 | Feature flag state defined. Configuration documented | Release notes draft |
| 17 | Deployed to the Test environment and verified by QA. No open defects above agreed severity | QA sign-off |
| 18 | Product owner accepts the story | Acceptance |

## 2. Sprint-level checklist

| # | Criterion |
|---|---|
| 1 | All committed stories done or moved back with a reason |
| 2 | Regression smoke and core suites pass on the integrated build |
| 3 | No open Critical defects. High defects have a plan |
| 4 | Sprint demo held, feedback recorded |
| 5 | Technical debt items logged with severity |

## 3. Release-level checklist

Release gates G1 to G9 in the [MVP Strategy](../00-Project-Management/MVP-Strategy.md) and the [Release Management](Release-Management.md) checklist.

## 4. Defect fix definition of done

The fix has a regression test that fails on the baseline commit and passes on the fix. The defect record links the test. QA retests and closes.

## 5. Exceptions

An item may be waived only by the product owner and QA lead together, in writing, with a reason and a follow-up story. Waivers on items 6, 7, 9, 10 need the architect or security officer too.
