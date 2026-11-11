# Module Feature Breakdown

Document status: **Partial, Draft v0.1 (Stage 1).**
Section 2 covers the MVP modules in depth.
Section 3 covers non-MVP modules at feature-list level only.
Per-module notifications, reports, integration points, user stories and test scenarios are completed in Stages 2 and 3.
Role names use the 18 default roles from the scope request.
All content is Proposed. See [Open Questions](../02-Business-Analysis/Open-Questions.md).

Abbreviations: C create, R read, U update, D delete (soft unless stated), SoD segregation of duties.

## 1. Fields documented per module

| Field | Meaning |
|---|---|
| Purpose | Why the module exists |
| Submodules and features | What it contains |
| CRUD | Which operations apply and any restriction |
| Roles | Who typically uses it |
| Status lifecycle | Document states |
| Key validations | Required fields and checks |
| Business rules | Links to BRL IDs |
| Dependencies | Modules it needs |
| Edge cases | Cases the design must handle |

## 2. MVP modules in depth

### 2.1 Core platform

| Module | Purpose | Submodules and features | CRUD | Roles | Status lifecycle | Key validations | Rules | Dependencies | Edge cases |
|---|---|---|---|---|---|---|---|---|---|
| M-002 Authentication | Prove identity and control sessions | Sign-in, sign-out, password reset, MFA (OQ-023), SSO (OQ-023), session list, lockout | C session, R session, D session (revoke). No user edit here. | All | Active, Locked, Expired, Revoked | Email format, password policy, MFA code format | BRL-005, BRL-006 | M-003 | Reset link reuse, concurrent sessions, clock skew on tokens, lockout of the only Super Admin |
| M-003 User Management | Create and maintain user accounts | Invite, activate, deactivate, assign company and scope, link to employee | CRUD. Delete is deactivate. | Super Admin, Organization Admin, Branch Admin | Invited, Active, Suspended, Deactivated | Unique email per tenant, required role and company | BRL-002, BRL-054, BRL-055 | M-004, M-005, M-016 | Same email in two companies, deactivating a user with pending approvals |
| M-004 RBAC | Define what each role can do | Roles, permission catalogue, role assignment, scope rules, temporary grants, SoD conflict check | CRUD on custom roles. System roles are read-only. | Super Admin, Organization Admin | Draft, Active, Retired | Role name unique per tenant, no empty permission set | BRL-003, BRL-004, BRL-057 to BRL-059 | M-003 | Removing a role in use, conflicting permissions on one user, expiring temporary grant mid-task |
| M-005 Organization and Company | Define legal entities | Company profile, base currency, fiscal settings link, tax identifiers, logo and legal text | CRUD. Delete blocked once transactions exist. | Super Admin, Organization Admin | Active, Inactive | Legal name, country, base currency required | BRL-001 | None | Changing base currency after postings (blocked) |
| M-006 Branch and Department | Structure operations | Branch tree, departments, cost centres, head of unit | CRUD. Delete blocked if referenced. | Organization Admin, Branch Admin | Active, Inactive | Unique code per company, valid parent | BRL-015 | M-005 | Moving a branch with open documents, circular parent |
| M-007 Employee Directory | Find people and reporting lines | Search, org chart, contact view | R only. Edits via M-016. | All | None | None | BRL-056 | M-016 | Hidden personal fields per role |
| M-008 System Configuration | Company and system settings | Numbering series, document templates, tolerance settings, policy flags, default accounts | CRU. No delete. | Super Admin, Organization Admin, Finance Manager | Draft, Active | Valid format tokens, no overlapping series | BRL-014, BRL-036 | M-005 | Changing a numbering format mid-year |
| M-009 Master Data | Shared reference data | Units, currencies, tax codes, payment terms, countries, parties base | CRUD. Soft delete. | Org Admin, role owners | Active, Inactive | Unique codes, valid ISO values | BRL-015 | M-005 | Merge of duplicate records |
| M-010 Audit Logs | Immutable change history | Event capture, search, filter, export, retention | R and export only. No U or D for any role. | Auditor, Super Admin (read) | Immutable | None (system generated) | BRL-007 | All | Log volume growth, logging failure must not lose business event |
| M-011 Document Management (attachments) | Evidence on records | Upload, preview, download, link to record, file type and size limits | C, R, soft D with audit | Record owners | Active, Removed | Type whitelist, size cap, malware scan (TR later) | BRL-060 | All | Large files, renamed executables, access after record scope change |
| M-012 Notifications | Tell users what needs action | In-app inbox, email, templates, preferences, retry | R own, U preference. Admin edits templates. | All | Queued, Sent, Failed, Read | Valid recipient, template variables | BRL-061 | M-013 | Provider outage, duplicate notices, user without email |
| M-013 Workflow and Approval Engine | Route documents for decision | Chain definition by type, value, scope. Approve, reject, return, delegate, escalate, history | CRUD on chains. Decisions are append-only. | Admin, approvers | Pending, Approved, Rejected, Returned, Cancelled, Escalated | Chain has at least one approver, no self-approval | BRL-008 to BRL-013 | M-003, M-004 | Approver leaves, chain changed mid-flight, delegation overlap, no eligible approver |
| M-014 Import and Export | Move data in and out | Templates, validate-only, commit, error report, export with filters | Import creates. Export reads. | Admin, data owners | Uploaded, Validated, Committed, Failed | Header match, type checks, duplicate keys | BRL-064 | M-009 | Partial failure, huge files, scope leaks in export |
| M-001 Dashboard (basic) | Landing view | Pending approvals, open documents, key figures by role | R only | All | None | None | BRL-003 | Many | Widget data beyond user scope |
| M-015 Localisation (formats) | Correct display | Date, number, currency formats, time zone | R, U own preference | All | None | Valid locale | None | M-005 | Daylight saving, users across zones |
| M-116 Data Governance (basic) | Retention and privacy handling | Retention rules per record type, legal hold, personal data export, anonymisation, deletion block inside retention | CRU on rules. Anonymise is irreversible and audited | Organization Admin, Super Admin (rules), Auditor (read) | Active, Inactive | Retention years above zero, no deletion inside retention | BRL-063 | M-010, M-009 | Legal hold during anonymisation request, personal data inside attachments and audit |

### 2.2 Finance

| Module | Purpose | Submodules and features | CRUD | Roles | Status lifecycle | Key validations | Rules | Dependencies | Edge cases |
|---|---|---|---|---|---|---|---|---|---|
| M-030 Finance Management | Finance settings | Default accounts, posting rules, dimensions | CRU | Finance Manager | Draft, Active | Mapped accounts exist and are postable | BRL-019 | M-031 | Changing a mapping with open documents |
| M-031 Chart of Accounts | Ledger structure | Account tree, type, normal balance, control flag, template load | CRUD. Delete only if unused. | Finance Manager, Accountant | Active, Inactive | Unique code, valid parent, type required | BRL-015, BRL-019 | M-005 | Re-parenting an account with balances |
| M-032 General Ledger | Financial record | Ledger entries, balances, drill-down, account statement | R only. Entries come from postings. | Accountant, Finance Manager, Auditor | Posted, Reversed | Balanced, open period, valid accounts | BRL-016 to BRL-018 | M-031, M-043 | Reversal of reversal, concurrent posting |
| M-035 Journal Entries | Manual and recurring postings | Create, submit, approve, post, reverse | CRUD while draft. Posted is immutable. | Accountant (maker), Finance Manager (checker) | Draft, Submitted, Approved, Posted, Rejected, Reversed | Two or more lines, balanced, narration, period open | BRL-008, BRL-016, BRL-017, BRL-030 | M-032 | Foreign currency journal, backdated entry, rounding line |
| M-033 Accounts Payable | Vendor liabilities | Vendor invoices, debit notes, open items, ageing | CRU in draft | Accountant | Draft, Submitted, Approved, Posted, Part-paid, Paid, Cancelled | Vendor active, tax, match result | BRL-036, BRL-037 | M-054, M-058 | Duplicate invoice, invoice before GRN, tax mismatch |
| M-034 Accounts Receivable | Customer receivables | Customer invoices, credit notes, open items, ageing | CRU in draft | Accountant | Draft, Posted, Part-paid, Paid, Cancelled | Customer active, tax, credit | BRL-047, BRL-049, BRL-050 | M-079, M-082 | Overpayment, credit note above invoice |
| M-036 Bank Account Management | Cash and bank accounts | Accounts, currency, GL mapping, signatories | CRUD. Delete blocked if used. | Finance Manager | Active, Closed | Unique account number per bank, currency | BRL-015 | M-031 | Closing an account with unreconciled items |
| M-040 Tax and VAT | Tax setup and reports | Tax codes, rates, effective dates, inclusive or exclusive, tax report | CRUD. Rate change by new effective record. | Finance Manager | Active, Inactive | Rate range, effective date non-overlap | BRL-025 | M-009 | Mixed tax lines, rate change mid-order, rounding |
| M-041 Multi-Currency | Foreign transactions | Currency list, rate tables, rate source, revaluation (later), exchange gain and loss | CRUD on rates | Finance Manager | Active, Inactive | Rate above zero, date unique per pair | BRL-023, BRL-024 | M-009 | Missing rate on a date, rate corrected after use |
| M-043 Financial Period | Posting windows | Periods per year, open, soft close, close | RU state | Finance Manager | Future, Open, Soft-closed, Closed | No overlap or gap | BRL-018, BRL-021, BRL-022 | M-044 | Reopen with later periods closed |
| M-044 Fiscal Year | Year definition | Year calendar, year-end | CRU | Finance Manager | Planned, Open, Closed | Valid start, 12 periods or configured | BRL-021 | M-005 | Short first year |
| M-045 Financial Closing | Controlled close | Close checklist, carry forward, retained earnings entry, opening balances | Execute, reopen | Finance Manager | Not started, In progress, Closed | All checklist items done | BRL-021, BRL-022 | M-043, M-044 | Open documents at close |
| M-046 Financial Statements | Reports | Trial balance, income statement, balance sheet, drill-down | R, export | Finance Manager, Auditor | None | Period selected | BRL-016 | M-032 | Prior period comparatives, currency translation (TBD) |
| M-047 Payment and Refund | Money movements | Payments out, receipts in, refunds, allocation, advances, reversal | CRU in draft. Post then reverse only. | Accountant, Finance Manager | Draft, Approved, Released, Failed, Reversed | Amount above zero, allocation within open, bank selected | BRL-026 to BRL-029, BRL-053 | M-033, M-034, M-036 | Partial payment, advance, duplicate submit, bank failure, refund above received |

### 2.3 Procure-to-Pay

| Module | Purpose | Submodules and features | CRUD | Roles | Status lifecycle | Key validations | Rules | Dependencies | Edge cases |
|---|---|---|---|---|---|---|---|---|---|
| M-049 Purchase Requisition | Raise a need | Lines, justification, budget code, attachments | CRUD while draft | Employee, Department Manager | Draft, Submitted, Approved, Rejected, Converted, Cancelled | Item or description, quantity above zero, needed-by date | BRL-031 | M-061, M-013 | Splitting to avoid limits, item inactive after submit |
| M-050 PR Approval | Control spend | Chain by value and department | Decisions only | Department Manager, Procurement Manager | As M-013 | Comment on reject | BRL-008 to BRL-013 | M-013 | Approver on leave |
| M-053 Purchase Order | Commit to vendor | Create from PR or direct, lines, taxes, terms, revisions, close | CRUD while draft | Procurement Officer, Procurement Manager | Draft, Submitted, Approved, Sent, Partly received, Received, Closed, Cancelled | Vendor active, price, tax, delivery date | BRL-031, BRL-032, BRL-034 | M-054, M-049 | Cancel after partial receipt, revision after approval, vendor blocked after PO |
| M-054 Vendor Management | Vendor master | Profile, tax id, bank, contacts, status, approval | CRUD. Delete blocked if used. | Procurement Officer, Procurement Manager | Draft, Pending approval, Active, Blocked | Unique tax id, bank format | BRL-032, BRL-033, BRL-058 | M-009 | Duplicate vendor, bank change fraud |
| M-057 Goods Receipt Note | Record receipt | Receive against PO, partial, reject, over-receipt tolerance | CRU in draft | Warehouse Staff | Draft, Posted, Reversed | Qty within PO plus tolerance, warehouse | BRL-035, BRL-038 | M-053, M-064 | Receipt for cancelled PO, damaged goods |
| M-058 Purchase Invoice | Record vendor bill | Enter invoice, match to PO and GRN, hold, approve, post | CRU in draft | Accountant | See M-033 | Unique per vendor and number, totals | BRL-036, BRL-037 | M-057 | Invoice before receipt, price variance |
| M-059 Vendor Payments | Pay vendors | Payment run or single payment, allocate, remit | See M-047 | Accountant, Finance Manager | See M-047 | Approved invoice, bank | BRL-057 | M-047 | Pay disputed invoice, partial payment |
| M-060 Procurement Reports | Visibility | Open PO, spend by vendor, receipt variance | R, export | Procurement Manager, Finance Manager | None | Date range | None | M-053 | Scope filtering |

### 2.4 Inventory

| Module | Purpose | Submodules and features | CRUD | Roles | Status lifecycle | Key validations | Rules | Dependencies | Edge cases |
|---|---|---|---|---|---|---|---|---|---|
| M-061 Catalog | Items and services | Item type (stock, non-stock, service), units, conversions, prices, default accounts | CRUD. Deactivate when used. | Inventory Manager | Draft, Active, Inactive | Unique code, unit, category | BRL-015, BRL-044 | M-062 | Unit change after movements |
| M-062 Categories | Classification | Category tree, default accounts, valuation default | CRUD | Inventory Manager | Active, Inactive | Unique name per parent | BRL-015 | M-031 | Re-parenting |
| M-063 Warehouse | Locations | Warehouse, bins (TBD), branch link | CRUD | Inventory Manager | Active, Inactive | Unique code, branch | BRL-015 | M-006 | Closing a warehouse with stock |
| M-064 Stock Management | Quantity on hand | Stock ledger, on-hand, available, in-transit, history | R only | Inventory Manager, Warehouse Staff | None | None | BRL-038 to BRL-041 | M-073 | Concurrent issue of last unit |
| M-065 Reservation | Hold stock | Reserve, release, expire | CRU via orders | Sales Executive, system | Active, Released, Consumed | Qty within available | BRL-041, BRL-052 | M-081 | Conflict between two orders |
| M-066 Stock Transfer | Move between locations | Request, dispatch, receive (full or partial) | CRU in draft | Warehouse Staff, Inventory Manager | Draft, Dispatched, Partly received, Received, Cancelled | Source stock, different locations | BRL-042 | M-063 | Loss in transit |
| M-067 Stock Adjustment | Correct stock | Count, adjust, reason, approval | CRU in draft | Inventory Manager | Draft, Submitted, Approved, Posted, Rejected | Reason, qty, value | BRL-043 | M-064 | Adjustment that makes stock negative |
| M-072 Valuation | Stock value | Method per item (OQ-009), revaluation, GL posting | R, run | Finance Manager | None | Method set | BRL-044 | M-064 | Method change, backdated receipt |
| M-073 Goods Issue and Receipt | Movement documents | Issue to cost centre, receipt without PO, return to vendor | CRU in draft | Warehouse Staff | Draft, Posted, Reversed | Qty, reason, account | BRL-038 | M-064 | Issue of reserved stock |
| M-075 Inventory Reports | Visibility | Stock on hand, movements, valuation, low stock | R, export | Inventory Manager | None | Warehouse, date | None | M-064 | Large data sets |

### 2.5 Order-to-Cash

| Module | Purpose | Submodules and features | CRUD | Roles | Status lifecycle | Key validations | Rules | Dependencies | Edge cases |
|---|---|---|---|---|---|---|---|---|---|
| M-079 Customer | Customer master | Profile, contacts, terms, credit limit, tax id, status | CRUD. Delete blocked if used. | Sales Manager, Sales Executive | Draft, Active, On hold, Inactive | Unique tax id, valid terms | BRL-046, BRL-047 | M-009 | Duplicate customer, credit limit change |
| M-080 Quotation | Offer | Lines, discount, validity, revisions, convert | CRUD while draft | Sales Executive | Draft, Sent, Accepted, Rejected, Expired, Converted | Item, price, validity date | BRL-046 | M-079, M-061 | Price change after quote, expiry |
| M-081 Sales Order | Customer commitment | Lines, delivery date, reserve, approval, cancel | CRUD while draft | Sales Executive, Sales Manager | Draft, Confirmed, Partly delivered, Delivered, Invoiced, Closed, Cancelled | Customer active, stock, credit | BRL-046 to BRL-049, BRL-052 | M-080, M-065 | Cancel after partial delivery |
| M-082 Sales Invoice | Bill customer | From delivery or order, tax, terms, post | CRU in draft | Accountant | Draft, Posted, Part-paid, Paid, Cancelled | Qty within delivered, tax | BRL-049 | M-083, M-034 | Invoice without delivery, price change |
| M-083 Delivery | Ship goods | Pick, dispatch, partial, proof of delivery | CRU in draft | Warehouse Staff | Draft, Dispatched, Delivered, Failed, Cancelled | Qty within order and stock | BRL-048 | M-081, M-064 | Short ship, failed delivery |
| M-084 Sales Returns | Take goods back | Return request, receive, inspect, restock or scrap | CRU in draft | Sales Executive, Warehouse Staff | Requested, Approved, Received, Closed, Rejected | Reference to delivery, qty limit | BRL-051 | M-083 | Return past policy window |
| M-085 Credit Notes | Correct invoice | Issue against invoice, allocate or refund | CRU in draft | Accountant | Draft, Approved, Posted | Amount within invoice | BRL-050 | M-082 | Credit on paid invoice |
| M-086 Customer Payments | Receive money | Receipt, allocate, advance, refund | See M-047 | Accountant | See M-047 | Amount, bank | BRL-026, BRL-027 | M-047 | Overpayment, bounced payment |
| M-088 Sales Reports | Visibility | Sales by customer and item, ageing, order status | R, export | Sales Manager | None | Date range | None | M-082 | Scope filtering |

### 2.6 People core

| Module | Purpose | Submodules and features | CRUD | Roles | Status lifecycle | Key validations | Rules | Dependencies | Edge cases |
|---|---|---|---|---|---|---|---|---|---|
| M-016 Employee (core) | Person record | Identity, job, branch, department, manager, status | CRUD. Delete is deactivate. | HR Manager, HR Executive | Draft, Active, On leave, Separated | Unique employee code, manager exists, no cycle | BRL-054 to BRL-056 | M-006 | Manager change with pending approvals, rehire |

## 3. Non-MVP modules (feature list only)

Detailed breakdown is added before the release that contains the module starts.

| Group | Modules | Core features to be detailed later |
|---|---|---|
| HR depth | M-017 to M-029 | Job requisition, applicants, onboarding checklist, offboarding clearance, attendance capture, shifts, leave types and balances, payroll runs and payslips, benefits, appraisal cycles, training plans, self-service requests, claims, HR reports |
| Finance depth | M-037 to M-039, M-042, M-048 | Statement import and matching, budget versions and control, expense policies, depreciation schedules, audit reports |
| Procurement depth | M-051, M-052, M-055, M-056 | RFQ rounds, quote scoring, supplier scorecards, contract terms and renewal |
| Inventory depth | M-068 to M-071, M-074 | Batch and serial capture, expiry alerts, reorder rules, barcode and QR generation and scan |
| CRM and sales depth | M-076 to M-078, M-087 | Lead capture and scoring, pipeline stages, activities, commission plans |
| Projects | M-089 to M-096 | Project plan, tasks, timesheets, resource allocation, budget, cost, billing, progress |
| Manufacturing and supply chain | M-097 to M-109 | Production plan, BOM versions, work orders, MRP run, schedule, WIP, inspection plans, defects, maintenance plans, forecasting |
| Enterprise | M-110 to M-120 | Asset register, fleet, helpdesk, SLA timers, subscriptions, compliance register, governance policies, knowledge articles, portals, BI |

Module counts in these groups reconcile to the [inventory](ERP-Module-Inventory.md).
