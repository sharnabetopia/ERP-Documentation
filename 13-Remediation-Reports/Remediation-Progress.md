# Remediation Progress

Phase 2 remediation, 2026-11-11. Related: [Remediation Progress](Remediation-Progress.md), [Final Revalidation Report](Final-Revalidation-Report.md), [Audit baseline](../12-Final-Audit/Final-Readiness-Assessment.md).
This file is the central remediation register (Step 1), the batch log and the change log.
No stakeholder approval is claimed anywhere in this report. Values marked PENDING APPROVAL are recommendations.

## 1. Summary

| Status | Findings |
|---|---|
| Closed pending approval | 20 |
| Partly closed | 9 |
| Open (deferred) | 16 |
| Open (stakeholder decision) | 4 |
| Closed (verified) | 18 |
| Total | 67 |

Status meanings: "Closed (verified)" means fixed and re-validated by script or re-render with no decision needed. "Closed pending approval" means fixed in the documents and consistent, but it contains PENDING APPROVAL values or depends on a stakeholder decision. "Partly closed" means some of the finding was fixed. "Open (deferred)" was not addressed in Phase 2. "Open (stakeholder decision)" needs a decision that documentation cannot make.
The register consolidates the findings of the ten audit reports. A finding is not counted as closed until its correction was checked. The two audit-baseline files are unchanged.

## 2. Batches

| Batch | Scope | Status |
|---|---|---|
| 1 | RBAC and financial security (Step 2): rules, FRs, permissions, SoD, threats, audit events, API, stories, 95 negative tests | Done, pending approval of values |
| 2 | Architecture decisions and review of four documents (Step 3) | Done, decisions PENDING APPROVAL |
| 3 | RTM rebuild and correction (Step 4) | Done, verified |
| 4 | Diagram contradictions, authoritative workflows (Step 5) | Done for the three priority processes, verified by re-render and assertions |
| 5 | Measurable NFRs (Step 6) | Done, values PENDING APPROVAL |
| 6 | QA coverage and regression suite (Step 7) | Done, nothing executed |
| 7 | Five lifecycles (Step 8) | Done, proposed |
| 8 | Open question classification (Step 9) | Done |
| 9 | Documentation updates, change log (Step 10) | Done |
| 10 | Re-validation and reports (Steps 11 to 13) | Done |

## 3. Remediation register

| ID | Source | Affected document | Requirement or module | Severity | Root cause | Correction | Dependencies | Status |
|---|---|---|---|---|---|---|---|---|
| RF-001 | RBAC E1; Critical Gaps 1 | RBAC-Matrix 8A.1 to 8A.2; Authentication-Authorization 5 | FR-131, FR-134; M-004 | P1 | Branch and Organization Admin could assign roles with no authority ranking | Role tiers, grant rules, branch-limited assignment | ADR-022, OQ-033 | Closed pending approval |
| RF-002 | RBAC E2 | RBAC-Matrix 8A.2, 8A.4 | FR-132, FR-133; M-004 | P1 | Org Admin could grant roles to self and had no checker | No self-grant, four-eyes approval by Super Admin, SOD-28, SOD-29, DBC-14 | OQ-033 | Closed pending approval |
| RF-003 | RBAC E3 | RBAC-Matrix 5, 8A | FR-142; M-004 | P1 | No code for SoD override or SoD list edit | Override request and approve codes, controlled change for SoD rules, SOD-27 | - | Closed pending approval |
| RF-004 | RBAC E4 | Authentication-Authorization 5 | US-008, FR-012 | P2 | Super Admin has no business permissions yet had to create business roles | v0.1 rule withdrawn. Tier from highest permission, four-eyes for approve, release, reverse roles | - | Closed pending approval |
| RF-005 | RBAC E5 | RBAC-Matrix 1, 8A.11 | M-004 | P2 | Employee role stacked onto Auditor and Read-only users | EM added only to T2 or T3 staff | - | Closed pending approval |
| RF-006 | RBAC E6 | Permission-Matrix; RBAC 8A.9 | FR-147, SEC-070 | P2 | Any staff user could delete attachments on approved documents | Delete only on own drafts. Void by Org Admin with reason | - | Closed pending approval |
| RF-007 | RBAC E7 | RBAC-Matrix 8A.11 | M-013 | P2 | Two approve codes, unclear which governs | Decision endpoint requires both generic and document-type code | - | Closed pending approval |
| RF-008 | RBAC E8 | Authentication-Authorization 8; RBAC 8A.7 | FR-016, BRL-059 | P2 | Temporary access had no approver rule or maximum | Same tier and four-eyes rules, 30 day maximum proposed | - | Closed pending approval |
| RF-009 | RBAC E9 | RBAC 8A.10; Permission-Matrix | FR-138, NFR-027 | P2 | Super Admin could shorten audit retention | Retention rules are controlled data with maker-checker | OQ-022 | Closed pending approval |
| RF-010 | RBAC E10 | Authentication-Authorization 3 | SEC-009, SEC-010 | P2 | 15 minute token and revocation on refresh contradicted SEC-009 and SEC-010 | 5 minute token proposed, per-request revocation | Security officer | Closed pending approval |
| RF-011 | Critical Gaps 1; RBAC section 3 | RBAC 8A.4; AWD-01 | FR-135, FR-136, FR-137; M-047, M-035 | P1 | Finance Manager could create, approve and release the same payment or journal | Separate release permission, three-user rule, journal system posting, DBC-13 | OQ-034 | Closed pending approval |
| RF-012 | RBAC section 3 | RBAC 8A.4; API-176 to API-178 | FR-139; M-054 | P1 | No permission code or checker for vendor bank change | Request, verify, approve by three users, payment hold, SOD-20 to SOD-22 | OQ-035 | Closed pending approval |
| RF-013 | RBAC section 3; Critical Gaps 1 | RBAC 8A.4; API-173 to API-175 | FR-140; M-043 | P1 | Period reopen had no code and no approver | Request and approve codes, auto re-close, SOD-24, SOD-25 | OQ-034 | Closed pending approval |
| RF-014 | RBAC section 3 | RBAC 8A.4; API-179, API-180 | FR-141; M-079 | P1 | No code or checker for credit limit change | Request and approve, SOD-23 | - | Closed pending approval |
| RF-015 | Critical Gaps 2; RBAC section 1 | Permission-Matrix Phase 2 additions | FR-146; M-001, M-012, M-015, M-065, M-044 | P1 | Four MVP modules had no permission rows | Codes added for the four modules and fiscal year. M-030, M-032, M-033, M-034, M-064 and M-116 partial coverage remains | - | Partly closed |
| RF-016 | RBAC section 3 | RBAC 5 | SOD-14 to SOD-32 | P1 | High-risk SoD pairs missing (journal, payment, PO editor, bank, settings, stock adjustment, period) | 19 SoD pairs added. Payroll deferred to R2 | - | Closed pending approval |
| RF-017 | RBAC section 3 | Audit-Trail 3A | AUD-101 to AUD-127 | P2 | Events missing for break-glass, service accounts, overrides | 27 events added. IdP mapping change and report-view reads not added | - | Partly closed |
| RF-018 | RBAC section 4 | Threat-Model 5A | THR-045 to THR-059 | P2 | Threat gaps for escalation, collusion, bank diversion, delegation, break-glass, service accounts | 15 threats added. XSS, CSRF, SSO linking and upload malware threats not added | - | Partly closed |
| RF-019 | RBAC section 5 | Authentication-Authorization 2, 10 | FR-144, FR-149 | P2 | Service accounts, break-glass undefined | Both defined. Impersonation, user API tokens, SSO claim mapping, admin reset not defined | OQ-023 | Partly closed |
| RF-020 | RBAC section 3 | Permission-Matrix | FR-148, SEC-071 | P2 | Generic export, no user export or unmask code | Export rule, user export and unmask codes. Subject export code not added | - | Partly closed |
| RF-021 | RBAC section 3 | RBAC 8A.11 | FR-019, SEC-017 | P2 | Group functions promised but no code | Declared out of MVP | - | Open (deferred) |
| RF-022 | RBAC section 3 | RBAC 8A.11 | US-017 | P2 | Shared master data conflicts with company predicate | Recorded as OQ-037 | OQ-037 | Open (stakeholder decision) |
| RF-023 | RBAC section 3 | Authentication-Authorization 4 | NOTF-001 | P3 | 403 vs 404 leaks company existence | 404 same body for unassigned company | - | Closed (verified) |
| RF-024 | RBAC section 1 | User-Stories | Roles | P3 | Story roles with no RBAC role (9 names) | Not mapped in Phase 2 | - | Open (deferred) |
| RF-025 | Critical Gaps 3 | ARD 6A; ADR-017 to ADR-021 | OQ-005, 015, 016, 019, 024 | P1 | Five foundational decisions unanswered | Alternatives, comparison and recommendation recorded. Decisions stay PENDING APPROVAL | Sponsor, IT manager, Architect | Closed pending approval |
| RF-026 | Documentation, Consistency | Integration, Infrastructure, Data-Migration, Deployment | ARD | P2 | Four architecture documents not inspected in depth | Integration, Infrastructure and Data-Migration reviewed with 12 corrections recorded. ADR-001 to ADR-016 reviewed for status and ADR-002 only | - | Partly closed |
| RF-027 | Requirements 4.1 | RTM 3A | 14 FR to NFR links | P1 | Substring match of FR number inside NFR number | Links judged by meaning, register with reasons, regenerated | - | Closed (verified) |
| RF-028 | Requirements 4.2 | RTM 1 | 41 FR to TC links | P1 | Same cause | Links rebuilt from exact IDs only | - | Closed (verified) |
| RF-029 | Requirements 4.3 | RTM 5; Security-Requirements | 21 SEC with no FR | P3 | Cross-cutting controls traced to NFR only | Reason column added, no FR forced | - | Closed (verified) |
| RF-030 | New finding in Phase 2 | RTM 5, 8.1 | THR | P2 | v0.1 RTM claimed 44 of 44 threats covered, but 30 rows showed no test | Coverage recomputed (direct, via SEC), tests added for untested threats | - | Closed (verified) |
| RF-031 | Requirements 4.4 | RTM 8 | FR with one test | P3 | Different definitions in v0.1 | Recomputed from exact IDs | - | Closed (verified) |
| RF-032 | Diagram D1, D2 | Process-Flow, Swimlane, AWD-02 | WF-01 | P1 | Variance and approval path contradicted | One definition AWD-02, diagrams and tables aligned, re-rendered | - | Closed (verified) |
| RF-033 | Diagram D3 | Process-Flow, Swimlane, AWD-03, FR-109 | WF-06 | P1 | Adjustment approval contradicted and FR-109 inconsistent | AWD-03, diagrams aligned, FR-109 clarified | OQ-038 | Closed pending approval |
| RF-034 | Diagram D4 | Process-Flow, Swimlane, AWD-04 | WF-12 | P1 | Refund conditions and failure path contradicted | AWD-04, diagrams aligned | - | Closed (verified) |
| RF-035 | Diagram D5 | SEQ-11 | WF-12 | P2 | Wrong direction and wrong engine | SEQ-11 rewritten | - | Closed (verified) |
| RF-036 | Diagram D6 | State diagrams, Data-Dictionary | requisition, invoice, payment, journal, adjustment | P2 | Terminal states disagreed | Dictionary and diagrams aligned for five entities. Others not checked | - | Partly closed |
| RF-037 | Diagram D7 | Sales order state | SEQ-06 | P3 | Credit hold state missing | Not changed | - | Open (deferred) |
| RF-038 | Diagram D8 | State diagrams, AWD-01 | payment, invoice | P1 | No reversal path, posting before bank acceptance | Bank clearing posting, system reversal on failure, invoice cancel transition | - | Closed (verified) |
| RF-039 | Diagram D9 | SEQ-02 | Authorization flow | P2 | Sequence dropped steps of the 9-step procedure | Not changed | - | Open (deferred) |
| RF-040 | Diagram D10 | SEQ-03, 04, 06, 09, 10 | Sequences | P3 | Permission and audit steps missing | SEQ-05 and SEQ-11 updated, others not | - | Partly closed |
| RF-041 | Diagram D11, D12 | ERD | open_item, stock_transfer, approval | P3 | Cardinality and parent links | Not changed. Section 7 added for new tables only | - | Open (deferred) |
| RF-042 | Diagram D13 | DFD | DFD-1 | P3 | Not balanced with DFD-0 | Not changed | - | Open (deferred) |
| RF-043 | Diagram D14 | Process-Flow WF-02 | WF-02 | P3 | Partial delivery and approval paths | Not changed | - | Open (deferred) |
| RF-044 | Diagram D15 | Process-Flow | WF-03, WF-07 and others | P3 | Exception paths missing | WF-04, 09, 13 covered by lifecycle documents. WF-03, WF-07 not changed | - | Partly closed |
| RF-045 | Diagram D16 | SEQ-01 | Auth | P3 | MFA failure branch | Not changed | - | Open (deferred) |
| RF-046 | Missing Documents | Component diagram, C4, ERD, state diagrams | Diagrams | P3 | Diagram types incomplete | Lifecycle documents add state diagrams for new entities. UML component diagram, C4 components, 19 ERD tables not added | - | Open (deferred) |
| RF-047 | Requirements 2, Critical Gaps 4 | NFR 1, 1A | 21 NFRs | P1 | No measurable target | Measurable proposed values, tests, method | OQ-015, 022, 028 | Closed pending approval |
| RF-048 | Test report 2, Critical Gaps 7 | Negative-Boundary-Edge-Test-Cases | 59 FRs | P2 | Missing negative, boundary, edge cases | 70 cases written | - | Closed (verified) |
| RF-049 | Test report 2 | Regression-Suite | Regression | P2 | No regression cases | Seven suites and 35 new cases | - | Closed (verified) |
| RF-050 | Test report 3 | Existing test files | 163 template descriptions, 23 untraced cases | P3 | Generated text | Not rewritten (existing IDs preserved) | - | Open (deferred) |
| RF-051 | Critical Gaps 8, Missing Features 3 | Lifecycles | WF-04, 05, 08, 09, 13 | P2 | Outline only | Five lifecycle documents with diagrams and test cases | - | Closed pending approval |
| RF-052 | Missing Features 4 | Notification and report catalogues | M-012 | P3 | No catalogue | Not written | - | Open (deferred) |
| RF-053 | Missing Features 2 | FRS, NFR | BR-062 | P2 | MVP BR with no FR and TBD targets | Covered by NFR section 1A measurable targets, no FR written | OQ-028 | Closed pending approval |
| RF-054 | Missing Features 2 | BRD | 16 BRs | P3 | Post-MVP | No FR written | - | Open (deferred) |
| RF-055 | Requirements 3 | Use-Cases, API | 71 FRs, 16 FRs | P3 | Use cases and APIs cover critical flows only | Not changed | - | Open (deferred) |
| RF-056 | Consistency 2 | SRS, ERD, Data-Dictionary, API doc | Stale counts | P3 | Counts not updated | Corrected | - | Closed (verified) |
| RF-057 | Consistency 2 | API-Documentation | INV prefix | P3 | Same prefix for errors and invariants | Error codes renamed STK-001 to STK-004 | - | Closed (verified) |
| RF-058 | Consistency 1 | User-Stories, Security-Requirements | US-004, SEC-006 | P3 | MFA priority differs | Not changed | OQ-023 | Open (stakeholder decision) |
| RF-059 | Documentation 3 | All documents | TBD | P2 | Business values unknown | Open questions classified. TBD count recomputed | Stakeholders | Open (stakeholder decision) |
| RF-060 | Documentation 2 | Deployment, Performance, Backup, Maintenance | Placeholders | P3 | Placeholder sections | Deployment capacity note only | - | Open (deferred) |
| RF-061 | Documentation 2 | PRD, SRS | Thin documents | P3 | Index style | SRS counts corrected only | - | Open (deferred) |
| RF-062 | Test report 1 | All test files | Execution | P1 | No test executed | Cannot be fixed by documentation | Build and QA | Open (deferred) |
| RF-063 | Critical Gaps 3, Readiness | All | Approval | P1 | Nothing approved by a stakeholder | No approval claimed | Sponsor | Open (stakeholder decision) |
| RF-064 | Test report 2 | Positive-Flow-Test-Cases | 25 FRs without a positive-flow case | P2 | Missing positive cases | 17 positive cases written | - | Closed (verified) |
| RF-065 | Phase 2 re-validation | User-Stories | FR-136, FR-138, FR-143 | P3 | New FRs had no story | US-099 to US-101 added | - | Closed (verified) |
| RF-066 | Phase 2 re-validation | API-Documentation, Permission-Matrix | New permission codes | P3 | Codes with no endpoint | 14 endpoints added (API-189 to API-202), 5 existing endpoints re-pointed | - | Closed (verified) |
| RF-067 | Phase 2 re-validation | Error codes | SOD-001, SOD-002 | P3 | Error codes shared a prefix with SoD rules SOD-01 to SOD-32 | Renamed DUTY-001 and DUTY-002 | - | Closed (verified) |

## 4. Pending items

Open (stakeholder decision) and PENDING APPROVAL items are listed in the Open Questions Resolution Report. Open (deferred) items are the diagram, ERD, catalogue and role-name gaps in the register.

## 5. Change log (existing files changed in Phase 2, 58 files)

Every changed file states its new version in its status line or has a Phase 2 section. Files not listed were not changed. The archive `12-Final-Audit/_self-audit-v0.1` and the ten audit reports in `12-Final-Audit` were not modified.

- `02-Business-Analysis/Business-Rules.md`
- `02-Business-Analysis/Open-Questions.md`
- `03-Product-and-System-Requirements/FRS.md`
- `03-Product-and-System-Requirements/NFR.md`
- `03-Product-and-System-Requirements/PRD.md`
- `03-Product-and-System-Requirements/SRS.md`
- `03-Product-and-System-Requirements/User-Stories.md`
- `04-System-Architecture/API-Documentation.md`
- `04-System-Architecture/ARD.md`
- `04-System-Architecture/Architecture-Decision-Records.md`
- `04-System-Architecture/Data-Dictionary.md`
- `04-System-Architecture/Data-Migration-Strategy.md`
- `04-System-Architecture/Database-Design.md`
- `04-System-Architecture/Deployment-Architecture.md`
- `04-System-Architecture/ERD.md`
- `04-System-Architecture/Infrastructure-Architecture.md`
- `04-System-Architecture/Integration-Architecture.md`
- `04-System-Architecture/System-Architecture.md`
- `05-Workflows-and-Diagrams/Activity-Diagrams.md`
- `05-Workflows-and-Diagrams/Authoritative-Workflow-Definitions.md`
- `05-Workflows-and-Diagrams/C4-Architecture-Diagrams.md`
- `05-Workflows-and-Diagrams/Cross-Module-Workflows.md`
- `05-Workflows-and-Diagrams/Data-Flow-Diagrams.md`
- `05-Workflows-and-Diagrams/Process-Flow.md`
- `05-Workflows-and-Diagrams/Sequence-Diagrams.md`
- `05-Workflows-and-Diagrams/State-Transition-Diagrams.md`
- `05-Workflows-and-Diagrams/Swimlane-Diagrams.md`
- `06-RBAC-and-Security/Audit-Trail.md`
- `06-RBAC-and-Security/Authentication-Authorization.md`
- `06-RBAC-and-Security/Permission-Matrix.md`
- `06-RBAC-and-Security/RBAC-Matrix.md`
- `06-RBAC-and-Security/Security-Requirements.md`
- `06-RBAC-and-Security/Threat-Model.md`
- `07-UI-UX-Specifications/Form-Validation-Rules.md`
- `07-UI-UX-Specifications/Screen-Specifications.md`
- `08-QA-and-Testing/API-Test-Cases.md`
- `08-QA-and-Testing/RTM.md`
- `08-QA-and-Testing/Regression-Suite.md`
- `08-QA-and-Testing/test-cases/Administration-Test-Cases.md`
- `08-QA-and-Testing/test-cases/Authorisation-Hardening-Test-Cases.md`
- `08-QA-and-Testing/test-cases/Finance-Test-Cases.md`
- `08-QA-and-Testing/test-cases/Inventory-Test-Cases.md`
- `08-QA-and-Testing/test-cases/NFR-Measurable-Test-Cases.md`
- `08-QA-and-Testing/test-cases/Negative-Boundary-Edge-Test-Cases.md`
- `08-QA-and-Testing/test-cases/Positive-Flow-Test-Cases.md`
- `08-QA-and-Testing/test-cases/Procurement-Test-Cases.md`
- `08-QA-and-Testing/test-cases/Security-Test-Cases.md`
- `README.md`

New files: `05-Workflows-and-Diagrams/Authoritative-Workflow-Definitions.md`, five lifecycle documents in `05-Workflows-and-Diagrams/Lifecycles/`, test case files `Authorisation-Hardening-Test-Cases.md`, `NFR-Measurable-Test-Cases.md`, `Negative-Boundary-Edge-Test-Cases.md`, `Positive-Flow-Test-Cases.md` and five `Lifecycle-WF-nn-Test-Cases.md`, `08-QA-and-Testing/Regression-Suite.md`, and this folder (`13-Remediation-Reports`, including `evidence/RTM-v0.1-before-remediation.md`).
