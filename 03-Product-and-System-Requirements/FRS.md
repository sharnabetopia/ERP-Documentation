# Functional Requirements Specification (FRS)

Document status: Draft v0.1 (Stage 2). Proposed.
Scope: MVP modules (58). Post-MVP functional requirements are written before each release (see section 9).
Related: [BRD](../02-Business-Analysis/BRD.md), [Business Rules](../02-Business-Analysis/Business-Rules.md), [NFR](NFR.md), [User Stories](User-Stories.md), [Use Cases](Use-Cases.md).

## 1. Conventions

| Item | Rule |
|---|---|
| Wording | "shall" means mandatory for the stated priority. |
| Business justification | Inherited from the linked BR. Source stakeholder is the BR source. |
| Dependencies | The linked modules (see [Module Dependencies](../01-ERP-Module-Analysis/Module-Dependencies.md)) and any FR named in the text. |
| Priority | P0 or P1, aligned with the BRD. All FRs are MVP. |
| Verification | T test, D demonstration, I inspection, A analysis. |
| Acceptance | Summary here. Given/When/Then detail is in [User Stories](User-Stories.md) and [Acceptance Criteria](Acceptance-Criteria.md). |
| Status | All Proposed. Items depending on an open question name it. |

## 2. Platform and administration

| ID | Title | Requirement | Module | BR | BRL | Pri | Acceptance summary | Verif. |
|---|---|---|---|---|---|---|---|---|
| FR-001 | Sign-in | The system shall authenticate a user by email and password and start a session. | M-002 | BR-004 | | P0 | Valid credentials open a session. Invalid credentials give a generic error. | T |
| FR-002 | Password policy | The system shall enforce a configurable password policy (length, complexity, history, expiry). Defaults TBD. | M-002 | BR-004 | | P0 | A password breaking the policy is refused with the reason. | T |
| FR-003 | Account lockout | The system shall lock an account after a configurable number of consecutive failed sign-ins and allow unlock by an administrator or by time. | M-002 | BR-004 | BRL-005 | P0 | The Nth failure locks the account. A locked account cannot sign in even with the right password. | T |
| FR-004 | Session management | The system shall expire idle sessions, limit session lifetime, and let a user and an administrator revoke sessions. | M-002 | BR-004 | BRL-006 | P0 | Idle session ends. A revoked session is refused at its next request. | T |
| FR-005 | Password reset | The system shall let a user reset a password through a time-limited, single-use link sent to their registered email. | M-002 | BR-004 | | P0 | A used or expired link is refused. The response does not reveal whether the email exists. | T |
| FR-006 | Multi-factor authentication | The system shall support a second factor (time-based code) and let a company require it by policy. | M-002 | BR-004 | | P1 | Required users cannot reach the application until they complete MFA. Depends on OQ-023. | T |
| FR-007 | Single sign-on | The system shall support sign-in through a corporate identity provider using a standard protocol. Protocol TBD. | M-002 | BR-004 | | P1 | A user from the configured provider signs in and maps to an existing user. Depends on OQ-023. | T |
| FR-008 | Invite user | The system shall let an authorised administrator create or invite a user with name, unique email, company, role and scope. | M-003 | BR-003 | BRL-002 | P0 | An invited user activates through a link. A duplicate email in a tenant is refused. | T |
| FR-009 | Deactivate user | The system shall deactivate a user, revoke their sessions, and keep their history. | M-003 | BR-004, BR-049 | BRL-055, BRL-015 | P0 | A deactivated user cannot sign in. Past records still show the name. | T |
| FR-010 | Assign scope | The system shall assign each user to one or more companies and, per company, to branches and departments. | M-003, M-006 | BR-002, BR-003 | BRL-002, BRL-003 | P0 | A user acts only in assigned companies. | T |
| FR-011 | Link user to employee | The system shall link a user to at most one employee record. | M-003, M-016 | BR-049 | BRL-054 | P0 | A second link attempt is refused. | T |
| FR-012 | Custom roles | The system shall let an authorised administrator create, edit and retire custom roles from the permission catalogue. System roles are read-only. | M-004 | BR-003 | BRL-004 | P0 | A role in use cannot be deleted, only retired. | T |
| FR-013 | Permission catalogue | The system shall maintain a catalogue of permissions by module, submodule and action (create, read, update, delete, approve, reject, export, import, assign, manage, submit, cancel, verify, reconcile, publish, view reports). | M-004 | BR-003 | BRL-004 | P0 | Each module action maps to a catalogue entry. | I |
| FR-014 | Default roles | The system shall provide the 18 default roles with an initial permission set to be approved. | M-004 | BR-003 | BRL-004 | P0 | The 18 roles exist after setup. The matrix is a Stage 2 RBAC document. | I |
| FR-015 | Scoped authorisation | The system shall allow an action only if the role grants the permission and the record is inside the user's company, branch, department and ownership scope. Default is deny. | M-004 | BR-003 | BRL-003, BRL-004 | P0 | A user with permission but wrong scope is refused. A refusal is logged. | T |
| FR-016 | Temporary access | The system shall let an authorised administrator grant a role for a bounded period with a reason, and expire it automatically. | M-004 | BR-003 | BRL-059 | P1 | Access ends at the end time without manual action. | T |
| FR-017 | Segregation conflict check | The system shall warn or block when one user holds conflicting permissions defined in a conflict list (for example create vendor and approve payment). | M-004 | BR-003, BR-021 | BRL-057, BRL-058 | P0 | A conflicting assignment is blocked, or needs a recorded override per setting. | T |
| FR-018 | Company management | The system shall create and maintain companies with legal name, country, base currency and tax identifiers. | M-005 | BR-001 | BRL-001 | P0 | Base currency cannot change once postings exist. | T |
| FR-019 | Company isolation | The system shall ensure no read or write crosses companies except through explicit, audited group functions. | M-005 | BR-001 | BRL-001, BRL-002 | P0 | A user of company A cannot access company B data by UI, API, report or export. | T |
| FR-020 | Branch and department tree | The system shall maintain branches and departments as hierarchies with unique codes within a company. | M-006 | BR-002 | BRL-015 | P0 | Circular parents are refused. A unit in use cannot be deleted. | T |
| FR-021 | Cost centres | The system shall maintain cost centres linked to branch or department for posting and reporting. | M-006 | BR-002 | | P1 | A posting line can carry a cost centre. | T |
| FR-022 | Directory search | The system shall let a user search the employee directory and see an organisation chart limited to fields their role permits. | M-007 | BR-049 | BRL-056 | P1 | Restricted fields are hidden. | T |
| FR-023 | Employee core record | The system shall maintain employee code, name, job, branch, department, manager and status. | M-016 | BR-049 | BRL-054 | P0 | Employee code is unique per company. | T |
| FR-024 | Reporting line | The system shall record a manager per employee and refuse cycles. | M-016 | BR-049, BR-006 | | P0 | A cycle is refused. | T |
| FR-025 | Numbering series | The system shall generate unique sequential document numbers per company, document type and fiscal year in a configurable format. | M-008 | BR-012 | BRL-014 | P0 | 100 concurrent requests yield 100 distinct numbers. | T |
| FR-026 | Print templates | The system shall let an administrator configure document templates with company legal details. | M-008 | BR-012 | | P1 | A printed document shows company details. Depends on OQ-027. | D |
| FR-027 | Policy settings | The system shall hold company policy settings (tolerances, approval thresholds, negative stock flag, rounding) with change history. | M-008 | BR-006, BR-061 | BRL-035, BRL-036, BRL-040 | P0 | A changed setting applies to new documents only and is audited. | T |
| FR-028 | Master data maintenance | The system shall maintain units, currencies, tax codes, payment terms and similar reference data with soft deletion. | M-009 | BR-008 | BRL-015 | P0 | Used values can be deactivated, not deleted. | T |
| FR-029 | Duplicate detection | The system shall warn when a new vendor, customer or item matches an existing record on configured keys. | M-009 | BR-008 | | P0 | A match shows the candidate before saving. | T |
| FR-030 | Audit capture | The system shall record create, update, status change, approval, posting, cancellation, permission change, sign-in and export events with actor, time, scope and before and after values. | M-010 | BR-005 | BRL-007 | P0 | Each listed event yields one record. | T |
| FR-031 | Audit search and export | The system shall let permitted users search, filter and export audit records within their scope. | M-010 | BR-005 | BRL-007 | P0 | No role can edit or delete audit records. | T |
| FR-032 | Audit resilience | The system shall not complete a business change without its audit record, and shall not lose either on failure. | M-010 | BR-005 | BRL-007 | P0 | A forced failure leaves neither or both. | T |
| FR-033 | Attachments | The system shall attach files to records with a file type whitelist, size limit and malware screening. Limits TBD. | M-011 | BR-010 | BRL-060 | P1 | A disallowed type is refused. | T |
| FR-034 | Attachment access | The system shall apply the parent record's access rules to attachments. | M-011 | BR-010 | BRL-060 | P1 | A user without access to the record cannot fetch the file by direct link. | T |
| FR-035 | In-app notifications | The system shall show notifications in an inbox with read state. | M-012 | BR-007 | | P0 | A pending approval appears in the approver inbox. | T |
| FR-036 | Email notifications | The system shall send email from managed templates. | M-012 | BR-007 | | P0 | A template variable missing at send time is flagged, not sent blank. | T |
| FR-037 | Notification retry | The system shall retry failed notifications a configurable number of times and flag permanent failures. | M-012 | BR-007, BR-060 | BRL-061 | P0 | A provider outage does not lose notifications. | T |
| FR-038 | Notification preferences | The system shall let a user choose channels for non-mandatory notices. | M-012 | BR-007 | | P1 | Mandatory approval notices cannot be switched off. | T |
| FR-039 | Approval chain definition | The system shall let an authorised administrator define approval chains by document type, value range, company, branch and department. | M-013 | BR-006 | BRL-009 | P0 | A chain needs at least one eligible approver. | T |
| FR-040 | Submit for approval | The system shall start an approval instance when a document is submitted and lock the document from edits. | M-013 | BR-006 | | P0 | A submitted document cannot be edited. | T |
| FR-041 | Decide | The system shall let an approver approve, reject or return a document. Reject and return need a reason. | M-013 | BR-006 | BRL-010 | P0 | A rejection without reason is refused. | T |
| FR-042 | No self-approval | The system shall prevent a user approving a document they created. | M-013 | BR-006, BR-021 | BRL-008 | P0 | The creator sees no approve action. An API attempt is refused. | T |
| FR-043 | Authority limit | The system shall route a document to the first approver whose authority covers its value. | M-013 | BR-006 | BRL-009 | P0 | A document above the limit goes to the next level. | T |
| FR-044 | Delegation | The system shall let an approver delegate to another user for a period and document types. Both names appear in the audit. | M-013 | BR-006 | BRL-012 | P0 | A delegation ends on time. Chained delegation is refused pending OQ-026. | T |
| FR-045 | Escalation | The system shall escalate an approval pending longer than the configured time. | M-013 | BR-006 | BRL-013 | P1 | An overdue item reaches the next approver and is notified. | T |
| FR-046 | Resubmission | The system shall let a requester revise a returned or rejected document and resubmit, keeping the history of earlier rounds. | M-013 | BR-006 | BRL-011 | P0 | History shows each round. | T |
| FR-047 | Approval inbox | The system shall show each user their pending, decided and delegated approvals. | M-013, M-001 | BR-006, BR-013 | | P0 | Only items in the user's scope are listed. | T |
| FR-048 | Import validate | The system shall provide import templates and a validate-only mode that reports row errors without saving. | M-014 | BR-009 | BRL-064 | P0 | Validate-only changes no data. | T |
| FR-049 | Import commit | The system shall commit a validated import in a chosen mode (all or nothing, or valid rows only) and report results. | M-014 | BR-009 | BRL-064 | P0 | All-or-nothing mode saves nothing on any error. | T |
| FR-050 | Export | The system shall export lists and reports, limited to user permission and scope, and audit the export. | M-014 | BR-009, BR-005 | BRL-003 | P0 | An export contains only rows the user can see. | T |
| FR-051 | Role dashboard | The system shall show role-based widgets with only in-scope data. | M-001 | BR-013 | BRL-003 | P1 | A branch user sees branch figures only. | T |
| FR-052 | Locale and time zone | The system shall show dates, numbers and currencies by user and company settings and store timestamps in UTC. | M-015 | BR-011 | | P1 | The same instant shows correctly in two time zones. | T |

## 3. Finance

| ID | Title | Requirement | Module | BR | BRL | Pri | Acceptance summary | Verif. |
|---|---|---|---|---|---|---|---|---|
| FR-053 | Chart of accounts | The system shall maintain a hierarchical chart of accounts per company with type, normal balance, postable flag and control flag. | M-031 | BR-015 | BRL-019 | P0 | A non-postable account refuses postings. | T |
| FR-054 | COA template | The system shall load a chart of accounts template and allow editing. Template content TBD (OQ-018). | M-031 | BR-015 | | P1 | Loaded accounts are editable. | D |
| FR-055 | Account protection | The system shall refuse deleting an account with postings. | M-031 | BR-015 | BRL-015 | P0 | Deactivation is allowed. Delete is refused. | T |
| FR-056 | Fiscal year | The system shall define fiscal years per company, with start and end, per the fiscal calendar (OQ-021). | M-044 | BR-016 | | P0 | Years do not overlap. | T |
| FR-057 | Periods | The system shall generate periods for a fiscal year without gaps or overlaps. | M-043 | BR-016 | | P0 | Every date falls in exactly one period. | T |
| FR-058 | Period states | The system shall support future, open, soft-closed and closed states, and refuse postings to closed periods. Soft-closed allows posting only by named roles. | M-043 | BR-016 | BRL-018 | P0 | A posting dated in a closed period is refused. | T |
| FR-059 | Period reopen | The system shall let a higher authority reopen a closed period with a reason, and audit it. | M-043 | BR-016 | BRL-022 | P0 | Reopen without reason is refused. | T |
| FR-060 | Close checklist | The system shall present a close checklist (unposted documents, pending approvals) and block closing until each item is done or carried forward. | M-045 | BR-016 | BRL-021 | P0 | An open item blocks close. | T |
| FR-061 | Year-end close | The system shall perform year-end close, transferring results to retained earnings and creating opening balances. | M-045 | BR-016 | BRL-021 | P0 | Opening balances equal prior closing balances. | T |
| FR-062 | Journal entry | The system shall create manual journals with two or more lines, narration and date, and refuse unbalanced journals in transaction and base currency. | M-035 | BR-014 | BRL-016, BRL-030 | P0 | Debits equal credits, or submit is refused. | T |
| FR-063 | Journal approval | The system shall route manual journals to a checker other than the maker. | M-035 | BR-021 | BRL-008, BRL-057 | P0 | The maker cannot approve. | T |
| FR-064 | Posting immutability | The system shall make posted ledger entries immutable. | M-032, M-035 | BR-014 | BRL-017 | P0 | No update or delete is possible, including by Super Admin. | T |
| FR-065 | Reversal | The system shall create a reversing entry that references the original, in an open period. | M-035 | BR-014 | BRL-017, BRL-018 | P0 | A reversed entry cannot be reversed twice. | T |
| FR-066 | Ledger inquiry | The system shall show account balances and transactions with drill-down to the source document. | M-032 | BR-019 | | P0 | Each line opens its source. | T |
| FR-067 | Currencies and rates | The system shall maintain currencies and dated exchange rates per company, entered or imported. | M-041 | BR-017 | BRL-023 | P0 | A rate must be above zero and unique per pair and date. | T |
| FR-068 | Currency conversion | The system shall store transaction currency, rate and base amount on each financial document. | M-041 | BR-017 | BRL-023 | P0 | A missing rate blocks the document with a clear message. | T |
| FR-069 | Exchange difference | The system shall post realised exchange differences when settlement rate differs from document rate. | M-041 | BR-017 | BRL-024 | P0 | The difference equals the computed gain or loss. | T |
| FR-070 | Tax codes | The system shall maintain tax codes with rates, effective dates, accounts and inclusive or exclusive treatment. | M-040 | BR-018 | BRL-025 | P0 | Effective periods do not overlap per code. Depends on OQ-007. | T |
| FR-071 | Tax calculation | The system shall calculate tax per line and per document using the line tax code and configured rounding, and summarise by code. | M-040 | BR-018 | BRL-025 | P0 | Totals match a hand-calculated reference set. | T |
| FR-072 | Bank accounts | The system shall maintain bank and cash accounts linked to ledger accounts and currency. | M-036 | BR-020 | BRL-015 | P0 | A used account cannot be deleted. | T |
| FR-073 | Allocation | The system shall record payments and receipts and allocate them to one or more open invoices, partly or fully. | M-047, M-059, M-086 | BR-020, BR-045 | BRL-026 | P0 | Allocation above the open amount is refused. | T |
| FR-074 | Advances and credit | The system shall hold overpayments and advances as unallocated credit that can later be allocated. | M-047 | BR-020, BR-045 | BRL-027 | P0 | The unallocated balance is visible per party. | T |
| FR-075 | Payment approval | The system shall require approval before releasing a vendor payment, with different people creating, approving and releasing as configured. | M-059, M-047 | BR-030 | BRL-057 | P0 | The invoice approver cannot also release payment where SoD is on. | T |
| FR-076 | Payment idempotency | The system shall create one payment for repeated submission of the same payment request. | M-047 | BR-020 | BRL-029 | P0 | A double click yields one payment. | T |
| FR-077 | Payment reversal | The system shall reverse a posted payment through a reversing document that restores open items. | M-047 | BR-020 | BRL-028 | P0 | The invoice returns to open. | T |
| FR-078 | Refund | The system shall refund a customer or vendor, limited to the amount received or paid, against a credit note or overpayment. | M-047, M-085 | BR-020, BR-044 | BRL-053 | P0 | A refund above the amount is refused. | T |
| FR-079 | Payable posting | The system shall post an approved purchase invoice to payables and the ledger. | M-033 | BR-014, BR-028 | BRL-016 | P0 | The ledger entry balances. | T |
| FR-080 | Receivable posting | The system shall post a sales invoice to receivables and the ledger. | M-034 | BR-014 | BRL-016 | P0 | The ledger entry balances. | T |
| FR-081 | Open items and ageing | The system shall show open items per party and ageing buckets that reconcile to the control account. | M-033, M-034 | BR-047 | | P1 | The ageing total equals the control account balance. | T |
| FR-082 | Trial balance | The system shall produce a trial balance per company, period and optional branch. | M-046 | BR-019 | | P0 | Total debits equal total credits. | T |
| FR-083 | Income statement | The system shall produce an income statement for a period range. | M-046 | BR-019 | | P0 | Net result equals the trial balance movement. | T |
| FR-084 | Balance sheet | The system shall produce a balance sheet at a date. | M-046 | BR-019 | | P0 | Assets equal liabilities plus equity. | T |
| FR-085 | Posting rules | The system shall map document types and item categories to ledger accounts and refuse posting where a mapping is missing. | M-030 | BR-014 | | P0 | A missing mapping blocks posting with a clear message. | T |

## 4. Procure-to-Pay

| ID | Title | Requirement | Module | BR | BRL | Pri | Acceptance summary | Verif. |
|---|---|---|---|---|---|---|---|---|
| FR-086 | Vendor creation | The system shall create vendors in draft and activate them after approval. | M-054 | BR-025 | BRL-032 | P0 | A draft vendor cannot be used on a PO. | T |
| FR-087 | Vendor block | The system shall block a vendor from new documents while leaving history. | M-054 | BR-025 | BRL-032 | P0 | Open documents are flagged. | T |
| FR-088 | Vendor bank change | The system shall require approval, by someone other than the editor, for changes to vendor bank details. | M-054 | BR-025 | BRL-033, BRL-058 | P0 | The change is inactive until approved. | T |
| FR-089 | Requisition entry | The system shall let a user create a requisition with lines, justification, needed-by date and attachments. | M-049 | BR-024 | | P0 | Quantity must be above zero. | T |
| FR-090 | Requisition approval | The system shall route a requisition through its approval chain. | M-050 | BR-024 | BRL-009, BRL-031 | P0 | An unapproved requisition cannot convert. | T |
| FR-091 | Purchase order creation | The system shall create a PO from approved requisitions or directly, with vendor, lines, tax and terms. | M-053 | BR-026 | BRL-031, BRL-032 | P0 | PO lines reference their requisition lines. | T |
| FR-092 | Purchase order approval | The system shall route a PO by authority matrix on value. | M-053 | BR-026 | BRL-009 | P0 | A value change after approval restarts approval. | T |
| FR-093 | PO issue and revision | The system shall record issue to vendor and keep revisions with history. | M-053 | BR-026 | | P0 | A revised PO retains the prior version. | T |
| FR-094 | PO cancellation | The system shall cancel a PO or its remaining quantity, keeping received quantities valid. | M-053 | BR-026 | BRL-034 | P0 | A cancelled PO accepts no receipt. | T |
| FR-095 | Goods receipt | The system shall receive goods against a PO in full or in part, within tolerance, increasing stock. | M-057 | BR-027 | BRL-035, BRL-038 | P0 | The remaining quantity stays open. | T |
| FR-096 | Receipt reversal and return | The system shall reverse a receipt or return goods to the vendor, adjusting stock and payables. | M-057, M-073 | BR-027 | BRL-017, BRL-038 | P0 | A reversal that would make stock negative is refused unless policy permits. | T |
| FR-097 | Purchase invoice entry | The system shall capture a vendor invoice linked to PO and GRN, with tax and totals. | M-058 | BR-028 | | P0 | Totals equal the sum of lines plus tax. | T |
| FR-098 | Three-way match | The system shall compare quantity and price of PO, GRN and invoice within configured tolerance and hold mismatches for approval. | M-058 | BR-028 | BRL-036 | P0 | A mismatch beyond tolerance blocks payment. | T |
| FR-099 | Duplicate invoice check | The system shall detect a vendor invoice with the same vendor and number (and date) and block or flag it. | M-058 | BR-029 | BRL-037 | P0 | A duplicate cannot be posted without a recorded override. | T |
| FR-100 | Procurement reports | The system shall report open POs, spend by vendor and receipt variance, in scope. | M-060 | BR-058 | | P1 | Totals reconcile to documents. | T |

## 5. Inventory

| ID | Title | Requirement | Module | BR | BRL | Pri | Acceptance summary | Verif. |
|---|---|---|---|---|---|---|---|---|
| FR-101 | Item master | The system shall maintain items and services with type, unit, conversions, category and default accounts. | M-061 | BR-033 | BRL-015 | P0 | An item with movements cannot change base unit. | T |
| FR-102 | Categories | The system shall maintain a category tree with default accounts and valuation settings. | M-062 | BR-033 | BRL-015 | P0 | Items inherit defaults. | T |
| FR-103 | Warehouses | The system shall maintain warehouses linked to branches. | M-063 | BR-033, BR-002 | BRL-015 | P0 | A warehouse with stock cannot be closed. | T |
| FR-104 | Stock ledger | The system shall record every stock change as a movement tied to a document, item and warehouse, and prevent direct edits to quantities. | M-064, M-073 | BR-034 | BRL-038, BRL-039 | P0 | On-hand equals the sum of movements. | T |
| FR-105 | Availability | The system shall compute on-hand, reserved, in-transit and available quantity. | M-064 | BR-034, BR-036 | BRL-041 | P0 | Available equals on-hand minus reserved. | T |
| FR-106 | Negative stock control | The system shall refuse issues that make stock negative unless policy allows. | M-064 | BR-035 | BRL-040 | P0 | The refusal states available quantity. Depends on OQ-020. | T |
| FR-107 | Reservation | The system shall reserve stock for confirmed sales orders and release it on cancellation or expiry. | M-065 | BR-036 | BRL-041, BRL-052 | P1 | Two orders cannot reserve the same unit. | T |
| FR-108 | Transfer | The system shall transfer stock between locations with dispatch, in-transit state and receipt, in full or in part. | M-066 | BR-037 | BRL-042 | P0 | Source falls on dispatch. Destination rises on receipt. | T |
| FR-109 | Adjustment | The system shall adjust stock with reason, show financial impact, require approval by an Inventory Manager who is not the creator or submitter, and require a second approval by a Finance Manager above a threshold. (v0.2 clarification of the v0.1 wording "approval above a threshold", which conflicted with SOD-07 and the workflow table. PENDING APPROVAL, OQ-038.) | M-067 | BR-038 | BRL-043 | P0 | Impact is shown before approval. | T |
| FR-110 | Issue and receipt documents | The system shall support goods issue (for example to a cost centre) and receipt without a PO. | M-073 | BR-034 | BRL-038 | P0 | Each posts stock movements and ledger entries. | T |
| FR-111 | Valuation | The system shall value stock by the configured method and post valuation entries to the ledger. Methods per OQ-009. | M-072 | BR-039 | BRL-044 | P0 | The valuation report equals the inventory control account. | T |
| FR-112 | Inventory reports | The system shall report stock on hand, movements and valuation by warehouse. | M-075 | BR-058 | | P1 | Report totals match the stock ledger. | T |
| FR-113 | Stock concurrency | The system shall serialise concurrent stock updates so that two users cannot both issue the last unit. | M-064 | BR-035 | BRL-065 | P0 | Under concurrent load, stock never goes below zero when policy forbids it. | T |

## 6. Order-to-Cash

| ID | Title | Requirement | Module | BR | BRL | Pri | Acceptance summary | Verif. |
|---|---|---|---|---|---|---|---|---|
| FR-114 | Customer master | The system shall maintain customers with contacts, tax ids, terms, credit limit and status. | M-079 | BR-041 | BRL-046 | P0 | A customer in use cannot be deleted. | T |
| FR-115 | Credit check | The system shall compare order value plus open exposure with the credit limit and route excess for approval. | M-079, M-081 | BR-046 | BRL-047 | P1 | The block message shows limit and exposure. | T |
| FR-116 | Quotation | The system shall create quotations with validity date, revisions and discount limits. | M-080 | BR-042 | | P0 | An expired quotation cannot convert. | T |
| FR-117 | Quote conversion | The system shall convert an accepted quotation to a sales order keeping the link. | M-080, M-081 | BR-042 | | P0 | Order lines reference quotation lines. | T |
| FR-118 | Sales order | The system shall create and confirm sales orders for active customers, reserving stock on confirmation. | M-081 | BR-042 | BRL-046 | P0 | A confirmed order shows the reservation result. | T |
| FR-119 | Order cancellation | The system shall cancel an order or its remaining quantity and release reservations. | M-081 | BR-043 | BRL-052 | P0 | Delivered quantities remain valid. | T |
| FR-120 | Delivery | The system shall deliver in full or in part within ordered quantity plus tolerance, issuing stock. | M-083 | BR-043 | BRL-048 | P0 | The remaining quantity stays open. | T |
| FR-121 | Sales invoice | The system shall invoice delivered quantities, in part or in full, with tax. | M-082 | BR-043 | BRL-049 | P0 | Invoiced quantity cannot exceed delivered unless advance invoicing is enabled. | T |
| FR-122 | Sales return | The system shall record a return against a delivery or invoice, receive goods, and restock or scrap them. | M-084 | BR-044 | BRL-051 | P0 | Return quantity cannot exceed delivered less earlier returns. | T |
| FR-123 | Credit note | The system shall issue credit notes against invoices, limited to the invoiced amount less earlier credits. | M-085 | BR-044 | BRL-050 | P0 | Credit above the limit is refused. | T |
| FR-124 | Sales reporting | The system shall report sales by customer and item, order status and receivable ageing, in scope. | M-088 | BR-047, BR-058 | | P1 | Ageing reconciles to the receivables control account. | T |

## 7. Cross-cutting

| ID | Title | Requirement | Module | BR | BRL | Pri | Acceptance summary | Verif. |
|---|---|---|---|---|---|---|---|---|
| FR-125 | Optimistic concurrency | The system shall refuse a save based on an out-of-date version of a record and show the conflict. | All | BR-014 | BRL-065 | P0 | The second save returns a conflict. | T |
| FR-126 | Report scope | The system shall apply user scope to every report, list and search. | All | BR-003, BR-058 | BRL-003 | P0 | Counts differ by scope. | T |
| FR-127 | Retention configuration | The system shall hold configurable retention periods per record type and block deletion within them. Values TBD (OQ-022). | M-116 (basic) | BR-059 | BRL-063 | P0 | Deletion inside retention is refused. | T |
| FR-128 | Outbound integration queue | The system shall queue calls to external services, retry with backoff and let an authorised user retry or cancel failed items. | M-012 | BR-060 | BRL-062 | P1 | A failed call keeps the business event committed. | T |
| FR-129 | Personal data handling | The system shall support export and, where law allows, anonymisation of personal data, with audit. | M-116 (basic) | BR-059 | BRL-063 | P0 | Anonymised records keep ledger integrity. Rules per OQ-010. | T |
| FR-130 | Configurable fields | The system shall let an administrator add configured custom fields and statuses to selected documents within limits. | M-008 | BR-061 | | P1 | A custom field appears on the form, in export and in audit. | D |

## 7A. Authorisation hardening (v0.2, Phase 2 remediation)

FR-131 to FR-150 close audit findings E1 to E10 and the missing-permission findings. They refine FR-017, FR-042, FR-043, FR-044, FR-059, FR-075, FR-088 and FR-115 without replacing them.
All are P0 or P1 and Proposed.

| ID | Title | Requirement | Module | BR | BRL | Pri | Acceptance summary | Verif. |
|---|---|---|---|---|---|---|---|---|
| FR-131 | Role tiers | The system shall give every role a tier (T0 platform, T1 organisation, T2 manager, T3 operator, T4 basic) and shall refuse a role grant when the role tier is not strictly lower than the grantor's highest tier. | M-004 | BR-003 | BRL-066 | P0 | A Branch Admin cannot grant Finance Manager. An Organization Admin cannot grant Organization Admin. | T |
| FR-132 | No self-grant | The system shall refuse any role grant, scope widening, grant approval or SoD override whose target is the acting user's own account. | M-004, M-003 | BR-003 | BRL-066 | P0 | A user who tries to add a role to their own account is refused and the attempt is audited. | T |
| FR-133 | Four-eyes role grant | The system shall hold a grant of a Manager-tier or Auditor role, a scope widening to another company, or a role with approve, release or reverse permission as Pending until a second administrator holding grant-approval authority approves it. | M-004, M-013 | BR-003, BR-006 | BRL-067 | P0 | The grant has no effect while Pending. The approver cannot be the grantor or the grantee. | T |
| FR-134 | Branch-limited assignment | The system shall let a Branch Admin assign only T3 and T4 roles, only to users, and only within the Branch Admin's own branches. | M-004 | BR-002, BR-003 | BRL-066 | P0 | A grant outside the branch or above T3 is refused. | T |
| FR-135 | Payment duty separation | The system shall refuse a second duty by the same user on one payment or refund among prepare, approve and release, and shall require a different approver for a payment reversal. | M-047, M-059 | BR-020, BR-030 | BRL-068 | P0 | The user who approved a payment cannot release it. The user who requested a reversal cannot approve it. | T |
| FR-136 | Journal duty separation | The system shall refuse approval of a manual journal or reversal journal by the user who created or requested it, and shall post journals only by system action at final approval. | M-035 | BR-021 | BRL-069 | P0 | A Finance Manager cannot approve a journal they created or a reversal they requested. | T |
| FR-137 | Release control | The system shall require step-up authentication to release or reverse a payment, and above the configured value shall require approval by two different approvers. | M-047 | BR-020, BR-030 | BRL-057, BRL-068 | P0 | Release without a fresh step-up is refused. Value limits are TBD (OQ-013). | T |
| FR-138 | Controlled change workflow | The system shall route changes to controlled data (approval limits, posting rules, SoD rules, retention rules, credit limits, vendor bank details) through request and approval by different users, with the old value active until approval. | M-013, M-008 | BR-006, BR-021 | BRL-070 | P0 | The new value is inactive until approved. The requester cannot approve. The approver cannot hold the authority being changed. | T |
| FR-139 | Vendor bank change control | The system shall require, for a vendor bank change, a request, out-of-band verification by a different user, and approval by a Finance Manager, and shall hold the first payment to a new account for a configured period. | M-054 | BR-025 | BRL-033, BRL-058, BRL-070 | P0 | No payment goes to the new account before approval. First payment waits for the hold period (default proposed 24 hours, PENDING APPROVAL). | T |
| FR-140 | Period reopen control | The system shall let a Finance Manager request reopening of a closed period with a reason, shall require approval by a different authority, shall close the period again after a configured window, and shall audit every posting made while it is open. | M-043 | BR-016, BR-021 | BRL-022, BRL-071 | P0 | The requester cannot approve. The period closes automatically at the end of the window. Approver role pending OQ-032. | T |
| FR-141 | Credit limit control | The system shall change a customer credit limit only through a request by a Sales Manager and approval by a Finance Manager, and shall remove the credit limit from ordinary customer edits. | M-079 | BR-046 | BRL-046, BRL-070 | P0 | A Sales Executive edit that carries a changed credit limit is refused. The old limit stays active until approval. | T |
| FR-142 | SoD override control | The system shall allow a hard SoD conflict only through a recorded override that is requested by one administrator, approved by a different administrator, limited in time and reported to the Auditor, and shall refuse an override of the requester's own conflict. | M-004 | BR-003 | BRL-067, BRL-074 | P0 | An override expires on its end date. The requester cannot approve it. | T |
| FR-143 | Delegation restrictions | The system shall apply the delegation limits of BRL-073: delegate holds permission and scope, SoD is checked at delegation and at use, no chains, and no delegation of release, reverse, reopen or grant approval. | M-013 | BR-006 | BRL-012, BRL-073 | P0 | A delegate who created a document cannot approve it for the delegator. | T |
| FR-144 | Emergency access | The system shall provide a sealed emergency account that is released by two authorised people, expires after a configured time, carries no financial approval or release permission, and is reviewed by the Auditor after use. | M-002, M-003 | BR-003, BR-005 | BRL-072 | P0 | Release needs two different users. Every action is audited. Access ends at expiry. | T |
| FR-145 | Step-up for sensitive actions | The system shall require recent re-authentication (step-up) for the sensitive actions listed in the RBAC Matrix section 8A.6. | M-002 | BR-004 | BRL-072 | P0 | A sensitive action with an old authentication is refused until the user re-authenticates. | T |
| FR-146 | Permissions for remaining MVP modules | The system shall define and enforce permission codes for dashboards (M-001), notifications (M-012), localisation (M-015) and stock reservation (M-065). | M-001, M-012, M-015, M-065 | BR-003 | BRL-004 | P0 | Each of the four modules has permission codes and a role mapping. A user without the code is refused. | T |
| FR-147 | Attachment immutability | The system shall allow deletion of an attachment only on a draft document by its owner, and shall make attachments on submitted, approved or posted documents removable only by a voiding action with reason by an administrator. | M-011 | BR-010, BR-005 | BRL-060 | P1 | A staff user cannot remove evidence from an approved document. | T |
| FR-148 | Export control | The system shall require the module read permission and a module export permission for any export, shall require step-up for user and bulk exports, and shall audit every export. | M-014 | BR-009, BR-003 | BRL-003 | P0 | A user without read permission on a module cannot export it, even if they hold the generic export permission. | T |
| FR-149 | Service accounts and tokens | The system shall support service accounts with a named owner, a fixed narrow permission set, scoped tokens that expire, rotation, and a rule that a service account can never hold approve, release, reverse, reopen or grant permissions. | M-002 | BR-004, BR-060 | BRL-074 | P0 | A service account token cannot approve a payment. | T |
| FR-150 | Audit read separation | The system shall separate permission to read security events from permission to read business events, and shall limit the Branch Admin and Finance Manager to business events of their scope. | M-010 | BR-005 | BRL-007 | P1 | A Finance Manager cannot read security events. Audit exports are themselves audited. | T |

## 8. Coverage check

Measured by script on 2026-11-11 (original 130). Section 7A added in the Phase 2 remediation.

| Check | Result |
|---|---|
| Functional requirements | 150 (P0 127, P1 23). FR-001 to FR-130 are from Stage 2, FR-131 to FR-150 were added in v0.2 (section 7A) |
| MVP modules (58) with at least one FR | 58 of 58 |
| BRs with no FR | 16: BR-022, 023, 031, 032, 040, 048, 050 to 057, 063 are post-MVP by design. BR-062 (service levels) is covered by the [NFR](NFR.md). |
| BRLs with no FR | BRL-020 (money precision) is covered by NFR and TR. BRL-045 (expired batches) is post-MVP. |
| Resolved | FR-127 and FR-129 sit under M-116. The inventory now lists M-116 as MVP (basic) and R3 (full) with P0 for the basic part (PG-07 closed). |

"Covered" here means traced to a requirement. It does not mean tested. No test cases exist yet.

## 9. Out of this FRS

| Item | Plan |
|---|---|
| FRs for R2, R3 and R4 modules | Written before each release starts. |
| Detailed field lists and screen behaviour | UI/UX specifications (Stage 3) |
| API shapes | [API Documentation](../04-System-Architecture/API-Documentation.md) |
