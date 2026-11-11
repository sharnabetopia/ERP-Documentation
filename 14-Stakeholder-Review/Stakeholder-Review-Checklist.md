# Stakeholder Review Checklist

Phase 3, 2026-11-11. No approval, name, date or decision is recorded. All boxes are unchecked because no review has taken place.
Related: [Decision Pack](Stakeholder-Decision-Pack.md), [Decision Papers](Open-Question-Decision-Papers.md), [Approval Register](Approval-Register.md), [Phase 3 Readiness](Phase-3-Readiness-Summary.md).

Use one copy per reviewer. A box is ticked only when the reviewer has read the cited document and records the result in the Approval Register with evidence. Reviewer names are TBD.

## A. Sponsor (OQ-001, OQ-003, OQ-014)

- [ ] Name the executive sponsor and the steering group, and record their approval limits (Decision Paper OQ-001).
- [ ] Accept or change the 58-module scope and the 8-module cut line (`MVP-Scope-Decision-Matrix.md`).
- [ ] Supply budget, team size and target dates (OQ-014).
- [ ] Confirm the order of decisions in `Decision-Dependency-Matrix.md`.
- [ ] Decide whether Phase 2 clarifications that changed v0.1 text are accepted (FR-109, error code renames).

## B. Security officer (ADR-022, RBAC 8A)

- [ ] Read `RBAC-Matrix.md` section 8A and `Authentication-Authorization.md` section 10.
- [ ] Confirm or change the role tiers and the grant rules (8A.1, 8A.2).
- [ ] Confirm the three-user rule for payments and for vendor bank changes.
- [ ] Confirm step-up window, token lifetime, break-glass duration and custody (OQ-023, OQ-036).
- [ ] Decide on impersonation and user API tokens (gap RF-019).
- [ ] Review the 95 authorisation test cases for sufficiency (`Authorisation-Hardening-Test-Cases.md`).
- [ ] Name who approves role grants when only one Super Admin team exists (OQ-033).

## C. Finance manager

- [ ] Confirm payment limits A and B and whether a role above Finance Manager exists (OQ-013, OQ-034).
- [ ] Confirm the hold period for the first payment to a new bank account (OQ-035).
- [ ] Confirm the reopen approver for closed periods (OQ-034).
- [ ] Confirm AWD-01 payment states, including that a Failed payment is final.
- [ ] Confirm the invoice variance rule (AWD-02) and the refund rule (AWD-04).
- [ ] Confirm inventory adjustment approval (AWD-03, OQ-038) with the inventory manager.
- [ ] Confirm tax, currency, valuation and fiscal calendar answers (OQ-007, OQ-008, OQ-009, OQ-021).

## D. Architect and IT manager (ADR-017 to ADR-021)

- [ ] Review the five options tables and record a decision for each of OQ-019, OQ-005, OQ-015, OQ-016, OQ-024.
- [ ] Decide master data ownership (OQ-037).
- [ ] Confirm the capacity starting point and the sizing envelope (NFR-005).
- [ ] Confirm the external integrations needed at go-live (OQ-017).
- [ ] Review ADR-001 and ADR-003 to ADR-016 technical content (gap RF-026).

## E. Compliance officer and legal adviser

- [ ] Provide a written determination of the residency and privacy laws (OQ-010).
- [ ] Confirm retention periods for financial, audit and personal records (OQ-022).
- [ ] Review privacy export and anonymisation targets (NFR-039, FR-129).
- [ ] Confirm that the 7 year placeholder is replaced or accepted.

## F. Department owners (HR, procurement, inventory, sales)

- [ ] Review the lifecycle document that applies to the department (WF-04, WF-08, WF-09, WF-13, WF-05) and its proposed rules.
- [ ] Confirm role names used in stories (gap RF-024).
- [ ] Confirm report and notification needs (gap RF-052).
- [ ] Confirm UAT participants and availability (OQ-029).

## G. QA lead and Auditor

- [ ] Confirm that no test has been executed and that all 1,038 cases are Not Run.
- [ ] Review the regression suite membership (`Regression-Suite.md`).
- [ ] Confirm the closure evidence in `Deferred-Findings-Closure-Plan.md` is objective.
- [ ] Confirm Auditor review duties for break-glass use and SoD overrides.

## H. Evidence rules for any approval

- [ ] The decision is written down with the decider's role, the date and the document version.
- [ ] The Approval Register row changes from PENDING to APPROVED, REJECTED or DEFERRED, and cites the evidence.
- [ ] The affected requirement, permission, diagram, test and RTM entries are updated by exact ID.
- [ ] A re-validation run is repeated after the change.

