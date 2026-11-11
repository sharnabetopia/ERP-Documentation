# Critical Gaps and Risks

Audit date 2026-11-11. Related: [Documentation Audit](Documentation-Audit-Report.md), [Requirements Coverage](Requirements-Coverage-Report.md), [RBAC Audit](RBAC-Audit-Report.md), [Diagram Validation](Diagram-Validation-Report.md), [Cross-Document Consistency](Cross-Document-Consistency-Report.md).
Priority: P1 blocks development of the affected area. P2 should be fixed before build. P3 is a quality fix.

## 1. Critical gaps (prioritised)

| # | Pri | Gap | Where | Why it matters |
|---|---|---|---|---|
| 1 | P1 | Access-control rules for the highest-risk actions are unsafe as written: Branch Admin and Org Admin can assign roles with no authority ranking or checker. Finance Manager creates, approves and releases payments and journals. Vendor bank change, period reopen and credit-limit change have no permission code or checker. | `06-RBAC-and-Security/RBAC-Matrix.md` sections 5 and 6, `Permission-Matrix.md`, `Authentication-Authorization.md` section 5 | Fraud and privilege escalation paths. See RBAC report E1 to E10. |
| 2 | P1 | Four MVP modules have no permission rows (M-001, M-012, M-015, M-065) and several have partial rows. | `Permission-Matrix.md` | Build teams cannot implement authorisation for those modules. |
| 3 | P1 | Five architecture decisions are unanswered: OQ-005 (single customer or SaaS), OQ-015 (volumes), OQ-016 (hosting), OQ-019 (tenant model), OQ-024 (technology constraints). All 32 open questions are Open. | `02-Business-Analysis/Open-Questions.md`, `04-System-Architecture/ARD.md` | Tenancy and data isolation drive the schema. A wrong guess means rework. |
| 4 | P1 | 21 of 42 NFRs have no measurable target. Service level targets (OQ-028), retention periods (OQ-022) and data residency (OQ-010) are TBD. BR-062 has no FR. | `03-Product-and-System-Requirements/NFR.md`, BR-062 | Cannot be tested or accepted. |
| 5 | P1 | RTM is wrong in 27 of 130 FR rows (14 spurious NFR links, up to 41 unsupported test links). The coverage figures built on it are overstated for those rows. | `08-QA-and-Testing/RTM.md` section 1 | Traceability is the release gate. Probable generator bug (substring match of FR inside NFR). |
| 6 | P1 | Workflow diagrams contradict each other and the written workflows for P2P (variance, approval), inventory adjustment approval, and refunds. Payment posts before bank acceptance with no reversal path. Invoices have no reversal transition. | `05-Workflows-and-Diagrams/Process-Flow.md`, `Swimlane-Diagrams.md`, `State-Transition-Diagrams.md` | Developers will implement different flows from different diagrams. |
| 7 | P2 | Test depth: 59 FRs have no negative, boundary, edge, validation or abuse case. 25 have no positive-flow case. 36 have one case. 14 finance FRs lack a negative case. 0 regression cases. | `08-QA-and-Testing/test-cases/*` | Money-handling logic is under-tested on paper. |
| 8 | P2 | Seven of ten requested lifecycles are outline or missing: WF-04, 05, 08, 09, 13 have no swimlane, sequence or state diagram. | `05-Workflows-and-Diagrams/Cross-Module-Workflows.md` | Not an MVP defect, but the brief asks for all ten. |
| 9 | P2 | SoD gap list in RBAC report section 3. Missing threat topics in section 4. Service accounts, API tokens, impersonation and break-glass undefined. | `06-RBAC-and-Security/` | |
| 10 | P2 | Contradictions: session revocation timing, approval limits location (chain vs settings), 403 vs 404 for unassigned scope, shared master data vs company predicate. | Consistency report section 2 | |
| 11 | P2 | No notification catalogue and no report catalogue. | FRS FR-035 to 038, FR-100, 112, 124 | |
| 12 | P3 | Stale counts: SRS test cases 0, "UI not written", 88 diagram blocks, "80 or so entities". INV prefix overloaded. | Consistency report | |
| 13 | P3 | ERD omits 19 tables and has wrong cardinality for open_item, two warehouse links, and missing approval parents. DFD-1 does not balance. No UML component diagram. | Diagram report D11 to D13 | |
| 14 | P3 | 21 SECs have no FR. 71 FRs have no use case. 16 FRs have no API. | Requirements report | |

## 2. Risk list

| Risk | Likelihood | Impact | Note |
|---|---|---|---|
| Build starts on a tenancy model that changes after OQ-019 | High | High | Schema and RLS design affected. |
| Role grant escalation reaches production | Medium | High | Gap 1. |
| RTM defect hides an untested requirement | Medium | Medium | Gap 5. The direct FR coverage is real (130 of 130), so the risk is depth. |
| Team treats "553 test cases" as proof of quality | High | Medium | None executed, 163 template descriptions. |
| Stakeholders approve a draft nobody reviewed | High | High | Status is Proposed throughout. |

## 3. Unresolved business decisions (separate list)

All 32 questions in `02-Business-Analysis/Open-Questions.md` are Open. Those that block the most work:

| ID | Question | Owner | Blocks |
|---|---|---|---|
| OQ-005 | Single customer or SaaS | Sponsor | Tenancy, pricing, release process |
| OQ-010 | Data location and privacy laws | Compliance officer | Hosting, retention, consent |
| OQ-015 | Users, companies, branches, volumes | IT manager | Sizing, performance targets |
| OQ-016 | Hosting preference and certifications | IT manager | Infrastructure |
| OQ-019 | One tenant with many companies, or shared | Architect, Sponsor | Isolation model |
| OQ-022 | Legal retention periods | Compliance officer | Archival, audit retention |
| OQ-023 | MFA and SSO choices | see file | SEC-006, SEC-007, US-004, US-005 |
| OQ-024 | Technology constraints | IT manager | Stack |
| OQ-028 | Service levels | Sponsor, IT manager | NFR, DR |
| OQ-032 | Who approves period reopen | see file | RBAC section 6 |

Decisions the audit adds (not yet in the question list): authority ranking for role assignment; whether master data is tenant-wide or company-owned; whether SoD conflicts are evaluated across companies; who may override SoD; who approves vendor bank change.
