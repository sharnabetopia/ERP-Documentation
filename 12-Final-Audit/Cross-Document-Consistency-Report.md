# Cross-Document Consistency Report

Audit date 2026-11-11. Related: [Documentation Audit](Documentation-Audit-Report.md), [Final Readiness](Final-Readiness-Assessment.md), [Critical Gaps](Critical-Gaps-and-Risks.md).
Method: scripted cross-checks of IDs and counts, plus reading of the paired documents. Pairings not fully read are marked NOT VERIFIED.

## 1. Pairings requested

| Pairing | Result | Detail |
|---|---|---|
| BRD vs PRD | PARTIAL | PRD defers all requirements to the BRD, so contradiction is unlikely. PRD success measures are TBD, BRD BR-062 targets are TBD. No conflict found. Not read line by line, NOT VERIFIED. |
| PRD vs SRS | PARTIAL | Release scope agrees (58 modules, 130 FRs, 80 stories). SRS says "18 default roles", PRD lists 8 personas mapped to default roles. Consistent. |
| SRS vs FRS | PASS | SRS section 5 FR ranges (FR-001 to 052, 053 to 085, 086 to 100, 101 to 113, 114 to 124, 125 to 130) cover all 130 FRs. |
| FRS vs User Stories | PASS | Every FR is in at least one story and every story names an FR (range references expanded). |
| User Stories vs Test Cases | PASS | All 130 FRs, and so all stories, have a case. Story ID column is filled. |
| Requirements vs RTM | FAIL | 14 spurious FR to NFR links and 41 unsupported FR to TC links. See [Requirements Coverage](Requirements-Coverage-Report.md) section 4. |
| ARD vs ERD | PARTIAL | No ERD entity is missing from `Database-Design.md`. 19 of 87 tables are not drawn. `Data-Dictionary.md` says "80 or so entities" and the catalogue has 87. |
| ARD vs API documentation | NOT VERIFIED | ARD and API doc not compared line by line. API IDs are consistent with the FRS (every API names an FR). |
| RBAC vs User Stories | FAIL | Story roles with no RBAC role (9 names). US-008 cannot be met under the Super Admin rule. MFA priority differs (US-004 P1, SEC-006 P0). See [RBAC Audit](RBAC-Audit-Report.md). |
| Workflows vs Sequence diagrams | FAIL | SEQ-02 drops steps of the 9-step authorization procedure. SEQ-05 lacks step-up. SEQ-11 has a wrong arrow and uses the wrong refund engine. See [Diagram Validation](Diagram-Validation-Report.md) D5, D9, D10. |
| Business rules vs test cases | PARTIAL | BRL IDs appear in test cases and no BRL is undefined. Rule-to-case completeness was not computed. NOT VERIFIED. |

## 2. Stale or conflicting statements found

| File and section | Statement | Measured or conflicting fact |
|---|---|---|
| `03-Product-and-System-Requirements/SRS.md` section 3 | Test cases: 0, "Stage 3" | 553 test cases exist. |
| same, section 3 | API requirements: "see document" | 169 |
| same, section 8 | "UI specification not written" | Folder 07 has 5 UI/UX documents. Stale. |
| same, section 8 | "Numbers above are re-verified by script in the final audit" | Not done in the SRS. This audit re-verified them. |
| `04-System-Architecture/ERD.md` header and 7 files in folder 05 | "all 88 diagram blocks parse" | 100 blocks measured, all parse. |
| `04-System-Architecture/Data-Dictionary.md` section on scope | "80 or so entities" | 87 in Database-Design section 2. |
| `README.md` | Final audit status "Draft (self-audit)" | Superseded by this independent audit. |
| `01-ERP-Module-Analysis/Module-Feature-Breakdown.md` header | "Role names use the 18 default roles from the scope request" | The scope request is not in the package, so the source cannot be checked. |
| ID prefix INV | INV-001 to 004 are error codes (`API-Documentation.md`), INV-01 to 15 are invariants (`Acceptance-Criteria.md`) | One prefix with two meanings. |
| Process flow vs swimlane vs table | WF-01 variance path, WF-06 approval, WF-12 refund | Contradictory. See Diagram report D1 to D4. |
| State diagrams vs Data Dictionary | Terminal states with outgoing transitions | See Diagram report D6. |
| RBAC section 6 vs FR-027 | Approval limits in chain steps vs in settings | Conflict. |
| Authentication-Authorization sections 3 and 4 vs SEC-009, 010 | Session revocation timing | Conflict. |

## 3. Business rule conflicts and gaps

- BRL-057 (no one approves and pays) is not enforced by any SoD pair for Finance Manager.
- BRL-058 pairs vendor bank change with payment approval, but the Permission-Matrix has no bank-change code.
- BR-062 (service levels) is MVP but has no FR and targets of TBD (OQ-015, OQ-016, OQ-028).

## 4. Result

Counts of requirements and test cases agree across documents. Statements of status, diagram counts and a few relationships do not. Cross-document consistency is PARTIAL.
