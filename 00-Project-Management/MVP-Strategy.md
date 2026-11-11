# MVP Strategy and Scope

Document status: Draft v0.1 (Stage 1). **Proposed, not approved** (OQ-003, OQ-014).
Related: [Module Inventory](../01-ERP-Module-Analysis/ERP-Module-Inventory.md), [Module Dependencies](../01-ERP-Module-Analysis/Module-Dependencies.md), [BRD](../02-Business-Analysis/BRD.md).

## 1. MVP definition

The MVP is the smallest release in which a company can run its **buy, stock, sell and book-keeping cycle** on the ERP with proper control, access restriction and an audit trail.
It is a usable operating system for a trading business, not a demo.

Test of MVP completeness: a company can take a purchase need through payment, receive and sell the stock to a customer through cash receipt, and produce a trial balance, income statement and balance sheet that reconcile, with every step approved, scoped and audited.

## 2. MVP scope

58 modules.

| Area | Modules |
|---|---|
| Platform | M-001 to M-015, M-116 (basic only) |
| People core | M-016 |
| Finance | M-030 to M-036, M-040, M-041, M-043 to M-047 |
| Procurement | M-049, M-050, M-053, M-054, M-057 to M-060 |
| Inventory | M-061 to M-067, M-072, M-073, M-075 |
| Sales | M-079 to M-086, M-088 |

Modules marked "MVP-lite" in the inventory ship a reduced feature set:

| Module | MVP content | Deferred |
|---|---|---|
| M-001 Dashboard | Pending tasks and key figures | Advanced analytics (R2) |
| M-011 Documents | Attachments with access rules | Versioning, retention workflow (R3) |
| M-015 Localisation | Formats, time zone | Extra languages (R3) |
| M-016 Employee | Core record, reporting line | Full HR file (R2) |
| M-116 Data Governance | Retention rules, legal hold, personal data export and anonymisation | Policy tooling and governance workflow (R3) |

## 3. Explicitly out of the MVP

| Out | Release | Reason |
|---|---|---|
| Payroll, attendance, leave, ESS | R2 | Jurisdiction rules unknown. Depends on HR core. |
| Bank reconciliation, budgets, expenses, fixed asset accounting | R2 | Controls with manual workaround. |
| RFQ and quote comparison | R2 | MVP orders directly under approval. |
| Batch, serial, expiry, reorder, barcode | R2, R3 | Industry dependent. |
| CRM, leads, opportunities | R2 | Sales runs from customer and quotation in MVP. |
| Projects, assets, helpdesk, portals, BI | R3 | Breadth, not backbone. |
| Manufacturing, QMS, maintenance, supply chain, fleet, subscriptions | R4 | Industry specific. |
| Native mobile app | TBD | OQ-025 |

## 4. Capabilities the MVP data model must reserve for later releases

See section 3 of [Module Dependencies](../01-ERP-Module-Analysis/Module-Dependencies.md).
Reserving the dimensions now avoids migration of posted financial history later.

## 5. MVP milestones (relative order, no calendar dates)

Dates and sprint counts are TBD until OQ-014 is answered.

| Milestone | Content | Exit criteria |
|---|---|---|
| MVP-A Platform | Auth, users, RBAC, companies, branches, config, master data, audit, notifications, approval engine, import and export, employee core, retention and privacy basics (M-116 basic) | Two companies with isolated data. A user sees only assigned scope. Every change audited. Approval chain configured and exercised. |
| MVP-B Finance core | COA, periods, fiscal year, currency, tax, GL, journals, bank accounts, payments engine, statements | Balanced postings only. Closed period blocks posting. Trial balance reconciles. Maker-checker enforced. |
| MVP-C Inventory and P2P | Catalog, warehouse, stock ledger, transfers, adjustments, valuation, PR, PO, vendor, GRN, purchase invoice, AP, vendor payment | Full P2P with partial receipt, duplicate invoice block and three-way match. Stock never negative under default policy. |
| MVP-D O2C and release | Customer, quote, SO, reservation, delivery, sales invoice, AR, returns, credit notes, receipts, reports, hardening | Full O2C with partial delivery and returns. Receivable ageing reconciles. All release gates pass. |

## 6. MVP release gates (proposed)

| Gate | Criterion |
|---|---|
| G1 | All P0 business requirements for MVP modules have passing tests. |
| G2 | No open Critical or High defects. Medium defects have an agreed plan. |
| G3 | Data isolation tests pass for company, branch and role scope. |
| G4 | Security test plan executed, with no open High findings. |
| G5 | Financial integrity checks pass: trial balance balances, sub-ledgers reconcile to control accounts, stock valuation reconciles to ledger. |
| G6 | Performance targets from the NFR met (targets TBD, OQ-015, OQ-028). |
| G7 | Backup restore rehearsed and recovery targets met. |
| G8 | UAT signed by business owners. |
| G9 | Data migration rehearsal reconciles (if migration is in scope, OQ-012). |

## 7. Success measures (targets TBD after baseline)

| Measure | Target |
|---|---|
| Share of in-scope documents with approval record | 100 percent |
| Share of postings with audit record | 100 percent |
| Trial balance imbalance | Zero |
| Duplicate vendor invoices paid | Zero |
| Month-end close time | Better than baseline (TBD) |

## 8. MVP risks

See [Risk Register](Risk-Register.md): R-001, R-003, R-004, R-005, R-006, R-009.
