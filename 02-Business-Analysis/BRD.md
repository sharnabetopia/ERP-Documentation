# Business Requirements Document (BRD)

Document status: Draft v0.1 (Stage 1). Not approved.
Owner: Business Analyst (to be assigned).
Related: [Project Charter](../00-Project-Management/Project-Charter.md), [Module Inventory](../01-ERP-Module-Analysis/ERP-Module-Inventory.md), [Business Rules](Business-Rules.md), [Open Questions](Open-Questions.md), [MVP Strategy](../00-Project-Management/MVP-Strategy.md).

## 1. Status of the content

No sponsor, customer or industry has been named for this ERP.
Every requirement below is therefore a **proposed requirement** derived from standard enterprise ERP practice and from the scope request.
The Source column names the stakeholder role that would normally own the requirement, not a confirmed approval.
Requirements that depend on unknown facts carry a TBD and point to an Open Question.

| Label | Meaning |
|---|---|
| Proposed | Drafted by the BA. Awaiting stakeholder confirmation. |
| TBD | A value or decision is unknown. See Open Questions. |
| Assumption (ASM) | Something treated as true for planning until disproved. |
| Constraint (CON) | A limit the solution must respect. |

## 2. Purpose

This document states **what the business needs** from the ERP.
It does not say how the system is built.
Functional detail is in the FRS and architecture is in the ARD (later stages).

## 3. Vision

A configurable, multi-company, multi-branch ERP that gives every legal entity one trusted record of its money, stock, people and commitments.
It starts with the commercial backbone and grows by module and by industry without rewriting the core.

## 4. Business objectives

| ID | Objective | Measure (target is TBD until baseline is known) |
|---|---|---|
| OBJ-01 | Replace disconnected spreadsheets and point tools with one system of record. | Number of retired tools. TBD. |
| OBJ-02 | Shorten the time from purchase request to paid invoice and from order to cash. | Cycle time against As-Is baseline. TBD. |
| OBJ-03 | Close the books faster and with fewer manual adjustments. | Days to close a period. TBD. |
| OBJ-04 | Enforce spending and approval controls. | Share of spend with a recorded approval. Target 100 percent for in-scope documents. |
| OBJ-05 | Provide a complete audit trail for finance and compliance. | Share of financial changes with an audit record. Target 100 percent. |
| OBJ-06 | Support growth by adding companies, branches and modules without a rebuild. | Time to onboard a new branch. TBD. |
| OBJ-07 | Give managers real-time visibility of stock, receivables and payables. | Report freshness. TBD. |

## 5. Business problems addressed

| ID | Problem (typical, to be validated in As-Is analysis) |
|---|---|
| PRB-01 | Data is duplicated across departments and does not agree. |
| PRB-02 | Approvals happen by email or chat with no reliable record. |
| PRB-03 | Stock levels are inaccurate, causing stock-outs or over-ordering. |
| PRB-04 | Duplicate or unmatched vendor invoices are paid. |
| PRB-05 | Month-end close depends on manual consolidation. |
| PRB-06 | Staff can see data they should not. |
| PRB-07 | Auditors cannot trace a figure to its source document. |
| PRB-08 | Each new branch or company requires a new set of tools. |

PRB-01 to PRB-08 are hypotheses until the As-Is workshops are held ([As-Is Analysis](As-Is-Analysis.md)).

## 6. Scope

### 6.1 In scope for the programme

All 120 modules in the [Module Inventory](../01-ERP-Module-Analysis/ERP-Module-Inventory.md), delivered across MVP, R2, R3 and R4.

### 6.2 In scope for the MVP (proposed)

58 modules whose earliest release is MVP (see [MVP Strategy](../00-Project-Management/MVP-Strategy.md)).

### 6.3 Out of scope (proposed)

| Item | Reason |
|---|---|
| Native mobile apps | Responsive web is assumed for MVP. Decision TBD (OQ-025). |
| Industry packs (POS, patient billing, and similar) | Industry not confirmed (OQ-002). |
| Payroll statutory filing | Jurisdiction unknown (OQ-006). |
| E-invoicing government connectors | Jurisdiction unknown (OQ-017). |
| Historical data older than the agreed migration window | Window TBD (OQ-012). |
| Hardware (scanners, printers) procurement | Not a software deliverable. |

### 6.4 System boundary

The ERP owns the master and transaction data of the in-scope modules.
It integrates with, but does not own, external email, banks, payment gateways, identity providers and tax authorities.
See [Business Context](Business-Context.md).

## 7. Target users and stakeholders

Detailed in [Stakeholder Register](../00-Project-Management/Stakeholder-Register.md) and [RACI Matrix](../00-Project-Management/RACI-Matrix.md).
Default roles are the 18 roles listed in the scope request (Super Admin to Read-only User).

## 8. Assumptions

| ID | Assumption | Risk if false | Linked question |
|---|---|---|---|
| ASM-01 | One deployment serves several companies with isolated data. | Tenancy design changes. | OQ-019 |
| ASM-02 | Users access the system through a modern web browser. | Mobile effort added. | OQ-025 |
| ASM-03 | A base currency is defined per company. | Reporting model changes. | OQ-008 |
| ASM-04 | Accounting follows double entry on an accrual basis. | Ledger design changes. | OQ-018 |
| ASM-05 | Email is the minimum notification channel. | Channel work grows. | OQ-017 |
| ASM-06 | Existing data can be extracted in CSV or spreadsheet form. | Migration effort grows. | OQ-012 |
| ASM-07 | A modular monolith is sufficient for MVP load. To be tested in the ADR. | Architecture changes. | OQ-015 |
| ASM-08 | Approval thresholds are set by each company, not by the vendor. | Rules need defaults. | OQ-013 |

## 9. Constraints

| ID | Constraint |
|---|---|
| CON-01 | Financial amounts must keep exact decimal precision. Binary floating point is not acceptable for money. |
| CON-02 | Posted financial records are never edited or deleted. Corrections use reversing entries. |
| CON-03 | Users see only data within their assigned scope. No default company-wide access. |
| CON-04 | Delivery capacity, budget and dates are unknown (OQ-014). The plan uses relative sprints, not dates. |
| CON-05 | Technology stack is not yet chosen (OQ-024). Stack evaluation is a later deliverable. |
| CON-06 | Data residency and legal retention rules are unknown (OQ-010, OQ-022). |

## 10. Dependencies

| ID | Dependency | Needed by |
|---|---|---|
| DEP-01 | Named sponsor and decision owner. | All sign-offs. |
| DEP-02 | Finance lead to approve chart of accounts template and posting rules. | M-030 to M-048 |
| DEP-03 | Tax advisor input on tax regimes. | M-040 |
| DEP-04 | Sample legacy data for migration rehearsal. | M-014, data migration |
| DEP-05 | Email and identity provider access. | M-002, M-012 |
| DEP-06 | Bank file format samples. | M-036, M-037 |

## 11. Business requirements

Columns: Priority and release follow the inventory scale.
Verification: T = test, D = demonstration, I = inspection, A = analysis.
Acceptance criteria are business-level statements.
Given/When/Then detail is written per user story in Stage 2.

### 11.1 Platform and administration

| ID | Title | Description | Justification | Pri / Rel | Source (role) | Depends on | Acceptance criteria | Modules | Verif. |
|---|---|---|---|---|---|---|---|---|---|
| BR-001 | Multi-company support | One deployment hosts several companies. Each company's data is isolated from the others. | Group structures and shared service providers need one platform. | P0 / MVP | Executive sponsor | OQ-019 | A user of company A cannot read, list or infer company B data by any screen or API. | M-005 | T |
| BR-002 | Multi-branch operation | A company has branches and departments. Transactions, stock and users can be scoped to them. | Branch-level control and reporting. | P0 / MVP | Operations head | BR-001 | A branch user sees only their branch unless granted wider scope. Branch totals roll up to company totals. | M-006 | T |
| BR-003 | Role and scope based access | Access is granted by role, and limited by company, branch, department and record ownership. | Prevents unauthorised data access and supports segregation of duties. | P0 / MVP | Security officer | BR-001, BR-002 | Each role has a documented permission set. An unauthorised action is refused and logged. | M-004 | T |
| BR-004 | Secure sign-in | Users authenticate with strong credentials and optional multi-factor. Sessions expire. | Protects financial data. | P0 / MVP | Security officer | OQ-023 | Weak passwords are refused. Repeated failures lock the account. Idle sessions end. | M-002, M-003 | T |
| BR-005 | Complete audit trail | Create, change, approve, post, cancel and permission changes are recorded with who, when, what and before/after values. | Financial control and compliance. | P0 / MVP | Auditor | None | Every in-scope change has an immutable audit record that authorised users can search and export. | M-010 | T |
| BR-006 | Configurable approvals | Approval chains are configurable by document type, value, branch and department. Delegation and escalation are supported. | Different companies have different authority limits. | P0 / MVP | Finance manager | OQ-013 | An administrator can change a chain without code. A rejected document returns to the requester with a reason. | M-013 | T |
| BR-007 | Actionable notifications | Users are told when action is needed or a decision is made. | Prevents stalled workflows. | P0 / MVP | All users | BR-006 | A pending approval produces a notification to each approver. Failed delivery is retried and visible. | M-012 | T |
| BR-008 | Governed master data | Shared records (parties, items, units, currencies, taxes, accounts) are created once and controlled. | Avoids duplicate and conflicting records. | P0 / MVP | Data owner | None | Duplicate detection runs on creation. Changes to key fields are audited. | M-009 | T |
| BR-009 | Bulk import and export | Users with permission can import and export data in standard file formats with validation reports. | Migration and audit needs. | P0 / MVP | Implementation lead | DEP-04 | An import with errors reports each failed row and imports nothing or only valid rows per the chosen mode. | M-014 | T |
| BR-010 | Attachments | Supporting documents can be attached to transactions. | Approvals and audits need evidence. | P1 / MVP | Auditor | None | Allowed file types and size are enforced. Attachments are covered by the same access rules as the parent record. | M-011 | T |
| BR-011 | Localisation | Dates, numbers, currencies and time zones follow company and user settings. | Multi-branch, multi-country use. | P1 / MVP | Operations head | OQ-011 | A document shows the same instant correctly for users in different time zones. | M-015 | T |
| BR-012 | Configurable numbering | Document numbers are unique and sequential per company, document type and fiscal year, with a configurable format. | Audit and legal requirements. | P0 / MVP | Finance manager | None | Two concurrent users never receive the same number. Gaps are explained or prevented per policy. | M-008 | T |
| BR-013 | Role-based dashboards | Each role sees relevant tasks and key figures on landing. | Fast daily use. | P1 / MVP | All managers | BR-003 | A user sees only widgets and figures within their scope. | M-001 | D |

### 11.2 Finance and accounting

| ID | Title | Description | Justification | Pri / Rel | Source (role) | Depends on | Acceptance criteria | Modules | Verif. |
|---|---|---|---|---|---|---|---|---|---|
| BR-014 | Double-entry ledger | Every financial posting is balanced and recorded in a general ledger. | Accounting integrity. | P0 / MVP | Finance manager | BR-015 | A posting where debits differ from credits is refused. Totals of the ledger always balance. | M-032, M-035 | T |
| BR-015 | Configurable chart of accounts | Each company maintains its chart of accounts, with account types and posting restrictions. | Companies differ in structure and standards. | P0 / MVP | Finance manager | OQ-018 | A template chart can be loaded and edited. An account with postings cannot be deleted. | M-031 | T |
| BR-016 | Period and year control | Posting is allowed only in open periods of an open fiscal year. Closing is controlled and reversible only by authorised users under audit. | Financial integrity. | P0 / MVP | Finance manager | OQ-021 | A posting dated in a closed period is refused. Year close produces opening balances. | M-043, M-044, M-045 | T |
| BR-017 | Multi-currency | Transactions can be in a foreign currency, converted to the company base currency with dated rates. Gains and losses are posted. | Cross-border trade. | P0 / MVP | Finance manager | OQ-008 | Document currency, rate and base amount are stored. Settlement posts exchange differences. | M-041 | T |
| BR-018 | Tax calculation | Taxes are configured and calculated on sales and purchase documents, and reported. | Legal compliance. | P0 / MVP | Finance manager | OQ-007 | Tax amounts match configured rates, rounding rule and inclusive or exclusive setting. | M-040 | T |
| BR-019 | Financial statements | The system produces trial balance, income statement and balance sheet per company and period. Cash flow follows later. | Core output of Record-to-Report. | P0 / MVP | Finance manager | BR-014 | Statement totals reconcile to the ledger. Drill-down reaches the source document. | M-046 | T |
| BR-020 | Payments and refunds | Payments and refunds are recorded against bank or cash accounts and allocated to invoices, including partial and advance payments. | Accurate open items. | P0 / MVP | Finance manager | BR-014 | Allocated amounts never exceed the open amount. Unallocated advances are tracked. | M-047, M-059, M-086 | T |
| BR-021 | Maker-checker on journals | Manual journals need a different approver from the creator. | Fraud prevention. | P0 / MVP | Auditor | BR-006 | The creator cannot approve their own journal. | M-035 | T |
| BR-022 | Bank reconciliation | Bank statement lines are matched to ledger entries and differences are explained. | Cash control. | P1 / R2 | Finance manager | DEP-06 | An unmatched item remains visible until resolved. | M-036, M-037 | T |
| BR-023 | Budgeting and control | Budgets are set per period, account and dimension. Spend is compared with budget and can warn or block. | Cost control. | P1 / R2 | Finance manager | BR-014 | A transaction that breaches a blocking budget is refused or routed for approval per setting. | M-038 | T |

### 11.3 Procure-to-Pay

| ID | Title | Description | Justification | Pri / Rel | Source (role) | Depends on | Acceptance criteria | Modules | Verif. |
|---|---|---|---|---|---|---|---|---|---|
| BR-024 | Requisition and approval | Staff raise purchase requisitions. Approval is required before ordering. | Spend control. | P0 / MVP | Procurement manager | BR-006 | An unapproved requisition cannot become a PO. | M-049, M-050 | T |
| BR-025 | Vendor master control | Vendors are registered with tax and bank details and an active status. Changes to bank details are approved. | Fraud and duplicate prevention. | P0 / MVP | Procurement manager | BR-008 | A blocked vendor cannot receive a new PO. | M-054 | T |
| BR-026 | Purchase order approval | POs are approved by authority matrix based on value. | Commitment control. | P0 / MVP | Finance manager | BR-006, OQ-013 | A PO above a user's limit is routed to the next approver. | M-053 | T |
| BR-027 | Goods receipt with partials | Goods are received against POs, in full or in part, and increase stock. | Accurate stock and liabilities. | P0 / MVP | Warehouse manager | BR-034 | Received quantity cannot exceed ordered plus tolerance. Remaining quantity stays open. | M-057 | T |
| BR-028 | Three-way match | Purchase invoices are matched to PO and GRN within tolerance before payment. | Prevents overpayment. | P0 / MVP | Finance manager | BR-027 | A mismatch beyond tolerance blocks payment pending approval. | M-058 | T |
| BR-029 | Duplicate invoice prevention | The system detects probable duplicate vendor invoices. | Prevents double payment. | P0 / MVP | Finance manager | None | Same vendor, number and amount is refused or flagged for review. | M-058 | T |
| BR-030 | Approved vendor payments | Vendor payments are approved, recorded and traceable to invoices. | Cash control. | P0 / MVP | Finance manager | BR-020 | A payment cannot be released without approval. | M-059 | T |
| BR-031 | Sourcing by quotation | Requests for quotation are sent to vendors and offers are compared. | Better pricing. | P1 / R2 | Procurement manager | BR-024 | An awarded quote converts to a PO with its prices. | M-051, M-052 | T |
| BR-032 | Contract management | Contracts have terms, dates, renewal alerts and links to POs. | Renewal control. | P1 / R3 | Procurement manager | BR-025 | An expiring contract raises a notice before expiry. | M-056 | T |

### 11.4 Inventory and warehouse

| ID | Title | Description | Justification | Pri / Rel | Source (role) | Depends on | Acceptance criteria | Modules | Verif. |
|---|---|---|---|---|---|---|---|---|---|
| BR-033 | Item and category master | Items and services have categories, units and default accounts. | Consistent transactions. | P0 / MVP | Inventory manager | BR-008 | An item in use cannot be deleted, only deactivated. | M-061, M-062 | T |
| BR-034 | Stock ledger | Every stock change is a recorded movement per item and warehouse. | Traceability. | P0 / MVP | Inventory manager | BR-033 | On-hand quantity equals the sum of movements. | M-064, M-073 | T |
| BR-035 | Negative stock control | Issues that would make stock negative are refused unless the company policy allows it. | Accuracy. | P0 / MVP | Inventory manager | OQ-020 | The refusal explains available quantity. | M-064 | T |
| BR-036 | Stock reservation | Confirmed sales orders can reserve stock. | Avoids overselling. | P1 / MVP | Sales manager | BR-034 | Reserved stock is not available to another order. Reservation expires or is released on cancel. | M-065 | T |
| BR-037 | Transfers between locations | Stock moves between warehouses and branches with an in-transit state. | Multi-branch operation. | P0 / MVP | Inventory manager | BR-034 | Source decreases on dispatch. Destination increases on receipt. In-transit stock is visible. | M-066 | T |
| BR-038 | Controlled adjustments | Adjustments need a reason and approval above a threshold. | Prevents shrinkage cover-ups. | P0 / MVP | Inventory manager | BR-006 | An adjustment shows its financial impact before approval. | M-067 | T |
| BR-039 | Inventory valuation | Stock is valued by a configured method and posted to the ledger. | Accurate cost and balance sheet. | P0 / MVP | Finance manager | OQ-009 | Stock valuation reconciles to the inventory control account. | M-072 | T |
| BR-040 | Batch, lot, serial and expiry | Items can be tracked by batch, serial number and expiry date. | Traceability in regulated industries. | P1 / R2 | Inventory manager | BR-034 | Expired stock is blocked from issue. | M-068, M-069, M-070 | T |

### 11.5 Order-to-Cash

| ID | Title | Description | Justification | Pri / Rel | Source (role) | Depends on | Acceptance criteria | Modules | Verif. |
|---|---|---|---|---|---|---|---|---|---|
| BR-041 | Customer master and terms | Customers have contacts, tax data, payment terms and credit limits. | Correct billing. | P0 / MVP | Sales manager | BR-008 | Duplicate customers are flagged on creation. | M-079 | T |
| BR-042 | Quote to invoice flow | Quotations convert to sales orders, deliveries and invoices with full traceability. | Complete commercial cycle. | P0 / MVP | Sales manager | BR-041 | Each document links to its predecessor. | M-080 to M-083 | T |
| BR-043 | Partial delivery and invoicing | Orders can be delivered and invoiced in parts. | Real trading patterns. | P0 / MVP | Sales manager | BR-042 | Delivered and invoiced quantities never exceed ordered quantity. | M-081 to M-083 | T |
| BR-044 | Returns and credit notes | Returns and credit notes link to the original sales documents. | Correct receivables and stock. | P0 / MVP | Sales manager | BR-042 | A credit note cannot exceed the invoiced amount. A return restores stock when goods are accepted. | M-084, M-085 | T |
| BR-045 | Customer receipts | Receipts are allocated to invoices, including partial and advance. | Accurate receivables. | P0 / MVP | Finance manager | BR-020 | Overpayment is held as customer credit. | M-086 | T |
| BR-046 | Credit limit control | Orders that exceed a customer's limit require approval. | Bad-debt control. | P1 / MVP | Finance manager | BR-041 | The block shows exposure and limit. | M-079, M-081 | T |
| BR-047 | Receivable ageing | Overdue receivables are reported by age bucket. | Collections. | P1 / MVP | Finance manager | BR-045 | Ageing totals reconcile to the receivables control account. | M-088 | T |
| BR-048 | Lead and opportunity pipeline | Leads become customers through qualification and opportunity stages. | Sales growth. | P1 / R2 | Sales manager | BR-041 | A converted lead creates a customer without retyping. | M-076 to M-078 | T |

### 11.6 People, projects and enterprise

| ID | Title | Description | Justification | Pri / Rel | Source (role) | Depends on | Acceptance criteria | Modules | Verif. |
|---|---|---|---|---|---|---|---|---|---|
| BR-049 | Single employee record | One employee record serves directory, approvals, self-service and payroll. | Avoids duplicate people data. | P0 / MVP | HR manager | BR-008 | A user can be linked to one employee. Deactivation ends access. | M-007, M-016 | T |
| BR-050 | Attendance, leave and payroll | Attendance and leave feed payroll under company policy. | Accurate pay. | P1 / R2 | HR manager | OQ-006 | Payroll uses approved leave and attendance only. | M-020, M-022, M-023 | T |
| BR-051 | Expense claims | Employees submit claims, approvers decide, finance reimburses. | Expense-to-Reimbursement. | P1 / R2 | Finance manager | BR-006 | An approved claim creates a payable. | M-028, M-039 | T |
| BR-052 | Employee self-service | Employees see and request their own HR items. | HR efficiency. | P1 / R2 | HR manager | BR-049 | An employee sees only their own data. | M-027, M-119 | T |
| BR-053 | Project costing and billing | Time and costs are tracked per project and billed. | Service industries. | P1 / R3 | Project manager | BR-014 | Billed amount links to project cost and time. | M-089 to M-096 | T |
| BR-054 | Asset lifecycle | Assets are acquired, depreciated, transferred and disposed with ledger postings. | Asset-to-Disposal. | P1 / R3 | Finance manager | BR-014 | Depreciation posts per schedule. Disposal posts gain or loss. | M-042, M-110 | T |
| BR-055 | Support and SLA tracking | Support requests are tracked against service levels. | Issue-to-Resolution. | P2 / R3 | Support manager | None | A ticket nearing breach raises an alert. | M-112, M-113 | T |
| BR-056 | External portals | Customers and vendors access their own documents and status. | Self-service. | P1 / R3 | Sales and procurement managers | BR-003 | An external user sees only their own organisation's records. | M-118 | T |
| BR-057 | Production and quality | Manufacturers plan production, consume materials and control quality. | Plan-to-Produce. | P2 / R4 | Operations head | OQ-002 | Material consumption posts to stock and WIP. | M-097 to M-109 | T |

### 11.7 Cross-cutting

| ID | Title | Description | Justification | Pri / Rel | Source (role) | Depends on | Acceptance criteria | Modules | Verif. |
|---|---|---|---|---|---|---|---|---|---|
| BR-058 | Operational reporting | Each MVP module offers standard reports with filters and export. | Day-to-day control. | P1 / MVP | All managers | BR-003 | Reports honour data scope. Export matches the screen. | M-060, M-075, M-088 | T |
| BR-059 | Data retention and privacy | Personal and financial data follow defined retention and privacy rules. | Legal duty. | P0 / MVP | Compliance officer | OQ-010, OQ-022 | Retention periods are configurable. Personal data can be exported and anonymised where law allows. | M-116 | I |
| BR-060 | External integration resilience | Calls to email, banks and gateways are retried and failures are visible and recoverable. | Reliability. | P1 / MVP | IT manager | OQ-017 | A failed call does not lose the business event. It can be retried or cancelled by an authorised user. | M-012 | T |
| BR-061 | Industry configurability | Industry needs are met through configuration and add-on modules, not by forking the core. | Reuse across industries. | P1 / MVP | Product manager | OQ-002 | New fields and statuses can be added by configuration within defined limits. | M-008 | A |
| BR-062 | Service levels | The system meets availability, performance and recovery targets agreed with the sponsor. | Business continuity. | P1 / MVP | Executive sponsor | OQ-015, OQ-016 | Targets are measured in test and production. Values are TBD. | All | T |
| BR-063 | Business intelligence | Cross-module analytics and advanced reports are available. | Decision support. | P1 / R3 | Executive sponsor | BR-058 | A report can combine data from at least two modules within user scope. | M-001, M-120 | D |

## 12. Requirement counts

Counts are re-derived by parsing the tables above and recorded in [Documentation-Progress](../00-Project-Management/Documentation-Progress.md).

## 13. Not yet in this BRD

Delivery stages agreed with the requester: Stage 1 is business requirements and MVP scope, Stage 2 is requirements detail and architecture, Stage 3 is QA and the remaining documents.

| Item | Where it will be done |
|---|---|
| Functional requirements (FR) | FRS, Stage 2 |
| Non-functional requirements (NFR) | NFR document, Stage 2 |
| Technical requirements (TR) and API requirements (API) | Architecture documents, Stage 2 |
| User stories (US) and use cases (UC) | Stage 2 |
| Test cases (TC) | QA documents, Stage 3 |
