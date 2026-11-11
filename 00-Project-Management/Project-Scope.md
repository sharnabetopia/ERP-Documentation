# Project Scope

Document status: Draft v0.1 (Stage 1). Proposed.

## 1. In scope

| Item | Detail |
|---|---|
| Module programme | M-001 to M-120 |
| MVP | 58 modules ([MVP Strategy](MVP-Strategy.md)) |
| Multi-company, multi-branch | Yes, from MVP |
| Documentation | The 13 folders of the package |
| Default roles | 18 roles from the scope request |

## 2. Out of scope

| Item | Reason |
|---|---|
| Application source code in this phase | Requested documentation only |
| UI mock-ups and UI code | Not part of this phase |
| Industry packs not named | OQ-002 |
| Hardware and network procurement | Not software |
| Statutory filings per country | OQ-006, OQ-007 |

## 3. Functional boundaries

The ERP owns master and transaction data for its modules.
It does not replace banks, payment gateways, tax portals or identity providers.
See [Business Context](../02-Business-Analysis/Business-Context.md).

## 4. System boundaries

| Boundary | Inside | Outside |
|---|---|---|
| Data | ERP database, documents, audit log | Third-party systems |
| Access | Web client, API | Direct database access by users |
| Operations | Application, jobs, notifications | Provider infrastructure (TBD, OQ-016) |

## 5. Technical constraints and dependencies

See [BRD](../02-Business-Analysis/BRD.md) sections 9 and 10.

## 6. Scope change

Scope changes are recorded as change requests. Until [Change-Management](../09-Development-and-Delivery/Change-Management.md) exists, record them in [Open Questions](../02-Business-Analysis/Open-Questions.md) or the [Risk Register](Risk-Register.md).
