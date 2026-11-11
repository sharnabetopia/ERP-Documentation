# Data Dictionary

Document status: Draft v0.2 (status lists corrected in Phase 2). v0.1 (Stage 2). Partial.
Covers shared data types, code lists and status lifecycles, and the definition of the core financial and stock attributes.
Per-column definitions for every entity are not yet written. Entity lists with key attributes are in [Database Design](Database-Design.md).

## 1. Data types

| Type | Definition | Used for |
|---|---|---|
| id | UUID, time-ordered | Primary and foreign keys |
| money | Exact decimal, 19 digits, 4 decimals | All monetary amounts |
| rate | Exact decimal, 18 digits, 8 decimals | Exchange and conversion rates |
| qty | Exact decimal, 18 digits, 4 decimals | Quantities |
| pct | Exact decimal, 7 digits, 4 decimals | Percentages |
| currency_code | 3 letters, ISO 4217 | Currency |
| country_code | 2 letters, ISO 3166 | Country |
| date | Calendar date without zone | Business dates |
| instant | Timestamp with zone, stored in UTC | Event times |
| code | Short upper-case token, unique within its scope, 1 to 30 characters | Business codes |
| status | Code from a status list below | Lifecycle state |
| text_short | Up to 255 characters | Names |
| text_long | Up to 4,000 characters | Narrations and notes |
| encrypted | Field-level encrypted value | Bank account numbers, tax ids where required |

Lengths and precisions are proposals to confirm against OQ-008 (currencies) and the physical design.

## 2. Status lists

| Entity | Statuses (in lifecycle order) | Terminal |
|---|---|---|
| user_account | Invited, Active, Suspended, Locked, Deactivated | Deactivated |
| role | Draft, Active, Retired | Retired |
| vendor | Draft, Pending approval, Active, Blocked | Blocked (reversible) |
| customer | Draft, Active, On hold, Inactive | Inactive |
| item | Draft, Active, Inactive | Inactive |
| purchase_requisition | Draft, Submitted, Approved, Rejected, Converted, Cancelled | Converted, Cancelled (Rejected returns to Draft) |
| purchase_order | Draft, Submitted, Approved, Sent, Partly received, Received, Closed, Cancelled | Closed, Cancelled |
| goods_receipt | Draft, Posted, Reversed | Reversed |
| purchase_invoice | Draft, Submitted, Held, Approved, Posted, Part-paid, Paid, Cancelled | Cancelled, Paid (reversible only by payment reversal) |
| quotation | Draft, Sent, Accepted, Rejected, Expired, Converted | Converted, Rejected, Expired |
| sales_order | Draft, Confirmed, Partly delivered, Delivered, Invoiced, Closed, Cancelled | Closed, Cancelled |
| delivery | Draft, Dispatched, Delivered, Failed, Cancelled | Delivered, Failed, Cancelled |
| sales_invoice | Draft, Posted, Part-paid, Paid, Cancelled | Paid, Cancelled |
| sales_return | Requested, Approved, Received, Closed, Rejected | Closed, Rejected |
| credit_note | Draft, Approved, Posted | Posted |
| payment | Draft, Submitted, Approved, Released, Settled, Failed, Rejected, Cancelled, Reversed | Failed, Cancelled, Reversed (Rejected returns to Draft) |
| journal | Draft, Submitted, Approved, Posted, Rejected, Reversed | Reversed (Rejected returns to Draft) |
| fiscal_year | Planned, Open, Closed | Closed |
| fiscal_period | Future, Open, Soft-closed, Closed | Closed (reopenable by authority) |
| stock_transfer | Draft, Dispatched, Partly received, Received, Cancelled | Received, Cancelled |
| stock_adjustment | Draft, Submitted, Approved, Posted, Rejected | Posted (Rejected returns to Draft) |
| stock_reservation | Active, Released, Consumed | Released, Consumed |
| approval_instance | Pending, Approved, Rejected, Returned, Cancelled, Escalated | Approved, Rejected, Cancelled |
| import_batch | Uploaded, Validated, Committed, Failed | Committed, Failed |
| outbox_event | Pending, Published, Failed, Dead | Published, Dead |
| notification | Queued, Sent, Failed, Read | Read, Failed |

State transition diagrams for the main lists are in [State Transition Diagrams](../05-Workflows-and-Diagrams/State-Transition-Diagrams.md).

## 3. Code lists

| List | Values |
|---|---|
| Account type | Asset, Liability, Equity, Income, Expense |
| Normal balance | Debit, Credit |
| Item type | Stock item, Non-stock item, Service |
| Valuation method | FIFO, Weighted average, Standard cost (final list per OQ-009) |
| Payment direction | Out (vendor payment, customer refund), In (customer receipt, vendor refund) |
| Movement type | Receipt, Issue, Transfer out, Transfer in, Adjustment in, Adjustment out, Return in, Return out, Reversal |
| Source type | purchase_invoice, sales_invoice, goods_receipt, delivery, payment, journal, stock_adjustment, stock_transfer, stock_document, credit_note, sales_return |
| Approval decision | Approved, Rejected, Returned |
| Audit action | Create, Update, Status change, Approve, Reject, Post, Reverse, Cancel, Permission change, Sign-in, Sign-in failed, Export, Import |
| Permission action | Create, Read, Update, Delete, Approve, Reject, Export, Import, Assign, Manage, Submit, Cancel, Verify, Reconcile, Publish, View reports |
| Scope level | Tenant, Company, Branch, Department, Own records |
| Disposition (return) | Restock, Scrap, Return to vendor |

## 4. Core attribute definitions

### ledger_entry

| Attribute | Definition | Rule |
|---|---|---|
| posting_date | Date the entry affects the ledger | Must fall in an open period (BRL-018) |
| debit, credit | Amount in transaction currency | Non-negative. Exactly one is above zero (DBC-01) |
| txn_currency, fx_rate | Currency of the document and the rate used | Rate above zero. Stored from the document (BRL-023) |
| base_debit, base_credit | Amount in company base currency | Equals amount times rate, rounded by company rule |
| source_type, source_id | Originating document | Required for system postings. Manual journals use `journal` |
| branch, department, cost_centre | Reporting dimensions | Optional by account setting |
| party_type, party_id | Customer or vendor for control accounts | Required when the account is a control account |

### stock_movement

| Attribute | Definition | Rule |
|---|---|---|
| qty_in, qty_out | Quantity added or removed in base unit | Non-negative. Exactly one is above zero |
| unit_cost, value | Cost per base unit and extended value | Set by valuation method at posting |
| movement_type | Kind of movement | From the code list |
| source_type, source_id, source_line_id | Document line causing the movement | Required |

### payment_allocation

| Attribute | Definition | Rule |
|---|---|---|
| amount | Amount in open item currency | Not above the open amount (BRL-026) |
| base_amount | Amount in base currency at settlement rate | |
| fx_difference | Realised exchange gain or loss | Posted to the exchange difference account (BRL-024) |

### approval_instance and approval_decision

| Attribute | Definition | Rule |
|---|---|---|
| round_no | Submission round for a document | Increments on resubmission (BRL-011) |
| decided_by | User who decided | Not the document creator (BRL-008) |
| on_behalf_of | Delegator, if any | Set when acting under delegation (BRL-012) |

### audit_log

| Attribute | Definition | Rule |
|---|---|---|
| before, after | Changed fields before and after | Sensitive values masked where required |
| occurred_at | Event instant | System generated, UTC |
| trace_id | Correlation id | Same as request logs (TR-018) |

## 5. Not yet covered

Per-column dictionaries for all 96 entities (87 plus 9 added in v0.2), field-level validation lengths, and sample values.
Planned with the physical design and the UI form validation rules (Stage 3).
