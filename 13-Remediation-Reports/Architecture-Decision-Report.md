# Architecture Decision Report

Phase 2 remediation, 2026-11-11. Related: [Remediation Progress](Remediation-Progress.md), [Final Revalidation Report](Final-Revalidation-Report.md), [Audit baseline](../12-Final-Audit/Final-Readiness-Assessment.md).

No stakeholder approval is claimed anywhere in this report. Values marked PENDING APPROVAL are recommendations.

## 1. Decisions prepared

Each decision has alternatives, a comparison (advantages, disadvantages, security, scalability, operations, data isolation), a recommendation, reasoning, assumptions and the impact of not approving. All are **PENDING APPROVAL**. The documentation does not decide for the business.

| Question | ADR | Recommendation | Why it needs a human |
|---|---|---|---|
| OQ-019 Tenant model | ADR-017 | Tenant per customer, many companies inside, shared schema with row-level security, tenant_id on every row. Database per tenant for regulated customers | Depends on whether more than one customer exists and on legal needs |
| OQ-005 Single customer or product | ADR-018 | Tenant-capable product, first deployed for one customer | A commercial decision for the sponsor |
| OQ-015 Sizing | ADR-019 | Design to the NFR-005 envelope, deploy at real size, load test | Needs real volumes from the IT manager |
| OQ-016 Hosting | ADR-020 | Provider-neutral design, public cloud preferred in a region meeting OQ-010 | Cost, residency and certification choices |
| OQ-024 Technology | ADR-021 | PostgreSQL or SQL Server, chosen on DBC-02 and SEC-016. No language recommended | Needs team skills and any mandated vendor |
| Authorisation | ADR-022 | Tiers, four-eyes, controlled changes | Security officer and sponsor |

## 2. Documents updated

ARD (section 6A decision register), Architecture-Decision-Records (ADR-017 to ADR-022, table, pending list), System-Architecture (Phase 2 notes), ERD (section 7, new diagram), Database-Design (section 2.9, DBC-13 to DBC-20, counts), Integration-Architecture (section 9), Infrastructure-Architecture (section 7), Data-Migration-Strategy (section 9), Deployment-Architecture (capacity note).

## 3. Review of the four previously uninspected documents

| Document | Result |
|---|---|
| Architecture-Decision-Records | Reviewed the ADR index and the tenancy ADR in full. ADR-001, ADR-003 to ADR-016 were reviewed for status and dependencies only, not for technical content. ADR-006, ADR-007 and ADR-013 are amended by ADR-022 |
| Integration-Architecture | Read in full. Five findings, all corrected in section 9 (payment release trigger, bank callback path, integration users, instruction snapshot, bank failure) |
| Infrastructure-Architecture | Read in full. Three findings corrected in section 7 (emergency access, open hosting, revocation latency). Diagram not changed |
| Data-Migration-Strategy | Read in full. Four findings corrected in section 9 (vendor bank load, bulk grants, opening credit limits, import commit approval) |

## 4. What was not done

- No decision was recorded as approved.
- The cost, performance and operations figures are qualitative.
- ADR-001, ADR-003 to ADR-016 technical content was not re-reviewed.
