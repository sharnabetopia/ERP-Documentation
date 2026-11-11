# Business Gap Analysis

Document status: Draft v0.1 (Stage 1).
The As-Is side is hypothetical (see [As-Is Analysis](As-Is-Analysis.md)).
The gap list is therefore indicative and must be re-scored after the workshops.

## 1. Capability gaps (hypothesised)

| ID | Area | Typical As-Is | Target (To-Be) | Gap | Closed by | Release |
|---|---|---|---|---|---|---|
| GAP-01 | Access control | Shared logins or broad access | Role and scope based access | Large | BR-003, BR-004 | MVP |
| GAP-02 | Approvals | Email and chat | Engine with limits and history | Large | BR-006 | MVP |
| GAP-03 | Audit | Partial or none | Complete trail | Large | BR-005 | MVP |
| GAP-04 | Ledger integrity | Spreadsheet or standalone accounting | Integrated double entry with period control | Medium | BR-014 to BR-019 | MVP |
| GAP-05 | Procurement control | Manual matching | Three-way match, duplicate check | Large | BR-027 to BR-029 | MVP |
| GAP-06 | Stock accuracy | Spreadsheet counts | Ledger-based stock, reservations | Large | BR-034 to BR-039 | MVP |
| GAP-07 | Sales cycle | Re-keyed documents | Linked quote to cash | Medium | BR-042 to BR-045 | MVP |
| GAP-08 | Multi-company | Separate tools per company | One platform | Large | BR-001, BR-002 | MVP |
| GAP-09 | Bank control | Manual statement checks | Reconciliation | Medium | BR-022 | R2 |
| GAP-10 | Budget control | Annual sheet | Live budget check | Medium | BR-023 | R2 |
| GAP-11 | Payroll | Spreadsheet | System payroll | Large, unknown rules | BR-050 | R2 |
| GAP-12 | Self-service | Requests to HR | Employee portal | Medium | BR-052 | R2 |
| GAP-13 | Projects | Separate tools | Integrated costing and billing | Medium | BR-053 | R3 |
| GAP-14 | Assets | Register in sheet | Lifecycle with depreciation | Medium | BR-054 | R3 |
| GAP-15 | External portals | Email | Portals | Medium | BR-056 | R3 |
| GAP-16 | Analytics | Manual exports | BI layer | Medium | BR-063 | R3 |
| GAP-17 | Production | Separate or none | Integrated manufacturing | TBD | BR-057 | R4 |

## 2. Requirement gaps within this package (self-check at Stage 1)

| ID | Gap | Resolution plan |
|---|---|---|
| DOC-GAP-01 | No FR, NFR, TR, API, US, UC, TC yet. | Stages 2 and 3 |
| DOC-GAP-02 | Module-level business rules exist for MVP only through the BRL list. Non-MVP modules have none. | Add with each release analysis. |
| DOC-GAP-03 | No confirmed stakeholders, so Source fields are role-based. | Resolve OQ-001. |
| DOC-GAP-04 | Targets for measures in the BRD are TBD. | After As-Is baseline. |
| DOC-GAP-05 | Module-level CRUD, roles, validations and edge cases are written for MVP-critical modules in [Module-Feature-Breakdown](../01-ERP-Module-Analysis/Module-Feature-Breakdown.md). Other modules have summary rows only. | Extend before each release begins. |

## 3. Scope creep watch

Requests to move a P1 or P2 module into the MVP go through [Change Management](../09-Development-and-Delivery/Change-Management.md) once that document exists, and are recorded in the [Risk Register](../00-Project-Management/Risk-Register.md) until then.
