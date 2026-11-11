# Acceptance Criteria

Document status: Draft v0.1 (Stage 2). Proposed.
Story-level Given/When/Then criteria are in [User Stories](User-Stories.md).
This document adds (1) system-wide invariants that must hold after any operation and (2) the definition of a testable criterion.
Each invariant is a measurable check that Stage 3 will turn into automated and manual tests.

## 1. Rules for a testable criterion

1. It names a precondition, an action and an observable result.
2. It uses exact values or named states, not words like "fast" or "easy". Numeric targets come from the [NFR](NFR.md).
3. It has a pass or fail outcome.
4. It names the role and scope under which the check runs.

## 2. System invariants

| ID | Invariant | Check | Related |
|---|---|---|---|
| INV-01 | Every posted journal balances. | Sum of debits equals sum of credits, per journal, in transaction and base currency. | FR-062, BRL-016 |
| INV-02 | The trial balance balances. | Total debits equal total credits for any company and period. | FR-082 |
| INV-03 | Accounts payable sub-ledger equals the payables control account. | Open items total equals ledger balance at any date. | FR-081 |
| INV-04 | Accounts receivable sub-ledger equals the receivables control account. | Same check for receivables. | FR-081 |
| INV-05 | Stock valuation equals the inventory control account. | Valuation report total equals ledger balance. | FR-111 |
| INV-06 | On-hand stock equals the sum of stock movements. | Per item and warehouse. | FR-104 |
| INV-07 | Stock is never negative when policy forbids it. | Holds under concurrent issue tests. | FR-106, FR-113 |
| INV-08 | No allocation exceeds an open amount. | For every invoice, allocated is at most invoiced. | FR-073 |
| INV-09 | Delivered at most ordered plus tolerance, invoiced at most delivered unless advance invoicing is enabled. | Per order line. | FR-120, FR-121 |
| INV-10 | No posting exists in a closed period after its close time, except by recorded reopen. | Compare posting dates and period states. | FR-058, FR-059 |
| INV-11 | Every audited event has an audit record and no audit record changes. | Count of events equals count of records. Hash or append-only check. | FR-030, FR-032 |
| INV-12 | No user sees data outside company and scope. | Isolation test matrix by role and scope. | FR-015, FR-019, FR-126 |
| INV-13 | No document is approved by its creator. | Query of approved documents joined to creators returns no rows. | FR-042 |
| INV-14 | Document numbers are unique and, where policy requires, gap-free. | Per company, type and year. | FR-025 |
| INV-15 | Opening balances of a new fiscal year equal closing balances of the prior year. | After year-end close. | FR-061 |

## 3. Definition of acceptance for the MVP

The MVP is accepted when:

1. Every P0 story passes its Given/When/Then scenarios.
2. INV-01 to INV-15 hold after the regression and load suites.
3. The release gates in [MVP Strategy](../00-Project-Management/MVP-Strategy.md) section 6 pass.
4. The sponsor signs the UAT document (Stage 3).

## 4. Not covered

Acceptance for R2, R3 and R4 is written with each release.
