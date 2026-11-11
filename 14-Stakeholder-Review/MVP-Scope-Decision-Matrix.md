# MVP Scope Decision Matrix

Phase 3, 2026-11-11. Status of every row: **PENDING**. Nothing here is approved.
Related: [Decision Pack](Stakeholder-Decision-Pack.md), [Decision Papers](Open-Question-Decision-Papers.md), [Approval Register](Approval-Register.md), [Phase 3 Readiness](Phase-3-Readiness-Summary.md).

This matrix supports OQ-003. It is a **proposal** built from the Module Inventory (priority and release), the FRS (features), the Module Feature Breakdown (dependencies) and the RBAC Matrix 8A (security). It does not change scope. Features shown are the titles of up to four FRs of the module, not the full list.
The cut line (8 P1 modules named as first to defer) is a proposal made in Phase 3. The Sprint Plan names no fallback cut, and the P1 priority in the Inventory is the only documented basis.

## 1. Mandatory and recommended MVP modules

| Module | Name | Core features (FRS titles) | Business priority | Dependencies | Security implications | Required integrations | MVP inclusion recommendation | Stakeholder decision |
|---|---|---|---|---|---|---|---|---|
| M-001 | Dashboard & Analytics | Approval inbox; Role dashboard; Permissions for remaining MVP modules | P1 | Many | Standard permission and scope checks (FR-015) | None | Recommended, first to defer if capacity falls short (P1) | PENDING |
| M-002 | Authentication & Authorization | Sign-in; Password policy; Account lockout; Session management | P0 | M-003 | SSO, MFA, lockout, step-up, service accounts, break-glass (FR-144, FR-145, FR-149) | Identity provider for SSO (OQ-023, optional) | Mandatory MVP (P0) | PENDING |
| M-003 | User Management | Invite user; Deactivate user; Assign scope; Link user to employee | P0 | M-004, M-005, M-016 | Role grants follow tiers and four-eyes (FR-131 to FR-134) | None | Mandatory MVP (P0) | PENDING |
| M-004 | Role & Permission Management (RBAC) | Custom roles; Permission catalogue; Default roles; Scoped authorisation | P0 | M-003 | Role tiers, SoD, custom roles (FR-131, FR-142) | None | Mandatory MVP (P0) | PENDING |
| M-005 | Organization & Company Management | Company management; Company isolation | P0 | None | Company isolation (FR-019) | None | Mandatory MVP (P0) | PENDING |
| M-006 | Branch & Department Management | Assign scope; Branch and department tree; Cost centres | P0 | M-005 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-007 | Employee Directory | Directory search | P1 | M-016 | Standard permission and scope checks (FR-015) | None | Recommended, first to defer if capacity falls short (P1) | PENDING |
| M-008 | System Configuration | Numbering series; Print templates; Policy settings; Configurable fields | P0 | M-005 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-009 | Master Data Management | Master data maintenance; Duplicate detection | P0 | M-005 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-010 | Audit Logs & Activity Tracking | Audit capture; Audit search and export; Audit resilience; Audit read separation | P0 | All | Audit read separation (FR-150) | None | Mandatory MVP (P0) | PENDING |
| M-011 | Document Management | Attachments; Attachment access; Attachment immutability | P1 | All | Attachment immutability (FR-147) | None | Recommended, first to defer if capacity falls short (P1) | PENDING |
| M-012 | Notification & Communication Management | In-app notifications; Email notifications; Notification retry; Notification preferences | P0 | M-013 | Standard permission and scope checks (FR-015) | Email provider (ASM-05). SMS TBD | Mandatory MVP (P0) | PENDING |
| M-013 | Workflow & Approval Engine | Approval chain definition; Submit for approval; Decide; No self-approval | P0 | M-003, M-004 | Delegation limits, controlled changes (FR-138, FR-143) | None | Mandatory MVP (P0) | PENDING |
| M-014 | Import & Export Management | Import validate; Import commit; Export; Export control | P0 | M-009 | Export control (FR-148) | Legacy migration files (OQ-012) | Mandatory MVP (P0) | PENDING |
| M-015 | Localization & Language Settings | Locale and time zone; Permissions for remaining MVP modules | P1 | M-005 | Standard permission and scope checks (FR-015) | None | Recommended, first to defer if capacity falls short (P1) | PENDING |
| M-016 | Employee Management | Link user to employee; Employee core record; Reporting line | P0 (core record) | M-006 | Standard permission and scope checks (FR-015) | None | Mandatory MVP, core record only; full HR is R2 | PENDING |
| M-030 | Finance Management (umbrella) | Posting rules | P0 | M-031 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-031 | Chart of Accounts | Chart of accounts; COA template; Account protection | P0 | M-005 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-032 | General Ledger | Posting immutability; Ledger inquiry | P0 | M-031, M-043 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-033 | Accounts Payable | Payable posting; Open items and ageing | P0 | M-054, M-058 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-034 | Accounts Receivable | Receivable posting; Open items and ageing | P0 | M-079, M-082 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-035 | Journal Entries | Journal entry; Journal approval; Posting immutability; Reversal | P0 | M-032 | Journal duty separation (FR-136) | None | Mandatory MVP (P0) | PENDING |
| M-036 | Bank Account Management | Bank accounts | P0 | M-031 | Standard permission and scope checks (FR-015) | Bank files (OQ-017) | Mandatory MVP (P0) | PENDING |
| M-040 | Tax & VAT Management | Tax codes; Tax calculation | P0 | M-009 | Standard permission and scope checks (FR-015) | Tax rules (OQ-007), e-invoicing not in MVP | Mandatory MVP (P0) | PENDING |
| M-041 | Multi-Currency Management | Currencies and rates; Currency conversion; Exchange difference | P0 | M-009 | Standard permission and scope checks (FR-015) | Exchange rate source (OQ-008) | Mandatory MVP (P0) | PENDING |
| M-043 | Financial Period Management | Periods; Period states; Period reopen; Period reopen control | P0 | M-044 | Period reopen control (FR-140) | None | Mandatory MVP (P0) | PENDING |
| M-044 | Fiscal Year Management | Fiscal year | P0 | M-005 | Year-end close step-up | None | Mandatory MVP (P0) | PENDING |
| M-045 | Financial Closing | Close checklist; Year-end close | P0 | M-043, M-044 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-046 | Financial Statements | Trial balance; Income statement; Balance sheet | P0 | M-032 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-047 | Payment & Refund Management | Allocation; Advances and credit; Payment approval; Payment idempotency | P0 | M-033, M-034, M-036 | Three-user payment duty, step-up, hold (FR-135, FR-137) | Bank adapter, payment gateway optional (OQ-017) | Mandatory MVP (P0) | PENDING |
| M-049 | Purchase Requisition (PR) | Requisition entry | P0 | M-061, M-013 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-050 | Purchase Request Approval | Requisition approval | P0 | M-013 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-053 | Purchase Order (PO) | Purchase order creation; Purchase order approval; PO issue and revision; PO cancellation | P0 | M-054, M-049 | PO approver not creator or last editor (SOD-32) | None | Mandatory MVP (P0) | PENDING |
| M-054 | Vendor Management | Vendor creation; Vendor block; Vendor bank change; Vendor bank change control | P0 | M-009 | Vendor bank change control (FR-139) | None | Mandatory MVP (P0) | PENDING |
| M-057 | Goods Receipt Note (GRN) | Goods receipt; Receipt reversal and return | P0 | M-053, M-064 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-058 | Purchase Invoice | Purchase invoice entry; Three-way match; Duplicate invoice check | P0 | M-057 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-059 | Vendor Payments | Allocation; Payment approval; Payment duty separation | P0 | M-047 | Payment release duties (FR-135) | Bank adapter (OQ-017) | Mandatory MVP (P0) | PENDING |
| M-060 | Procurement Reports | Procurement reports | P1 | M-053 | Standard permission and scope checks (FR-015) | None | Recommended, first to defer if capacity falls short (P1) | PENDING |
| M-061 | Product & Service Catalog | Item master | P0 | M-062 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-062 | Product Categories | Categories | P0 | M-031 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-063 | Warehouse Management | Warehouses | P0 | M-006 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-064 | Stock Management | Stock ledger; Availability; Negative stock control; Stock concurrency | P0 | M-073 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-065 | Stock Reservation | Reservation; Permissions for remaining MVP modules | P1 | M-081 | Standard permission and scope checks (FR-015) | None | Recommended, first to defer if capacity falls short (P1) | PENDING |
| M-066 | Stock Transfer | Transfer | P0 | M-063 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-067 | Stock Adjustment | Adjustment | P0 | M-064 | Adjustment approval duties (FR-109, SOD-31) | None | Mandatory MVP (P0) | PENDING |
| M-072 | Inventory Valuation | Valuation | P0 | M-064 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-073 | Goods Issue & Goods Receipt | Receipt reversal and return; Stock ledger; Issue and receipt documents | P0 | M-064 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-075 | Inventory Reports | Inventory reports | P1 | M-064 | Standard permission and scope checks (FR-015) | None | Recommended, first to defer if capacity falls short (P1) | PENDING |
| M-079 | Customer Management | Customer master; Credit check; Credit limit control | P0 | M-009 | Credit limit control (FR-141) | None | Mandatory MVP (P0) | PENDING |
| M-080 | Quotation Management | Quotation; Quote conversion | P0 | M-079, M-061 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-081 | Sales Order Management | Credit check; Quote conversion; Sales order; Order cancellation | P0 | M-080, M-065 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-082 | Sales Invoice Management | Sales invoice | P0 | M-083, M-034 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-083 | Delivery Management | Delivery | P0 | M-081, M-064 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-084 | Sales Returns | Sales return | P0 | M-083 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-085 | Credit Notes | Refund; Credit note | P0 | M-082 | Standard permission and scope checks (FR-015) | None | Mandatory MVP (P0) | PENDING |
| M-086 | Customer Payments | Allocation | P0 | M-047 | Standard permission and scope checks (FR-015) | Bank or gateway for receipts (OQ-017) | Mandatory MVP (P0) | PENDING |
| M-088 | Sales Reports | Sales reporting | P1 | M-082 | Standard permission and scope checks (FR-015) | None | Recommended, first to defer if capacity falls short (P1) | PENDING |
| M-116 | Data Governance | Retention configuration; Personal data handling | P0 (basic), P2 (full) | M-010, M-009 | Privacy, retention (FR-127, FR-129) | None | Mandatory MVP (P0) | PENDING |

Count: 58 modules. Mandatory (P0): 50. Recommended and deferrable (P1): 8 (M-001, M-007, M-011, M-015, M-060, M-065, M-075, M-088). Stakeholder decision: PENDING for all 58.

## 2. Mandatory capabilities versus future-phase capabilities

| Class | Content | Evidence |
|---|---|---|
| Mandatory MVP | The 50 P0 modules above. They support the end-to-end buy, stock, sell and book-keeping cycle (MVP-Strategy section 1) and the authorisation hardening FR-131 to FR-150 | FRS, RTM |
| Recommended MVP, deferrable | M-001, M-007, M-011, M-015, M-060, M-065, M-075, M-088 | Priority P1 in the Inventory |
| Future phase, not in MVP | 62 modules in releases R2 (20), R3 (25), R4 (17) | Inventory. No FRs |
| Future phase, documented | Hire-to-Retire (WF-04), Plan-to-Produce (WF-05), Expense-to-Reimbursement (WF-08), Asset Acquisition-to-Disposal (WF-09), Contract-to-Renewal (WF-13). Documents exist, rules proposed | `05-Workflows-and-Diagrams/Lifecycles/` |
| Excluded by design | Group and cross-company functions, impersonation, payroll, SMS, e-invoicing, native mobile apps, advanced analytics | RBAC 8A.11, OQ-025, BRD out of scope |

## 3. Decisions that change the matrix

| Decision | Effect |
|---|---|
| OQ-003 scope accepted or reduced | Changes the inclusion column |
| OQ-014 budget, team, dates | Decides whether the cut line is needed |
| OQ-037 master data ownership | Changes M-009, M-054, M-061, M-062, M-079 |
| OQ-019 tenant model | Changes M-005 and the data layer |
| OQ-017 integrations | Decides which integration rows are needed at go-live |
| OQ-023 MFA and SSO | Changes M-002 |

## 4. Future-phase modules (names only)

| Module | Name | Priority | Release |
|---|---|---|---|
| M-017 | Recruitment & Applicant Tracking | P2 | R3 |
| M-018 | Employee Onboarding | P2 | R3 |
| M-019 | Employee Offboarding | P2 | R3 |
| M-020 | Attendance Management | P1 | R2 |
| M-021 | Shift & Roster Management | P2 | R3 |
| M-022 | Leave Management | P1 | R2 |
| M-023 | Payroll Management | P1 | R2 |
| M-024 | Benefits & Compensation | P2 | R3 |
| M-025 | Performance Management | P2 | R3 |
| M-026 | Training & Development | P2 | R4 |
| M-027 | Employee Self-Service | P1 | R2 |
| M-028 | Travel & Expense Claims | P1 | R2 |
| M-029 | HR Reports & Analytics | P1 | R2 |
| M-037 | Bank Reconciliation | P1 | R2 |
| M-038 | Budget Management | P1 | R2 |
| M-039 | Expense Management | P1 | R2 |
| M-042 | Fixed Asset Accounting | P1 | R2 |
| M-048 | Financial Audit Reports | P1 | R2 |
| M-051 | Request for Quotation (RFQ) | P1 | R2 |
| M-052 | Vendor Quotation Comparison | P1 | R2 |
| M-055 | Supplier Evaluation | P2 | R3 |
| M-056 | Contract Management | P1 | R3 |
| M-068 | Batch & Lot Tracking | P1 | R2 |
| M-069 | Serial Number Tracking | P2 | R3 |
| M-070 | Expiry Date Management | P1 | R2 |
| M-071 | Reorder Level Management | P1 | R2 |
| M-074 | Barcode & QR Code Management | P2 | R3 |
| M-076 | Customer Relationship Management (CRM) | P1 | R2 |
| M-077 | Lead Management | P1 | R2 |
| M-078 | Opportunity Management | P1 | R2 |
| M-087 | Commission Management | P2 | R3 |
| M-089 | Project Management | P1 | R3 |
| M-090 | Task Management | P1 | R3 |
| M-091 | Timesheet Management | P1 | R3 |
| M-092 | Resource Allocation | P2 | R3 |
| M-093 | Project Budgeting | P2 | R3 |
| M-094 | Project Cost Tracking | P2 | R3 |
| M-095 | Project Billing | P1 | R3 |
| M-096 | Project Progress Tracking | P2 | R3 |
| M-097 | Production Planning | P2 | R4 |
| M-098 | Bill of Materials (BOM) | P2 | R4 |
| M-099 | Work Order Management | P2 | R4 |
| M-100 | Material Requirements Planning (MRP) | P2 | R4 |
| M-101 | Production Scheduling | P2 | R4 |
| M-102 | Work-in-Progress Tracking | P2 | R4 |
| M-103 | Quality Management (QMS) | P2 | R4 |
| M-104 | Quality Inspection | P2 | R4 |
| M-105 | Defect Management | P2 | R4 |
| M-106 | Maintenance Management | P2 | R4 |
| M-107 | Preventive Maintenance | P2 | R4 |
| M-108 | Supply Chain Management | P2 | R4 |
| M-109 | Demand Forecasting | P2 | R4 |
| M-110 | Asset Management | P1 | R3 |
| M-111 | Fleet Management | P2 | R4 |
| M-112 | Helpdesk & Support Management | P2 | R3 |
| M-113 | Ticket & SLA Management | P2 | R3 |
| M-114 | Subscription Management | P2 | R4 |
| M-115 | Compliance Management | P1 | R3 |
| M-117 | Knowledge Base | P2 | R4 |
| M-118 | Customer & Vendor Portal | P1 | R3 |
| M-119 | Employee Self-Service Portal | P1 | R2 |
| M-120 | Business Intelligence & Advanced Reporting | P1 | R3 |
