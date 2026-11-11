# Diagram Validation Report

Audit date 2026-11-11. Related: [Documentation Audit](Documentation-Audit-Report.md), [Final Readiness](Final-Readiness-Assessment.md), [Critical Gaps](Critical-Gaps-and-Risks.md).

## 1. Syntax validation (executed)

| Item | Value |
|---|---|
| Tool | `@mermaid-js/mermaid-cli` 10.9.1 (`mmdc`), headless Chromium with `--no-sandbox`, installed in a scratch directory |
| Method | Every fenced `mermaid` block extracted with file and line, rendered one by one to SVG |
| Blocks found | 100 in 28 files |
| Empty blocks | 0 |
| Rendered without error | 100 |
| Failed | 0 |
| Control | A deliberately broken block was rejected with a parse error, so the checker does detect failures |
| Types | flowchart 65, stateDiagram-v2 18, sequenceDiagram 12, erDiagram 5 |

Result: syntax PASS for all 100. Layout and visual quality were NOT VERIFIED (no human viewed the SVGs).
`04-System-Architecture/ERD.md` and seven files in folder 05 state "all 88 diagram blocks parse". The measured count is 100, so the header is stale.

## 2. Diagram inventory against the brief

| Required diagram | Where | Status |
|---|---|---|
| System architecture | `04-System-Architecture/System-Architecture.md` sections 1, 2, 4 | PASS |
| C4 context | `05-Workflows-and-Diagrams/C4-Architecture-Diagrams.md` Level 1 | PASS (no customer, vendor or legacy actors) |
| C4 container | same file, Level 2 | PASS |
| C4 component | same file, Level 3 Finance and Procurement only | PARTIAL: Inventory, Sales, Identity, Platform missing |
| UML component diagram | none distinct; System-Architecture section 3 is a flowchart without interfaces and omits Identity and Reporting | FAIL |
| ERD | `ERD.md` sections 1 to 5: 68 entities, 84 relationships | PARTIAL: 87 tables in `Database-Design.md`, 19 not drawn |
| Sequence | `Sequence-Diagrams.md` SEQ-01 to SEQ-11 | PARTIAL |
| Swimlane | `Swimlane-Diagrams.md`: WF-01, 02, 03, 06, 07, 12 | PARTIAL |
| Process flow | `Process-Flow.md`: WF-01 to WF-14 | PASS for presence |
| Activity | `Activity-Diagrams.md` ACT-01 to ACT-08 | PASS |
| State transition | `State-Transition-Diagrams.md`: 15 sections | PARTIAL: 10 entities have none |
| Data flow | `Data-Flow-Diagrams.md`: DFD-0, 1, 2a to 2c, 3, 4 | PARTIAL: DFD-1 does not balance with DFD-0 |
| RBAC authorization flow | SEQ-02 only. `Authentication-Authorization.md` has a table | PARTIAL |
| Approval workflow | WF-07 process flow, swimlane, SEQ-03, ACT-01, approval-instance state | PASS (strongest area) |
| Cross-module integration | System-Architecture sections 3 and 4, DFD-1, `Integration-Architecture.md`, SEQ-07 | PASS (Integration-Architecture diagrams not read in depth, NOT VERIFIED) |

Entities without a state diagram: employee, asset, contract, work order, expense claim, quotation, sales return, goods receipt, budget, ticket.
Tables not drawn in the ERD: account_balance, attachment, audit_log, credential_token, currency, custom_field_def, custom_field_value, idempotency_record, import_batch, import_error, notification, notification_template, number_series, outbox_event, payment_term, retention_policy, setting, sod_conflict, unit_of_measure.

## 3. Semantic defects found (read from the Mermaid source)

Findings were produced by a reviewing agent and I re-checked four of them against the files (SEQ-11 line 401, the Rejected to Draft transitions at State-Transition lines 18 and 144, Process-Flow K2, Swimlane F1).

| # | File and section | Defect | Severity |
|---|---|---|---|
| D1 | `Process-Flow.md` WF-01 (node K2) vs `Swimlane-Diagrams.md` WF-01 (F1) | Process flow sends a variance hold back to invoice entry. Swimlane sends it to variance approval then match. The two contradict. | High |
| D2 | same files | The no-variance path posts the invoice with no approval, but the workflow table, ACT-02 and the invoice state diagram require approval. | High |
| D3 | `Process-Flow.md` WF-06 vs workflow table | "Above threshold? No" posts with no approval. The table requires Inventory Manager approval below the threshold and Finance Manager above. | High |
| D4 | `Process-Flow.md` and `Swimlane-Diagrams.md` WF-12 | Swimlane always raises a refund. Process flow, table and SEQ-11 make it conditional on prior payment. Bank failure: swimlane retries, table says Failed and credit stays held. | High |
| D5 | `Sequence-Diagrams.md` SEQ-11, line 401 | `D-->>B: Worker sends instruction` has the wrong source and direction. The bank also writes straight to the database. Refund goes via Finance module and approval engine while WF-12 and SEQ-05 use the payment engine. | Medium |
| D6 | `State-Transition-Diagrams.md` vs `Data-Dictionary.md` section 2 | Dictionary lists Rejected as terminal for purchase requisition and journal, Failed for payment, Paid for purchase invoice. Diagrams give each an outgoing transition and the page header says terminal states have none unless marked reversible. None is marked. | Medium |
| D7 | SEQ-06 vs state diagrams | SEQ-06 sets a pending status on credit breach. No such state exists in the sales order diagram or the dictionary. | Medium |
| D8 | State diagrams | No Posted to Reversed or Credited transition for either invoice, despite BRL-017 (reversal). Payment is posted at Release, before bank acceptance, with no reversing entry on bank failure. | High |
| D9 | `Sequence-Diagrams.md` SEQ-02 vs `Authentication-Authorization.md` section 4 | The 9-step decision procedure is collapsed. Company check, STATE-001, SOD-001, SOD-002 and step-up are missing, and audit appears on one branch only. | High |
| D10 | SEQ-03, 04, 05, 06, 09, 10, 11 | No permission check shown. SEQ-03 has no audit write. SEQ-05 has no step-up or MFA for payment release, which the security spec requires. | Medium |
| D11 | `ERD.md` | `PURCHASE_INVOICE ||--o| OPEN_ITEM` and the sales equivalent conflict with `Database-Design.md`, where open_item is unique on (source_type, source_id, installment_no), so the relationship should be one-to-many. | Medium |
| D12 | `ERD.md` | `WAREHOUSE ||--o{ STOCK_TRANSFER` is one edge for two foreign keys (from and to). Approval instance and decision have missing parents and links. No sales invoice to sales order edge. | Medium |
| D13 | `Data-Flow-Diagrams.md` | DFD-1 omits customers, vendors, banks, identity provider and tax authority. Process 6 has no output. DFD-2a omits PO and payment approval. | Medium |
| D14 | `Process-Flow.md` WF-02 | Partial delivery loops to full delivery before invoicing. Table and state diagram say invoice delivered quantities. Discount approval and credit rejection paths are missing. | Medium |
| D15 | `Process-Flow.md` WF-03, 07, 09, 04, 13 | Missing: journal reversal, delegation, approver deactivated, requester withdrawal, repair path (WF-09), offer rejected (WF-04), legal and Finance approval (WF-13). | Medium |
| D16 | SEQ-01 | No MFA failure, no deactivated-user branch, no audit on the locked branch. | Low |

## 4. Lifecycle matrix (Full, Outline, Missing)

| WF | Swimlane | Sequence | State | Business rules | Exception and recovery |
|---|---|---|---|---|---|
| 01 P2P | Full | Outline | Full (no GRN) | Full | Full |
| 02 O2C | Full | Outline | Outline | Full | Full |
| 03 R2R | Full | Outline | Full | Full | Full |
| 04 H2R | Missing | Missing | Missing | Outline | Outline |
| 05 Plan-to-Produce | Missing | Missing | Missing | Missing | Outline, recovery missing |
| 06 Inventory-to-Fulfillment | Full | Outline | Full | Full | Full |
| 07 Request-to-Approval | Full | Full | Full | Full | Full |
| 08 Expense-to-Reimbursement | Missing | Missing | Missing | Outline | recovery missing |
| 09 Asset | Missing | Missing | Missing | Missing | recovery missing |
| 13 Contract-to-Renewal | Missing | Missing | Missing | Missing | recovery missing |
