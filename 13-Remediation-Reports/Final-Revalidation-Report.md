# Final Revalidation Report

Phase 2 remediation, 2026-11-11. Related: [Remediation Progress](Remediation-Progress.md), [Final Revalidation Report](Final-Revalidation-Report.md), [Audit baseline](../12-Final-Audit/Final-Readiness-Assessment.md).
Every number below was recomputed on the updated files by scripts written for this phase. No old score was reused. The old scores appear only as the baseline.
No stakeholder approval is claimed anywhere in this report. Values marked PENDING APPROVAL are recommendations.

## 1. Classification

**NOT READY** for development sign-off.

Reasons:
1. The remaining blockers are stakeholder decisions, not documentation gaps. Seven open questions block the start of MVP build: OQ-001 (sponsor), OQ-003 (MVP scope), OQ-010 (data residency), OQ-016 (hosting), OQ-019 (tenant model), OQ-024 (technology) and OQ-037 (master data ownership).
2. Nothing has been approved. The new security design (ADR-022), the architecture recommendations (ADR-017 to ADR-021), every proposed NFR value, and the five lifecycles are PENDING APPROVAL.
3. No test has been executed. 1,038 cases are written and all are Not Run.
4. Open (deferred) items remain, including diagram gaps (SEQ-02, ERD cardinality, DFD balance), role-name mapping, and the notification and report catalogues.

What improved: the privilege escalation and financial authorisation gaps now have a complete proposed design with requirements, tests and database checks. The RTM defect is fixed and independently verified. The three contradicting workflows now have one definition. All 42 NFRs have a measurable proposed target and a test. Every FR has a story, a positive and a negative-type test. Five lifecycles are written.
The package moves to **READY WITH GAPS** when the seven build-blocking questions are answered, the PENDING APPROVAL items for security and architecture are approved or changed, and the diagram gaps in the register are closed.

## 2. Score comparison

| Dimension | Original (audit) | Updated | Basis | Status |
|---|---|---|---|---|
| Physical document completeness | 89.3 percent (92 of 103) | 90.8 percent (108 of 119) | 119 planned and new files; 11 incomplete as before (3 declared-empty test files, HR tests, 4 placeholder sections, 3 labelled Partial). 16 new files all substantive | PARTIAL |
| ERP module completeness | 48.3 percent (58 of 120) | 48.3 percent (58 of 120) | R2 to R4 modules still have no feature rows or FRs. Five lifecycles add workflow detail only | PARTIAL |
| Business requirements | 74.6 percent (47 of 63) | 85.7 percent (54 of 63) | BRs with a test via FR or lifecycle case. Without: BR-022, BR-023, BR-031, BR-048, BR-053, BR-055, BR-056, BR-062, BR-063 | PARTIAL |
| Functional requirements | 100 percent documented (130), 0 verified | 100 percent documented (150 of 150), 0 verified by execution | FR with BR, story, acceptance text and direct test. Execution is outside documentation | PARTIAL |
| User stories | 100 percent of structure (80). UC cover 45.4 percent of FRs | 100 percent of structure (101 of 101). UC cover 39.3 percent of FRs | 91 FRs have no use case. No story lacks Given/When/Then | PASS (structure) |
| RBAC and security | 14.3 percent strict (53.6 weighted) | 35.7 percent strict (67.9 weighted) | 14 checklist items. PASS: approve and reject, company scope, maker-checker, SoD, authorisation. 9 PARTIAL. All new rules are Proposed | PARTIAL |
| Architecture | 18.2 percent (2 of 11) | 45.5 percent (5 of 11) | Consistent after review: API, Database Design, Integration, Infrastructure, Data Migration. Open: ARD, ERD, Data Dictionary, Deployment, System Architecture, ADR (decisions pending) | PARTIAL |
| Diagram completeness | 46.7 percent (7 of 15 types); 100 of 100 syntax | 46.7 percent (7 of 15 types); 121 of 121 render | Types unchanged. Priority contradictions fixed. UML component, C4 components, ERD gaps remain | PARTIAL |
| Test case completeness | 21.4 percent (3 of 14 categories); 39.2 percent FRs with positive and negative | 42.9 percent (6 of 14 categories); 100 percent of FRs have positive and negative-type cases | PASS: positive, negative, RBAC, API, security, regression. CRUD not tagged. 7 PARTIAL. 0 executed | PARTIAL |
| RTM traceability | 79.2 percent (103 of 130 rows clean) | 100.0 percent (150 of 150 rows clean) | Independent script found 0 mismatches. Workflow column and sections 6 and 7 are carried from v0.1, not re-derived | PASS (with carried sections) |
| Cross-document consistency | 27.3 percent (3 of 11) | 36.4 percent (4 of 11) | SRS-FRS, FRS-US, US-TC, Requirements-RTM pass. Others partial or not verified | PARTIAL |
| End-to-end workflow completeness | 10.0 percent (1 of 10) | 60.0 percent (6 of 10) | WF-07 plus five new lifecycle documents (structure verified, content proposed). WF-01, 02, 03, 06 remain partial | PARTIAL |

The updated figures use the same calculation methods as the audit. Where the denominator grew (new files, FRs, stories) the percentage reflects the larger set.

## 3. Checks re-run (original audit checks)

| Check | Method | Items | Result |
|---|---|---|---|
| Markdown files, folders, empty files, duplicates | Python scan | 119 files in 12 folders, plus the 13-Remediation-Reports folder written after this scan | 0 empty (smallest 857 bytes), 0 duplicates |
| Duplicate IDs | Definition scan of BR, FR, NFR, TR, US, UC, API, SEC, THR, ADR, OQ, BRL, WF, M, DBC, SOD, INV, TC | 2177 definitions | 0 duplicates, 0 duplicate test case IDs |
| Dangling references | Regex with range expansion | all families | 0 |
| Broken links | Relative link resolution | 486 links | 3 found before the 13 reports were written (all three point to the new reports). Re-run result below |
| RTM mappings | Independent script against FRS, stories, use cases, API, test cases | 150 rows | mismatches none; same-number NFR links ['FR-015/NFR-015', 'FR-016/NFR-016'] (both judged valid, see RTM 3A) |
| Test case fields | Column and cell check | 1038 cases | 0 missing columns, 0 empty cells (Actual Result excluded), 0 malformed rows, 0 with an Actual Result, status {'Not Run': 1038} |
| Mermaid syntax | Mermaid CLI 10.9.1, one render per block | 121 blocks | 121 rendered, 0 empty. Layout not reviewed by a person |
| RBAC permissions | Matrix and SoD parse | 249 active codes, 17 hard and 15 record-level SoD pairs | hard SoD violations in default roles: none; Super Admin business codes: none; Auditor non-read codes: none; Read-only non-read codes: none; API codes missing from matrix: none; new codes with no endpoint or mention: none |
| Requirement traceability | Direct test per requirement | FR 150, NFR 42, SEC 73, TR 18, API 202 | FR without test 0, NFR 0, SEC 0, TR 0, API 14, FR without story 0, THR with no test 0 |
| NFR measurability | Text rule: a number, named standard or explicit rule | 42 | 0 flagged unmeasurable. This does not prove the numbers are right |
| Workflow consistency | Text assertions | 14 | 14 pass, 0 fail |
| Unresolved TBD | Count of the word TBD (excluding the new reports) | 119 files | 257 (v0.1 audit: 218). It rose because the lifecycle documents add new proposals. Many are tied to open questions |
| PENDING APPROVAL markers | Count | | 278 |

Workflow assertions:

| Assertion | Result |
|---|---|
| P2P: process flow has no hold-to-entry loop (K2 --> I removed) | PASS |
| P2P: process flow, swimlane and sequence all show Finance Manager variance decision | PASS |
| P2P: invoice state Held to Approved by Finance Manager in state diagram and AWD | PASS |
| Payment: Settled and Failed states in state diagram, data dictionary and SEQ-05 | PASS |
| Payment: release by different user in process flow, swimlane, sequence and API | PASS |
| Payment: API release uses finance.payment.release | PASS |
| Adjustment: process flow has Inventory Manager decision before posting | PASS |
| Adjustment: swimlane routes above threshold to Finance Manager after Inventory Manager | PASS |
| Adjustment: state diagram has Rejected to Draft and dictionary says Rejected returns to Draft | PASS |
| Refund: process flow and swimlane make refund conditional on payment | PASS |
| Refund: SEQ-11 refund goes through payment engine | PASS |
| Refund: wrong-direction line removed from SEQ-11 | PASS |
| Cross-module workflow table mentions Settled and release by a different Finance Manager | PASS |
| FR-109 text no longer says only "above a threshold" | PASS |

Re-run after all ten reports existed: 129 Markdown files, 516 internal links, 0 broken, 0 dangling references, 0 duplicate IDs, 1,038 test cases all Not Run, 121 of 121 Mermaid blocks rendered.

## 4. Remaining gaps and critical unresolved findings

Critical (block development sign-off):
1. Seven build-blocking questions unanswered (section 1).
2. No stakeholder approval of the authorisation design, the architecture recommendations, or the NFR values.
3. No test executed.

Not critical but open: see "Open (deferred)" and "Partly closed" in the register (16 deferred, 9 partly closed, 4 awaiting a stakeholder).

## 5. Pending stakeholder approvals

See the Open Questions Resolution Report section 4 and the PENDING APPROVAL items in ADR-017 to ADR-022, `RBAC-Matrix.md` section 8A and NFR section 1A.

## 6. Evidence

Scripts were run in a scratch directory outside the project. The pre-remediation RTM is preserved in `evidence/RTM-v0.1-before-remediation.md`. The audit reports in `12-Final-Audit` and the archive `_self-audit-v0.1` were not modified. Diagram findings from the earlier audit were produced by an agent and a sample was checked; the lifecycle documents were written by agents and checked by script, not by a second human reader.
