# Use Case Specification (MVP critical flows)

Document status: Draft v0.1 (Stage 2). Proposed.
16 use cases cover the flows where money, stock or access is at risk.
Remaining MVP flows are covered by [User Stories](User-Stories.md).
Related: [FRS](FRS.md), [Business Rules](../02-Business-Analysis/Business-Rules.md).

Each use case lists: actors, preconditions, trigger, main flow (MF), alternate flows (AF), exception flows (EF), postconditions, business rules, related FR and stories.

## UC-001 Sign in

| Field | Content |
|---|---|
| Actors | Any user. Identity provider (if SSO). |
| Preconditions | The user is active and has at least one company assignment. |
| Trigger | The user opens the application. |
| Main flow | 1. User enters email and password. 2. System checks lockout state. 3. System verifies credentials. 4. If MFA is required, system asks for a code and verifies it. 5. System starts a session and loads the user's scope. 6. System records the sign-in. |
| Alternate flows | AF1 SSO: the system redirects to the provider and maps the returned identity to a user. AF2 Password expired: the user must set a new password before step 5. AF3 Multiple companies: the user picks the working company. |
| Exception flows | EF1 Wrong credentials: generic error, failure counted. EF2 Account locked: refused with unlock guidance. EF3 MFA failure: refused and counted. EF4 Provider unavailable: SSO fails with a message, local sign-in follows policy. |
| Postconditions | A session exists with scope. An audit record exists for success or failure. |
| Business rules | BRL-004, BRL-005, BRL-006 |
| Related | FR-001, FR-003, FR-004, FR-006, FR-007. US-001 to US-005 |

## UC-002 Create a user and assign scope

| Field | Content |
|---|---|
| Actors | Organization Admin, Branch Admin, new user. |
| Preconditions | Company, branches and roles exist. Actor holds user management permission in that scope. |
| Trigger | A new person needs access. |
| Main flow | 1. Admin enters name, email, company, role, branch and department scope. 2. System checks that email is unique and that the role does not conflict with the person's other roles. 3. System creates the user as Invited and emails an activation link. 4. User sets a password and activates. 5. System records the events. |
| Alternate flows | AF1 Link to employee: admin links the user to an employee record. AF2 Temporary role: admin sets start, end and reason. |
| Exception flows | EF1 Duplicate email: refused. EF2 SoD conflict: blocked or override recorded. EF3 Admin outside scope: refused. EF4 Link expired: admin resends. |
| Postconditions | An active user exists with only the granted scope. |
| Business rules | BRL-002, BRL-003, BRL-054, BRL-057, BRL-058, BRL-059 |
| Related | FR-008, FR-010, FR-011, FR-016, FR-017. US-006, US-010, US-011 |

## UC-003 Approve a document

| Field | Content |
|---|---|
| Actors | Requester, approver, delegate, system. |
| Preconditions | The document is complete and an approval chain matches it. |
| Trigger | The requester submits the document. |
| Main flow | 1. System validates the document. 2. System locks it and selects the approver whose authority covers its value. 3. System notifies the approver. 4. Approver reviews and approves. 5. If more levels exist, steps 3 and 4 repeat. 6. System sets the document to Approved and notifies the requester. 7. System records every step. |
| Alternate flows | AF1 Reject with reason: document returns to the requester as Rejected. AF2 Return for change: the requester edits and resubmits, a new round starts. AF3 Delegation: a delegate acts within the delegation. AF4 Escalation: an overdue step goes to the next approver. |
| Exception flows | EF1 Approver is the creator: refused. EF2 No eligible approver: the submit is refused with a message to the administrator. EF3 Value changed during approval: approval restarts. EF4 Approver deactivated: item is rerouted. |
| Postconditions | The document is Approved, Rejected or Returned, with full history. |
| Business rules | BRL-008 to BRL-013 |
| Related | FR-039 to FR-047. US-021 to US-027 |

## UC-004 Create and post a manual journal

| Field | Content |
|---|---|
| Actors | Accountant (maker), Finance Manager (checker). |
| Preconditions | An open period covers the date. Accounts are postable. |
| Trigger | An adjustment must be recorded. |
| Main flow | 1. Maker enters date, narration and lines. 2. System checks balance in transaction and base currency. 3. Maker submits. 4. System routes to a checker other than the maker (UC-003). 5. Checker approves. 6. System posts to the ledger and numbers the entry. 7. System records the audit event. |
| Alternate flows | AF1 Foreign currency lines use the rate for the date. AF2 Reject: the maker corrects and resubmits. |
| Exception flows | EF1 Unbalanced: refused. EF2 Closed period: refused. EF3 Control account without sub-ledger source: refused. EF4 Period closes between approval and posting: posting refused, the checker is told. |
| Postconditions | A balanced, immutable ledger entry exists. Corrections only by reversal. |
| Business rules | BRL-008, BRL-016 to BRL-020, BRL-030, BRL-057 |
| Related | FR-062 to FR-065. US-036 to US-038 |

## UC-005 Close a period

| Field | Content |
|---|---|
| Actors | Finance Manager, Accountant. |
| Preconditions | The period is Open. |
| Trigger | Period end. |
| Main flow | 1. Manager opens the close checklist. 2. System lists unposted documents and pending approvals in the period. 3. Accountants resolve or carry forward each item. 4. Manager runs reconciliation checks (sub-ledgers to control accounts, stock valuation to ledger). 5. Manager closes the period. 6. System blocks further posting and records the event. |
| Alternate flows | AF1 Soft close: only named roles may post. AF2 Reopen: higher authority reopens with a reason. |
| Exception flows | EF1 Open items remain: closing refused. EF2 Reconciliation difference: closing refused, difference report shown. EF3 Later period already closed when reopening: reopening refused until later periods are reopened. |
| Postconditions | The period is Closed. Statements for the period are stable. |
| Business rules | BRL-018, BRL-021, BRL-022 |
| Related | FR-058 to FR-060. US-034, US-035 |

## UC-006 Requisition to purchase order

| Field | Content |
|---|---|
| Actors | Employee, Department Manager, Procurement Officer, Procurement Manager. |
| Preconditions | Items or descriptions exist. A vendor is active. |
| Trigger | An employee needs goods or services. |
| Main flow | 1. Employee creates and submits a PR. 2. PR is approved (UC-003). 3. Procurement Officer converts approved lines to a PO for an active vendor. 4. PO is approved by value (UC-003). 5. PO is issued to the vendor and recorded as Sent. |
| Alternate flows | AF1 Direct PO without PR where policy allows. AF2 Partial conversion: remaining PR lines stay open. AF3 PO revision after issue: restarts approval if value changes. |
| Exception flows | EF1 PR rejected: returns to requester. EF2 Vendor blocked: PO refused. EF3 PR split to avoid limits: flagged by the duplicate and total-by-vendor report (detection rule TBD). EF4 PO cancellation: remaining quantity cancelled. |
| Postconditions | An approved PO exists linked to its PR. |
| Business rules | BRL-009, BRL-031, BRL-032, BRL-034 |
| Related | FR-089 to FR-094. US-048 to US-053 |

## UC-007 Receive goods (full or partial)

| Field | Content |
|---|---|
| Actors | Warehouse Staff. |
| Preconditions | The PO is Approved or Sent. The warehouse is active. |
| Trigger | Goods arrive. |
| Main flow | 1. Staff selects the PO. 2. Staff enters received quantity per line. 3. System checks quantity against open quantity plus tolerance. 4. System posts the GRN, writes stock movements and updates the PO to Partly received or Received. 5. System posts the ledger entry for goods received not invoiced if configured. |
| Alternate flows | AF1 Partial receipt: remaining quantity stays open. AF2 Reject goods: rejected quantity is recorded and not added to stock. |
| Exception flows | EF1 PO cancelled: refused. EF2 Over-receipt beyond tolerance: refused or needs approval. EF3 Receipt reversal would make stock negative: refused. EF4 Posting fails: no stock change and no GRN number is consumed unless gaps are allowed. |
| Postconditions | Stock and PO status reflect the receipt. |
| Business rules | BRL-034, BRL-035, BRL-038 |
| Related | FR-095, FR-096, FR-104. US-054, US-055 |

## UC-008 Process a vendor invoice with three-way match

| Field | Content |
|---|---|
| Actors | Accountant, Finance Manager. |
| Preconditions | A PO exists. A GRN exists for the lines being invoiced. |
| Trigger | The vendor invoice arrives. |
| Main flow | 1. Accountant enters invoice number, date, lines and tax. 2. System checks for a duplicate. 3. System matches quantity and price to PO and GRN. 4. Within tolerance, the invoice goes to approval. 5. After approval, the system posts the payable and ledger entry. |
| Alternate flows | AF1 Variance beyond tolerance: held, approver decides to accept, reject or request a credit note. AF2 Partial invoice for part of the receipt. |
| Exception flows | EF1 Duplicate: refused or flagged with recorded override. EF2 No GRN: held until receipt. EF3 Tax mismatch: held. EF4 Period closed: posting date moved to the next open period by a user decision. |
| Postconditions | An approved payable exists. The invoice is Posted or Held with a reason. |
| Business rules | BRL-036, BRL-037, BRL-016 |
| Related | FR-097 to FR-099, FR-079. US-056, US-057 |

## UC-009 Pay a vendor

| Field | Content |
|---|---|
| Actors | Accountant (prepares), Finance Manager (approves), releaser (may equal approver where policy allows). |
| Preconditions | The invoice is Posted and not on hold. A bank account is active. |
| Trigger | The invoice is due. |
| Main flow | 1. Accountant selects invoices and bank account. 2. System proposes allocation. 3. Payment goes to approval. 4. Approver approves. 5. Releaser releases. 6. System posts the payment, allocates it and updates invoice status. |
| Alternate flows | AF1 Partial payment: invoice becomes Part-paid. AF2 Advance payment before invoice: held as credit and allocated later. AF3 Foreign currency: exchange difference is posted. |
| Exception flows | EF1 Duplicate submission: one payment created (idempotency). EF2 Bank rejection: payment marked Failed and invoice reopens. EF3 Approver equals preparer: refused. EF4 Allocation above open: refused. |
| Postconditions | The payment and allocations are recorded and reversible only by a reversal document. |
| Business rules | BRL-026 to BRL-029, BRL-057 |
| Related | FR-073 to FR-077. US-043 to US-045 |

## UC-010 Confirm a sales order

| Field | Content |
|---|---|
| Actors | Sales Executive, Sales Manager, Finance Manager. |
| Preconditions | Active customer. Items exist. |
| Trigger | The customer accepts a quotation or places an order. |
| Main flow | 1. Executive creates the order (from quotation if any). 2. System checks credit limit plus exposure. 3. System checks stock availability. 4. Executive confirms. 5. System reserves stock and sets Confirmed. |
| Alternate flows | AF1 Credit exceeded: routed to finance approval. AF2 Stock short: partial reserve with backorder. AF3 Approval rejected: order returns to draft. |
| Exception flows | EF1 Customer on hold: refused. EF2 Two orders compete for the last unit: the first confirmation reserves, the second is told. EF3 Quotation expired: conversion refused. |
| Postconditions | A confirmed order with reservation result. |
| Business rules | BRL-041, BRL-046, BRL-047 |
| Related | FR-115, FR-116 to FR-118, FR-107. US-069, US-070 |

## UC-011 Deliver and invoice

| Field | Content |
|---|---|
| Actors | Warehouse Staff, Accountant. |
| Preconditions | The order is Confirmed with reserved stock. |
| Trigger | Goods are ready to ship. |
| Main flow | 1. Staff creates a delivery for chosen quantities. 2. System checks quantity against ordered plus tolerance and stock. 3. System posts the delivery, issues stock and posts cost of goods sold. 4. Accountant creates an invoice for delivered quantities. 5. System calculates tax and posts the receivable. |
| Alternate flows | AF1 Partial delivery: remaining quantity stays open. AF2 Partial invoice. AF3 Advance invoicing where enabled. |
| Exception flows | EF1 Insufficient stock: refused with available quantity. EF2 Failed delivery: the delivery is marked Failed and stock returns. EF3 Invoice above delivered: refused. |
| Postconditions | Stock reduced, receivable posted, order status updated. |
| Business rules | BRL-048, BRL-049, BRL-016 |
| Related | FR-120, FR-121, FR-080. US-072, US-073 |

## UC-012 Return, credit note and refund

| Field | Content |
|---|---|
| Actors | Sales Executive, Warehouse Staff, Accountant, Finance Manager. |
| Preconditions | A delivered and invoiced order. |
| Trigger | The customer returns goods or disputes the invoice. |
| Main flow | 1. Executive records a return against the delivery. 2. Warehouse receives and inspects. 3. System restocks or scraps. 4. Accountant issues a credit note within the invoiced amount. 5. If the customer paid, the credit is held or refunded (approved). |
| Alternate flows | AF1 Credit only, no goods. AF2 Part return. |
| Exception flows | EF1 Return above delivered: refused. EF2 Credit above invoiced less earlier credits: refused. EF3 Refund above received: refused. EF4 Return outside window: needs approval. |
| Postconditions | Stock, receivable and cash positions are corrected with linked documents. |
| Business rules | BRL-050, BRL-051, BRL-053 |
| Related | FR-078, FR-122, FR-123. US-045, US-074, US-075 |

## UC-013 Transfer stock between warehouses

| Field | Content |
|---|---|
| Actors | Inventory Manager, Warehouse Staff at both sites. |
| Preconditions | Source stock is available. Both warehouses are active. |
| Trigger | A branch needs stock from another. |
| Main flow | 1. Source staff creates a transfer. 2. Source dispatches. System reduces source and records in-transit. 3. Destination receives. System increases destination and clears in-transit. |
| Alternate flows | AF1 Partial receipt: remainder stays in transit. AF2 Cancel before dispatch. |
| Exception flows | EF1 Insufficient source stock: refused. EF2 Goods lost in transit: adjustment with approval. EF3 Destination warehouse closed: refused. |
| Postconditions | Stock moved with no unexplained difference. |
| Business rules | BRL-038, BRL-042 |
| Related | FR-108. US-063 |

## UC-014 Adjust stock

| Field | Content |
|---|---|
| Actors | Inventory Manager, approver. |
| Preconditions | A count or discrepancy exists. |
| Trigger | Counted quantity differs from system quantity. |
| Main flow | 1. Manager enters the counted quantities and reason. 2. System shows the difference and its financial impact. 3. Above threshold, the adjustment goes to approval. 4. System posts stock movements and the ledger entry. |
| Alternate flows | AF1 Below threshold posts directly. |
| Exception flows | EF1 Reason missing: refused. EF2 Adjustment would make stock negative: refused. EF3 Period closed: refused. |
| Postconditions | Stock matches the count. A ledger entry exists. |
| Business rules | BRL-040, BRL-043 |
| Related | FR-109. US-064 |

## UC-015 Import master or opening data

| Field | Content |
|---|---|
| Actors | Organization Admin or data owner. |
| Preconditions | Import permission. A template is downloaded. |
| Trigger | Initial load or bulk update. |
| Main flow | 1. User uploads a file. 2. System validates in validate-only mode and reports row errors. 3. User fixes the file. 4. User commits in the chosen mode. 5. System creates records, audits the batch and reports results. |
| Alternate flows | AF1 Valid rows only mode. |
| Exception flows | EF1 Wrong template: refused. EF2 Duplicate keys: reported per row. EF3 Scope violation (row for another company): refused. EF4 System failure in commit: nothing is saved in all-or-nothing mode. |
| Postconditions | Records exist as in the file, or no change. |
| Business rules | BRL-064 |
| Related | FR-048, FR-049. US-028 |

## UC-016 Delegate approval authority

| Field | Content |
|---|---|
| Actors | Approver, delegate. |
| Preconditions | The delegate holds the required role and scope. |
| Trigger | The approver will be absent. |
| Main flow | 1. Approver selects delegate, period and document types. 2. System checks that the delegate is not the creator of pending items and holds the scope. 3. System activates the delegation. 4. Items arrive for the delegate. 5. Audit shows both names. 6. Delegation ends on the end date. |
| Alternate flows | AF1 Revoke early. |
| Exception flows | EF1 Delegate lacks scope: refused. EF2 Chained delegation: refused pending OQ-026. EF3 Delegate is the creator of an item: that item is skipped to the next approver. |
| Postconditions | Approvals continue without gaps. |
| Business rules | BRL-008, BRL-012 |
| Related | FR-044. US-025 |

## Coverage

16 use cases. Measured by script, they reference 59 of the 130 functional requirements: FR-001, FR-003 to FR-004, FR-006 to FR-008, FR-010 to FR-011, FR-016 to FR-017, FR-039 to FR-049, FR-058 to FR-060, FR-062 to FR-065, FR-073 to FR-080, FR-089 to FR-099, FR-104, FR-107 to FR-109, FR-115 to FR-118, FR-120 to FR-123.
Not every FR has a use case. Every FR has at least one user story (see [User Stories](User-Stories.md)).
