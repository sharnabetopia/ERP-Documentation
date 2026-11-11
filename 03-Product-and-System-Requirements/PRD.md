# Product Requirements Document (PRD)

Document status: Draft v0.2 (Phase 2: counts corrected). Proposed. No confirmed customer or market data.
The PRD states the product view. Requirements live in the [BRD](../02-Business-Analysis/BRD.md), [FRS](FRS.md) and [NFR](NFR.md) and are not repeated here.

## 1. Product summary

A configurable, multi-company, multi-branch ERP. Customers run purchasing, stock, sales and accounting in one system with approvals, scoped access and a full audit trail. Industry needs are met by configuration and add-on modules.

## 2. Target users

| Persona (proposed) | Default role | Main jobs |
|---|---|---|
| Finance lead | Finance Manager | Control postings, close periods, approve payments |
| Accountant | Accountant | Enter and match invoices, journals, payments |
| Buyer | Procurement Officer | Raise POs, manage vendors |
| Storekeeper | Warehouse Staff | Receive, issue, transfer, count |
| Sales rep | Sales Executive | Quote, order, deliver |
| Approver | Department Manager | Decide requests quickly |
| Administrator | Organization Admin | Users, roles, structure, settings |
| Auditor | Auditor | Inspect without changing |

## 3. Value proposition (proposed, to validate)

1. One record of money, stock and commitments per company.
2. Control built in: approvals, maker-checker, scoped access, period lock.
3. Grows by company, branch and module without rebuild.

## 4. Product scope by release

| Release | Content | Source |
|---|---|---|
| MVP | 58 modules, 150 FRs (130 plus 20 authorisation hardening FRs added in v0.2), 101 stories | [MVP Strategy](../00-Project-Management/MVP-Strategy.md) |
| R2 | HR core, payroll, bank reconciliation, budgets, CRM, RFQ, batch and expiry | [Roadmap](../00-Project-Management/Project-Roadmap.md) |
| R3 | Projects, assets, helpdesk, portals, BI, contracts | Roadmap |
| R4 | Manufacturing, quality, maintenance, supply chain, fleet, subscriptions | Roadmap |

## 5. Product principles

1. Control before convenience: no shortcut around approval, scope or audit.
2. Documents link forward and back.
3. Posted records never change. Corrections are new documents.
4. Configure first. Customise last.
5. Every screen works with keyboard and meets accessibility targets.

## 6. Product success measures

See [MVP Strategy](../00-Project-Management/MVP-Strategy.md) section 7. Targets remain TBD until the As-Is baseline exists.

## 7. Out of scope and open points

Out of scope is in the BRD section 6.3. Open product questions: OQ-002 (industry), OQ-005 (single customer or SaaS), OQ-011 (languages), OQ-025 (mobile).

## 8. Dependencies on later documents

RBAC matrices and workflows are in folders 05 and 06 (draft). UI and UX specifications are in folder 07.
