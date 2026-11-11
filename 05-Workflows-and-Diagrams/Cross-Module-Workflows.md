# Cross-Module Workflows (14 mandatory end-to-end processes)

Document status: Draft v0.1 (Stage 2 part 2). Proposed.
IDs `WF-01` to `WF-14`. MVP workflows (WF-01, 02, 03, 06, 07, 12) are written in full. Post-MVP workflows (WF-04, 05, 08, 09, 10, 11, 13, 14) are **outlines** so that the MVP data model and approval engine can serve them later. They need refinement with the business before their release starts.
Phase 2 (v0.2): WF-04, WF-05, WF-08, WF-09 and WF-13 were promoted from outline to implementation-ready documents with their own process flow, swimlane, sequence and state diagrams, in the Lifecycles folder. Their rules are marked proposed and need approval. The detail level "Full" here means the document is complete, not approved.
Diagrams for each workflow: [Process Flow](Process-Flow.md). Swimlanes and sequences for the critical ones: [Swimlane Diagrams](Swimlane-Diagrams.md), [Sequence Diagrams](Sequence-Diagrams.md).
API dependencies are written as proposed paths from [API Documentation](../04-System-Architecture/API-Documentation.md). Status lists are in the [Data Dictionary](../04-System-Architecture/Data-Dictionary.md). Approval limits are TBD (OQ-013).

| ID | Workflow | Release | Detail level | Main modules |
|---|---|---|---|---|
| WF-01 | Procure-to-Pay | MVP | Full | M-049 to M-059, M-057, M-064, M-033, M-047 |
| WF-02 | Order-to-Cash | MVP | Full | M-079 to M-086, M-064, M-034, M-047 |
| WF-03 | Record-to-Report | MVP | Full | M-030 to M-046 |
| WF-04 | Hire-to-Retire | R2 and R3 (core record in MVP) | Full (v0.2, proposed, see [Lifecycles/WF-04-Hire-to-Retire](Lifecycles/WF-04-Hire-to-Retire.md)) | M-016 to M-029 |
| WF-05 | Plan-to-Produce | R4 | Full (v0.2, proposed, see [Lifecycles/WF-05-Plan-to-Produce](Lifecycles/WF-05-Plan-to-Produce.md)) | M-097 to M-109 |
| WF-06 | Inventory-to-Fulfillment | MVP | Full | M-061 to M-067, M-072, M-073, M-083 |
| WF-07 | Request-to-Approval | MVP | Full | M-013, M-012, M-010 |
| WF-08 | Expense-to-Reimbursement | R2 | Full (v0.2, proposed, see [Lifecycles/WF-08-Expense-to-Reimbursement](Lifecycles/WF-08-Expense-to-Reimbursement.md)) | M-028, M-039, M-047 |
| WF-09 | Asset Acquisition-to-Disposal | R2 and R3 | Full (v0.2, proposed, see [Lifecycles/WF-09-Asset-Acquisition-to-Disposal](Lifecycles/WF-09-Asset-Acquisition-to-Disposal.md)) | M-042, M-110 |
| WF-10 | Lead-to-Customer | R2 | Outline | M-076 to M-079 |
| WF-11 | Budget-to-Actual | R2 | Outline | M-038, M-032 |
| WF-12 | Return-to-Refund | MVP | Full | M-084, M-085, M-047, M-064 |
| WF-13 | Contract-to-Renewal | R3 | Full (v0.2, proposed, see [Lifecycles/WF-13-Contract-to-Renewal](Lifecycles/WF-13-Contract-to-Renewal.md)) | M-056 |
| WF-14 | Issue-to-Resolution | R3 | Outline | M-112, M-113 |

## WF-01 Procure-to-Pay

| Item | Content |
|---|---|
| Initiating actor | Employee, Department Manager |
| Trigger | A need for goods or services |
| Preconditions | Active items or descriptions, active vendor, open fiscal period, approval chains defined, bank account active |
| Steps | 1 Requester creates and submits a PR. 2 Approver decides (WF-07). 3 Procurement Officer converts approved lines to a PO for an active vendor. 4 PO is approved by value (WF-07) and sent to the vendor. 5 Warehouse Staff posts a GRN for received goods (full or partial). 6 Accountant enters the vendor invoice. 7 System checks duplicates and runs three-way match. 8 Approved invoice posts to payables and the ledger. 9 Accountant prepares a payment and allocates it. 10 Finance Manager approves (a second approver above the limit). A different Finance Manager releases with step-up. 11 The system posts the payment to bank clearing at release, marks it Settled when the bank accepts, and closes open items |
| Approval hierarchy | PR: Department Manager then Procurement Manager. PO: Procurement Manager, Finance Manager above limit. Invoice variance: Finance Manager. Payment: prepare (Accountant), approve (Finance Manager, a second approver above the limit), release (a different Finance Manager, step-up). See Authoritative Workflow Definitions AWD-01 |
| Responsible departments | Requesting department, Procurement, Warehouse, Finance |
| Business rules | BRL-009, 031, 032, 033, 034, 035, 036, 037, 038, 016 to 018, 026 to 029, 057, 058 |
| Input data | PR lines, vendor, item, price, tax code, warehouse, invoice number and amounts, bank account |
| Output data | PR, PO (with revisions), GRN, stock movements, purchase invoice, match result, open item, ledger entries, payment, allocations, audit and notifications |
| Status transitions | PR: Draft, Submitted, Approved, Converted. PO: Draft, Submitted, Approved, Sent, Partly received, Received, Closed. GRN: Draft, Posted. Invoice: Draft, Submitted, Held, Approved, Posted, Part-paid, Paid, Cancelled. Payment: Draft, Submitted, Approved, Released, Settled, Failed, Rejected, Cancelled, Reversed |
| Notifications | Approver notified on each submission. Requester on decision. Warehouse on PO sent (optional). Finance on held invoice. Vendor remittance (if email configured) |
| Related modules | M-013, M-012, M-010, M-054, M-061, M-063, M-064, M-040, M-041, M-032, M-036 |
| API dependencies | POST /purchase-requisitions, /submit, /convert. POST /purchase-orders, /submit, /send. POST /goods-receipts. POST /purchase-invoices, /match, /submit, /post. POST /payments, /submit, /release. POST /approvals/{id}/decisions |
| Exception scenarios | **Rejected approval:** returns to requester with reason, resubmission starts a new round. **Cancellation:** PO cancelled for remaining quantity, received quantity stays valid. **Partial delivery:** PO stays Partly received, open quantity remains. **Duplicate invoice:** same vendor and number is refused or flagged with recorded override. **Price or quantity variance:** invoice held for approval or credit note requested. **Partial payment:** invoice becomes Part-paid. **Advance payment:** held as unallocated credit then allocated. **Vendor blocked after PO:** receipt allowed, new PO refused. **Receipt reversal after invoice:** reversal refused until invoice is handled. **Period closed:** posting date moved to next open period by a user decision |
| Failure recovery | Failed posting rolls back fully and the user retries (idempotent). Bank failure moves payment to Failed, the system reverses the posting and the invoice reopens. A retry is a new payment. Notification failure is retried (FR-037). Approver unavailable: delegation or escalation |
| Final outcome | Goods in stock, liability settled, ledger balanced, all steps audited |

## WF-02 Order-to-Cash

| Item | Content |
|---|---|
| Initiating actor | Sales Executive |
| Trigger | Customer enquiry or order |
| Preconditions | Active customer, items with prices, warehouse with stock or planned supply, open period, bank account |
| Steps | 1 Executive creates a quotation and sends it. 2 Customer accepts. 3 Quotation converts to a sales order. 4 System checks credit limit plus exposure. 5 Stock is checked and reserved on confirmation. 6 Warehouse Staff posts a delivery (full or partial). 7 Accountant invoices delivered quantities. 8 Customer pays. 9 Accountant records and allocates the receipt. 10 Invoice closes |
| Approval hierarchy | Credit breach: Finance Manager. Discount beyond policy: Sales Manager (TBD). Credit note: Finance Manager |
| Responsible departments | Sales, Warehouse, Finance |
| Business rules | BRL-040, 041, 046 to 052, 016, 025, 026, 027 |
| Input data | Customer, items, prices, discounts, tax codes, delivery quantities, payment details |
| Output data | Quotation, sales order, reservations, delivery, stock movements, cost of goods entries, invoice, open item, receipt, allocations |
| Status transitions | Quotation: Draft, Sent, Accepted, Converted. Order: Draft, Confirmed, Partly delivered, Delivered, Invoiced, Closed. Delivery: Draft, Dispatched, Delivered. Invoice: Draft, Posted, Part-paid, Paid |
| Notifications | Finance on credit approval request. Customer receives quote and invoice (email). Salesperson on payment received |
| Related modules | M-079 to M-086, M-064, M-065, M-034, M-047, M-040, M-013 |
| API dependencies | POST /quotations, /send, /convert. POST /sales-orders, /confirm. POST /deliveries. POST /sales-invoices, /post. POST /payments (direction in) |
| Exception scenarios | **Rejected credit approval:** order returns to draft. **Partial delivery:** remainder stays open or is backordered. **Cancellation:** remaining quantity cancelled and reservations released. **Stock conflict:** two orders for the last unit, first reserves. **Partial payment:** invoice Part-paid. **Overpayment:** held as customer credit. **Advance receipt:** allocated on invoicing. **Quote expired:** conversion refused. **Failed delivery:** stock returns, delivery Failed |
| Failure recovery | Delivery and invoice posting are single transactions. Idempotent retries. Reservation expiry job releases stale holds |
| Final outcome | Goods delivered, revenue and receivable recorded, cash received, stock and ledger consistent |

## WF-03 Record-to-Report

| Item | Content |
|---|---|
| Initiating actor | Accountant, system postings |
| Trigger | Any financial event, then period end |
| Preconditions | Chart of accounts, posting rules, open period, currency rates |
| Steps | 1 Sub-ledger documents post to the ledger. 2 Accountant prepares manual adjustments (journals). 3 Checker approves. 4 Month-end: Finance Manager opens the close checklist. 5 Resolve unposted and pending items. 6 Run reconciliation checks (AP, AR, stock to ledger). 7 Soft close then close the period. 8 Produce trial balance, income statement, balance sheet. 9 At year-end run year-end close and create opening balances |
| Approval hierarchy | Manual journal: Finance Manager as checker. Period reopen: a higher authority (OQ-032) |
| Responsible departments | Finance |
| Business rules | BRL-016, 017, 018, 019, 020, 021, 022, 023, 024, 030, 008 |
| Input data | Documents, journals, rates, tax, period settings |
| Output data | Ledger entries, balances, close record, statements |
| Status transitions | Journal: Draft, Submitted, Approved, Posted, Reversed. Period: Open, Soft-closed, Closed. Year: Open, Closed |
| Notifications | Finance team on close start and result. Auditor on reopen |
| Related modules | M-030 to M-046, all sub-ledgers |
| API dependencies | POST /journals, /submit, /post, /reverse. POST /fiscal-periods/{id}/close, /reopen. POST /fiscal-years/{id}/close. GET /reports/trial-balance, /income-statement, /balance-sheet |
| Exception scenarios | **Unbalanced journal:** refused. **Closed period posting:** refused. **Reconciliation difference:** closing blocked with report. **Missing rate:** document blocked. **Late document after close:** reopen with approval or post to next period. **Reversal of an error:** reversing entry referencing the original |
| Failure recovery | Posting is atomic. Close is re-runnable. Summary tables can be rebuilt from ledger and are verified nightly |
| Final outcome | Closed period with reconciled, reproducible statements |

## WF-04 Hire-to-Retire (outline)

| Item | Content |
|---|---|
| Initiating actor | Department Manager (job requisition), HR Manager |
| Trigger | Staffing need |
| Preconditions | Approved position, employee core record model (MVP) |
| Steps (proposed) | Requisition approved, applicant tracking, offer, onboarding checklist, employee record and user created, attendance and leave, payroll, performance, offboarding clearance, access revoked, final settlement |
| Approval hierarchy | Requisition: Department Manager, HR Manager, Finance Manager (budget). Offer: HR Manager |
| Responsible departments | HR, hiring department, IT, Finance |
| Business rules | BRL-054, 055, 056. Payroll rules TBD (OQ-006) |
| Input and output data | Candidate, offer, employee record, attendance, leave, payslips, clearance record |
| Status transitions | Employee: Draft, Active, On leave, Separated |
| Notifications | Manager, HR, IT (access), employee |
| Related modules | M-016 to M-029, M-003 |
| API dependencies | MVP: /employees, /users/{id}/employee-link, /users/{id}/deactivate. Others not designed |
| Exception scenarios | Offer rejected, rehire, leaver with pending approvals, manager change |
| Failure recovery | Deactivation must revoke access within 60 seconds regardless of other steps |
| Final outcome | Person onboarded or separated with correct access and records |

## WF-05 Plan-to-Produce (outline, R4)

| Item | Content |
|---|---|
| Initiating actor | Production planner |
| Trigger | Demand or sales orders |
| Preconditions | Items, BOMs, work centres, stock |
| Steps (proposed) | Forecast demand, run MRP, create planned orders, release work orders, issue materials, record production and WIP, quality inspection, receive finished goods, cost the order |
| Approval hierarchy | TBD |
| Responsible departments | Production, Quality, Warehouse, Finance |
| Business rules | None written. Material and WIP posting rules TBD |
| Input and output data | BOM, routing, work order, material issues, finished goods, scrap |
| Status transitions | Work order: Planned, Released, In progress, Completed, Closed |
| Notifications | Planner, warehouse, quality |
| Related modules | M-097 to M-109, M-064, M-072 |
| API dependencies | None designed |
| Exception scenarios | Material shortage, defect, rework, machine downtime |
| Failure recovery | TBD |
| Final outcome | Finished goods in stock at cost. Reserved dimensions in MVP: stock ledger source types, work centre as ledger dimension |

## WF-06 Inventory-to-Fulfillment

| Item | Content |
|---|---|
| Initiating actor | Warehouse Staff, Inventory Manager |
| Trigger | Receipt, issue, transfer, count, or order to fulfil |
| Preconditions | Items, warehouses, posting rules, open period |
| Steps | 1 Receipts raise stock (GRN or receipt document). 2 Stock ledger records each movement. 3 Reservations hold stock for confirmed orders. 4 Transfers move stock between warehouses with in-transit state. 5 Deliveries issue stock. 6 Counts lead to adjustments. Every adjustment needs Inventory Manager approval (not the creator or submitter), and Finance Manager approval above the threshold. 7 Valuation run posts stock value to the ledger. 8 Reports show on-hand, reserved, in-transit and available |
| Approval hierarchy | Adjustment: Inventory Manager always, then Finance Manager above the threshold (AWD-03) |
| Responsible departments | Warehouse, Inventory, Finance |
| Business rules | BRL-038 to 045, 052, 065 |
| Input and output data | Movement documents, counts, reasons. Stock movements, balances, valuation entries |
| Status transitions | Transfer: Draft, Dispatched, Partly received, Received. Adjustment: Draft, Submitted, Approved, Posted (Rejected returns to Draft). Reservation: Active, Released, Consumed |
| Notifications | Inventory Manager on large adjustment. Sales on reservation shortage |
| Related modules | M-061 to M-067, M-072, M-073, M-057, M-083 |
| API dependencies | POST /stock-transfers, /dispatch, /receive. POST /stock-adjustments, /submit. POST /stock-documents. POST /valuation/runs. GET /stock/balances |
| Exception scenarios | **Negative stock request:** refused with available quantity. **Concurrent issue of the last unit:** one succeeds. **Loss in transit:** adjustment with approval. **Count difference:** adjustment. **Reservation conflict:** second order told. **Backdated receipt:** valuation recalculation per method (OQ-009) |
| Failure recovery | Row locks and short transactions. On failure no partial movement. Balances rebuildable from movements |
| Final outcome | Accurate stock by location, valued and reconciled to the ledger |

## WF-07 Request-to-Approval

| Item | Content |
|---|---|
| Initiating actor | Any requester with submit permission |
| Trigger | Submit of any approvable document (PR, PO, invoice variance, payment, journal, adjustment, order over credit, vendor, credit note) |
| Preconditions | Document valid and complete. A chain matches. Eligible approver exists |
| Steps | 1 System validates the document and locks it. 2 Engine selects the chain by type, value, branch and department. 3 First eligible approver (not the creator) is notified. 4 Approver approves, rejects or returns. 5 Next step runs if any. 6 Final approval sets the document to Approved and notifies the requester. 7 Overdue steps escalate. 8 Delegates act within delegation |
| Approval hierarchy | From the chain definition |
| Responsible departments | Owner of the document type |
| Business rules | BRL-008 to BRL-013, 057 |
| Input and output data | Document id and value. Instance, decisions with reasons, history |
| Status transitions | Instance: Pending, Approved, Rejected, Returned, Cancelled, Escalated |
| Notifications | Approver on arrival and on escalation. Requester on decision. Delegator when delegate acts |
| Related modules | M-013, M-012, M-010, M-003 |
| API dependencies | GET /approvals. POST /approvals/{id}/decisions. POST /delegations. POST /approval-chains |
| Exception scenarios | **Rejected:** requester revises. **Resubmission:** new round, old rounds kept. **Value changed after approval:** restart. **No eligible approver:** submit refused. **Approver deactivated:** rerouted. **Creator is the only approver:** skipped to next, else refused |
| Failure recovery | Decision is idempotent and append-only. Notification retry. Stuck instance visible to administrators |
| Final outcome | A decided document with a complete, tamper-evident approval history |

## WF-08 Expense-to-Reimbursement (outline, R2)

| Item | Content |
|---|---|
| Initiating actor | Employee |
| Trigger | Business expense or travel |
| Steps (proposed) | Claim with receipts, manager approval, finance verification against policy, payable created, payment through the payment engine, ledger posting |
| Approval hierarchy | Department Manager, Finance Manager above limit |
| Business rules | Policy limits TBD. Reuses BRL-008, BRL-026 to BRL-029 |
| Status transitions | Claim: Draft, Submitted, Approved, Paid, Rejected |
| Related modules | M-028, M-039, M-047, M-013 |
| API dependencies | None designed |
| Exception scenarios | Missing receipt, over limit, duplicate claim, rejected then resubmitted, partial reimbursement |
| Final outcome | Employee reimbursed and expense posted |

## WF-09 Asset Acquisition-to-Disposal (outline, R2 and R3)

| Item | Content |
|---|---|
| Initiating actor | Department Manager, Finance |
| Trigger | Capital purchase |
| Steps (proposed) | Capital PR and PO, receipt, asset registration, depreciation schedule, transfers, maintenance, disposal with gain or loss |
| Approval hierarchy | Capital expenditure chain (TBD) |
| Business rules | Depreciation methods TBD |
| Status transitions | Asset: Draft, In service, Transferred, Disposed |
| Related modules | M-042, M-110, M-049 to M-059 |
| API dependencies | None designed. MVP reserves a capital flag on item categories |
| Exception scenarios | Partial receipt of a capital item, impairment, disposal below book value |
| Final outcome | Asset tracked, depreciated and retired with ledger postings |

## WF-10 Lead-to-Customer (outline, R2)

| Item | Content |
|---|---|
| Initiating actor | Sales Executive |
| Trigger | A lead arrives |
| Steps (proposed) | Capture lead, qualify, create opportunity, quote, win, convert to customer without retyping |
| Approval hierarchy | Discount approval by Sales Manager |
| Status transitions | Lead: New, Qualified, Converted, Lost. Opportunity: Open, Won, Lost |
| Related modules | M-076 to M-080, M-079 |
| API dependencies | None designed. MVP customer creation is reused |
| Exception scenarios | Duplicate lead, lost opportunity reopened |
| Final outcome | A customer record with history |

## WF-11 Budget-to-Actual (outline, R2)

| Item | Content |
|---|---|
| Initiating actor | Finance Manager |
| Trigger | Budget cycle |
| Steps (proposed) | Budget versions per account and dimension, approval, spend control on PR, PO and invoices, actual versus budget reports, revisions |
| Approval hierarchy | Department Manager, Finance Manager, executive (TBD) |
| Business rules | Warn or block on overrun per setting (BR-023) |
| Status transitions | Budget: Draft, Submitted, Approved, Revised, Closed |
| Related modules | M-038, M-032, M-049, M-053 |
| API dependencies | None designed. MVP reserves branch, department and cost centre on every posting line |
| Exception scenarios | Overrun, transfer between budgets, late approval |
| Final outcome | Spend controlled and variances reported |

## WF-12 Return-to-Refund

| Item | Content |
|---|---|
| Initiating actor | Sales Executive (customer return). Warehouse Staff (vendor return, see exceptions) |
| Trigger | Customer returns goods or disputes an invoice |
| Preconditions | Delivered and invoiced goods, return reason, policy window |
| Steps | 1 Executive records a return against the delivery. 2 Approval if outside policy. 3 Warehouse receives and inspects. 4 System restocks, scraps or marks for vendor return. 5 Accountant issues a credit note within the invoiced amount. 6 Finance Manager approves. 7 If the customer paid, the credit is held or a refund is created. 8 Refund is prepared, approved and released by three different users through the payment engine (AWD-01) |
| Approval hierarchy | Return outside window: Sales Manager. Credit note: Finance Manager. Refund: Finance Manager |
| Responsible departments | Sales, Warehouse, Finance |
| Business rules | BRL-050, 051, 053, 027, 028 |
| Input data | Delivery, invoice, quantities, reason, disposition, bank account |
| Output data | Return, stock movements, credit note, ledger entries, refund payment |
| Status transitions | Return: Requested, Approved, Received, Closed, Rejected. Credit note: Draft, Approved, Posted. Refund payment: Draft, Submitted, Approved, Released, Settled, Failed |
| Notifications | Customer on credit note and refund. Finance on refund request |
| Related modules | M-084, M-085, M-082, M-064, M-047 |
| API dependencies | POST /sales-returns, /receive. POST /credit-notes. POST /refunds. POST /payments/{id}/release |
| Exception scenarios | **Refund:** capped at amount received. **Return above delivered:** refused. **Credit above invoice less earlier credits:** refused. **Partial return:** partial credit. **Return of goods already consumed:** disposition Scrap. **Refund fails at the bank:** payment Failed, credit stays held. **Duplicate refund request:** idempotent. **Vendor return of defective goods:** goods return to vendor (FR-096) then debit note |
| Failure recovery | Atomic postings. Refund retries use the same idempotency key |
| Final outcome | Stock, receivable and cash corrected with a linked document chain |

## WF-13 Contract-to-Renewal (outline, R3)

| Item | Content |
|---|---|
| Initiating actor | Procurement Manager |
| Trigger | Need for a vendor agreement or approaching expiry |
| Steps (proposed) | Draft, review, approve, sign, link to POs, track obligations, alert before expiry, renew, amend or terminate |
| Approval hierarchy | Procurement Manager, legal (TBD), Finance Manager above value |
| Status transitions | Contract: Draft, Under review, Active, Expiring, Renewed, Expired, Terminated |
| Related modules | M-056, M-054, M-053 |
| API dependencies | None designed |
| Exception scenarios | Missed renewal, price change, early termination |
| Final outcome | Contracts governed with timely renewal decisions |

## WF-14 Issue-to-Resolution (outline, R3)

| Item | Content |
|---|---|
| Initiating actor | Employee or customer |
| Trigger | Incident or support request |
| Steps (proposed) | Log ticket, categorise, assign, SLA timer, resolve, confirm with requester, close, review |
| Approval hierarchy | Escalation chain by SLA |
| Status transitions | Ticket: New, Assigned, In progress, Resolved, Closed, Reopened |
| Related modules | M-112, M-113, M-117 |
| API dependencies | None designed |
| Exception scenarios | SLA breach, reassignment, duplicate ticket, reopen |
| Final outcome | Issue closed within the service level |

## Coverage of the requested scenarios

| Scenario | Where |
|---|---|
| Partial payment | WF-01, WF-02 |
| Partial delivery | WF-01 (receipt), WF-02 (delivery) |
| Rejected approval | WF-01, WF-02, WF-07 |
| Cancellation | WF-01, WF-02 |
| Refund | WF-12 |
| Duplicate invoice | WF-01 |
| Resubmission | WF-07, WF-01 |
