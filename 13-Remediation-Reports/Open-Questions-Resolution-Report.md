# Open Questions Resolution Report

Phase 2 remediation, 2026-11-11. Related: [Remediation Progress](Remediation-Progress.md), [Final Revalidation Report](Final-Revalidation-Report.md), [Audit baseline](../12-Final-Audit/Final-Readiness-Assessment.md).

No stakeholder approval is claimed anywhere in this report. Values marked PENDING APPROVAL are recommendations.

## 1. Summary

| Classification | Count |
|---|---|
| Requires business stakeholder decision | 23 |
| Proposed solution awaiting approval | 13 |
| Requires technical stakeholder decision | 2 |
| Resolved through verified existing documentation | 0 |
| No longer applicable | 0 |

38 questions (32 original, OQ-033 to OQ-038 added in Phase 2). No question is resolved, because resolving needs a named stakeholder. A "Proposed solution" is a recommendation recorded in this package. Nothing was marked as a confirmed decision.

| Effect on MVP | Count |
|---|---|
| Blocks MVP build start | 7 |
| Does not block MVP | 10 |
| Blocks MVP configuration or go-live, not build | 21 |

## 2. Questions that block the start of MVP build

| ID | Question | Classification | Why |
|---|---|---|---|
| OQ-001 | No one can approve any document in this package until a sponsor is named. | Requires business stakeholder decision | see note |
| OQ-003 | Release 1 scope is not confirmed. MVP-Strategy proposes 58 modules. | Requires business stakeholder decision | see note |
| OQ-010 | Residency drives hosting (ADR-020), retention and privacy. Compliance officer decision. | Requires business stakeholder decision | see note |
| OQ-016 | ADR-020 recommends provider-neutral design with public cloud preferred. Infrastructure cannot be built until decided. | Proposed solution awaiting approval | see note |
| OQ-019 | ADR-017 recommends tenant per customer with shared schema and row-level security. The schema needs tenant_id either way. | Proposed solution awaiting approval | see note |
| OQ-024 | ADR-021 gives criteria and a database recommendation, not a language. The team must choose. | Requires technical stakeholder decision | see note |
| OQ-037 | Master data ownership changes the schema (company-owned or tenant-shared). Needs a decision with ADR-017. | Requires business stakeholder decision | see note |

## 3. Full classification

| ID | Classification | Effect on MVP | Evidence or note |
|---|---|---|---|
| OQ-001 | Requires business stakeholder decision | Blocks MVP build start | No one can approve any document in this package until a sponsor is named. |
| OQ-002 | Requires business stakeholder decision | Does not block MVP | Affects R4 industry modules only. |
| OQ-003 | Requires business stakeholder decision | Blocks MVP build start | Release 1 scope is not confirmed. MVP-Strategy proposes 58 modules. |
| OQ-004 | Requires business stakeholder decision | Blocks MVP configuration or go-live, not build | P0 to P2 assignments are proposals in Module-Prioritization. |
| OQ-005 | Proposed solution awaiting approval | Does not block MVP | ADR-018 recommends a tenant-capable product deployed first for one customer. PENDING APPROVAL. |
| OQ-006 | Requires business stakeholder decision | Does not block MVP | Payroll is R2. |
| OQ-007 | Requires business stakeholder decision | Blocks MVP configuration or go-live, not build | Tax codes and rules are configuration, but the MVP tax FRs cannot be accepted without the regime. |
| OQ-008 | Requires business stakeholder decision | Blocks MVP configuration or go-live, not build | Currency list and rate source are configuration. |
| OQ-009 | Requires business stakeholder decision | Blocks MVP configuration or go-live, not build | FR-111 valuation method is configurable, ADR-005 and the design allow any. A first method must still be chosen before UAT. |
| OQ-010 | Requires business stakeholder decision | Blocks MVP build start | Residency drives hosting (ADR-020), retention and privacy. Compliance officer decision. |
| OQ-011 | Requires business stakeholder decision | Does not block MVP | English at launch is the working assumption (NFR-034). Further languages are configuration. |
| OQ-012 | Requires business stakeholder decision | Blocks MVP configuration or go-live, not build | Migration scope is needed before the migration plan, not before build. |
| OQ-013 | Requires business stakeholder decision | Blocks MVP configuration or go-live, not build | Approval limit values are company data. The structure is defined in RBAC Matrix 8A.5. |
| OQ-014 | Requires business stakeholder decision | Blocks MVP configuration or go-live, not build | Planning input. No effect on documentation consistency. |
| OQ-015 | Proposed solution awaiting approval | Blocks MVP configuration or go-live, not build | ADR-019 proposes the NFR-005 envelope as the design target. IT manager must supply real volumes. |
| OQ-016 | Proposed solution awaiting approval | Blocks MVP build start | ADR-020 recommends provider-neutral design with public cloud preferred. Infrastructure cannot be built until decided. |
| OQ-017 | Requires technical stakeholder decision | Blocks MVP configuration or go-live, not build | Integration partners are adapters. No MVP function needs a named partner except email (ASM-05). |
| OQ-018 | Requires business stakeholder decision | Blocks MVP configuration or go-live, not build | FR-054 template content depends on it. |
| OQ-019 | Proposed solution awaiting approval | Blocks MVP build start | ADR-017 recommends tenant per customer with shared schema and row-level security. The schema needs tenant_id either way. PENDING APPROVAL. |
| OQ-020 | Proposed solution awaiting approval | Blocks MVP configuration or go-live, not build | BRL-040 default is refuse negative stock. Awaiting approval. |
| OQ-021 | Requires business stakeholder decision | Blocks MVP configuration or go-live, not build | Fiscal calendar is configuration of M-043 and M-044. |
| OQ-022 | Requires business stakeholder decision | Blocks MVP configuration or go-live, not build | Retention values are placeholders (NFR-027, NFR-040, NFR-042). Legal advice needed. |
| OQ-023 | Proposed solution awaiting approval | Blocks MVP configuration or go-live, not build | Authentication-Authorization proposes MFA mandatory for privileged roles and OpenID Connect for SSO. Security officer must confirm. |
| OQ-024 | Requires technical stakeholder decision | Blocks MVP build start | ADR-021 gives criteria and a database recommendation, not a language. The team must choose. |
| OQ-025 | Proposed solution awaiting approval | Does not block MVP | Responsive web is assumed (NFR-033). Native apps and offline use are not in the MVP. |
| OQ-026 | Proposed solution awaiting approval | Does not block MVP | BRL-073 and FR-143 propose no chained delegation and a 30 day maximum. Awaiting approval. |
| OQ-027 | Requires business stakeholder decision | Does not block MVP | Print template content. |
| OQ-028 | Proposed solution awaiting approval | Blocks MVP configuration or go-live, not build | NFR-007, NFR-009 and NFR-010 propose 99.5 percent, 15 minutes and 4 hours. Sponsor must confirm. |
| OQ-029 | Requires business stakeholder decision | Does not block MVP | Blocks UAT scheduling, not build. |
| OQ-030 | Requires business stakeholder decision | Does not block MVP | Affects migration timing. |
| OQ-031 | Requires business stakeholder decision | Does not block MVP | ADR-012 and the numbering tests accept either policy. |
| OQ-032 | Proposed solution awaiting approval | Blocks MVP configuration or go-live, not build | FR-142 proposes override requested by an Organization Admin and approved by a Super Admin. The reopen approver moved to OQ-034. Awaiting approval. |
| OQ-033 | Proposed solution awaiting approval | Blocks MVP configuration or go-live, not build | RBAC Matrix 8A.2 proposes a Super Admin as grant approver. |
| OQ-034 | Requires business stakeholder decision | Blocks MVP configuration or go-live, not build | Payment limits, a role above Finance Manager and the reopen approver are business choices. |
| OQ-035 | Proposed solution awaiting approval | Blocks MVP configuration or go-live, not build | FR-139 proposes a 24 hour hold. Awaiting approval. |
| OQ-036 | Proposed solution awaiting approval | Blocks MVP configuration or go-live, not build | RBAC Matrix 8A.8 proposes 2 hours. Awaiting approval. |
| OQ-037 | Requires business stakeholder decision | Blocks MVP build start | Master data ownership changes the schema (company-owned or tenant-shared). Needs a decision with ADR-017. |
| OQ-038 | Requires business stakeholder decision | Blocks MVP configuration or go-live, not build | Whether all adjustments need approval. AWD-03 proposes yes. |

## 4. Pending stakeholder approvals (decisions the package cannot make)

Sponsor: OQ-001, OQ-003, OQ-005, OQ-014, OQ-028, OQ-034.
Finance: OQ-007, OQ-008, OQ-009, OQ-013, OQ-018, OQ-021, OQ-031, OQ-034, OQ-035, OQ-038.
Compliance: OQ-010, OQ-022.
IT and architecture: OQ-015, OQ-016, OQ-017, OQ-019, OQ-024, OQ-037.
Security: OQ-023, OQ-033, OQ-035, OQ-036.
