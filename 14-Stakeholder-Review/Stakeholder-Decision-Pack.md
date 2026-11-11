# Stakeholder Decision Pack

Phase 3, 2026-11-11. No approval, name, date or decision is recorded. All boxes are unchecked because no review has taken place.
Related: [Decision Pack](Stakeholder-Decision-Pack.md), [Decision Papers](Open-Question-Decision-Papers.md), [Approval Register](Approval-Register.md), [Phase 3 Readiness](Phase-3-Readiness-Summary.md).

## 1. Purpose

This pack asks stakeholders to make the decisions that the documentation cannot make. Phase 2 removed the documentation defects that the independent audit found. What remains is a set of business, technical and security choices that need a named person. The package stays **NOT READY** until those choices are made and recorded.

## 2. What is in this review folder

| File | Use |
|---|---|
| `Open-Question-Decision-Papers.md` | One paper for each of the seven build-blocking questions, with options and a recommendation |
| `Approval-Register.md` | Every item awaiting approval, with owner, reviewer, status and version |
| `Deferred-Findings-Closure-Plan.md` | 16 deferred and 9 partly closed findings, with the smallest fix and the evidence that closes each |
| `MVP-Scope-Decision-Matrix.md` | Proposed MVP scope for OQ-003 |
| `Decision-Dependency-Matrix.md` | Order in which decisions unblock each other |
| `Stakeholder-Review-Checklist.md` | Per-role checklist |
| `Phase-3-Readiness-Summary.md` | Current status and remaining sign-off blockers |

## 3. The seven build-blocking decisions

| Question | Subject | Recommendation (not a decision) | Owner | Status |
|---|---|---|---|---|
| OQ-001 | Project sponsor | Sponsor with a steering group holding delegated limits | TBD | PENDING |
| OQ-003 | MVP scope | Accept the 58 modules with a pre-agreed cut line | Sponsor (TBD) | PENDING |
| OQ-010 | Data residency | No country recommended. Support a region per tenant. Fix the region after a written legal determination | Compliance officer (TBD) | PENDING |
| OQ-016 | Hosting model | Public cloud in a region that meets OQ-010, provider-neutral design | IT manager (TBD) | PENDING |
| OQ-019 | Tenant model | Tenant per customer, shared schema with row-level security, tenant_id on every row | Architect and sponsor (TBD) | PENDING |
| OQ-024 | Technology stack | PostgreSQL or SQL Server. No language recommended | IT manager (TBD) | PENDING |
| OQ-037 | Master data ownership | Company-owned for the MVP, with an ownership scope column | Architect and sponsor (TBD) | PENDING |

## 4. How to decide

1. Read the paper for the question. Answer the stakeholder questions in section 7 of the paper.
2. Record the decision in the "Final decision" field of the paper, with role, date and evidence reference.
3. Change the row in `Approval-Register.md` from PENDING to APPROVED, REJECTED or DEFERRED.
4. Update the Open Questions register status and the affected ADR.
5. Ask for a re-validation of the affected documents and tests.

## 5. Other questions by classification

Of 38 open questions: Requires business stakeholder decision: 23, Proposed solution awaiting approval: 13, Requires technical stakeholder decision: 2. The full list is in `13-Remediation-Reports/Open-Questions-Resolution-Report.md`. This pack covers the seven that block the start of build. The other questions block configuration or go-live, or do not block the MVP.

## 6. Rules kept in this pack

No recommendation is presented as approved. No name, date or approval has been invented. No question is marked resolved. No test has been run.
