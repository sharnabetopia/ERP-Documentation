# Missing Features Report

Audit date 2026-11-11. Related: [Documentation Audit](Documentation-Audit-Report.md), [Final Readiness](Final-Readiness-Assessment.md), [Critical Gaps](Critical-Gaps-and-Risks.md).
Sources: `01-ERP-Module-Analysis/ERP-Module-Inventory.md`, `Module-Feature-Breakdown.md`, FRS, `06-RBAC-and-Security/*`.

## 1. Module completeness (measured)

| Measure | Value |
|---|---|
| Modules in inventory | 120 (MVP 58, R2 20, R3 25, R4 17, plus mixed-release rows) |
| MVP modules with a 10-field row in the feature breakdown | 58 of 58 |
| MVP modules with at least one FR | 58 of 58 |
| MVP modules with at least one test case | 58 of 58 (10 modules have exactly one) |
| Non-MVP modules (62) | List level only. No FR, no story, no test. By design, but outside "complete ERP". |
| MVP rows with an empty or "none" cell | Status lifecycle 9, Rules 4, Key validations 3, Dependencies 1 |

Fields the brief asks for that the feature breakdown does not hold per module: notifications, reports, permissions, workflows, positive and negative scenarios.
Permissions are in the RBAC files, scenarios are in test cases, and workflows are cross-module. Notifications and reports have no per-module home (see [Missing Documents](Missing-Documents-Report.md) section 2).

## 2. Business requirements with no functional requirement

16 BRs have no FR, no story, no API and no test: BR-022, 023, 031, 032, 040, 048, 050, 051, 052, 053, 054, 055, 056, 057, 062, 063.
All are R2 or later except BR-062 (service levels, MVP, P1), which is a non-functional need whose targets are TBD. BR-062 is an MVP requirement with no FR and no direct test, so it is the only genuine MVP gap among the 16.
Source: `02-Business-Analysis/BRD.md` requirement table.

## 3. Requested lifecycles

| Lifecycle | WF | Detail level | Swimlane | Sequence | State diagram | Test cases | Verdict |
|---|---|---|---|---|---|---|---|
| Procure-to-Pay | WF-01 | Full | Yes | Partial (GRN, payment, approval only) | Yes, no GRN | 102 | PARTIAL |
| Order-to-Cash | WF-02 | Full | Yes | Partial (SEQ-06 only) | Partial, no quotation | 66 | PARTIAL |
| Record-to-Report | WF-03 | Full | Yes | Partial (SEQ-09, no journal posting) | Yes | 89 | PARTIAL |
| Hire-to-Retire | WF-04 | Outline | No | No | No | 23 (core employee record) | FAIL |
| Plan-to-Produce | WF-05 | Outline, R4 | No | No | No | 0 | FAIL |
| Inventory-to-Fulfillment | WF-06 | Full | Yes | Partial (transfer only) | Yes | 66 | PARTIAL |
| Request-to-Approval | WF-07 | Full | Yes | Yes | Yes | 45 | PASS with the gaps in the Diagram report |
| Expense-to-Reimbursement | WF-08 | Outline, R2 | No | No | No | 0 | FAIL |
| Asset Acquisition-to-Disposal | WF-09 | Outline, R2/R3 | No | No | No | 0 | FAIL |
| Contract-to-Renewal | WF-13 | Outline, R3 | No | No | No | 0 | FAIL |

Test case counts for workflows come from `08-QA-and-Testing/RTM.md` section 6 and were not independently recounted per workflow. Marked NOT VERIFIED for the exact numbers.
Failure and recovery paths: full for WF-01, 02, 03, 06, 07. Outline or missing for the other five (details in [Diagram Validation](Diagram-Validation-Report.md)).

## 4. Features missing inside MVP scope

| Gap | Where |
|---|---|
| No notification catalogue per module or event | FRS FR-035 to FR-038 only |
| No financial statement or report catalogue | FR-100, 112, 124 are module reports. Finance statements are not named as FRs. NOT VERIFIED beyond a grep of FRS titles. |
| Group or cross-company functions promised (FR-019, SEC-017) but no permission code or role | `06-RBAC-and-Security/Permission-Matrix.md` |
| Vendor bank change approval has no code | `RBAC-Matrix.md` section 6 |
| Credit limit change (AUD-033) has no code or checker | `Audit-Trail.md` |
| Year-end close has no permission code | AUD-051 |
| Draft delete has no permission code except attachments | `RBAC-Matrix.md` section 1 vs Permission-Matrix |
| Service accounts, API tokens, impersonation, break-glass | `Authentication-Authorization.md` section 2 (one line) |
| 71 of 130 FRs have no use case | `Use-Cases.md` has 16 use cases |
| 16 FRs have no API endpoint | e.g. FR-014, 015, 019, 030, 032, 036, 037, 040, 045, 046, 068, 069, 071, 106, 113, 126 |
