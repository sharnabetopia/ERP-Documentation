# Business Rules

Document status: Draft v0.1 (Stage 1).
Every rule is **Proposed** (industry-standard ERP practice) until confirmed by the owner role.
Where a value is company-specific, the rule says "configurable" and the default is TBD.

Related: [BRD](BRD.md), [Open Questions](Open-Questions.md).

## 1. Rules

| ID | Area | Rule | Linked BR | Configurable | Status |
|---|---|---|---|---|---|
| BRL-001 | Tenancy | Every business record belongs to exactly one company. | BR-001 | No | Proposed |
| BRL-002 | Tenancy | A user can act in a company only if explicitly assigned to it. | BR-001, BR-003 | No | Proposed |
| BRL-003 | Access | Access is the intersection of role permission and scope (company, branch, department, ownership). | BR-003 | No | Proposed |
| BRL-004 | Access | Permissions default to deny. | BR-003 | No | Proposed |
| BRL-005 | Authentication | Accounts lock after a configurable number of failed sign-ins. Default TBD. | BR-004 | Yes | Proposed |
| BRL-006 | Authentication | Sessions end after a configurable idle time. Default TBD. | BR-004 | Yes | Proposed |
| BRL-007 | Audit | Audit records cannot be edited or deleted by any user including Super Admin. | BR-005 | No | Proposed |
| BRL-008 | Approval | A user cannot approve a document they created. | BR-006, BR-021 | No | Proposed |
| BRL-009 | Approval | An approver must hold authority at or above the document value for the document type. | BR-006 | Yes (limits) | Proposed |
| BRL-010 | Approval | A rejection requires a reason and returns the document to the requester. | BR-006 | No | Proposed |
| BRL-011 | Approval | A resubmitted document starts a new approval round and keeps the history of earlier rounds. | BR-006 | No | Proposed |
| BRL-012 | Approval | A delegate acts with the delegator's authority only for the delegation period and the delegated document types. The audit log shows both names. | BR-006 | Yes | Proposed |
| BRL-013 | Approval | An approval pending beyond its configured time escalates to the next level. | BR-006 | Yes | Proposed |
| BRL-014 | Numbering | Document numbers are unique per company, type and fiscal year. | BR-012 | Format only | Proposed |
| BRL-015 | Master data | Records referenced by transactions are deactivated, never deleted. | BR-008, BR-033 | No | Proposed |
| BRL-016 | Ledger | Every journal balances in the transaction currency and in the base currency. | BR-014 | No | Proposed |
| BRL-017 | Ledger | Posted entries are immutable. Corrections use a reversing entry that references the original. | BR-014 | No | Proposed |
| BRL-018 | Ledger | Postings go to open periods only. | BR-016 | No | Proposed |
| BRL-019 | Ledger | An account flagged control-only accepts postings only from its sub-ledger. | BR-015 | Yes | Proposed |
| BRL-020 | Ledger | Money uses fixed decimal precision with a configured rounding rule. Currency-specific decimals apply. | BR-014, BR-017 | Yes | Proposed |
| BRL-021 | Period | Closing a period requires all in-scope documents dated in it to be posted or cancelled, or explicitly carried forward. | BR-016 | Yes | Proposed |
| BRL-022 | Period | Reopening a closed period requires a higher authority and a written reason. | BR-016 | Yes | Proposed |
| BRL-023 | Currency | The exchange rate used on a document is the rate on the document date from the configured source. The rate is stored on the document. | BR-017 | Yes | Proposed |
| BRL-024 | Currency | Settlement at a different rate posts a realised exchange difference. | BR-017 | No | Proposed |
| BRL-025 | Tax | Tax is calculated per line using the tax code on the line. Rounding follows company policy. | BR-018 | Yes | Proposed |
| BRL-026 | Payment | The sum allocated to an invoice cannot exceed its open amount. | BR-020 | No | Proposed |
| BRL-027 | Payment | A payment exceeding the open amount is stored as unallocated credit. | BR-020, BR-045 | No | Proposed |
| BRL-028 | Payment | A payment or refund cannot be edited after posting. It can be reversed. | BR-020 | No | Proposed |
| BRL-029 | Payment | Idempotency: the same payment request submitted twice creates one payment. | BR-020 | No | Proposed |
| BRL-030 | Journals | A manual journal needs a narration and at least two lines. | BR-021 | No | Proposed |
| BRL-031 | Procurement | A PO requires an approved requisition when the company policy enforces it. | BR-024, BR-026 | Yes | Proposed |
| BRL-032 | Procurement | A blocked or inactive vendor cannot be used on new documents. | BR-025 | No | Proposed |
| BRL-033 | Procurement | A change to vendor bank details needs approval from someone other than the editor. | BR-025 | No | Proposed |
| BRL-034 | Procurement | A cancelled PO cannot be received against. Quantities already received stay valid. | BR-026, BR-027 | No | Proposed |
| BRL-035 | Procurement | Received quantity may exceed ordered quantity only within the tolerance set per item or company. Default TBD. | BR-027 | Yes | Proposed |
| BRL-036 | Procurement | Invoice matching tolerance is configurable by amount and percentage. Default TBD. | BR-028 | Yes | Proposed |
| BRL-037 | Procurement | A vendor invoice with the same vendor, invoice number and date within the company is a duplicate. | BR-029 | No | Proposed |
| BRL-038 | Inventory | Every stock change is created by a document and written to the stock ledger. | BR-034 | No | Proposed |
| BRL-039 | Inventory | Stock cannot be edited directly. Only documents change it. | BR-034 | No | Proposed |
| BRL-040 | Inventory | Negative stock is refused unless allowed by policy. Default: refused. Subject to OQ-020. | BR-035 | Yes | Proposed |
| BRL-041 | Inventory | Reserved quantity reduces available quantity. | BR-036 | No | Proposed |
| BRL-042 | Inventory | A transfer remains in transit until received. A transfer can be received in parts. | BR-037 | No | Proposed |
| BRL-043 | Inventory | An adjustment above a configurable value needs approval. | BR-038 | Yes | Proposed |
| BRL-044 | Inventory | Valuation method is fixed per item once it has movements. Changing it needs a controlled revaluation. | BR-039 | Yes | Proposed |
| BRL-045 | Inventory | Expired or quarantined batches cannot be issued. | BR-040 | No | Proposed |
| BRL-046 | Sales | A sales order needs an active customer. | BR-041 | No | Proposed |
| BRL-047 | Sales | An order exceeding the customer credit limit needs approval from authorised finance staff. | BR-046 | Yes | Proposed |
| BRL-048 | Sales | Delivered quantity cannot exceed ordered quantity plus tolerance. | BR-043 | Yes | Proposed |
| BRL-049 | Sales | Invoiced quantity cannot exceed delivered quantity unless the company allows advance invoicing. | BR-043 | Yes | Proposed |
| BRL-050 | Sales | A credit note cannot exceed the invoiced amount less earlier credits. | BR-044 | No | Proposed |
| BRL-051 | Sales | A return must reference a delivery or invoice. Return quantity cannot exceed delivered quantity less earlier returns. | BR-044 | No | Proposed |
| BRL-052 | Sales | A cancelled order releases its reservations. | BR-036 | No | Proposed |
| BRL-053 | Refund | A refund cannot exceed the amount received for the related invoice. | BR-020, BR-044 | No | Proposed |
| BRL-054 | HR | One employee record maps to at most one active user. | BR-049 | No | Proposed |
| BRL-055 | HR | Deactivating an employee disables sign-in within a configurable time. Default TBD. | BR-049 | Yes | Proposed |
| BRL-056 | HR | An employee can view only their own personal data in self-service. | BR-052 | No | Proposed |
| BRL-057 | Segregation | The same user cannot both create and approve, or both approve and pay, the same payable. | BR-021, BR-030 | No | Proposed |
| BRL-058 | Segregation | The same user cannot both create a vendor and approve payments to that vendor. | BR-025, BR-030 | No | Proposed |
| BRL-059 | Temporary access | Temporary role grants have a start, an end and a reason, and expire automatically. | BR-003 | Yes | Proposed |
| BRL-060 | Documents | Attachments inherit the access rules of the parent record. | BR-010 | No | Proposed |
| BRL-061 | Notifications | A failed notification is retried a configurable number of times, then flagged. | BR-007, BR-060 | Yes | Proposed |
| BRL-062 | Integration | An external call that fails does not roll back a committed business event. It is queued for retry. | BR-060 | No | Proposed |
| BRL-063 | Retention | Records are retained for the longest applicable legal period. Default TBD (OQ-022). | BR-059 | Yes | Proposed |
| BRL-064 | Import | An import runs in validate-only mode before commit. | BR-009 | No | Proposed |
| BRL-065 | Concurrency | Two users editing the same record: the second save is refused if the record changed since it was opened. | BR-014 | No | Proposed |

### 1A. Authorisation hardening rules (v0.2, Phase 2 remediation)

Rules BRL-066 to BRL-074 close audit findings RBAC E1 to E10. All are Proposed and need security officer and sponsor confirmation.

| ID | Area | Rule | Linked BR | Configurable | Status |
|---|---|---|---|---|---|
| BRL-066 | Access | A user can grant only roles whose tier is lower than the grantor's highest tier, only inside the grantor's own scope, and never to the grantor's own account. | BR-003 | No | Proposed |
| BRL-067 | Access | Granting a Manager-tier or Auditor role, widening a user's scope to another company, or granting a role that holds an approve, release or reverse permission needs approval by a second administrator who is neither grantor nor grantee. | BR-003 | No | Proposed |
| BRL-068 | Payments | On one payment or refund, the person who prepares, the person who approves and the person who releases must be three different users. A reversal needs its own approver, different from the person who requested it. | BR-020, BR-030 | No | Proposed |
| BRL-069 | Journals | On one manual journal, the creator and the approver must differ. A reversal is a new journal that needs approval by a user other than the one who requested the reversal. Posting happens only by the system at final approval. | BR-021 | No | Proposed |
| BRL-070 | Controlled data | Changes to vendor bank details, customer credit limits, approval limits, posting rules, SoD rules and retention rules are controlled changes. They are requested by one user, approved by another, and take effect only after approval. | BR-003, BR-006, BR-021, BR-025, BR-046 | No | Proposed |
| BRL-071 | Periods | A closed period is reopened only through a request with reason, approved by a different authority than the requester. The period closes again automatically at the end of a set window. Every posting in the window is audited. | BR-016, BR-021 | Window length | Proposed |
| BRL-072 | Emergency | Emergency (break-glass) access is a sealed account, released by two people, valid for a short time, fully recorded and reviewed afterwards by the Auditor. It never carries financial approval or release permissions. | BR-003, BR-005 | Duration | Proposed |
| BRL-073 | Delegation | A delegate must hold the underlying permission and scope, must pass the SoD check at delegation and at use, cannot approve a document the delegator created, and cannot receive delegation of release, reverse, reopen or grant-approval authority. | BR-006 | Maximum period | Proposed |
| BRL-074 | Access | SoD is evaluated on permissions, not on role names, and across all of a user's assignments in every company. A custom role is checked when it is created and when it is assigned. | BR-003 | No | Proposed |

## 2. Rule conflicts and gaps noticed

| Item | Note |
|---|---|
| BRL-040 vs OQ-020 | Default is refuse. Some businesses allow negative stock for fast-moving items. Needs a decision. |
| BRL-049 | Advance invoicing exists in some industries. Left configurable. |
| BRL-012 vs OQ-026 | Chained delegation is not allowed in this draft pending OQ-026. |

Counts are recorded in [Documentation-Progress](../00-Project-Management/Documentation-Progress.md).
