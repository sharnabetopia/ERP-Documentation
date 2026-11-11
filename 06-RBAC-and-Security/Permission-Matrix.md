# Permission Matrix

Document status: Draft v0.2 (Phase 2 remediation, 2026-11-11). Original v0.1 content kept except as marked Superseded. Generated from the same data as the [RBAC Matrix](RBAC-Matrix.md). Proposed.
One row per permission code. Code format: `module.submodule.action` (ADR-006). Role codes are listed in the RBAC Matrix.

How to use: the API names one permission per endpoint. Look up the code here to see which default roles hold it.
Approve and Reject are separate codes. A role granted approval holds both. The decision endpoint requires `approve` for an approval and `reject` for a rejection or return.

The 16 actions required by FR-013 are all present: create, read, update, delete, approve, reject, export, import, assign, manage, submit, cancel, verify, reconcile, publish, view reports (`view`).

| Permission code | Area | Granted to default roles |
|---|---|---|
| `finance.account.create` | Chart of accounts | FM, AC |
| `finance.account.import` | Chart of accounts | FM |
| `finance.account.manage` | Chart of accounts | FM |
| `finance.account.read` | Chart of accounts | OA, FM, AC, AU |
| `finance.account.update` | Chart of accounts | FM |
| `finance.bank.create` | Bank accounts | AC |
| `finance.bank.manage` | Bank accounts | FM |
| `finance.bank.read` | Bank accounts | FM, AC, AU |
| `finance.bank.update` | Bank accounts | AC |
| `finance.bankrec.reconcile` | Bank reconciliation (R2) (reserved) | FM, AC |
| `finance.currency.create` | Currency and rates | FM |
| `finance.currency.read` | Currency and rates | FM, AC, PM, PO, SM, SE, AU |
| `finance.currency.update` | Currency and rates | FM |
| `finance.journal.approve` | Journals | FM |
| `finance.journal.cancel` | Journals | FM |
| `finance.journal.create` | Journals | FM, AC |
| `finance.journal.manage` | Journals | **Superseded in v0.2** by finance.journal.reverse. Held by none |
| `finance.journal.read` | Journals | FM, AC, AU |
| `finance.journal.reject` | Journals | FM |
| `finance.journal.submit` | Journals | FM, AC |
| `finance.journal.update` | Journals | FM, AC |
| `finance.ledger.read` | Ledger inquiry | FM, AC, AU |
| `finance.openitem.read` | Open items (AP and AR) | FM, AC, PM, SM, AU |
| `finance.payment.approve` | Payments and refunds | FM |
| `finance.payment.create` | Payments and refunds | AC |
| `finance.payment.manage` | Payments and refunds | **Superseded in v0.2** by finance.payment.release, finance.payment.reverse. Held by none |
| `finance.payment.read` | Payments and refunds | FM, AC, AU |
| `finance.payment.reject` | Payments and refunds | FM |
| `finance.payment.submit` | Payments and refunds | AC |
| `finance.period.create` | Periods and close | FM |
| `finance.period.manage` | Periods and close | **Superseded in v0.2** by finance.period.close, finance.period.reopen.request, finance.fiscalyear.close. Held by none |
| `finance.period.read` | Periods and close | FM, AC, AU |
| `finance.postingrule.manage` | Posting rules | FM |
| `finance.postingrule.read` | Posting rules | FM, AC, AU |
| `finance.report.view` | Finance reports | OA, FM, AC, AU |
| `finance.tax.create` | Tax codes | FM |
| `finance.tax.read` | Tax codes | FM, AC, PO, SE, AU |
| `finance.tax.update` | Tax codes | FM |
| `identity.role.assign` | Roles and permissions | **Superseded in v0.2** by identity.role.assign.company, identity.role.assign.branch. Held by none |
| `identity.role.create` | Roles and permissions | SA |
| `identity.role.manage` | Roles and permissions | SA |
| `identity.role.read` | Roles and permissions | SA, OA, BA, AU |
| `identity.role.update` | Roles and permissions | SA |
| `identity.session.manage` | Sessions | SA, OA, BA |
| `identity.user.assign` | Users | SA, OA, BA |
| `identity.user.create` | Users | SA, OA, BA |
| `identity.user.manage` | Users | SA, OA, BA |
| `identity.user.read` | Users | SA, OA, BA, HM, AU |
| `identity.user.update` | Users | SA, OA, BA |
| `inv.adjustment.approve` | Adjustments | FM, IM |
| `inv.adjustment.create` | Adjustments | IM, WS |
| `inv.adjustment.read` | Adjustments | FM, IM, WS, AU |
| `inv.adjustment.reject` | Adjustments | FM, IM |
| `inv.adjustment.submit` | Adjustments | IM, WS |
| `inv.category.create` | Item categories | FM, IM |
| `inv.category.read` | Item categories | FM, AC, PO, IM, AU, RO |
| `inv.category.update` | Item categories | FM, IM |
| `inv.document.create` | Goods issue and receipt documents | IM, WS |
| `inv.document.manage` | Goods issue and receipt documents | WS |
| `inv.document.read` | Goods issue and receipt documents | IM, WS, AU |
| `inv.item.create` | Items | IM |
| `inv.item.import` | Items | IM |
| `inv.item.manage` | Items | IM |
| `inv.item.read` | Items | DM, FM, AC, PM, PO, IM, WS, SM, SE, EM, AU, RO |
| `inv.item.update` | Items | IM |
| `inv.report.view` | Inventory reports | FM, PM, IM, WS, AU |
| `inv.stock.read` | Stock inquiry | FM, AC, PO, IM, WS, SM, SE, AU, RO |
| `inv.transfer.approve` | Transfers | IM |
| `inv.transfer.cancel` | Transfers | IM |
| `inv.transfer.create` | Transfers | IM, WS |
| `inv.transfer.manage` | Transfers | IM, WS |
| `inv.transfer.read` | Transfers | IM, WS, AU |
| `inv.transfer.reject` | Transfers | IM |
| `inv.transfer.submit` | Transfers | IM, WS |
| `inv.valuation.manage` | Valuation | FM |
| `inv.valuation.read` | Valuation | FM, AC, IM, AU |
| `inv.warehouse.create` | Warehouses | IM |
| `inv.warehouse.manage` | Warehouses | IM |
| `inv.warehouse.read` | Warehouses | BA, AC, PO, IM, WS, SM, AU, RO |
| `inv.warehouse.update` | Warehouses | IM |
| `org.branch.create` | Branches | SA, OA |
| `org.branch.manage` | Branches | OA |
| `org.branch.read` | Branches | SA, OA, BA, DM, HM, HE, FM, AC, PM, PO, IM, WS, SM, SE, PJ, EM, AU, RO |
| `org.branch.update` | Branches | SA, OA, BA |
| `org.company.create` | Companies | SA |
| `org.company.read` | Companies | SA, OA, BA, DM, HM, HE, FM, AC, PM, PO, IM, WS, SM, SE, PJ, EM, AU, RO |
| `org.company.update` | Companies | SA, OA |
| `org.costcentre.create` | Cost centres | OA, FM |
| `org.costcentre.manage` | Cost centres | OA |
| `org.costcentre.read` | Cost centres | OA, BA, DM, FM, AC, PM, PO, IM, WS, SM, PJ, AU |
| `org.costcentre.update` | Cost centres | OA, BA, FM |
| `org.department.create` | Departments | SA, OA |
| `org.department.manage` | Departments | OA |
| `org.department.read` | Departments | SA, OA, BA, DM, HM, HE, FM, AC, PM, PO, IM, WS, SM, SE, PJ, EM, AU, RO |
| `org.department.update` | Departments | SA, OA, BA |
| `org.employee.create` | Employees | HM, HE |
| `org.employee.export` | Employees | HM |
| `org.employee.manage` | Employees | HM |
| `org.employee.read` | Employees | SA, OA, BA, DM, HM, HE, FM, AC, PM, PO, IM, WS, SM, SE, PJ, EM, AU, RO |
| `org.employee.update` | Employees | HM, HE |
| `platform.attachment.create` | Attachments | OA, BA, DM, HM, HE, FM, AC, PM, PO, IM, WS, SM, SE, PJ, EM |
| `platform.attachment.delete` | Attachments | EM |
| `platform.attachment.read` | Attachments | SA, OA, BA, DM, HM, HE, FM, AC, PM, PO, IM, WS, SM, SE, PJ, EM, AU, RO |
| `platform.audit.export` | Audit log | SA, AU |
| `platform.audit.read` | Audit log | **Superseded in v0.2** by platform.audit.read.business, platform.audit.read.security, platform.audit.read.own. Held by none |
| `platform.customfield.create` | Custom fields | OA |
| `platform.customfield.manage` | Custom fields | OA |
| `platform.customfield.read` | Custom fields | SA, OA, AU |
| `platform.customfield.update` | Custom fields | OA |
| `platform.export.export` | Export | OA, BA, HM, FM, AC, PM, IM, SM, AU |
| `platform.import.import` | Import | OA, HM, FM, PM, IM, SM |
| `platform.import.read` | Import | OA, HM, FM, PM, IM, SM |
| `platform.integration.manage` | Integration queue | SA, OA |
| `platform.integration.read` | Integration queue | SA, OA |
| `platform.masterdata.create` | Master data | OA, FM, PM, IM |
| `platform.masterdata.manage` | Master data | OA |
| `platform.masterdata.read` | Master data | OA, BA, DM, FM, AC, PM, PO, IM, WS, SM, SE, AU, RO |
| `platform.masterdata.update` | Master data | OA, FM |
| `platform.numbering.manage` | Numbering | SA, OA |
| `platform.numbering.read` | Numbering | SA, OA, FM, AU |
| `platform.portal.publish` | Portal publishing (R3) (reserved) | OA |
| `platform.privacy.manage` | Privacy requests | OA |
| `platform.retention.manage` | Retention rules | SA |
| `platform.retention.read` | Retention rules | SA, OA, AU |
| `platform.settings.manage` | Settings and policies | SA, OA, FM |
| `platform.settings.read` | Settings and policies | SA, OA, FM, AU |
| `platform.template.manage` | Document templates | OA |
| `platform.template.read` | Document templates | OA, FM, AU |
| `proc.grn.create` | Goods receipts | IM, WS |
| `proc.grn.manage` | Goods receipts | IM, WS |
| `proc.grn.read` | Goods receipts | AC, PO, IM, WS, AU |
| `proc.invoice.approve` | Purchase invoices | FM |
| `proc.invoice.create` | Purchase invoices | AC |
| `proc.invoice.read` | Purchase invoices | FM, AC, PM, PO, AU |
| `proc.invoice.reject` | Purchase invoices | FM |
| `proc.invoice.submit` | Purchase invoices | AC |
| `proc.invoice.update` | Purchase invoices | AC |
| `proc.invoice.verify` | Purchase invoices | AC |
| `proc.po.approve` | Purchase orders | FM, PM |
| `proc.po.cancel` | Purchase orders | PM, PO |
| `proc.po.create` | Purchase orders | PM, PO |
| `proc.po.manage` | Purchase orders | PM, PO |
| `proc.po.read` | Purchase orders | FM, AC, PM, PO, IM, WS, AU |
| `proc.po.reject` | Purchase orders | FM, PM |
| `proc.po.submit` | Purchase orders | PM, PO |
| `proc.po.update` | Purchase orders | PM, PO |
| `proc.pr.approve` | Purchase requisitions | DM |
| `proc.pr.cancel` | Purchase requisitions | DM |
| `proc.pr.create` | Purchase requisitions | DM, PJ, EM |
| `proc.pr.read` | Purchase requisitions | DM, FM, PM, PO, PJ, EM, AU |
| `proc.pr.reject` | Purchase requisitions | DM |
| `proc.pr.submit` | Purchase requisitions | DM, PJ, EM |
| `proc.pr.update` | Purchase requisitions | DM, PJ, EM |
| `proc.report.view` | Procurement reports | FM, PM, PO, IM, AU |
| `proc.vendor.approve` | Vendors | PM |
| `proc.vendor.create` | Vendors | PM, PO |
| `proc.vendor.manage` | Vendors | PM |
| `proc.vendor.read` | Vendors | DM, FM, AC, PM, PO, WS, AU |
| `proc.vendor.reject` | Vendors | PM |
| `proc.vendor.submit` | Vendors | PO |
| `proc.vendor.update` | Vendors | PM, PO |
| `sales.creditnote.approve` | Credit notes | FM |
| `sales.creditnote.create` | Credit notes | AC |
| `sales.creditnote.read` | Credit notes | FM, AC, SM, AU |
| `sales.creditnote.reject` | Credit notes | FM |
| `sales.creditnote.submit` | Credit notes | AC |
| `sales.customer.approve` | Customers | SM |
| `sales.customer.create` | Customers | SM, SE |
| `sales.customer.manage` | Customers | SM |
| `sales.customer.read` | Customers | FM, AC, WS, SM, SE, AU, RO |
| `sales.customer.reject` | Customers | SM |
| `sales.customer.submit` | Customers | SE |
| `sales.customer.update` | Customers | SM, SE |
| `sales.delivery.create` | Deliveries | WS |
| `sales.delivery.manage` | Deliveries | WS |
| `sales.delivery.read` | Deliveries | AC, IM, WS, SM, SE, AU |
| `sales.invoice.approve` | Sales invoices | FM |
| `sales.invoice.create` | Sales invoices | AC |
| `sales.invoice.read` | Sales invoices | FM, AC, SM, SE, AU |
| `sales.invoice.reject` | Sales invoices | FM |
| `sales.invoice.submit` | Sales invoices | AC |
| `sales.invoice.update` | Sales invoices | AC |
| `sales.order.approve` | Sales orders | FM, SM |
| `sales.order.cancel` | Sales orders | SM |
| `sales.order.create` | Sales orders | SM, SE |
| `sales.order.read` | Sales orders | FM, AC, WS, SM, SE, AU, RO |
| `sales.order.reject` | Sales orders | FM, SM |
| `sales.order.submit` | Sales orders | SM, SE |
| `sales.order.update` | Sales orders | SM, SE |
| `sales.quotation.approve` | Quotations | SM |
| `sales.quotation.cancel` | Quotations | SM |
| `sales.quotation.create` | Quotations | SM, SE |
| `sales.quotation.manage` | Quotations | SM |
| `sales.quotation.publish` | Quotations | SM, SE |
| `sales.quotation.read` | Quotations | SM, SE, AU, RO |
| `sales.quotation.reject` | Quotations | SM |
| `sales.quotation.submit` | Quotations | SM, SE |
| `sales.quotation.update` | Quotations | SM, SE |
| `sales.report.view` | Sales reports | FM, AC, SM, SE, AU |
| `sales.return.approve` | Sales returns | SM |
| `sales.return.create` | Sales returns | SM, SE |
| `sales.return.manage` | Sales returns | WS |
| `sales.return.read` | Sales returns | WS, SM, SE, AU |
| `sales.return.reject` | Sales returns | SM |
| `workflow.approval.approve` | Approvals | DM, HM, FM, PM, IM, SM |
| `workflow.approval.reject` | Approvals | DM, HM, FM, PM, IM, SM |
| `workflow.chain.create` | Approval chains | OA |
| `workflow.chain.manage` | Approval chains | OA |
| `workflow.chain.read` | Approval chains | SA, OA, FM, PM, AU |
| `workflow.chain.update` | Approval chains | OA |
| `workflow.delegation.create` | Delegation | OA, BA, DM, HM, FM, PM, IM, SM |


## Phase 2 additions (v0.2)

These codes close audit findings E1 to E10 and the four modules with no permissions. A code marked Replaces supersedes the old code, which stays in the table above as Superseded so that older references remain readable.
Role codes are as in the RBAC Matrix.

| Permission code | Area | Granted to default roles | Note |
|---|---|---|---|
| `identity.role.assign.company` | Roles and permissions | OA | Replaces identity.role.assign for Organization Admin. Tier rules apply (BRL-066). |
| `identity.role.assign.branch` | Roles and permissions | BA | Replaces identity.role.assign for Branch Admin. T3 and T4 roles only, own branches only. |
| `identity.role.grant.approve` | Roles and permissions | SA | Second-person approval of grants (BRL-067). Two Super Admin users are required. |
| `identity.user.export` | Users | SA, OA | Step-up required. Audited. |
| `identity.serviceaccount.create` | Service accounts | OA | Requires approval by identity.serviceaccount.approve. |
| `identity.serviceaccount.approve` | Service accounts | SA | Cannot approve approve, release, reverse, reopen or grant scopes (FR-149). |
| `identity.serviceaccount.read` | Service accounts | SA, OA, AU |  |
| `identity.breakglass.release` | Emergency access | SA | Two different Super Admin users release the sealed account (BRL-072). |
| `identity.breakglass.review` | Emergency access | AU | Post-use review. |
| `finance.payment.release` | Payments and refunds | FM | Step-up. Record-level SoD with create and approve (SOD-14, SOD-15). Replaces release through approve. |
| `finance.payment.reverse` | Payments and refunds | FM | Step-up. Needs a different approver (BRL-068). |
| `finance.journal.reverse` | Journals | FM | Creates a reversing journal that still needs approval by another user (BRL-069). |
| `finance.period.close` | Periods and close | FM | Replaces finance.period.manage for close. |
| `finance.period.reopen.request` | Periods and close | FM | Reason required (BRL-071). |
| `finance.period.reopen.approve` | Periods and close | OA | PENDING APPROVAL: approver role is proposed, see OQ-032. Step-up. |
| `finance.fiscalyear.read` | Fiscal year | OA, FM, AC, AU | Closes a gap for M-044. |
| `finance.fiscalyear.close` | Fiscal year | FM | Year-end close. Step-up. |
| `proc.vendor.bank.request` | Vendor bank details | PO, PM | Maker. Bank fields are removed from proc.vendor.update. |
| `proc.vendor.bank.verify` | Vendor bank details | AC | Out-of-band verification (call-back to a known number). |
| `proc.vendor.bank.approve` | Vendor bank details | FM | Checker. Step-up. |
| `sales.customer.creditlimit.request` | Credit limit | SM | Maker. |
| `sales.customer.creditlimit.approve` | Credit limit | FM | Checker. |
| `platform.config.request` | Controlled changes | OA, FM | Maker for approval limits, posting rules, SoD rules, retention rules. |
| `platform.config.approve` | Controlled changes | OA, FM | Checker. Cannot approve a change to the authority the approver holds (FR-138). |
| `platform.sod.override.request` | SoD overrides | OA | Maker. |
| `platform.sod.override.approve` | SoD overrides | SA | Checker. Time-limited. Reported to the Auditor. |
| `platform.attachment.void` | Attachments | OA | Voids an attachment on a submitted, approved or posted document with a reason. |
| `platform.field.unmask` | Sensitive fields | FM, HM | Step-up. Audited (AUD-074). |
| `platform.audit.read.business` | Audit log | OA, BA, FM, AU | Business events in scope. Replaces platform.audit.read. |
| `platform.audit.read.security` | Audit log | SA, AU | Security events. |
| `platform.audit.read.own` | Audit log | SA, OA, BA, DM, HM, HE, FM, AC, PM, PO, IM, WS, SM, SE, PJ, EM, AU, RO | Own sign-in history only. |
| `dashboard.widget.view` | Dashboards | SA, OA, BA, DM, HM, HE, FM, AC, PM, PO, IM, WS, SM, SE, PJ, EM, AU, RO | M-001. Widgets show in-scope data only (FR-051). |
| `dashboard.config.manage` | Dashboards | OA | M-001. |
| `notification.inbox.read` | Notifications | SA, OA, BA, DM, HM, HE, FM, AC, PM, PO, IM, WS, SM, SE, PJ, EM, AU, RO | M-012. |
| `notification.preference.update` | Notifications | SA, OA, BA, DM, HM, HE, FM, AC, PM, PO, IM, WS, SM, SE, PJ, EM | M-012. Non-mandatory notices only. Auditor and Read-only users use the default notices so that they hold no change permission |
| `notification.template.manage` | Notifications | OA | M-012. |
| `notification.queue.manage` | Notifications | SA, OA | M-012. Retry or cancel failed items (FR-128). |
| `platform.locale.read` | Localisation | SA, OA, BA, DM, HM, HE, FM, AC, PM, PO, IM, WS, SM, SE, PJ, EM, AU, RO | M-015. |
| `platform.locale.manage` | Localisation | OA | M-015. |
| `inv.reservation.read` | Stock reservation | FM, IM, WS, SM, SE, AU, RO | M-065. |
| `inv.reservation.create` | Stock reservation | SM, SE | M-065. |
| `inv.reservation.release` | Stock reservation | IM, SM | M-065. |
| `inv.reservation.manage` | Stock reservation | IM | M-065. Overrides and expiry rules. |

## Counts

Recomputed on 2026-11-11 after the Phase 2 additions by a new script that reads this file. The v0.1 figures (211 codes) are no longer valid.

Total active permission codes: 249 (5 superseded codes remain listed as Superseded).

| Last code segment | Codes |
|---|---|
| read | 52 |
| create | 39 |
| manage | 36 |
| update | 27 |
| approve | 22 |
| reject | 15 |
| submit | 13 |
| cancel | 6 |
| view | 5 |
| request | 5 |
| export | 4 |
| import | 3 |
| release | 3 |
| publish | 2 |
| verify | 2 |
| reverse | 2 |
| close | 2 |
| reconcile | 1 |
| assign | 1 |
| delete | 1 |
| company | 1 |
| branch | 1 |
| review | 1 |
| void | 1 |
| unmask | 1 |
| business | 1 |
| security | 1 |
| own | 1 |

## Permissions per role

| Role | Permission codes held |
|---|---|
| SA Super Admin | 45 |
| OA Organization Admin | 73 |
| BA Branch Admin | 28 |
| DM Department Manager | 25 |
| HM HR Manager | 23 |
| HE HR Executive | 13 |
| FM Finance Manager | 104 |
| AC Accountant | 60 |
| PM Procurement Manager | 43 |
| PO Procurement Officer | 34 |
| IM Inventory Manager | 58 |
| WS Warehouse Staff | 40 |
| SM Sales Manager | 58 |
| SE Sales Executive | 36 |
| PJ Project Manager | 16 |
| EM Employee | 17 |
| AU Auditor | 61 |
| RO Read-only User | 18 |

Higher counts do not mean higher risk. Review the SoD table in the RBAC Matrix.
