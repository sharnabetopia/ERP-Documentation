# QA Coverage Improvement Report

Phase 2 remediation, 2026-11-11. Related: [Remediation Progress](Remediation-Progress.md), [Final Revalidation Report](Final-Revalidation-Report.md), [Audit baseline](../12-Final-Audit/Final-Readiness-Assessment.md).

No stakeholder approval is claimed anywhere in this report. Values marked PENDING APPROVAL are recommendations.

## 1. Before and after (recomputed on the updated files)

| Measure | Audit baseline | Now |
|---|---|---|
| Test cases | 553 | 1038 |
| FRs | 130 | 150 |
| FRs with a direct test case | 130 | 150 |
| FRs with exactly one direct test case | 36 | 0 |
| FRs with no negative, boundary, edge, validation, security or concurrency case | 59 | 0 |
| FRs with no positive, end-to-end, UAT, integration, functional or regression case | 25 | 0 |
| Test cases of type Regression | 0 | 38 |
| NFRs, TRs, SECs without a test | 0, 0, 0 | 0, 0, 0 |
| Threats without a direct or mitigation test | 9 by the corrected count | 0  |
| Test cases with empty required cells (Actual Result excluded) | 0 | 0 |
| Test cases executed | 0 | 0. Status Not Run for 1038 of 1038. Actual Result filled for 0 |

## 2. What was added

| File | Cases | Purpose |
|---|---|---|
| `test-cases/Authorisation-Hardening-Test-Cases.md` | 95 (TC-0554 to TC-0648) | Privilege escalation, self-approval, unauthorised approval and financial modification for FR-131 to FR-150 |
| `test-cases/NFR-Measurable-Test-Cases.md` | 31 (TC-0649 to TC-0679) | The 21 NFRs that gained measurable targets |
| `test-cases/Negative-Boundary-Edge-Test-Cases.md` | 70 (TC-0680 to TC-0749) | The 59 FRs without negative or boundary cases and 8 untested threats |
| `Regression-Suite.md` | 35 (TC-0750 to TC-0784) plus 7 suites over existing cases | Dedicated regression suite |
| `test-cases/Positive-Flow-Test-Cases.md` | 17 (TC-1035 to TC-1051) | FRs that had no positive-flow case |
| Lifecycle test case files | 237 (TC-0785 to TC-1034, with gaps in the range) | Five lifecycles, see the Lifecycle Completion Report |

## 3. Regression suite

Seven suites: functional, integration, RBAC, financial, data integrity, workflow, API. Part 1 of `Regression-Suite.md` lists membership by rule over existing IDs. Part 2 holds the new cases for the behaviour changed in Phase 2 (invoice approval path, payment states, adjustment approval, refund failure, database constraints DBC-13 to DBC-20, new API endpoints).

## 4. Review of existing cases

All 553 original cases keep their IDs and content. Review findings:
- Field completeness: all 15 required fields are present as columns, with no empty required cell.
- Test data and steps are present. Steps in 163 generated cases are short and the description restates the requirement. They were not rewritten, so that existing IDs and content stay valid. This is listed as a residual weakness.
- 23 cases cite threats or database constraints only. They are now traced through the rebuilt RTM.
- Priority is inflated (P0 share is high, as the v0.1 Test Strategy already notes). Re-triage is still to be done after the first cycle.
- New cases were written with concrete data, expected results, requirement references and IDs that exist. Whether they are executable in practice can only be confirmed by a QA engineer running them.

## 5. What this does not show

No test was executed. A written case is not a pass. Test data uses proposed values (limits, windows, retention). The ERP system does not exist, so none of these cases can run yet.
