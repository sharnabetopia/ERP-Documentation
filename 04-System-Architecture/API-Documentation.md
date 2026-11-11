# API Documentation (Proposed Design)

Document status: Draft v0.1 (Stage 2). **Every endpoint below is a proposed API design, not an existing or confirmed interface.**
Paths, names and shapes may change when the technology stack and integration partners are confirmed (OQ-017, OQ-024).
Related: [FRS](../03-Product-and-System-Requirements/FRS.md), [ADR-009](Architecture-Decision-Records.md), [ADR-010](Architecture-Decision-Records.md), [Database Design](Database-Design.md).

API requirement IDs (`API-001` onward) identify one proposed operation each. Each links to the FRs it supports.
The RTM (Stage 3) will map API to FR to test.

## 1. Conventions

| Topic | Convention |
|---|---|
| Base path | `/api/v1`. A breaking change needs a new version (TR-013). |
| Format | JSON, UTF-8. Money, rates and quantities are strings holding exact decimals (ADR-005). Dates are `YYYY-MM-DD`. Instants are ISO 8601 UTC. |
| Authentication | Bearer access token from `/auth/login` or SSO, bound to a server-side session (ADR-013). Public endpoints are marked `public`. |
| Authorisation | Each endpoint names a permission code `module.submodule.action` (ADR-006). The caller's scope is applied to the target record and to every list. A record outside scope returns `404 NOTF-001`, not 403, so existence is not revealed. |
| Tenant and company context | Taken from the session. A `X-Company-Id` header selects the working company among the user's assigned companies. |
| Pagination | `page_size` (default 50, maximum 200) and `cursor`. Response has `items`, `next_cursor`, and `total` only where cheap. |
| Filtering and sorting | `filter[field]=value`, `sort=field,-field`, `q=text`. Only documented fields are accepted. |
| Concurrency | Mutable resources return `version`. Updates send `If-Match: <version>`. Mismatch returns `409 CONF-002`. |
| Idempotency | Endpoints marked `Req` require an `Idempotency-Key` header (ADR-009). `Opt` accepts one. Same key and body returns the stored result. Same key with a different body returns `422 IDEM-001`. |
| Rate limiting | Classes: A authentication (strict), W writes, R reads, X reports, exports and imports (lowest limit). Values TBD (NFR-017). Exceeding returns `429 RATE-001` with `Retry-After`. |
| Correlation | Every response carries `X-Correlation-Id`, also in logs and audit (TR-018). |
| Standard error body | `{ "error": { "code": "FIN-002", "message": "...", "details": [ { "field": "posting_date", "issue": "..." } ], "correlation_id": "..." } }` |
| Long operations | Imports, exports and large reports return `202` with a job resource to poll. |
| Document lifecycle | Actions are sub-resources (`/submit`, `/post`, `/cancel`, `/reverse`). Posted records have no PATCH or DELETE (FR-064). |

Rate classes: A, W, R, X as above. Idem.: `Req` required, `Opt` optional, `-` not applicable.

## 2. Endpoints

### Authentication and sessions (M-002)

| API ID | Method | Path | Purpose | Permission | Idem. | Rate | FR | Error codes |
|---|---|---|---|---|---|---|---|---|
| API-001 | POST | `/api/v1/auth/login` | Sign in with email and password | `public` | - | A | FR-001, FR-003 | AUTH-001, AUTH-002, AUTH-003 |
| API-002 | POST | `/api/v1/auth/mfa/verify` | Complete second factor | `session-pending` | - | A | FR-006 | AUTH-004 |
| API-003 | POST | `/api/v1/auth/refresh` | Exchange refresh token for a new access token | `refresh-token` | - | A | FR-004 | AUTH-005 |
| API-004 | POST | `/api/v1/auth/logout` | End the current session | `authenticated` | - | W | FR-004 |  |
| API-005 | POST | `/api/v1/auth/password/forgot` | Request a reset link | `public` | - | A | FR-005 |  |
| API-006 | POST | `/api/v1/auth/password/reset` | Set a new password with a reset token | `public` | - | A | FR-002, FR-005 | AUTH-006, VAL-001 |
| API-007 | GET | `/api/v1/auth/sso/{provider}/callback` | Complete SSO sign-in | `public` | - | A | FR-007 | AUTH-001 |
| API-008 | GET | `/api/v1/auth/sessions` | List own sessions | `authenticated` | - | R | FR-004 |  |
| API-009 | DELETE | `/api/v1/auth/sessions/{id}` | Revoke a session (own, or any with permission) | `identity.session.manage` | - | W | FR-004 | AUTHZ-001 |

### Users, roles and scope (M-003, M-004)

| API ID | Method | Path | Purpose | Permission | Idem. | Rate | FR | Error codes |
|---|---|---|---|---|---|---|---|---|
| API-010 | GET | `/api/v1/users` | List users in scope | `identity.user.read` | - | R | FR-008 |  |
| API-011 | POST | `/api/v1/users` | Create or invite a user | `identity.user.create` | Opt | W | FR-008, FR-010 | VAL-001, CONF-001, DUTY-001 |
| API-012 | GET | `/api/v1/users/{id}` | Read a user | `identity.user.read` | - | R | FR-008 | NOTF-001 |
| API-013 | PATCH | `/api/v1/users/{id}` | Update a user (version required) | `identity.user.update` | - | W | FR-008, FR-125 | CONF-002 |
| API-014 | POST | `/api/v1/users/{id}/deactivate` | Deactivate and revoke sessions | `identity.user.manage` | - | W | FR-009 |  |
| API-015 | POST | `/api/v1/users/{id}/employee-link` | Link a user to an employee | `identity.user.assign` | - | W | FR-011 | CONF-001 |
| API-016 | POST | `/api/v1/users/{id}/role-assignments` | Assign role with scope and optional validity. Tier and self-grant rules apply, a Pending grant returns 202 | `identity.role.assign.company` or `identity.role.assign.branch` | Opt | W | FR-010, FR-016, FR-017, FR-131, FR-132, FR-133, FR-134 | DUTY-001, VAL-001, TIER-001, SELF-001 |
| API-017 | DELETE | `/api/v1/users/{id}/role-assignments/{aid}` | Remove an assignment | `identity.role.assign.company` or `identity.role.assign.branch` | - | W | FR-016 |  |
| API-018 | GET | `/api/v1/roles` | List roles | `identity.role.read` | - | R | FR-012 |  |
| API-019 | POST | `/api/v1/roles` | Create a custom role | `identity.role.create` | - | W | FR-012 | VAL-001 |
| API-020 | PATCH | `/api/v1/roles/{id}` | Update a custom role | `identity.role.update` | - | W | FR-012 | CONF-002, STATE-001 |
| API-021 | POST | `/api/v1/roles/{id}/retire` | Retire a role | `identity.role.manage` | - | W | FR-012 | STATE-001 |
| API-022 | GET | `/api/v1/permissions` | List the permission catalogue | `identity.role.read` | - | R | FR-013 |  |

### Organisation (M-005, M-006, M-007, M-016)

| API ID | Method | Path | Purpose | Permission | Idem. | Rate | FR | Error codes |
|---|---|---|---|---|---|---|---|---|
| API-023 | GET | `/api/v1/companies` | List companies in scope | `org.company.read` | - | R | FR-018 |  |
| API-024 | POST | `/api/v1/companies` | Create a company | `org.company.create` | - | W | FR-018 | VAL-001 |
| API-025 | PATCH | `/api/v1/companies/{id}` | Update a company | `org.company.update` | - | W | FR-018 | STATE-001, CONF-002 |
| API-026 | GET | `/api/v1/branches` | List branches | `org.branch.read` | - | R | FR-020 |  |
| API-027 | POST | `/api/v1/branches` | Create a branch | `org.branch.create` | - | W | FR-020 | VAL-001 |
| API-028 | PATCH | `/api/v1/branches/{id}` | Update or move a branch | `org.branch.update` | - | W | FR-020 | STATE-001, CONF-002 |
| API-029 | GET | `/api/v1/departments` | List departments | `org.department.read` | - | R | FR-020 |  |
| API-030 | POST | `/api/v1/departments` | Create a department | `org.department.create` | - | W | FR-020 | VAL-001 |
| API-031 | GET | `/api/v1/cost-centres` | List cost centres | `org.costcentre.read` | - | R | FR-021 |  |
| API-032 | POST | `/api/v1/cost-centres` | Create a cost centre | `org.costcentre.create` | - | W | FR-021 | VAL-001 |
| API-033 | GET | `/api/v1/employees` | List and search employees (directory) | `org.employee.read` | - | R | FR-022, FR-023 |  |
| API-034 | POST | `/api/v1/employees` | Create an employee | `org.employee.create` | - | W | FR-023, FR-024 | VAL-001, CONF-001 |
| API-035 | PATCH | `/api/v1/employees/{id}` | Update an employee | `org.employee.update` | - | W | FR-023, FR-024 | CONF-002, VAL-001 |

### Platform services (M-008 to M-014)

| API ID | Method | Path | Purpose | Permission | Idem. | Rate | FR | Error codes |
|---|---|---|---|---|---|---|---|---|
| API-036 | GET | `/api/v1/number-series` | List numbering series | `platform.numbering.read` | - | R | FR-025 |  |
| API-037 | PUT | `/api/v1/number-series/{id}` | Configure a numbering series | `platform.numbering.manage` | - | W | FR-025 | VAL-001 |
| API-038 | GET | `/api/v1/settings` | Read company settings | `platform.settings.read` | - | R | FR-027 |  |
| API-039 | PUT | `/api/v1/settings/{key}` | Change a setting (history kept). Controlled keys are refused, use API-181 | `platform.settings.manage` | - | W | FR-027, FR-138 | VAL-001, CTRL-001 |
| API-040 | GET | `/api/v1/master-data/{type}` | List reference data (units, payment terms, and so on) | `platform.masterdata.read` | - | R | FR-028 |  |
| API-041 | POST | `/api/v1/master-data/{type}` | Create reference data | `platform.masterdata.create` | - | W | FR-028, FR-029 | VAL-001, CONF-001 |
| API-042 | POST | `/api/v1/attachments` | Upload a file and link it to a record | `platform.attachment.create` | Opt | W | FR-033 | FILE-001, FILE-002, AUTHZ-001 |
| API-043 | GET | `/api/v1/attachments/{id}/download` | Download via authorised short-lived link | `platform.attachment.read` | - | R | FR-034 | AUTHZ-001, NOTF-001 |
| API-044 | GET | `/api/v1/notifications` | List own notifications | `notification.inbox.read` | - | R | FR-035 |  |
| API-045 | POST | `/api/v1/notifications/{id}/read` | Mark as read | `notification.inbox.read` | - | W | FR-035 |  |
| API-046 | PUT | `/api/v1/notification-preferences` | Set channel preferences | `authenticated` | - | W | FR-038 |  |
| API-047 | POST | `/api/v1/imports` | Upload a file for validation | `platform.import.import` | Opt | X | FR-048 | FILE-001, VAL-001 |
| API-048 | POST | `/api/v1/imports/{id}/commit` | Commit a validated import | `platform.import.import` | Req | X | FR-049 | STATE-001, VAL-001 |
| API-049 | GET | `/api/v1/imports/{id}/errors` | Get row errors | `platform.import.read` | - | R | FR-048 |  |
| API-050 | POST | `/api/v1/exports` | Request an export (large exports run as jobs). Also needs read permission on the module and step-up for user and bulk exports | `platform.export.export` | Opt | X | FR-050, FR-148 | AUTHZ-001, STEP-001 |
| API-051 | GET | `/api/v1/audit-logs` | Search audit records in scope. Security events need the security permission | `platform.audit.read.business` or `platform.audit.read.security` | - | R | FR-031, FR-150 |  |
| API-052 | POST | `/api/v1/audit-logs/export` | Export audit records | `platform.audit.export` | Opt | X | FR-031 | AUTHZ-001 |
| API-053 | GET | `/api/v1/integration-queue` | List outbound integration items | `platform.integration.manage` | - | R | FR-128 |  |
| API-054 | POST | `/api/v1/integration-queue/{id}/retry` | Retry or cancel a failed item | `platform.integration.manage` | Req | W | FR-128 | STATE-001 |
| API-055 | GET | `/api/v1/retention-policies` | Read retention rules | `platform.retention.read` | - | R | FR-127 |  |
| API-056 | POST | `/api/v1/privacy/anonymisation-requests` | Anonymise personal data of a person | `platform.privacy.manage` | Req | W | FR-129 | RET-001, STATE-001 |
| API-057 | GET | `/api/v1/dashboard` | Role dashboard widgets in scope | `dashboard.widget.view` | - | R | FR-051 |  |

### Approvals (M-013)

| API ID | Method | Path | Purpose | Permission | Idem. | Rate | FR | Error codes |
|---|---|---|---|---|---|---|---|---|
| API-058 | GET | `/api/v1/approval-chains` | List chains | `workflow.chain.read` | - | R | FR-039 |  |
| API-059 | POST | `/api/v1/approval-chains` | Create a chain | `workflow.chain.create` | - | W | FR-039 | VAL-001 |
| API-060 | PATCH | `/api/v1/approval-chains/{id}` | Update a chain | `workflow.chain.update` | - | W | FR-039 | CONF-002 |
| API-061 | GET | `/api/v1/approvals` | Own inbox: pending, decided, delegated | `authenticated` | - | R | FR-047 |  |
| API-062 | POST | `/api/v1/approvals/{id}/decisions` | Approve, reject or return a document | `workflow.approval.approve` | Req | W | FR-041, FR-042, FR-043 | DUTY-002, VAL-001, STATE-001, AUTHZ-001 |
| API-063 | POST | `/api/v1/delegations` | Delegate authority for a period | `workflow.delegation.create` | - | W | FR-044 | VAL-001, DUTY-002 |
| API-064 | DELETE | `/api/v1/delegations/{id}` | Revoke a delegation | `workflow.delegation.create` | - | W | FR-044 |  |

### Finance (M-030 to M-047)

| API ID | Method | Path | Purpose | Permission | Idem. | Rate | FR | Error codes |
|---|---|---|---|---|---|---|---|---|
| API-065 | GET | `/api/v1/accounts` | List chart of accounts | `finance.account.read` | - | R | FR-053 |  |
| API-066 | POST | `/api/v1/accounts` | Create an account | `finance.account.create` | - | W | FR-053 | VAL-001, CONF-001 |
| API-067 | PATCH | `/api/v1/accounts/{id}` | Update or deactivate an account | `finance.account.update` | - | W | FR-053, FR-055 | FIN-005, CONF-002 |
| API-068 | POST | `/api/v1/accounts/template-load` | Load a chart template | `finance.account.import` | Req | W | FR-054 | STATE-001 |
| API-069 | GET | `/api/v1/posting-rules` | List posting rules | `finance.postingrule.read` | - | R | FR-085 |  |
| API-070 | PUT | `/api/v1/posting-rules/{id}` | Set a posting rule | `finance.postingrule.manage` | - | W | FR-085 | VAL-001 |
| API-071 | POST | `/api/v1/fiscal-years` | Create a fiscal year with periods | `finance.period.create` | - | W | FR-056, FR-057 | VAL-001 |
| API-072 | GET | `/api/v1/fiscal-periods` | List periods and states | `finance.period.read` | - | R | FR-057 |  |
| API-073 | POST | `/api/v1/fiscal-periods/{id}/soft-close` | Soft-close a period | `finance.period.close` | - | W | FR-058 | STATE-001 |
| API-074 | POST | `/api/v1/fiscal-periods/{id}/close` | Close a period (checklist must pass) | `finance.period.close` | Req | W | FR-058, FR-060 | FIN-003, FIN-004 |
| API-075 | POST | `/api/v1/fiscal-periods/{id}/reopen` | Superseded in v0.2 by API-173 to API-175. Returns 409 CTRL-001 | `finance.period.reopen.approve` | Req | W | FR-059, FR-140 | VAL-001, STATE-001, CTRL-001 |
| API-076 | GET | `/api/v1/fiscal-periods/{id}/close-checklist` | Show close checklist | `finance.period.read` | - | R | FR-060 |  |
| API-077 | POST | `/api/v1/fiscal-years/{id}/close` | Run year-end close | `finance.fiscalyear.close` | Req | W | FR-061 | FIN-003, FIN-004 |
| API-078 | GET | `/api/v1/journals` | List journals | `finance.journal.read` | - | R | FR-062 |  |
| API-079 | POST | `/api/v1/journals` | Create a draft journal | `finance.journal.create` | Opt | W | FR-062 | FIN-001, VAL-001 |
| API-080 | PATCH | `/api/v1/journals/{id}` | Edit a draft journal | `finance.journal.update` | - | W | FR-062, FR-125 | CONF-002, STATE-001 |
| API-081 | POST | `/api/v1/journals/{id}/submit` | Submit for approval | `finance.journal.submit` | Req | W | FR-062, FR-063 | FIN-001, FIN-002 |
| API-082 | POST | `/api/v1/journals/{id}/post` | Post an approved journal | `finance.journal.approve` | Req | W | FR-064 | FIN-002, STATE-001 |
| API-083 | POST | `/api/v1/journals/{id}/reverse` | Create a reversing journal that still needs approval by another user | `finance.journal.reverse` | Req | W | FR-065, FR-136 | FIN-002, STATE-002, DUTY-002 |
| API-084 | GET | `/api/v1/ledger-entries` | Ledger inquiry with drill-down | `finance.ledger.read` | - | R | FR-066 |  |
| API-085 | GET | `/api/v1/currency-rates` | List rates | `finance.currency.read` | - | R | FR-067 |  |
| API-086 | POST | `/api/v1/currency-rates` | Enter a rate | `finance.currency.create` | - | W | FR-067 | VAL-001, CONF-001 |
| API-087 | GET | `/api/v1/tax-codes` | List tax codes | `finance.tax.read` | - | R | FR-070 |  |
| API-088 | POST | `/api/v1/tax-codes` | Create a tax code | `finance.tax.create` | - | W | FR-070 | VAL-001, CONF-001 |
| API-089 | GET | `/api/v1/bank-accounts` | List bank accounts | `finance.bank.read` | - | R | FR-072 |  |
| API-090 | POST | `/api/v1/bank-accounts` | Create a bank account | `finance.bank.create` | - | W | FR-072 | VAL-001, CONF-001 |
| API-091 | GET | `/api/v1/payments` | List payments and receipts | `finance.payment.read` | - | R | FR-073 |  |
| API-092 | POST | `/api/v1/payments` | Create a payment or receipt with allocations | `finance.payment.create` | Req | W | FR-073, FR-074, FR-076 | FIN-006, VAL-001, IDEM-001 |
| API-093 | POST | `/api/v1/payments/{id}/submit` | Submit for approval | `finance.payment.submit` | Req | W | FR-075 | STATE-001 |
| API-094 | POST | `/api/v1/payments/{id}/release` | Release an approved payment. Releaser differs from preparer and approver. Step-up required | `finance.payment.release` | Req | W | FR-075, FR-076, FR-135, FR-137 | DUTY-002, STATE-001, EXT-001, STEP-001, HOLD-001 |
| API-095 | POST | `/api/v1/payments/{id}/reverse` | Request reversal of a posted payment. A different user approves. Step-up required | `finance.payment.reverse` | Req | W | FR-077, FR-135, FR-137 | STATE-002, FIN-002, DUTY-002, STEP-001 |
| API-096 | POST | `/api/v1/refunds` | Refund against credit note or overpayment | `finance.payment.create` | Req | W | FR-078 | FIN-007, VAL-001 |
| API-097 | GET | `/api/v1/open-items` | List open items and ageing buckets | `finance.openitem.read` | - | R | FR-081 |  |
| API-098 | GET | `/api/v1/reports/trial-balance` | Trial balance | `finance.report.view` | - | X | FR-082 |  |
| API-099 | GET | `/api/v1/reports/income-statement` | Income statement | `finance.report.view` | - | X | FR-083 |  |
| API-100 | GET | `/api/v1/reports/balance-sheet` | Balance sheet | `finance.report.view` | - | X | FR-084 |  |

### Procurement (M-049 to M-060)

| API ID | Method | Path | Purpose | Permission | Idem. | Rate | FR | Error codes |
|---|---|---|---|---|---|---|---|---|
| API-101 | GET | `/api/v1/vendors` | List vendors | `proc.vendor.read` | - | R | FR-086 |  |
| API-102 | POST | `/api/v1/vendors` | Create a vendor (draft) | `proc.vendor.create` | Opt | W | FR-086, FR-029 | VAL-001, CONF-001 |
| API-103 | PATCH | `/api/v1/vendors/{id}` | Update a vendor (bank fields are not accepted) | `proc.vendor.update` | - | W | FR-086, FR-125 | CONF-002 |
| API-104 | POST | `/api/v1/vendors/{id}/activate` | Activate after approval | `proc.vendor.approve` | - | W | FR-086 | DUTY-002, STATE-001 |
| API-105 | POST | `/api/v1/vendors/{id}/block` | Block a vendor | `proc.vendor.manage` | - | W | FR-087 |  |
| API-106 | POST | `/api/v1/vendors/{id}/bank-accounts` | Request bank details. Inactive until verified and approved (API-176 to API-178) | `proc.vendor.bank.request` | - | W | FR-088, FR-139 | DUTY-002 |
| API-107 | POST | `/api/v1/purchase-requisitions` | Create a requisition | `proc.pr.create` | Opt | W | FR-089 | VAL-001 |
| API-108 | PATCH | `/api/v1/purchase-requisitions/{id}` | Edit a draft requisition | `proc.pr.update` | - | W | FR-089, FR-125 | CONF-002, STATE-001 |
| API-109 | POST | `/api/v1/purchase-requisitions/{id}/submit` | Submit for approval | `proc.pr.submit` | Req | W | FR-090 | STATE-001 |
| API-110 | POST | `/api/v1/purchase-requisitions/{id}/convert` | Convert approved lines to a PO | `proc.po.create` | Req | W | FR-091 | STATE-001, PROC-001 |
| API-111 | GET | `/api/v1/purchase-requisitions` | List requisitions | `proc.pr.read` | - | R | FR-089 |  |
| API-112 | POST | `/api/v1/purchase-orders` | Create a PO | `proc.po.create` | Opt | W | FR-091 | VAL-001, PROC-001 |
| API-113 | PATCH | `/api/v1/purchase-orders/{id}` | Edit a draft PO | `proc.po.update` | - | W | FR-091, FR-125 | CONF-002 |
| API-114 | POST | `/api/v1/purchase-orders/{id}/submit` | Submit for approval | `proc.po.submit` | Req | W | FR-092 | STATE-001 |
| API-115 | POST | `/api/v1/purchase-orders/{id}/send` | Record issue to the vendor | `proc.po.manage` | Req | W | FR-093 | STATE-001, EXT-001 |
| API-116 | POST | `/api/v1/purchase-orders/{id}/revise` | Create a revision | `proc.po.update` | Req | W | FR-093 | STATE-001 |
| API-117 | POST | `/api/v1/purchase-orders/{id}/cancel` | Cancel the PO or remaining quantity | `proc.po.cancel` | Req | W | FR-094 | STATE-001 |
| API-118 | GET | `/api/v1/purchase-orders` | List POs | `proc.po.read` | - | R | FR-091 |  |
| API-119 | POST | `/api/v1/goods-receipts` | Post a receipt against a PO | `proc.grn.create` | Req | W | FR-095, FR-104 | PROC-002, STK-002, PROC-003 |
| API-120 | POST | `/api/v1/goods-receipts/{id}/reverse` | Reverse a receipt | `proc.grn.manage` | Req | W | FR-096 | STK-001, STATE-002 |
| API-121 | POST | `/api/v1/purchase-invoices` | Enter a vendor invoice | `proc.invoice.create` | Opt | W | FR-097, FR-099 | PROC-004, VAL-001 |
| API-122 | POST | `/api/v1/purchase-invoices/{id}/match` | Run three-way match | `proc.invoice.verify` | - | W | FR-098 | PROC-005 |
| API-123 | POST | `/api/v1/purchase-invoices/{id}/submit` | Submit for approval | `proc.invoice.submit` | Req | W | FR-098 | PROC-005, STATE-001 |
| API-124 | POST | `/api/v1/purchase-invoices/{id}/post` | Post to payables and ledger | `proc.invoice.approve` | Req | W | FR-079 | FIN-001, FIN-002, STATE-001 |
| API-125 | GET | `/api/v1/reports/procurement/open-pos` | Open purchase orders | `proc.report.view` | - | X | FR-100 |  |
| API-126 | GET | `/api/v1/reports/procurement/spend` | Spend by vendor | `proc.report.view` | - | X | FR-100 |  |

### Inventory (M-061 to M-075)

| API ID | Method | Path | Purpose | Permission | Idem. | Rate | FR | Error codes |
|---|---|---|---|---|---|---|---|---|
| API-127 | GET | `/api/v1/items` | List and search items | `inv.item.read` | - | R | FR-101 |  |
| API-128 | POST | `/api/v1/items` | Create an item | `inv.item.create` | Opt | W | FR-101, FR-029 | VAL-001, CONF-001 |
| API-129 | PATCH | `/api/v1/items/{id}` | Update or deactivate an item | `inv.item.update` | - | W | FR-101, FR-125 | STK-004, CONF-002 |
| API-130 | GET | `/api/v1/item-categories` | List categories | `inv.category.read` | - | R | FR-102 |  |
| API-131 | POST | `/api/v1/item-categories` | Create a category | `inv.category.create` | - | W | FR-102 | VAL-001 |
| API-132 | GET | `/api/v1/warehouses` | List warehouses | `inv.warehouse.read` | - | R | FR-103 |  |
| API-133 | POST | `/api/v1/warehouses` | Create a warehouse | `inv.warehouse.create` | - | W | FR-103 | VAL-001 |
| API-134 | GET | `/api/v1/stock/balances` | On-hand, reserved, in-transit, available | `inv.stock.read` | - | R | FR-105 |  |
| API-135 | GET | `/api/v1/stock/movements` | Stock ledger inquiry | `inv.stock.read` | - | R | FR-104 |  |
| API-136 | POST | `/api/v1/stock-transfers` | Create a transfer | `inv.transfer.create` | Opt | W | FR-108 | VAL-001 |
| API-137 | POST | `/api/v1/stock-transfers/{id}/dispatch` | Dispatch | `inv.transfer.manage` | Req | W | FR-108 | STK-001, STATE-001 |
| API-138 | POST | `/api/v1/stock-transfers/{id}/receive` | Receive in full or in part | `inv.transfer.manage` | Req | W | FR-108 | STATE-001, STK-003 |
| API-139 | POST | `/api/v1/stock-adjustments` | Create an adjustment | `inv.adjustment.create` | Opt | W | FR-109 | VAL-001 |
| API-140 | POST | `/api/v1/stock-adjustments/{id}/submit` | Submit (approval above threshold) | `inv.adjustment.submit` | Req | W | FR-109 | STK-001, STATE-001 |
| API-141 | POST | `/api/v1/stock-documents` | Post goods issue or receipt without PO | `inv.document.create` | Req | W | FR-110 | STK-001, FIN-002 |
| API-142 | POST | `/api/v1/valuation/runs` | Run valuation and post entries | `inv.valuation.manage` | Req | W | FR-111 | FIN-002, FIN-005 |
| API-143 | GET | `/api/v1/reports/inventory/stock-on-hand` | Stock on hand | `inv.report.view` | - | X | FR-112 |  |
| API-144 | GET | `/api/v1/reports/inventory/valuation` | Stock valuation | `inv.report.view` | - | X | FR-112 |  |

### Sales (M-079 to M-088)

| API ID | Method | Path | Purpose | Permission | Idem. | Rate | FR | Error codes |
|---|---|---|---|---|---|---|---|---|
| API-145 | GET | `/api/v1/customers` | List customers | `sales.customer.read` | - | R | FR-114 |  |
| API-146 | POST | `/api/v1/customers` | Create a customer | `sales.customer.create` | Opt | W | FR-114, FR-029 | VAL-001, CONF-001 |
| API-147 | PATCH | `/api/v1/customers/{id}` | Update a customer (credit limit is not accepted, use API-179) | `sales.customer.update` | - | W | FR-114, FR-125, FR-141 | CONF-002, CTRL-001 |
| API-148 | POST | `/api/v1/quotations` | Create a quotation | `sales.quotation.create` | Opt | W | FR-116 | VAL-001 |
| API-149 | POST | `/api/v1/quotations/{id}/send` | Mark as sent | `sales.quotation.manage` | - | W | FR-116 | STATE-001 |
| API-150 | POST | `/api/v1/quotations/{id}/convert` | Convert to a sales order | `sales.order.create` | Req | W | FR-117 | SALES-001, STATE-001 |
| API-151 | POST | `/api/v1/sales-orders` | Create a sales order | `sales.order.create` | Opt | W | FR-118 | VAL-001, SALES-002 |
| API-152 | POST | `/api/v1/sales-orders/{id}/confirm` | Confirm, check credit, reserve stock | `sales.order.submit` | Req | W | FR-115, FR-118, FR-107 | SALES-003, STK-001 |
| API-153 | POST | `/api/v1/sales-orders/{id}/cancel` | Cancel order or remaining quantity | `sales.order.cancel` | Req | W | FR-119 | STATE-001 |
| API-154 | GET | `/api/v1/sales-orders` | List orders | `sales.order.read` | - | R | FR-118 |  |
| API-155 | POST | `/api/v1/deliveries` | Post a delivery | `sales.delivery.create` | Req | W | FR-120 | SALES-004, STK-001 |
| API-156 | POST | `/api/v1/sales-invoices` | Create an invoice from deliveries | `sales.invoice.create` | Opt | W | FR-121 | SALES-005, VAL-001 |
| API-157 | POST | `/api/v1/sales-invoices/{id}/post` | Post to receivables and ledger | `sales.invoice.approve` | Req | W | FR-080 | FIN-001, FIN-002 |
| API-158 | POST | `/api/v1/sales-returns` | Record a return | `sales.return.create` | Opt | W | FR-122 | SALES-006 |
| API-159 | POST | `/api/v1/sales-returns/{id}/receive` | Receive returned goods | `sales.return.manage` | Req | W | FR-122 | STATE-001 |
| API-160 | POST | `/api/v1/credit-notes` | Create and post a credit note | `sales.creditnote.create` | Req | W | FR-123 | SALES-007, FIN-002 |
| API-161 | GET | `/api/v1/reports/sales/summary` | Sales by customer and item | `sales.report.view` | - | X | FR-124 |  |
| API-162 | GET | `/api/v1/reports/sales/receivable-ageing` | Receivable ageing | `sales.report.view` | - | X | FR-124, FR-081 |  |

### Additional platform endpoints added to close PG-08 (M-008, M-015)

| API ID | Method | Path | Purpose | Permission | Idem. | Rate | FR | Error codes |
|---|---|---|---|---|---|---|---|---|
| API-163 | GET | `/api/v1/document-templates` | List print templates | `platform.template.read` | - | R | FR-026 | |
| API-164 | PUT | `/api/v1/document-templates/{id}` | Configure a print template with company legal details | `platform.template.manage` | - | W | FR-026 | VAL-001, CONF-002 |
| API-165 | GET | `/api/v1/me/preferences` | Read own locale, time zone and notification preferences | `platform.locale.read` | - | R | FR-052, FR-038 | |
| API-166 | PUT | `/api/v1/me/preferences` | Set own locale, time zone and display formats | `notification.preference.update` | - | W | FR-052 | VAL-001 |
| API-167 | GET | `/api/v1/custom-fields` | List configured custom fields by entity type | `platform.customfield.read` | - | R | FR-130 | |
| API-168 | POST | `/api/v1/custom-fields` | Add a custom field to a document type within limits | `platform.customfield.create` | - | W | FR-130 | VAL-001, CONF-001 |
| API-169 | PATCH | `/api/v1/custom-fields/{id}` | Change or retire a custom field | `platform.customfield.update` | - | W | FR-130 | CONF-002, STATE-001 |

### Authorisation hardening endpoints (v0.2, M-002, M-003, M-004, M-008, M-011, M-013, M-043, M-054, M-079)

Added by the Phase 2 remediation for FR-131 to FR-150.

| ID | Method | Path | Purpose | Permission | Idem. | Kind | FR | Errors |
|---|---|---|---|---|---|---|---|---|
| API-170 | POST | `/api/v1/role-grant-requests/{id}/approve` | Approve a Pending role grant. Approver is not grantor, grantee or user creator. Step-up required | `identity.role.grant.approve` | Req | W | FR-133 | DUTY-002, STEP-001, STATE-001 |
| API-171 | POST | `/api/v1/role-grant-requests/{id}/reject` | Reject a Pending role grant with reason | `identity.role.grant.approve` | - | W | FR-133 | VAL-001, STATE-001 |
| API-172 | GET | `/api/v1/role-grant-requests` | List grant requests in scope | `identity.role.read` | - | R | FR-133 |  |
| API-173 | POST | `/api/v1/fiscal-periods/{id}/reopen-requests` | Request reopening of a closed period with reason | `finance.period.reopen.request` | Req | W | FR-140 | VAL-001, STATE-001 |
| API-174 | POST | `/api/v1/reopen-requests/{id}/approve` | Approve a reopen request. Approver is not the requester. Step-up required. Opens the window | `finance.period.reopen.approve` | Req | W | FR-140 | DUTY-002, STEP-001, STATE-001 |
| API-175 | POST | `/api/v1/reopen-requests/{id}/reject` | Reject a reopen request with reason | `finance.period.reopen.approve` | - | W | FR-140 | VAL-001, STATE-001 |
| API-176 | POST | `/api/v1/vendors/{id}/bank-accounts/{bid}/verify` | Record out-of-band verification of new bank details | `proc.vendor.bank.verify` | - | W | FR-139 | DUTY-002, STATE-001 |
| API-177 | POST | `/api/v1/vendors/{id}/bank-accounts/{bid}/approve` | Approve verified bank details. Starts the payment hold. Step-up required | `proc.vendor.bank.approve` | Req | W | FR-139 | DUTY-002, STEP-001, STATE-001 |
| API-178 | POST | `/api/v1/vendors/{id}/bank-accounts/{bid}/reject` | Reject bank details with reason | `proc.vendor.bank.approve` | - | W | FR-139 | VAL-001, STATE-001 |
| API-179 | POST | `/api/v1/customers/{id}/credit-limit-requests` | Request a credit limit change | `sales.customer.creditlimit.request` | Req | W | FR-141 | VAL-001 |
| API-180 | POST | `/api/v1/credit-limit-requests/{id}/approve` | Approve a credit limit change | `sales.customer.creditlimit.approve` | Req | W | FR-141 | DUTY-002, STATE-001 |
| API-181 | POST | `/api/v1/config-change-requests` | Request a change to a controlled key (approval limit, posting rule, SoD rule, retention rule, hold period) | `platform.config.request` | Req | W | FR-138 | VAL-001 |
| API-182 | POST | `/api/v1/config-change-requests/{id}/approve` | Approve a controlled change. Approver does not hold the authority being changed. Step-up required | `platform.config.approve` | Req | W | FR-138 | DUTY-002, STEP-001, STATE-001 |
| API-183 | POST | `/api/v1/sod-overrides` | Request a time-limited SoD override | `platform.sod.override.request` | Req | W | FR-142 | VAL-001, SELF-001 |
| API-184 | POST | `/api/v1/sod-overrides/{id}/approve` | Approve an SoD override. Step-up required | `platform.sod.override.approve` | Req | W | FR-142 | DUTY-002, STEP-001, SELF-001 |
| API-185 | POST | `/api/v1/break-glass/releases` | Release the sealed emergency account. Needs two different Super Admin users | `identity.breakglass.release` | Req | W | FR-144 | DUTY-002, STEP-001 |
| API-186 | POST | `/api/v1/attachments/{id}/void` | Void an attachment on a submitted, approved or posted document with reason | `platform.attachment.void` | - | W | FR-147 | VAL-001, STATE-001 |
| API-187 | POST | `/api/v1/service-accounts` | Create a service account request with owner, scopes and expiry | `identity.serviceaccount.create` | Req | W | FR-149 | VAL-001, AUTHZ-001 |
| API-188 | POST | `/api/v1/service-accounts/{id}/approve` | Approve a service account. Refuses forbidden scopes. Step-up required | `identity.serviceaccount.approve` | Req | W | FR-149 | AUTHZ-001, STEP-001 |

### Authorisation hardening endpoints, part 2 (v0.2)

Endpoints that give the new permission codes of FR-146, FR-148 and FR-149 a home.

| ID | Method | Path | Purpose | Permission | Idem. | Kind | FR | Errors |
|---|---|---|---|---|---|---|---|---|
| API-189 | GET | `/api/v1/stock-reservations` | List reservations in scope | `inv.reservation.read` | - | R | FR-107, FR-146 |  |
| API-190 | POST | `/api/v1/stock-reservations` | Reserve stock for a confirmed order line | `inv.reservation.create` | Req | W | FR-107, FR-146 | STK-001 |
| API-191 | POST | `/api/v1/stock-reservations/{id}/release` | Release a reservation | `inv.reservation.release` | - | W | FR-107, FR-146 | STATE-001 |
| API-192 | PUT | `/api/v1/stock-reservations/rules` | Change expiry and override rules | `inv.reservation.manage` | - | W | FR-107, FR-146 | VAL-001 |
| API-193 | PUT | `/api/v1/notification-templates/{id}` | Change a notification template | `notification.template.manage` | - | W | FR-036, FR-146 | VAL-001 |
| API-194 | POST | `/api/v1/integration-queue/{id}/retry` | Retry or cancel a failed outbound item | `notification.queue.manage` | - | W | FR-128, FR-146 | STATE-001 |
| API-195 | PUT | `/api/v1/company/locale-settings` | Set company language, time zone and formats | `platform.locale.manage` | - | W | FR-052, FR-146 | VAL-001 |
| API-196 | PUT | `/api/v1/dashboard/config` | Configure role dashboard widgets | `dashboard.config.manage` | - | W | FR-051, FR-146 | VAL-001 |
| API-197 | GET | `/api/v1/fiscal-years` | List fiscal years | `finance.fiscalyear.read` | - | R | FR-056, FR-146 |  |
| API-198 | POST | `/api/v1/users/exports` | Export users. Step-up required | `identity.user.export` | Opt | X | FR-148, FR-050 | STEP-001, AUTHZ-001 |
| API-199 | POST | `/api/v1/fields/{id}/unmask` | Reveal a masked sensitive field for one record, audited. Step-up required | `platform.field.unmask` | - | R | FR-148, FR-129 | STEP-001, AUTHZ-001 |
| API-200 | GET | `/api/v1/service-accounts` | List service accounts | `identity.serviceaccount.read` | - | R | FR-149 |  |
| API-201 | POST | `/api/v1/break-glass/{id}/review` | Record the Auditor review of an emergency session | `identity.breakglass.review` | - | W | FR-144 | VAL-001, DUTY-002 |
| API-202 | GET | `/api/v1/audit-logs/own` | Read own sign-in and activity history | `platform.audit.read.own` | - | R | FR-150 |  |

## 3. Illustrative examples

These show shape only. Field names are proposals.

### 3.1 Create a payment with allocations (API for FR-073, FR-076)

Request `POST /api/v1/payments`, header `Idempotency-Key: 7f1c...`

```json
{
  "direction": "out",
  "party_type": "vendor",
  "party_id": "b3c1...",
  "bank_account_id": "9a77...",
  "payment_date": "2026-11-20",
  "txn_currency": "USD",
  "amount": "1500.0000",
  "allocations": [
    { "open_item_id": "d4e2...", "amount": "1000.0000" },
    { "open_item_id": "e5f3...", "amount": "400.0000" }
  ]
}
```

Response `201`: the payment with `status: "Draft"`, `unallocated_amount: "100.0000"`, `version: 1`. The 100.0000 stays as unallocated credit (BRL-027).
Errors: `FIN-006` if an allocation exceeds an open amount, `IDEM-001` if the key was used with a different body.

### 3.2 Post a goods receipt (FR-095)

Request `POST /api/v1/goods-receipts`, header `Idempotency-Key`

```json
{
  "po_id": "a1b2...",
  "warehouse_id": "c3d4...",
  "receipt_date": "2026-11-18",
  "lines": [ { "po_line_id": "e5f6...", "quantity": "60.0000", "rejected_quantity": "0.0000" } ]
}
```

Response `201`: GRN number, line quantities, `po_status: "Partly received"`, remaining quantity per line.
Errors: `PROC-003` PO cancelled, `PROC-002` quantity above open plus tolerance, `STK-002` warehouse inactive.

### 3.3 Decide an approval (FR-041, FR-042)

Request `POST /api/v1/approvals/{id}/decisions`, header `Idempotency-Key`

```json
{ "decision": "reject", "reason": "Price above quoted amount" }
```

Response `200`: instance status, next approver if any.
Errors: `DUTY-002` caller created the document, `VAL-001` reject without reason, `STATE-001` instance already decided, `AUTHZ-001` caller lacks authority for the value.

### 3.4 Confirm a sales order (FR-115, FR-118, FR-107)

Request `POST /api/v1/sales-orders/{id}/confirm`

Response `200` with per-line reservation results (`reserved_qty`, `backordered_qty`) or `409 SALES-003` with `details` holding limit and exposure and a link to the approval request.

## 4. Error code catalogue

| Code | HTTP | Meaning |
|---|---|---|
| AUTH-001 | 401 | Invalid credentials or SSO assertion |
| AUTH-002 | 423 | Account locked |
| AUTH-003 | 403 | Password expired, change required |
| AUTH-004 | 401 | Invalid MFA code |
| AUTH-005 | 401 | Invalid or revoked refresh token |
| AUTH-006 | 410 | Reset or activation token expired or already used |
| AUTHZ-001 | 403 | Permission missing for the action |
| NOTF-001 | 404 | Not found, or outside the caller's scope |
| VAL-001 | 422 | Validation failed. Details list each field |
| CONF-001 | 409 | Duplicate of an existing unique value |
| CONF-002 | 409 | Version conflict, record changed since read |
| STATE-001 | 409 | Action not allowed in the current status |
| STATE-002 | 409 | Already reversed |
| DUTY-001 | 409 | Segregation of duties conflict |
| DUTY-002 | 403 | Same user cannot create and approve or approve and release. v0.2: DUTY-001 and DUTY-002 were numbered 001 and 002 under the SOD prefix, renamed because SOD-01 to SOD-32 are segregation rules |
| TIER-001 | 403 | Role tier or scope not allowed for this grantor |
| SELF-001 | 403 | A user cannot grant, approve or override for their own account |
| STEP-001 | 401 | Recent authentication (step-up) required |
| CTRL-001 | 409 | Controlled data: use a change request |
| HOLD-001 | 409 | First payment to a new bank account is on hold |
| FILE-001 | 415 | File type not allowed |
| FILE-002 | 413 | File too large |
| FIN-001 | 422 | Journal or posting not balanced |
| FIN-002 | 422 | Posting date in a closed period |
| FIN-003 | 409 | Close checklist incomplete |
| FIN-004 | 409 | Reconciliation difference blocks close |
| FIN-005 | 409 | Account protected, or posting rule missing |
| FIN-006 | 422 | Allocation exceeds open amount |
| FIN-007 | 422 | Refund exceeds amount received or paid |
| PROC-001 | 409 | Requisition not approved, or vendor not active |
| PROC-002 | 422 | Receipt quantity exceeds open quantity plus tolerance |
| PROC-003 | 409 | PO cancelled or closed |
| PROC-004 | 409 | Duplicate vendor invoice |
| PROC-005 | 409 | Three-way match failed beyond tolerance |
| STK-001 | 409 | Insufficient stock (body shows available quantity). v0.2: STK-001 to STK-004 were the four codes numbered 001 to 004 under the prefix of the system invariants, renamed because INV-01 to INV-15 are system invariants |
| STK-002 | 409 | Warehouse or item inactive |
| STK-003 | 422 | Receipt exceeds quantity sent |
| STK-004 | 409 | Item in use, change not allowed |
| SALES-001 | 409 | Quotation expired or not accepted |
| SALES-002 | 409 | Customer inactive or on hold |
| SALES-003 | 409 | Credit limit exceeded, approval required |
| SALES-004 | 422 | Delivery exceeds ordered quantity plus tolerance |
| SALES-005 | 422 | Invoice exceeds delivered quantity |
| SALES-006 | 422 | Return exceeds delivered quantity |
| SALES-007 | 422 | Credit note exceeds invoiced amount |
| EXT-001 | 202 or 502 | External call failed or queued for retry. Business event stays committed (BRL-062) |
| IDEM-001 | 422 | Idempotency key reused with a different request |
| RET-001 | 409 | Action blocked by retention rule |
| RATE-001 | 429 | Rate limit exceeded |

## 5. Business rules enforced at the API

Each endpoint applies the BRL rules through its FR links. Rules that need data from several modules (credit check, three-way match, closed period, stock availability) run inside the single transaction of the operation (ARD section 6).

## 6. Coverage check

Measured by script on 2026-11-11.

| Check | Result |
|---|---|
| API requirements | 202 (API-001 to API-169 from v0.1, API-170 to API-202 added in v0.2 for FR-131 to FR-150) |
| Error codes used by endpoints and not defined | 0 |
| FRs referenced by at least one endpoint | 114 of 130 (v0.1 figure, recounted in the Final Revalidation Report) |
| FRs with no endpoint of their own | 16 (listed below) |

The 16 remaining FRs are of two kinds. PG-08 is closed by API-163 to API-169.

| Kind | FRs | Handling |
|---|---|---|
| Behaviour inside other operations, no endpoint needed | FR-015 (scoped authorisation), FR-019 (isolation), FR-030, FR-032 (audit capture), FR-036, FR-037 (email and retry, worker behaviour), FR-040 (lock on submit), FR-045 (escalation job), FR-068, FR-069 (currency conversion in posting), FR-071 (tax calculation), FR-106, FR-113 (stock checks), FR-126 (report scope) | Verified through the endpoints that trigger them |
| Reached through an existing endpoint | FR-046 (resubmission is a repeat of `/submit`), FR-014 (default roles are seeded data, readable through `/roles`) | Confirm in review |

## 7. Not covered

| Item | Plan |
|---|---|
| Full request and response schemas per endpoint | Written with the physical design and published as a machine-readable specification (format TBD) |
| Webhooks and outbound integrations | [Integration Architecture](Integration-Architecture.md) |
| APIs for R2 to R4 modules | With each release |
| Rate limit values and idempotency window | OQ-015, NFR-017 |
