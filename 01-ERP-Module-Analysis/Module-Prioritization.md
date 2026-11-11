# Module Prioritization

Document status: Draft v0.1 (Stage 1). Proposed, pending OQ-003 and OQ-004.
The per-module table with justification is in [ERP-Module-Inventory](ERP-Module-Inventory.md).
This document explains the method.

## 1. Criteria

| Criterion | Question | Weight (proposed) |
|---|---|---|
| Control and compliance | Is the business unable to operate lawfully or auditably without it? | High |
| Process backbone | Is it on a mandatory flow (P2P, O2C, R2R)? | High |
| Dependency | Do other MVP modules need it? | High |
| Manual workaround | Can the business cope manually for a period? | Medium |
| Industry specificity | Does it apply to every customer? | Medium |
| Effort and risk | Is it large or dependent on unknown rules? | Medium |

## 2. Decision rules

| Rule | Result |
|---|---|
| Needed for control and on a mandatory flow | P0, MVP |
| Needed by a P0 module but has a manual workaround | P1, MVP if capacity allows |
| Valuable but industry specific or dependent on unknown law | P1 or P2, R2 or later |
| No dependants and not mandatory | P2 |

## 3. Notable decisions and trade-offs

| Decision | Rationale | Risk | Linked question |
|---|---|---|---|
| Payroll is P1, R2, not MVP | Statutory rules unknown. Payroll done wrong creates legal harm. | Customers expecting payroll at launch | OQ-006 |
| Multi-currency is P0 | Multi-company groups often trade across currencies. Retrofitting currency into a ledger is costly. | Extra MVP effort | OQ-008 |
| Bank reconciliation is P1, R2 | Books can run for the first period without it. Data keys are reserved in the MVP. | Cash errors discovered late | None |
| RFQ and quote comparison are P1, R2 | MVP can raise POs directly with approval. | Weaker sourcing control | None |
| Stock reservation is P1 but in MVP | Cheap once stock ledger exists. Prevents overselling in O2C. | Edge-case effort | None |
| Manufacturing is P2, R4 | Industry is unconfirmed. Large effort. | A manufacturing customer arrives early | OQ-002 |
| Batch and expiry are P1, R2 | Needed by some industries only. Stock ledger line keeps an optional tracking key. | Late retrofit of food or pharma customers | OQ-002 |
| Employee core is P0 | Approval chains need reporting lines. | None | None |

## 4. Priority review triggers

Re-run prioritisation when any of these happen:

1. OQ-002 is answered (industry confirmed).
2. A customer commits to a date for a P1 or P2 module.
3. A regulation change makes a module mandatory.
4. MVP capacity is measured and is lower or higher than assumed.
