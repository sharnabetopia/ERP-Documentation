# Screen Specifications

Document status: Draft v0.1 (Stage 3 part 2). Proposed. **Describes screens and behaviour only. No UI code and no visual mock-ups.**
Related: [Navigation Structure](Navigation-Structure.md), [Form Validation Rules](Form-Validation-Rules.md), [User Journeys](User-Journeys.md), [Accessibility Requirements](Accessibility-Requirements.md), [FRS](../03-Product-and-System-Requirements/FRS.md).

## 1. Standard screen patterns

Every screen group below uses these patterns unless it states an exception.

| Pattern ID | Pattern | Specification |
|---|---|---|
| PAT-01 | List page | Table with the listed columns, filter bar, search box (number, name, reference), sort on every column that is indexed, pagination (default 50 rows, selectable 25, 50, 100, maximum 200), saved filters per user, column chooser, export (permission and scope limited, audited), bulk actions only where the FRS allows, row click opens detail. Total count is shown only when cheap, otherwise "50+" |
| PAT-02 | Detail page | Header with number, status badge, key amounts and action buttons allowed by status and permission. Tabs: Details, Lines, Attachments, Approval history, Related documents, Audit. Related documents link forward and back (PR, PO, GRN, invoice, payment). Read-only once posted |
| PAT-03 | Create and edit form | Sections, required fields marked, inline validation on blur and on submit, a summary of errors at the top with links to fields, save draft, submit, cancel with unsaved-changes warning, version conflict banner (FR-125), keyboard-friendly line grids (add row, tab, delete row) |
| PAT-04 | Approval panel | On the document and in the inbox: document summary, value, history of earlier rounds, comments, Approve, Reject (reason required), Return (reason required), delegated-by label. The creator sees no approve control (FR-042) |
| PAT-05 | Status workflow bar | Shows current status and next allowed actions. Disabled actions explain why on hover and to screen readers |
| PAT-06 | Dialogs | Confirm destructive or irreversible actions (post, reverse, cancel, close period, anonymise) with a clear description of the effect. Focus is trapped and returned on close. Never used for long forms |
| PAT-07 | Reports | Parameter panel, run, results table, drill-down to source, export, large reports run as background jobs with a notification (NFR-002) |
| PAT-08 | Wizard | Used for import and period close. Shows steps, allows going back, shows validation before commit |
| PAT-09 | Money and quantity fields | Right-aligned, decimals per currency, thousand separators by locale, exact decimal values, currency code visible. Paste and keyboard entry supported |
| PAT-10 | Masked fields | Bank accounts and tax ids show masked values. An entitled role can reveal, and each reveal is audited (AUD-074) |

## 2. Screen states (apply to every screen)

| State | Requirement |
|---|---|
| Empty | Explains what the list is, why it is empty (no data or filters too narrow) and offers the primary action if the user may take it. No blank page |
| Loading | Skeleton or progress indicator within 300 ms. Layout does not jump when data arrives. Long loads show elapsed activity |
| Error | Plain message with what failed and what to do, the correlation id, retry button. Business errors show the API message (for example STK-001 with available quantity). No stack traces (SEC-030) |
| No permission | Screen is not in the menu. A direct link shows "not found" (SEC-014) |
| Conflict | Banner when someone else changed the record, with option to reload (CONF-002) |
| Offline or timeout | Message with retry. Unsaved form data is kept in the page where possible. Session expiry shows a sign-in prompt and returns to the page (SEC-009) |
| Read-only | Posted, closed or locked documents show a lock icon and the reason |

## 3. Screen group catalogue (MVP)

Required permission codes follow the API Documentation: list and detail use `<area>.read`, create form `<area>.create`, edit `<area>.update`, other actions as named in the API. Statuses are from the [Data Dictionary](../04-System-Architecture/Data-Dictionary.md).

| ID | Group | Screen group | Permission area | FRs | List columns or content | Filters | Forms and actions | Statuses |
|---|---|---|---|---|---|---|---|---|
| SCR-001 | Home | Dashboard | platform.dashboard | FR-051 | Widgets: pending approvals, open documents by status, key figures by role, recent activity | Period, branch (within scope) | n/a | n/a |
| SCR-002 | Home | Approval inbox | `workflow.approval` | FR-047 | Document, type, requester, value, submitted, age, delegated flag | Type, status (pending, decided, delegated), date | Approve, Reject, Return with reason; open document | Pending, Approved, Rejected, Returned, Escalated |
| SCR-003 | Home | Notifications | platform.notification | FR-035, FR-038 | Message, type, time, read | Unread, type | Mark read, preferences | Queued, Sent, Failed, Read |
| SCR-004 | Home | My profile and preferences | platform.preference | FR-052, FR-038, FR-004 | n/a | n/a | Locale, time zone, formats, notification channels, password change, MFA, active sessions with revoke | n/a |
| SCR-005 | Access | Sign-in, MFA, reset | public | FR-001..FR-007 | n/a | n/a | Email, password, MFA code, forgot password, SSO button, new password | n/a |
| SCR-006 | Administration | Users | `identity.user` | FR-008..FR-011 | Name, email, status, roles, company, last sign-in | Status, role, branch | Invite, edit, deactivate, link employee, revoke sessions | Invited, Active, Suspended, Locked, Deactivated |
| SCR-007 | Administration | Roles and permissions | `identity.role` | FR-012..FR-017 | Role, type (system or custom), users count, status | Type, status | Create from catalogue, retire, assign with scope and validity, SoD warning panel | Draft, Active, Retired |
| SCR-008 | Administration | Companies | `org.company` | FR-018, FR-019 | Code, legal name, country, base currency, status | Status | Create, edit (base currency locked after postings) | Active, Inactive |
| SCR-009 | Administration | Branches, departments, cost centres | `org.branch` | FR-020, FR-021 | Tree view with code, name, head, status | Status | Add child, move (cycle check), deactivate | Active, Inactive |
| SCR-010 | People | Employees and directory | `org.employee` | FR-022..FR-024 | Code, name, job, branch, department, manager, status | Branch, department, status | Create, edit, set manager (cycle check), org chart view | Draft, Active, On leave, Separated |
| SCR-011 | Administration | Numbering and settings | `platform.settings` | FR-025, FR-027 | Series: type, year, format, next value. Settings: key, value, changed by | Type, year | Edit series, edit setting with history | Draft, Active |
| SCR-012 | Administration | Master data | `platform.masterdata` | FR-028, FR-029 | Code, name, status by type (units, payment terms, and so on) | Type, status | Create, deactivate, duplicate warning | Active, Inactive |
| SCR-013 | Administration | Print templates and custom fields | `platform.template` | FR-026, FR-130 | Template or field, entity type, status | Entity type | Edit template, add custom field | Draft, Active |
| SCR-014 | Administration | Approval chains and delegations | `workflow.chain` | FR-039, FR-044 | Document type, value range, scope, steps, status; delegations: from, to, types, dates | Type, status | Create chain with steps, create or revoke delegation | Draft, Active |
| SCR-015 | Administration | Import and export | `platform.import` | FR-048..FR-050 | Batch, template, mode, rows, errors, status | Status, date | Wizard: choose template, upload, validate, view errors, commit mode, commit. Export dialog with filters | Uploaded, Validated, Committed, Failed |
| SCR-016 | Audit | Audit log | `platform.audit` | FR-030..FR-032 | Time, actor, action, entity, before and after, outcome | Actor, entity, action, date, company, branch | Search, drill to record, export (audited) | Immutable |
| SCR-017 | Administration | Integration queue and retention | `platform.integration` | FR-127..FR-129 | Item, partner, attempts, status, error | Status, partner | Retry, cancel; retention rules; privacy requests | Pending, Published, Failed, Dead |
| SCR-018 | Finance | Chart of accounts | `finance.account` | FR-053..FR-055 | Tree: code, name, type, postable, control, status | Type, status | Create, edit, template load, deactivate (delete blocked when used) | Active, Inactive |
| SCR-019 | Finance | Fiscal years, periods and close | `finance.period` | FR-056..FR-061 | Year, period, dates, state | Year, state | Create year, soft close, close with checklist, reopen with reason and step-up, year-end | Future, Open, Soft-closed, Closed |
| SCR-020 | Finance | Journals | `finance.journal` | FR-062..FR-065 | Number, date, narration, total, status, maker | Status, period, maker | Lines grid with balance indicator, submit, approve, post, reverse | Draft, Submitted, Approved, Posted, Rejected, Reversed |
| SCR-021 | Finance | Ledger inquiry | `finance.ledger` | FR-066 | Account, date, document, debit, credit, running balance | Account, period, branch, cost centre | Drill to source document | n/a |
| SCR-022 | Finance | Currency rates and tax codes | `finance.currency` | FR-067..FR-071 | Pair, date, rate; tax code, rate, effective dates | Currency, date | Enter rate, import rates, create tax code | Active, Inactive |
| SCR-023 | Finance | Bank accounts | `finance.bank` | FR-072 | Code, bank, account (masked), currency, ledger account, status | Status | Create, edit, close | Active, Closed |
| SCR-024 | Finance | Payments and receipts | `finance.payment` | FR-073..FR-078 | Number, party, date, amount, unallocated, status | Direction, status, party, date | Allocation grid, advance, submit, approve, release, reverse, refund | Draft, Approved, Released, Failed, Reversed |
| SCR-025 | Finance | Open items and ageing | `finance.openitem` | FR-079..FR-081 | Party, document, due, amount, open, age bucket | Party, age, type | Ageing view, drill to document | Open, Part-paid, Paid |
| SCR-026 | Finance | Trial balance and statements | `finance.report` | FR-082..FR-084 | Account lines with totals | Company, period, branch | Run, drill down, export | n/a |
| SCR-027 | Finance | Posting rules | `finance.postingrule` | FR-085 | Document type, category, accounts | Document type | Set mapping | Active |
| SCR-028 | Procurement | Vendors | `proc.vendor` | FR-086..FR-088 | Code, name, tax id, status, payment terms | Status | Create, submit for approval, activate, block, bank change with independent approval banner | Draft, Pending approval, Active, Blocked |
| SCR-029 | Procurement | Purchase requisitions | `proc.pr` | FR-089, FR-090 | Number, requester, department, needed by, value, status | Status, requester, department | Lines grid, attachments, submit, convert to PO | Draft, Submitted, Approved, Rejected, Converted, Cancelled |
| SCR-030 | Procurement | Purchase orders | `proc.po` | FR-091..FR-094 | Number, vendor, date, total, received %, status | Vendor, status, date | Create from PR or direct, lines with tax, submit, send, revise, cancel, receipt progress bar | Draft, Submitted, Approved, Sent, Partly received, Received, Closed, Cancelled |
| SCR-031 | Procurement | Goods receipts | `proc.grn` | FR-095, FR-096 | Number, PO, warehouse, date, status | PO, warehouse, status | Select PO, enter received and rejected quantities, tolerance indicator, reverse | Draft, Posted, Reversed |
| SCR-032 | Procurement | Purchase invoices | `proc.invoice` | FR-097..FR-099 | Internal no, vendor invoice no, vendor, date, total, match status, status | Vendor, status, match | Entry form with PO and GRN link, match panel (variance by line), hold, submit, post | Draft, Submitted, Held, Approved, Posted, Part-paid, Paid, Cancelled |
| SCR-033 | Procurement | Procurement reports | `proc.report` | FR-100 | Open POs, spend by vendor, receipt variance | Date, vendor, category | Run, export | n/a |
| SCR-034 | Inventory | Items and categories | `inv.item` | FR-101, FR-102 | Code, name, type, category, unit, status | Type, category, status | Create, edit, deactivate, unit conversions | Draft, Active, Inactive |
| SCR-035 | Inventory | Warehouses | `inv.warehouse` | FR-103 | Code, name, branch, status | Branch, status | Create, close (blocked with stock) | Active, Inactive |
| SCR-036 | Inventory | Stock inquiry | `inv.stock` | FR-104, FR-105 | Item, warehouse, on hand, reserved, in transit, available | Item, warehouse, category | Movement history drill-down | n/a |
| SCR-037 | Inventory | Stock transfers | `inv.transfer` | FR-108 | Number, from, to, status | Status, warehouse | Create, dispatch, receive full or partial, cancel | Draft, Dispatched, Partly received, Received, Cancelled |
| SCR-038 | Inventory | Stock adjustments | `inv.adjustment` | FR-109 | Number, warehouse, reason, value impact, status | Status, warehouse | Count entry, reason, impact preview, submit | Draft, Submitted, Approved, Posted, Rejected |
| SCR-039 | Inventory | Goods issue and receipt documents | `inv.document` | FR-110 | Number, type, warehouse, cost centre, status | Type, status | Create and post | Draft, Posted, Reversed |
| SCR-040 | Inventory | Valuation and inventory reports | `inv.valuation` | FR-111, FR-112 | Item, warehouse, quantity, cost, value | Warehouse, date | Run valuation, reports | n/a |
| SCR-041 | Sales | Customers | `sales.customer` | FR-114, FR-115 | Code, name, terms, credit limit, exposure, status | Status | Create, credit limit, on hold | Draft, Active, On hold, Inactive |
| SCR-042 | Sales | Quotations | `sales.quotation` | FR-116, FR-117 | Number, customer, date, valid until, total, status | Status, customer | Lines, discounts, send, convert to order | Draft, Sent, Accepted, Rejected, Expired, Converted |
| SCR-043 | Sales | Sales orders | `sales.order` | FR-115, FR-118, FR-119 | Number, customer, date, total, delivered %, invoiced %, status | Status, customer | Credit and stock indicators, confirm, cancel | Draft, Confirmed, Partly delivered, Delivered, Invoiced, Closed, Cancelled |
| SCR-044 | Sales | Deliveries | `sales.delivery` | FR-120 | Number, order, warehouse, date, status | Status, order | Select order lines, quantities, dispatch | Draft, Dispatched, Delivered, Failed, Cancelled |
| SCR-045 | Sales | Sales invoices | `sales.invoice` | FR-121 | Number, customer, date, total, open, status | Status, customer | Create from deliveries, tax summary, post | Draft, Posted, Part-paid, Paid, Cancelled |
| SCR-046 | Sales | Returns and credit notes | `sales.return` | FR-122, FR-123 | Number, delivery or invoice, status | Status | Record return, receive, disposition, credit note within limit | Requested, Approved, Received, Closed, Rejected |
| SCR-047 | Sales | Sales reports and ageing | `sales.report` | FR-124 | Sales by customer and item, order status, receivable ageing | Date, customer | Run, export | n/a |

Screen groups: 47. Each group normally comprises a list, a detail page and a create or edit form (PAT-01 to PAT-03), plus the special screens named in "Forms and actions".

## 4. Dashboard widgets (MVP, FR-051)

| Widget | Content | Roles (proposed) |
|---|---|---|
| Pending approvals | Count and top 5 items | Approvers |
| My drafts and returned items | Documents needing the user | All creators |
| Open purchase orders | Count, late receipts | Procurement, Inventory, Finance |
| Stock alerts | Items with zero available stock (low stock alerts need reorder levels, R2) | Inventory, Warehouse |
| Open sales orders and ageing | Orders awaiting delivery, overdue invoices | Sales, Finance |
| Cash and open items | Payables and receivables totals | Finance |
| Period status | Current period state and close checklist progress | Finance |
| Integration and job health | Failed items count | Admins |

Widget data respects scope. Charts must have a table alternative (Accessibility Requirements).

## 5. Responsive behaviour

| Width | Layout |
|---|---|
| Desktop (wide) | Sidebar expanded, tables with all columns, side-by-side detail panels |
| Tablet | Sidebar collapses to icons, tables hide low-priority columns (column chooser restores them), forms single column |
| Phone | Navigation in a drawer, tables become stacked cards for the most used lists (approvals, stock, orders), line grids scroll horizontally with the first column pinned. Complex screens (journals, period close, import) are supported but not optimised. A phone-first warehouse scanner flow is not in the MVP (OQ-025) |

Touch targets are at least 44 by 44 CSS pixels. Zoom to 200 percent and text spacing changes do not hide content (Accessibility Requirements).

## 6. Keyboard and productivity features

Shortcut keys for search, create, save, submit. Enter and Tab behaviour in line grids. Last-used filters remembered. Copy a document as a new draft where the FRS allows (not specified yet, noted as gap UX-GAP-01).

## 7. Open points and gaps

| ID | Item |
|---|---|
| UX-GAP-01 | "Copy document" and bulk actions are not in the FRS. Decide before build |
| UX-GAP-02 | Final labels and terminology need business review (glossary) |
| UX-GAP-03 | No visual design, design system or brand guidance exists (OQ-027) |
| UX-GAP-04 | Print layouts for documents depend on template content (FR-026, OQ-027) |
| UX-GAP-05 | Screen groups for R2 to R4 are not specified |
| UX-GAP-06 | Forms for settings, print templates, custom fields, numbering and master data are not detailed in the Form Validation Rules |
