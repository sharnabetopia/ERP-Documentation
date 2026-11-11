# ERP Module Inventory

Document status: Draft v0.1 (Stage 1)
Classification of every item is a **proposal by the Business Analyst**. No priority below is a confirmed sponsor decision (see [Open Questions](../02-Business-Analysis/Open-Questions.md), OQ-001 to OQ-004).

## 1. How to read this document

Module IDs `M-001` to `M-120` follow the numbering of the original scope request.
They are stable and are used in every other document.

**Priority scale**

| Priority | Meaning | Release rule |
|---|---|---|
| P0 | Critical / Must Have. The ERP cannot go live or cannot be audited without it. | MVP |
| P1 | Important / Should Have. Needed for a complete operation, but a manual workaround exists for a limited time. | MVP if capacity allows, otherwise R2 or R3 |
| P2 | Optional / Could Have. Adds value for specific industries or maturity levels. | R3 or later |

**Release scale**

| Release | Theme |
|---|---|
| MVP | Platform foundation plus the commercial backbone: Procure-to-Pay, Order-to-Cash, Record-to-Report, inventory |
| R2 | Workforce and financial depth: HR core, payroll, budgets, bank reconciliation, CRM |
| R3 | Operations breadth: projects, assets, helpdesk, portals, BI |
| R4 | Industry modules: manufacturing, quality, supply chain, fleet |

## 2. Inventory

### A. Core System & Administration

| ID | Module | Priority | Release | Business justification |
|---|---|---|---|---|
| M-001 | Dashboard & Analytics | P1 | MVP (basic), R2 (advanced) | Users need a landing view of pending approvals and key figures. Advanced analytics can follow once data exists. |
| M-002 | Authentication & Authorization | P0 | MVP | No secure access without it. |
| M-003 | User Management | P0 | MVP | Required to onboard any user. |
| M-004 | Role & Permission Management (RBAC) | P0 | MVP | Data must never be open to all users. Required for segregation of duties. |
| M-005 | Organization & Company Management | P0 | MVP | Multi-company is a stated design goal. All data is owned by a company. |
| M-006 | Branch & Department Management | P0 | MVP | Multi-branch is a stated design goal. Approval and data scoping depend on it. |
| M-007 | Employee Directory | P1 | MVP | Lookup of people and reporting lines for approvals. Lightweight. |
| M-008 | System Configuration | P0 | MVP | Numbering, tax, currency and policy settings must be configurable, not hard-coded. |
| M-009 | Master Data Management | P0 | MVP | Shared parties, units, currencies and taxes underpin every transaction. |
| M-010 | Audit Logs & Activity Tracking | P0 | MVP | Financial and compliance traceability. |
| M-011 | Document Management | P1 | MVP (attachments), R3 (full DMS) | Approvals need supporting documents. Versioning and retention can follow. |
| M-012 | Notification & Communication Management | P0 | MVP (email, in-app) | Approvals stall without notifications. |
| M-013 | Workflow & Approval Engine | P0 | MVP | Shared engine for PR, PO, invoice, payment and journal approvals. Building it once avoids per-module rework. |
| M-014 | Import & Export Management | P0 | MVP | Required for data migration and for auditors. |
| M-015 | Localization & Language Settings | P1 | MVP (formats, time zone), R3 (additional languages) | Multi-branch operation needs correct date, number and currency formats. Translation catalogue size is TBD. |

### B. Human Resource Management

| ID | Module | Priority | Release | Business justification |
|---|---|---|---|---|
| M-016 | Employee Management | P0 (core record) | MVP (core), R2 (full) | Approvers and users map to employee records. Full HR file follows with HR release. |
| M-017 | Recruitment & Applicant Tracking | P2 | R3 | Hiring is not on the commercial critical path. |
| M-018 | Employee Onboarding | P2 | R3 | Depends on recruitment and HR core. |
| M-019 | Employee Offboarding | P2 | R3 | Depends on HR core. Access revocation is handled in M-003 for MVP. |
| M-020 | Attendance Management | P1 | R2 | Feeds payroll. |
| M-021 | Shift & Roster Management | P2 | R3 | Needed only by shift-based operations. |
| M-022 | Leave Management | P1 | R2 | Feeds payroll and approval delegation. |
| M-023 | Payroll Management | P1 | R2 | High value but statutory rules are jurisdiction specific and unknown (OQ-006). |
| M-024 | Benefits & Compensation | P2 | R3 | Extends payroll. |
| M-025 | Performance Management | P2 | R3 | Not transactional. |
| M-026 | Training & Development | P2 | R4 | Not transactional. |
| M-027 | Employee Self-Service | P1 | R2 | Reduces HR workload. Overlaps with M-119 (see section 4). |
| M-028 | Travel & Expense Claims | P1 | R2 | Needs finance posting and approvals. Overlaps with M-039. |
| M-029 | HR Reports & Analytics | P1 | R2 | Depends on HR data. |

### C. Finance & Accounting

| ID | Module | Priority | Release | Business justification |
|---|---|---|---|---|
| M-030 | Finance Management (umbrella) | P0 | MVP | Parent capability for M-031 to M-048. Holds finance settings and posting rules. |
| M-031 | Chart of Accounts | P0 | MVP | Foundation of the ledger. |
| M-032 | General Ledger | P0 | MVP | System of record for financial truth. |
| M-033 | Accounts Payable | P0 | MVP | Completes Procure-to-Pay. |
| M-034 | Accounts Receivable | P0 | MVP | Completes Order-to-Cash. |
| M-035 | Journal Entries | P0 | MVP | Manual adjustments and all automated postings. |
| M-036 | Bank Account Management | P0 | MVP | Payments and receipts reference bank accounts. |
| M-037 | Bank Reconciliation | P1 | R2 | Control, but books can run without it in the first period. |
| M-038 | Budget Management | P1 | R2 | Control. Budget-to-Actual needs it. |
| M-039 | Expense Management | P1 | R2 | Needed after HR claims exist. |
| M-040 | Tax & VAT Management | P0 | MVP | Invoices cannot be issued correctly without tax. Jurisdiction rules TBD (OQ-007). |
| M-041 | Multi-Currency Management | P0 | MVP | Multi-company implies more than one currency. Treated as P0 pending OQ-008. |
| M-042 | Fixed Asset Accounting | P1 | R2 | Depreciation postings. Pairs with M-110. |
| M-043 | Financial Period Management | P0 | MVP | Posting control. |
| M-044 | Fiscal Year Management | P0 | MVP | Posting control. |
| M-045 | Financial Closing | P0 | MVP | Period and year-end integrity. |
| M-046 | Financial Statements | P0 | MVP | Primary output of Record-to-Report. |
| M-047 | Payment & Refund Management | P0 | MVP | Shared by vendor payments, customer receipts and refunds. |
| M-048 | Financial Audit Reports | P1 | R2 | Audit support beyond the base audit log. |

### D. Procurement & Vendor Management

| ID | Module | Priority | Release | Business justification |
|---|---|---|---|---|
| M-049 | Purchase Requisition (PR) | P0 | MVP | Start of Procure-to-Pay. |
| M-050 | Purchase Request Approval | P0 | MVP | Spend control through M-013. |
| M-051 | Request for Quotation (RFQ) | P1 | R2 | Competitive sourcing. MVP can create a PO directly. |
| M-052 | Vendor Quotation Comparison | P1 | R2 | Depends on M-051. |
| M-053 | Purchase Order (PO) | P0 | MVP | Commitment document. |
| M-054 | Vendor Management | P0 | MVP | Vendor master and status. |
| M-055 | Supplier Evaluation | P2 | R3 | Depends on transaction history. |
| M-056 | Contract Management | P1 | R3 | Contract-to-Renewal workflow. |
| M-057 | Goods Receipt Note (GRN) | P0 | MVP | Three-way match and stock increase. |
| M-058 | Purchase Invoice | P0 | MVP | AP liability. |
| M-059 | Vendor Payments | P0 | MVP | Closes Procure-to-Pay. |
| M-060 | Procurement Reports | P1 | MVP | Basic open-PO and spend reports. |

### E. Inventory & Warehouse Management

| ID | Module | Priority | Release | Business justification |
|---|---|---|---|---|
| M-061 | Product & Service Catalog | P0 | MVP | Items are referenced by purchasing, sales and stock. |
| M-062 | Product Categories | P0 | MVP | Classification and default accounts. |
| M-063 | Warehouse Management | P0 | MVP | Stock locations. |
| M-064 | Stock Management | P0 | MVP | Quantity on hand with ledger. |
| M-065 | Stock Reservation | P1 | MVP | Prevents overselling. |
| M-066 | Stock Transfer | P0 | MVP | Multi-branch movement. |
| M-067 | Stock Adjustment | P0 | MVP | Corrections and counts. |
| M-068 | Batch & Lot Tracking | P1 | R2 | Industry dependent. |
| M-069 | Serial Number Tracking | P2 | R3 | Industry dependent. |
| M-070 | Expiry Date Management | P1 | R2 | Industry dependent. |
| M-071 | Reorder Level Management | P1 | R2 | Replenishment alerts. |
| M-072 | Inventory Valuation | P0 | MVP | Needed for COGS and balance sheet. Method TBD (OQ-009). |
| M-073 | Goods Issue & Goods Receipt | P0 | MVP | Stock movement documents. |
| M-074 | Barcode & QR Code Management | P2 | R3 | Efficiency, not control. |
| M-075 | Inventory Reports | P1 | MVP | Stock on hand and movement. |

### F. Sales & Customer Management

| ID | Module | Priority | Release | Business justification |
|---|---|---|---|---|
| M-076 | Customer Relationship Management (CRM) | P1 | R2 | Umbrella for M-077, M-078. |
| M-077 | Lead Management | P1 | R2 | Lead-to-Customer workflow. |
| M-078 | Opportunity Management | P1 | R2 | Pipeline view. |
| M-079 | Customer Management | P0 | MVP | Customer master. |
| M-080 | Quotation Management | P0 | MVP | Start of Order-to-Cash. |
| M-081 | Sales Order Management | P0 | MVP | Commitment and reservation trigger. |
| M-082 | Sales Invoice Management | P0 | MVP | AR receivable. |
| M-083 | Delivery Management | P0 | MVP | Goods issue and partial delivery. |
| M-084 | Sales Returns | P0 | MVP | Return-to-Refund. |
| M-085 | Credit Notes | P0 | MVP | Correct invoices after return or dispute. |
| M-086 | Customer Payments | P0 | MVP | Closes Order-to-Cash. |
| M-087 | Commission Management | P2 | R3 | Optional incentive model. |
| M-088 | Sales Reports | P1 | MVP | Basic sales and receivable ageing. |

### G. Project & Operations Management

| ID | Module | Priority | Release | Business justification |
|---|---|---|---|---|
| M-089 | Project Management | P1 | R3 | Needed by service industries. |
| M-090 | Task Management | P1 | R3 | Part of project delivery. |
| M-091 | Timesheet Management | P1 | R3 | Feeds project cost and billing. |
| M-092 | Resource Allocation | P2 | R3 | Planning aid. |
| M-093 | Project Budgeting | P2 | R3 | Depends on M-089 and M-038. |
| M-094 | Project Cost Tracking | P2 | R3 | Depends on M-091. |
| M-095 | Project Billing | P1 | R3 | Depends on AR. |
| M-096 | Project Progress Tracking | P2 | R3 | Reporting. |

### H. Manufacturing & Supply Chain

All modules in this group are industry specific. They are P2 and R4 until a manufacturing customer is confirmed (OQ-002).

| ID | Module | Priority | Release | Business justification |
|---|---|---|---|---|
| M-097 | Production Planning | P2 | R4 | Manufacturing only. |
| M-098 | Bill of Materials (BOM) | P2 | R4 | Manufacturing only. |
| M-099 | Work Order Management | P2 | R4 | Manufacturing only. |
| M-100 | Material Requirements Planning (MRP) | P2 | R4 | Needs BOM, stock and demand. |
| M-101 | Production Scheduling | P2 | R4 | Manufacturing only. |
| M-102 | Work-in-Progress Tracking | P2 | R4 | Manufacturing only. |
| M-103 | Quality Management (QMS) | P2 | R4 | Regulated industries may promote this. |
| M-104 | Quality Inspection | P2 | R4 | Pairs with GRN and production. |
| M-105 | Defect Management | P2 | R4 | Pairs with M-103. |
| M-106 | Maintenance Management | P2 | R4 | Asset-heavy industries. |
| M-107 | Preventive Maintenance | P2 | R4 | Pairs with M-106. |
| M-108 | Supply Chain Management | P2 | R4 | Needs mature transaction data. |
| M-109 | Demand Forecasting | P2 | R4 | Needs history. |

### I. Enterprise & Additional Modules

| ID | Module | Priority | Release | Business justification |
|---|---|---|---|---|
| M-110 | Asset Management | P1 | R3 | Operational asset register. Pairs with M-042. |
| M-111 | Fleet Management | P2 | R4 | Industry specific. |
| M-112 | Helpdesk & Support Management | P2 | R3 | Internal and customer support. |
| M-113 | Ticket & SLA Management | P2 | R3 | Part of helpdesk. Overlaps with M-112. |
| M-114 | Subscription Management | P2 | R4 | Recurring billing for specific models. |
| M-115 | Compliance Management | P1 | R3 | Regulatory tracking. |
| M-116 | Data Governance | P0 (basic), P2 (full) | MVP (basic retention and privacy handling), R3 (full policy tooling) | BR-059 needs retention rules, legal hold and personal data export or anonymisation from the first release. Policy tooling beyond that can follow. Changed from R3 after review gap PG-07. |
| M-117 | Knowledge Base | P2 | R4 | Support aid. |
| M-118 | Customer & Vendor Portal | P1 | R3 | External self-service. |
| M-119 | Employee Self-Service Portal | P1 | R2 | Front end of M-027. |
| M-120 | Business Intelligence & Advanced Reporting | P1 | R3 | Cross-module analytics. |

## 3. Summary counts

| Measure | Count |
|---|---|
| Modules identified | 120 |
| Earliest release MVP (includes MVP-lite items that finish in a later release) | 58 |
| Earliest release R2 | 20 |
| Earliest release R3 | 25 |
| Earliest release R4 | 17 |
| P0 | 50 |
| P1 | 37 |
| P2 | 33 |

Counts were produced by parsing the tables in section 2 (first token of the Release column, first two characters of the Priority column) and must be re-run whenever the tables change.

## 4. Overlaps and ambiguities found in the scope request

| Item | Finding | Proposed handling |
|---|---|---|
| M-016 vs M-007 | Employee Management and Employee Directory share the employee record. | One employee entity. M-007 is a read view of M-016. |
| M-027 vs M-119 | Self-service and the self-service portal are the same capability. | Treat M-119 as the UI channel of M-027. Count as one feature set in requirements. |
| M-028 vs M-039 | Travel claims and expense management overlap. | M-028 captures and approves claims. M-039 owns the finance posting and reimbursement. |
| M-030 vs M-031 to M-048 | M-030 is an umbrella. | M-030 owns finance settings and posting rules only. |
| M-047 vs M-059, M-086 | Payment and refund management overlaps vendor and customer payments. | M-047 is the shared payment engine. M-059 and M-086 are its payable and receivable entry points. |
| M-112 vs M-113 | Helpdesk and ticket/SLA are one domain. | Specify together, count as two modules. |
| M-076 vs M-077, M-078 | CRM is an umbrella. | M-076 owns the shared contact and activity model. |
| M-002 vs M-003, M-004 | Authentication, user and role management are tightly coupled. | Specify as one security domain with three modules. |

## 5. Industry-specific candidates not in the original 120

These are **not** added to the inventory. They are listed so the sponsor can say which, if any, apply (OQ-002).

| Candidate | Typical industry |
|---|---|
| Point of Sale | Retail, hospitality |
| Services scheduling and booking | Service businesses (for example vehicle care) |
| Loyalty and membership | Retail, services |
| Lot-based traceability and recall | Food, pharma |
| Construction progress billing | Construction |
| Student and fee management | Education |
| Patient billing | Healthcare |
| Customs and landed cost | Import and export |
| E-invoicing connectors | Jurisdictions that mandate them |
