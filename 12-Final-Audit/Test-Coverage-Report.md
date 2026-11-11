# Test Coverage Report

Audit date 2026-11-11. Related: [Documentation Audit](Documentation-Audit-Report.md), [Final Readiness](Final-Readiness-Assessment.md), [Critical Gaps](Critical-Gaps-and-Risks.md).
Scope: 553 test cases in 12 files under `08-QA-and-Testing/`. Parsed by script. Nothing was executed, because the documents describe a system that does not exist yet.

## 1. Field completeness

| Required field | Result |
|---|---|
| Test Case ID, Requirement ID, Module, Submodule, Scenario, Title, Preconditions, Test Data, Execution Steps, Expected Result, Priority, Severity, Test Type, Execution Status | Present as columns in all 12 files. 0 empty cells in 553 rows. |
| Actual Result | Column present, blank in 553 of 553 (correct for unexecuted tests). |
| Execution Status | "Not Run" in 553 of 553. |
| Story ID, Description | Extra columns, present. |

Cases per file: API 186, Administration 67, Security 66, Cross-Module Integration 61, Finance 46, Procurement 22, UAT 20, Inventory 20, Authentication 18, Sales 16, E2E 16, Performance 12, HR 3, Asset 0, Manufacturing 0, Project 0.
Priority: P0 361, P1 186, P2 6 (matches the progress file). Severity split measured on the 541 cases outside the Performance plan: Critical 166, High 299, Medium 72, Low 4.

## 2. Coverage by test category

| Category (brief) | Cases (by Test Type) | Assessment |
|---|---|---|
| Positive | Functional 87 | PARTIAL: 25 FRs have no positive, E2E, UAT or integration case |
| Negative | Negative 54 (Scenario "Negative" 100) | PARTIAL: 59 FRs have no negative, boundary, edge, validation or abuse case |
| Boundary values | 14 | WEAK: 13 FRs. Money rounding, quantity limits and date limits are mostly untested at the edges. |
| Validation | 3 | WEAK. `07-UI-UX-Specifications/Form-Validation-Rules.md` defines 18 forms and 30 messages. Only 3 cases named Validation. |
| CRUD | Within Functional | NOT VERIFIED as a separate category. No case is tagged CRUD. |
| RBAC | Permission 131, Data isolation 7 | GOOD in volume. Cases attach to API IDs, so only a few FRs name a Permission case directly (125 of 130 FRs have none). |
| Integration | 41 | PARTIAL |
| End-to-end | 12 (plus 16 E2E-file rows with Scenario End-to-end) | PARTIAL: WF-01, 02, 03, 06, 07, 12 only |
| API | 58 | GOOD for contract checks. 14 endpoints have no direct case. |
| Security | 76 plus Abuse case 13 | GOOD |
| Performance | 8 (12 in the plan) | PARTIAL: targets are TBD for several (OQ-015, OQ-028) |
| Regression | 0 | FAIL: no case carries the type. |
| UAT | 20 | PARTIAL: 20 for 80 stories |
| Edge cases | 11 | WEAK |
| Accessibility, cross-browser, UI | 1, 1, 2 | WEAK against NFR-032 and NFR-033 |
| Concurrency | 10 | PARTIAL against NFR-024 |

## 3. Quality of the cases

| Issue | Count | Evidence |
|---|---|---|
| Descriptions generated from a template ("Positive test of X: ...") | 163 of 553 | Finance, Procurement, Inventory, Sales files. These restate the requirement instead of describing the test. |
| Identical steps, data and expected result | 1 duplicate pair | script check on steps, expected result and test data |
| Identical titles | 0 | |
| Steps that are a single line ("1. Create account") | Common in generated rows, for example TC-0090, TC-0091. | These are short but executable by a person who knows the system. NOT counted as defective. |
| Test data that is a placeholder | Present where the value is TBD, for example TC-0091 COA template (OQ-018) | PARTIAL |
| Cases that cannot run until a decision is made | The 12 performance cases and any case citing a TBD value | NOT EXECUTABLE yet |
| Cases with no FR, BR, NFR, TR, API or SEC | 23 | They cite THR or DBC IDs. Valid but not requirement-traced. |
| Incorrectly linked cases | 41 RTM links not backed by the case itself | See [Requirements Coverage](Requirements-Coverage-Report.md) section 4. |

Duplicates and template restatements are not counted as meaningful coverage below.

## 4. Meaningful coverage estimate

| Measure | Value | Method |
|---|---|---|
| FRs with at least one direct case | 130 of 130 | direct FR ID match |
| FRs with both a positive-flow case and a negative-type case (direct FR IDs only) | 51 of 130 (39.2 percent) | computed exactly: 130 minus the union of the 25 and 59 FRs lacking each kind. Permission cases attached through API IDs are not counted. |
| FRs with exactly one direct case | 36 | |
| MVP modules with a case | 58 of 58 (10 with one case) | |
| Test cases executed | 0 of 553 | |
| Requirements verified | 0 of 130 FRs | |

## 5. Gaps by module

- Asset (M-042, M-110), Manufacturing (M-097 to M-109), Project: 0 cases by design (post-MVP).
- HR: 3 cases for M-016 only. WF-04 has 23 cases in the RTM, which come mostly from Administration cases that mention the employee link. NOT VERIFIED.
- Finance FRs without a negative-type case: FR-054, 063, 064, 066, 069, 074, 075, 077, 079, 080, 081, 082, 083, 084. These are money-handling requirements.
- Workflows with no test case: WF-05, 08, 09, 10, 11, 13, 14.
