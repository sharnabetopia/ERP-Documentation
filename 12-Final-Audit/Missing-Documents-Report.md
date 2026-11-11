# Missing Documents Report

Audit date 2026-11-11. Related: [Documentation Audit](Documentation-Audit-Report.md), [Final Readiness](Final-Readiness-Assessment.md), [Critical Gaps](Critical-Gaps-and-Risks.md).
All 103 planned files exist. This report lists documents that are absent, thin, or declared empty, and documents the brief expects that have no home in the package.

## 1. Declared empty or near-empty

| File | Size | Finding | Status |
|---|---|---|---|
| `08-QA-and-Testing/test-cases/Asset-Test-Cases.md` | 857 B | "Contains no test cases, by design." M-042 and M-110 have 0 percent coverage. | FAIL for R2/R3 scope, NOT APPLICABLE for MVP |
| `08-QA-and-Testing/test-cases/Manufacturing-Test-Cases.md` | 926 B | No test cases (R4). | same |
| `08-QA-and-Testing/test-cases/Project-Test-Cases.md` | 1,128 B | No test cases (R3). | same |
| `08-QA-and-Testing/test-cases/HR-Test-Cases.md` | 2,191 B | 3 test cases only. Hire-to-Retire has no functional requirements beyond the core employee record. | PARTIAL |

## 2. Documents the audit brief expects that are not present as a distinct artefact

| Expected item | Finding | Where the nearest content is |
|---|---|---|
| UML Component Diagram | No separate artefact. `04-System-Architecture/System-Architecture.md` section 3 is a flowchart without interfaces and omits Identity and Reporting. | PARTIAL |
| Module-wise test case files for CRM, Budget, Payroll, Contracts, Helpdesk, Quality, Maintenance, Fleet | None exist. These modules are R2 to R4 and have no FRs. | NOT APPLICABLE until their requirements are written |
| Notification catalogue (event, recipient, channel, template per module) | Not present. Only FR-035 to FR-038 and `07-UI-UX-Specifications/Form-Validation-Rules.md` messages MSG-001 onward. | FAIL |
| Report catalogue (financial statements, aged payables, stock valuation, etc.) | Not present. Reporting is FR-100, FR-112, FR-124, FR-126 only. | FAIL |
| Per-module permission definitions for M-001, M-012, M-015, M-065 | No rows in `06-RBAC-and-Security/Permission-Matrix.md`. | FAIL |
| Authorization flow diagram in the security folder | `06-RBAC-and-Security/Authentication-Authorization.md` has a table only. The diagram is SEQ-02 in `05-Workflows-and-Diagrams/Sequence-Diagrams.md`. | PARTIAL |
| Break-glass, service-account and impersonation procedure | Mentioned in one line. No document. | FAIL |
| Regression test cases | `08-QA-and-Testing/Regression-Test-Plan.md` exists, but 0 of 553 test cases have type Regression. | PARTIAL |
| State diagrams for employee, asset, contract, work order, expense claim, quotation, sales return, goods receipt, budget, ticket | Not drawn. `05-Workflows-and-Diagrams/State-Transition-Diagrams.md` has 15 sections. | PARTIAL |
| Swimlane and sequence diagrams for WF-04, 05, 08, 09, 13 | Missing. Only WF-01, 02, 03, 06, 07, 12 have swimlanes. | FAIL for the brief, expected for outline workflows |

## 3. Sections inside documents that are placeholders

- `04-System-Architecture/Deployment-Architecture.md` section 5 "Capacity starting point (placeholder)".
- `08-QA-and-Testing/Performance-Test-Plan.md` section 3 "Workload model (placeholder until OQ-015)".
- `10-Deployment-and-Operations/Backup-and-Recovery.md` section 7 "Runbooks to be written".
- `10-Deployment-and-Operations/Maintenance-SOP.md` section 4 "Standard operational procedures (to be written as runbooks)".
- `04-System-Architecture/Data-Dictionary.md`: no per-column dictionary (the progress file calls it Partial).
- `02-Business-Analysis/As-Is-Analysis.md`: hypotheses only, no baseline from the business.
