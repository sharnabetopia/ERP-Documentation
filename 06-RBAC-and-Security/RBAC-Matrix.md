# RBAC Matrix

Document status: Draft v0.2 (Phase 2 remediation, 2026-11-11). Sections 1 to 9 are v0.1 except where marked "v0.2". Section 8A is new. **Proposed default permission sets.** They are starting points for approval by the business, not confirmed access policy.
Generated from one data definition and checked by script (see section 8). Related: [Permission Matrix](Permission-Matrix.md), [Authentication and Authorization](Authentication-Authorization.md), [ADR-006](../04-System-Architecture/Architecture-Decision-Records.md), FR-012 to FR-017.

## 1. How to read the matrix

Cells hold action letters. An empty cell means no permission (deny by default, BRL-004).

| Letter | Action | Letter | Action |
|---|---|---|---|
| R | Read | N | Cancel |
| C | Create | Y | Verify |
| U | Update | K | Reconcile |
| D | Delete (draft or own attachment only, otherwise deactivate) | P | Publish or issue externally |
| S | Submit | G | Assign |
| A | Approve and Reject (two permissions, both granted) | M | Manage (status changes, configuration, reversal, close) |
| X | Export | I | Import |
| V | View reports | | |

Role stacking: every staff user also holds the **Employee** role. Rows for other roles list only what the role adds. Roles can be combined by an administrator, subject to the segregation checks in section 5.

Scope: a letter is always limited by the user's scope assignment (section 3). A cell never means company-wide access by itself.

## 2. Module-level and action-level matrix (MVP modules)

Role codes: SA Super Admin, OA Organization Admin, BA Branch Admin, DM Department Manager, HM HR Manager, HE HR Executive, FM Finance Manager, AC Accountant, PM Procurement Manager, PO Procurement Officer, IM Inventory Manager, WS Warehouse Staff, SM Sales Manager, SE Sales Executive, PJ Project Manager, EM Employee, AU Auditor, RO Read-only User.

| Area (permission prefix) | SA | OA | BA | DM | HM | HE | FM | AC | PM | PO | IM | WS | SM | SE | PJ | EM | AU | RO |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Users (`identity.user`) | RCUMG | RCUMG | RCUMG |  | R |  |  |  |  |  |  |  |  |  |  |  | R |  |
| Roles and permissions (`identity.role`) | RCUM | RG | RG |  |  |  |  |  |  |  |  |  |  |  |  |  | R |  |
| Sessions (`identity.session`) | M | M | M |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| Companies (`org.company`) | RCU | RU | R | R | R | R | R | R | R | R | R | R | R | R | R | R | R | R |
| Branches (`org.branch`) | RCU | RCUM | RU | R | R | R | R | R | R | R | R | R | R | R | R | R | R | R |
| Departments (`org.department`) | RCU | RCUM | RU | R | R | R | R | R | R | R | R | R | R | R | R | R | R | R |
| Cost centres (`org.costcentre`) |  | RCUM | RU | R |  |  | RCU | R | R | R | R | R | R |  | R |  | R |  |
| Employees (`org.employee`) | R | R | R | R | RCUMX | RCU | R | R | R | R | R | R | R | R | R | R | R | R |
| Document templates (`platform.template`) |  | RM |  |  |  |  | R |  |  |  |  |  |  |  |  |  | R |  |
| Custom fields (`platform.customfield`) | R | RCUM |  |  |  |  |  |  |  |  |  |  |  |  |  |  | R |  |
| Numbering (`platform.numbering`) | RM | RM |  |  |  |  | R |  |  |  |  |  |  |  |  |  | R |  |
| Settings and policies (`platform.settings`) | RM | RM |  |  |  |  | RM |  |  |  |  |  |  |  |  |  | R |  |
| Master data (`platform.masterdata`) |  | RCUM | R | R |  |  | RCU | R | RC | R | RC | R | R | R |  |  | R | R |
| Attachments (`platform.attachment`) | R | RC | RC | RC | RC | RC | RC | RC | RC | RC | RC | RC | RC | RC | RC | RCD | R | R |
| Import (`platform.import`) |  | RI |  |  | RI |  | RI |  | RI |  | RI |  | RI |  |  |  |  |  |
| Export (`platform.export`) |  | X | X |  | X |  | X | X | X |  | X |  | X |  |  |  | X |  |
| Audit log (`platform.audit`) | RX | R | R |  |  |  | R |  |  |  |  |  |  |  |  |  | RX |  |
| Integration queue (`platform.integration`) | RM | RM |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| Retention rules (`platform.retention`) | RM | R |  |  |  |  |  |  |  |  |  |  |  |  |  |  | R |  |
| Privacy requests (`platform.privacy`) |  | M |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| Approval chains (`workflow.chain`) | R | RCUM |  |  |  |  | R |  | R |  |  |  |  |  |  |  | R |  |
| Approvals (`workflow.approval`) |  |  |  | A | A |  | A |  | A |  | A |  | A |  |  |  |  |  |
| Delegation (`workflow.delegation`) |  | C | C | C | C |  | C |  | C |  | C |  | C |  |  |  |  |  |
| Chart of accounts (`finance.account`) |  | R |  |  |  |  | RCUMI | RC |  |  |  |  |  |  |  |  | R |  |
| Posting rules (`finance.postingrule`) |  |  |  |  |  |  | RM | R |  |  |  |  |  |  |  |  | R |  |
| Periods and close (`finance.period`) |  | A |  |  |  |  | RCM | R |  |  |  |  |  |  |  |  | R |  |
| Journals (`finance.journal`) |  |  |  |  |  |  | RCUSAMN | RCUS |  |  |  |  |  |  |  |  | R |  |
| Ledger inquiry (`finance.ledger`) |  |  |  |  |  |  | R | R |  |  |  |  |  |  |  |  | R |  |
| Currency and rates (`finance.currency`) |  |  |  |  |  |  | RCU | R | R | R |  |  | R | R |  |  | R |  |
| Tax codes (`finance.tax`) |  |  |  |  |  |  | RCU | R |  | R |  |  |  | R |  |  | R |  |
| Bank accounts (`finance.bank`) |  |  |  |  |  |  | RM | RCU |  |  |  |  |  |  |  |  | R |  |
| Payments and refunds (`finance.payment`) |  |  |  |  |  |  | RAM | RCS |  |  |  |  |  |  |  |  | R |  |
| Open items (AP and AR) (`finance.openitem`) |  |  |  |  |  |  | R | R | R |  |  |  | R |  |  |  | R |  |
| Finance reports (`finance.report`) |  | V |  |  |  |  | V | V |  |  |  |  |  |  |  |  | V |  |
| Bank reconciliation (R2) (reserved) (`finance.bankrec`) |  |  |  |  |  |  | K | K |  |  |  |  |  |  |  |  |  |  |
| Vendors (`proc.vendor`) |  |  |  | R |  |  | R | R | RCUMA | RCUS |  | R |  |  |  |  | R |  |
| Purchase requisitions (`proc.pr`) |  |  |  | RCUSAN |  |  | R |  | R | R |  |  |  |  | RCUS | RCUS | R |  |
| Purchase orders (`proc.po`) |  |  |  |  |  |  | RA | R | RCUSANM | RCUSNM | R | R |  |  |  |  | R |  |
| Goods receipts (`proc.grn`) |  |  |  |  |  |  |  | R |  | R | RCM | RCM |  |  |  |  | R |  |
| Purchase invoices (`proc.invoice`) |  |  |  |  |  |  | RA | RCUSY | R | R |  |  |  |  |  |  | R |  |
| Procurement reports (`proc.report`) |  |  |  |  |  |  | V |  | V | V | V |  |  |  |  |  | V |  |
| Items (`inv.item`) |  |  |  | R |  |  | R | R | R | R | RCUMI | R | R | R |  | R | R | R |
| Item categories (`inv.category`) |  |  |  |  |  |  | RCU | R |  | R | RCU |  |  |  |  |  | R | R |
| Warehouses (`inv.warehouse`) |  |  | R |  |  |  |  | R |  | R | RCUM | R | R |  |  |  | R | R |
| Stock inquiry (`inv.stock`) |  |  |  |  |  |  | R | R |  | R | R | R | R | R |  |  | R | R |
| Transfers (`inv.transfer`) |  |  |  |  |  |  |  |  |  |  | RCSMNA | RCSM |  |  |  |  | R |  |
| Adjustments (`inv.adjustment`) |  |  |  |  |  |  | RA |  |  |  | RCSA | RCS |  |  |  |  | R |  |
| Goods issue and receipt documents (`inv.document`) |  |  |  |  |  |  |  |  |  |  | RC | RCM |  |  |  |  | R |  |
| Valuation (`inv.valuation`) |  |  |  |  |  |  | RM | R |  |  | R |  |  |  |  |  | R |  |
| Inventory reports (`inv.report`) |  |  |  |  |  |  | V |  | V |  | V | V |  |  |  |  | V |  |
| Customers (`sales.customer`) |  |  |  |  |  |  | R | R |  |  |  | R | RCUMA | RCUS |  |  | R | R |
| Quotations (`sales.quotation`) |  |  |  |  |  |  |  |  |  |  |  |  | RCUSAPNM | RCUSP |  |  | R | R |
| Sales orders (`sales.order`) |  |  |  |  |  |  | RA | R |  |  |  | R | RCUSAN | RCUS |  |  | R | R |
| Deliveries (`sales.delivery`) |  |  |  |  |  |  |  | R |  |  | R | RCM | R | R |  |  | R |  |
| Sales invoices (`sales.invoice`) |  |  |  |  |  |  | RA | RCUS |  |  |  |  | R | R |  |  | R |  |
| Sales returns (`sales.return`) |  |  |  |  |  |  |  |  |  |  |  | RM | RCA | RC |  |  | R |  |
| Credit notes (`sales.creditnote`) |  |  |  |  |  |  | RA | RCS |  |  |  |  | R |  |  |  | R |  |
| Sales reports (`sales.report`) |  |  |  |  |  |  | V | V |  |  |  |  | V | V |  |  | V |  |
| Portal publishing (R3) (reserved) (`platform.portal`) |  | P |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |

Rows marked reserved exist in the catalogue to satisfy FR-013 (reconcile, publish) and are used by R2 and R3 modules. No MVP function uses them.
Project Manager has no MVP module of its own (projects are R3). Its MVP rights are those of a staff user who raises requisitions and reads items.
Super Admin holds platform administration only. It has **no** finance, procurement, inventory or sales permission, by design (SoD, section 5).

## 3. Default scope per role

| Role | Default data scope (assigned per user, FR-010) | Record-level rule |
|---|---|---|
| Super Admin | Tenant (platform administration, no business data rights) | Cannot read personal HR detail beyond directory |
| Organization Admin | Assigned companies | |
| Branch Admin | Assigned branches | Cannot see other branches' documents |
| Department Manager | Own department and its sub-units | Approves only items of own department |
| HR Manager | HR data of assigned companies | Salary and sensitive fields visible to this role only (R2) |
| HR Executive | HR data of assigned companies | Cannot see salary fields (R2) |
| Finance Manager | Assigned companies, all branches unless limited | |
| Accountant | Assigned companies or branches | |
| Procurement Manager | Assigned companies or branches | |
| Procurement Officer | Assigned branches | Edits own drafts only |
| Inventory Manager | Assigned warehouses and branches | |
| Warehouse Staff | Assigned warehouses | Own warehouse documents |
| Sales Manager | Assigned branches and the sales team | |
| Sales Executive | Own customers and own documents, plus team read where set | Own records unless widened |
| Project Manager | Own department and own created documents (MVP) | |
| Employee | Own records and own requests | Own only |
| Auditor | Assigned companies, read only, all branches | Cannot create or change anything |
| Read-only User | Assigned scope, read only, operational documents | No ledger, payroll or audit access |

Scope levels: Tenant, Company, Branch, Department, Own records (see [Data Dictionary](../04-System-Architecture/Data-Dictionary.md)).
Organization-level restriction means a user acts only in companies they are assigned to (BRL-002). Branch and department restrictions use the same assignment (FR-010).

## 4. Authorisation layers

| # | Layer | Rule | Source |
|---|---|---|---|
| 1 | Module level | The role must hold at least one permission of the module, or the menu and API are refused | FR-013 |
| 2 | Submodule level | The permission prefix names the submodule (`finance.journal`) | FR-013 |
| 3 | Action level | Each endpoint names one permission code | API Documentation |
| 4 | Record level | The record's company, branch, department and owner must be inside the assignment. Out of scope returns not found | FR-015 |
| 5 | Organization level | Company isolation in the database as well as in code | FR-019, ADR-002 |
| 6 | Branch and department | Scope dimensions in the policy component | ADR-006 |
| 7 | Data ownership | `own_only` assignments restrict to the creator's records | FR-010 |
| 8 | Approval authority | Value limits are attributes of approval chain steps, not of roles | FR-043 |
| 9 | Approval delegation | Time-boxed and per document type, audited with both names | FR-044 |
| 10 | Maker-checker | A user cannot approve a document they created | FR-042, BRL-008 |
| 11 | Segregation of duties | Conflict list prevents hard conflicts and flags record-level ones | FR-017 |
| 12 | Temporary access | Grants with start, end and reason expire automatically | FR-016 |
| 13 | Audit | Every permission change, refusal and sensitive read is logged | FR-030 |

## 5. Segregation of duties

Hard conflicts must never be held by one role or one user. Record-level conflicts may coexist in a role, but the system refuses them on the same record.

| ID | Permission A | Permission B | Type | Reason |
|---|---|---|---|---|
| SOD-01 | `proc.vendor.create` | `finance.payment.approve` | Hard | Create vendor and approve payments (BRL-058) |
| SOD-02 | `proc.vendor.update` | `finance.payment.approve` | Hard | Change vendor bank details and approve payments (BRL-058) |
| SOD-03 | `finance.journal.create` | `finance.journal.approve` | Record-level | Prepare and approve the same journal (BRL-008) |
| SOD-04 | `proc.po.create` | `proc.grn.create` | Hard | Order goods and receive them |
| SOD-05 | `proc.invoice.create` | `finance.payment.approve` | Hard | Enter vendor invoices and approve their payment (BRL-057) |
| SOD-06 | `finance.payment.create` | `finance.payment.approve` | Record-level | Prepare and approve the same payment (BRL-057) |
| SOD-07 | `inv.adjustment.create` | `inv.adjustment.approve` | Record-level | Create and approve the same stock adjustment (BRL-008) |
| SOD-08 | `identity.role.assign.company`, `identity.role.assign.branch` | `finance.payment.approve`, `finance.payment.release` | Hard | Grant access and use financial approval (v0.2: split codes) |
| SOD-09 | `sales.creditnote.create` | `sales.creditnote.approve` | Record-level | Create and approve the same credit note (BRL-008) |
| SOD-10 | `finance.bank.create` | `finance.payment.approve` | Hard | Maintain bank accounts and approve payments |
| SOD-11 | `finance.bank.update` | `finance.payment.approve` | Hard | Change bank accounts and approve payments |
| SOD-12 | `workflow.chain.update` | `workflow.approval.approve` | Hard | Change approval chains and approve documents |
| SOD-13 | `identity.role.assign.company`, `identity.role.assign.branch` | `workflow.approval.approve` | Hard | Grant access and approve documents (v0.2: split codes) |
| SOD-14 | `finance.payment.approve` | `finance.payment.release` | Record-level | Approve and release the same payment (BRL-068) |
| SOD-15 | `finance.payment.create` | `finance.payment.release` | Record-level | Prepare and release the same payment (BRL-068) |
| SOD-16 | `finance.payment.reverse` | `finance.payment.approve` | Record-level | Request and approve the same payment reversal (BRL-068) |
| SOD-17 | `finance.journal.create` | `finance.journal.reverse` | Record-level | Create a journal and reverse it without a second person (BRL-069) |
| SOD-18 | `finance.journal.reverse` | `finance.journal.approve` | Record-level | Request and approve the same reversal journal (BRL-069) |
| SOD-19 | `proc.vendor.create` | `proc.vendor.approve` | Record-level | Create and activate the same vendor (BRL-008) |
| SOD-20 | `proc.vendor.bank.request` | `proc.vendor.bank.approve` | Hard | Request and approve vendor bank details (BRL-058, BRL-070) |
| SOD-21 | `proc.vendor.bank.verify` | `proc.vendor.bank.request` | Hard | Verify what you requested (FR-139) |
| SOD-22 | `proc.vendor.bank.approve` | `finance.payment.release` | Record-level | Approve a new bank account and release the first payment to it (FR-139) |
| SOD-23 | `sales.customer.creditlimit.request` | `sales.customer.creditlimit.approve` | Hard | Request and approve a credit limit (BRL-070) |
| SOD-24 | `finance.period.reopen.request` | `finance.period.reopen.approve` | Hard | Request and approve a period reopen (BRL-071) |
| SOD-25 | `finance.period.reopen.approve` | `finance.journal.create` | Hard | Approve a reopen and post into the reopened period (BRL-071) |
| SOD-26 | `platform.config.request` | `platform.config.approve` | Record-level | Request and approve the same controlled change (BRL-070) |
| SOD-27 | `platform.sod.override.request` | `platform.sod.override.approve` | Hard | Request and approve an SoD override (FR-142) |
| SOD-28 | `identity.role.assign.company` | `identity.role.grant.approve` | Hard | Grant a role and approve the grant (BRL-067) |
| SOD-29 | `identity.user.create` | `identity.role.grant.approve` | Record-level | Create a user and approve a grant to that user (BRL-067) |
| SOD-30 | `identity.breakglass.release` | `identity.breakglass.review` | Hard | Release emergency access and review its use (BRL-072) |
| SOD-31 | `inv.adjustment.submit` | `inv.adjustment.approve` | Record-level | Submit and approve the same stock adjustment (BRL-008) |
| SOD-32 | `proc.po.update` | `proc.po.approve` | Record-level | Edit a purchase order and then approve it, even if someone else created it (BRL-008 extended to last editor) |

Handling of exceptions: a hard conflict may be overridden only by a recorded, time-limited override requested by an Organization Admin, approved by a Super Admin, and reported to the Auditor (FR-017, FR-142, SOD-27). v0.2 replaces the v0.1 wording "approved by an Organization Admin". Small companies may need overrides. The policy is TBD (new question OQ-032).

## 6. Approval authority (structure only)

Approval limits are company data, not defaults of this document. Value limits are TBD (OQ-013).

| Document type | Initiator roles | Approver roles (chain, in order of authority) |
|---|---|---|
| Purchase requisition | Employee, Department Manager, Project Manager | Department Manager, Procurement Manager |
| Purchase order | Procurement Officer, Procurement Manager | Procurement Manager, Finance Manager (high value) |
| Vendor activation | Procurement Officer | Procurement Manager |
| Vendor bank change | Procurement Officer or Procurement Manager (request), Accountant (verify) | Finance Manager (never the requester or verifier). First payment held (FR-139) |
| Purchase invoice with variance | Accountant | Finance Manager |
| Vendor payment | Accountant | Finance Manager |
| Manual journal | Accountant | Finance Manager |
| Period reopen | Finance Manager (request) | Organization Admin, PENDING APPROVAL (OQ-032, OQ-034). Never the requester |
| Stock adjustment | Warehouse Staff, Inventory Manager | Inventory Manager (below threshold), Finance Manager (above) |
| Sales order over credit limit | Sales Executive | Finance Manager |
| Credit limit change | Sales Manager | Finance Manager (FR-141) |
| Approval limit, posting rule, SoD rule, retention rule change | Organization Admin or Finance Manager | The other of the two, never a holder of the authority being changed (FR-138) |
| Role grant (Manager tier, Auditor, scope widening) | Organization Admin | Super Admin (FR-133) |
| Sales order discount (policy TBD) | Sales Executive | Sales Manager |
| Credit note | Accountant | Finance Manager |
| Sales return outside policy | Sales Executive | Sales Manager |
| Import commit (master data) | Data owner | None by default, validate-only first |

## 7. Delegation and temporary access rules

1. Delegation transfers approval authority only for listed document types and dates (FR-044, BRL-012).
2. The delegate must hold the permission and scope. Delegation does not grant new permissions.
3. Delegation is refused if the delegate created the item. Such items go to the next approver (UC-016).
4. Chained delegation is not allowed in this draft (OQ-026).
5. Temporary role grants carry a reason and an end date and expire automatically (BRL-059). They are shown to the Auditor.

## 8. Checks performed on this matrix (script, 2026-11-11, v0.1 figures)

These figures describe v0.1 and are kept as history. The v0.2 checks are in section 8A.12 and in the Final Revalidation Report.

| Check | Result |
|---|---|
| Permission codes in the catalogue | 211 |
| Permission codes used by the API endpoints that exist in the catalogue | All 132 distinct codes found, 169 endpoints |
| Hard SoD conflicts inside any default role | 0 |
| Auditor holds only read, export or view permissions | Yes |
| Read-only User holds only read permissions | Yes |
| Super Admin holds any finance, procurement, inventory or sales permission | No |
| Roles in the matrix | 18 |

Limits of this check: it proves internal consistency and consistency with the API document. It does not prove the permission sets are what the business needs. That needs workshops with each department.

## 8A. Authorisation hardening (v0.2)

Added by the Phase 2 remediation to close the RBAC audit findings E1 to E10 and the missing-rule list. Requirements: FR-131 to FR-150. Rules: BRL-066 to BRL-074. Security requirements: SEC-054 to SEC-073.
Status: Proposed. Items marked PENDING APPROVAL need a stakeholder decision and are not confirmed.

### 8A.1 Role authority tiers

| Tier | Meaning | Roles |
|---|---|---|
| T0 | Platform | Super Admin (SA) |
| T1 | Organisation | Organization Admin (OA) |
| T2 | Manager | Branch Admin (BA), Department Manager (DM), HR Manager (HM), Finance Manager (FM), Procurement Manager (PM), Inventory Manager (IM), Sales Manager (SM) |
| T3 | Operator | HR Executive (HE), Accountant (AC), Procurement Officer (PO), Warehouse Staff (WS), Sales Executive (SE), Project Manager (PJ) |
| T4 | Basic | Employee (EM), Read-only User (RO) |
| Oversight | Outside the ladder | Auditor (AU). Granted only with four-eyes approval and never combined in one company with a role that can create, update, approve, release or reverse |

Rule (BRL-066): a grantor can grant only roles of a tier number higher than the grantor's own (a lower authority), in the grantor's own scope, never to the grantor's own account.
Custom roles inherit the tier of the highest-tier permission they contain, using the tier table in 8A.3. A custom role that contains approve, release, reverse, reopen or grant permissions is T2 at best.

### 8A.2 Who may grant what

| Grantor | May grant | May not grant | Second approval needed |
|---|---|---|---|
| SA | Organization Admin (T1) | Any business role. Itself. | Yes, a second Super Admin. At least two Super Admin users are required (FR-133) |
| OA | T2, T3, T4 roles and Auditor, in assigned companies | OA, SA, itself, any role to a user outside its companies | T2 roles, Auditor, any scope widening to another company, any custom role with approve, release or reverse. Approver is a Super Admin (`identity.role.grant.approve`) |
| BA | T3 and T4 roles, own branches only, existing users in those branches | T2 or higher, any company-wide role, itself | No, but every grant is audited (AUD-101) |
| All other roles | Nothing | | |

A Pending grant has no effect until approved. The grantor, the grantee and the creator of the user can never be the approver (SOD-28, SOD-29, FR-132).
PENDING APPROVAL (OQ-033): whether a second Organization Admin of another company may approve instead of a Super Admin when only one Super Admin team exists.

### 8A.3 Permission ownership

| Control area | Maker | Checker | Owner of the rule |
|---|---|---|---|
| Role definitions and tiers | SA (custom role definition) | OA (assignment) | Security officer |
| Role grants | OA, BA (within limits) | SA | Security officer |
| Approval limits and chains | OA | FM (never a holder of a limited step) | Finance owner |
| Posting rules, chart of accounts rules | FM | OA | Finance owner |
| SoD rules and overrides | OA | SA | Security officer |
| Retention rules | SA | OA | Compliance officer |
| Vendor bank details | PO or PM | AC verifies, FM approves | Procurement and Finance owners |
| Customer credit limits | SM | FM | Sales and Finance owners |
| Payments | AC prepares | FM approves, a different FM releases above the limit | Finance owner |
| Journals | AC or FM creates | A different FM approves | Finance owner |
| Period reopen | FM | OA (PENDING APPROVAL) | Finance owner |
| Break-glass | SA releases (two people) | AU reviews | Security officer |

Tier of a permission (for custom roles): approve, release, reverse, reopen, grant, override and controlled-change approve are T2 minimum. create, update and submit are T3 minimum. read and view are T4.

### 8A.4 Maker-checker and duty separation by transaction

| Transaction | Duties | Hard rule |
|---|---|---|
| Payment or refund | prepare (AC), approve (FM), release (FM) | Three different users on one payment (BRL-068, SOD-14, SOD-15) |
| Payment reversal | request (FM), approve (another FM) | SOD-16. Step-up on both |
| Manual journal | create (AC or FM), approve (FM) | Approver is never the creator. System posts at final approval (BRL-069, SOD-03) |
| Reversal journal | request (FM), approve (another FM) | SOD-17, SOD-18 |
| Purchase order | create (PO or PM), edit, approve (PM, FM) | Approver is never the creator or last editor (SOD-32) |
| Vendor activation | create (PO or PM), approve (PM) | Never the same user (SOD-19) |
| Vendor bank details | request (PO or PM), verify (AC), approve (FM) | Three different users. First payment to the new account waits for the hold period (SOD-20 to SOD-22, FR-139) |
| Stock adjustment | create, submit, approve | Approver is never the creator or submitter (SOD-07, SOD-31) |
| Credit note | create (AC), approve (FM) | SOD-09 |
| Credit limit | request (SM), approve (FM) | SOD-23 |
| Period reopen | request (FM), approve (OA) | SOD-24, SOD-25 |
| Controlled configuration | request (OA or FM), approve (the other) | SOD-26 |
| Role grant | grant (OA or BA), approve (SA) | SOD-28, SOD-29 |

Where only one person holds the approving role in a company, the transaction waits. It never falls back to self-approval. A company that cannot staff two Finance Managers must either use the SoD override (FR-142) or raise the payment limit that triggers two approvers. This is a business consequence that needs sponsor acceptance (OQ-034).

### 8A.5 Approval limits and multi-level approval

Values are company data and remain TBD (OQ-013). The structure is fixed here.

| Document | Level 1 | Level 2 (above limit A) | Level 3 (above limit B) | Rule |
|---|---|---|---|---|
| Vendor payment | FM | A second FM, different user | PENDING APPROVAL (OQ-034): a role above FM, for example a Finance Director | Each level a different user. Release by a user who did not approve |
| Purchase order | PM | FM | Same as payment level 3 | Approver is never creator or last editor |
| Manual journal | FM (different from creator) | Second FM above limit A | | |
| Stock adjustment | IM | FM above limit A | | |
| Credit note and refund | FM | Second FM above limit A | | |
| Sales discount | SM | FM | | |

Rules:
1. An approval step has a role, a value range and a minimum count of distinct approvers.
2. Steps run in order. The creator, last editor and releaser are skipped (BRL-008).
3. A change of value, vendor or bank details after approval restarts the chain (FR-092).
4. A step with no eligible approver pauses and alerts. It never auto-approves and never skips (FR-043 refined).
5. Approval limits themselves are controlled data (BRL-070).

### 8A.6 Sensitive actions that require step-up authentication

Release payment. Reverse payment. Approve vendor bank details. Approve period reopen. Close fiscal year. Approve role grant. Approve SoD override. Approve controlled change. Release break-glass. Export audit log. Export users. Bulk export. Unmask a sensitive field. Change retention rules. Create or approve a service account.
Step-up means the user has re-authenticated, with MFA where enabled, within the last 5 minutes (proposed value, PENDING APPROVAL). The proposal also needs OQ-023 to be answered for the MFA method.

### 8A.7 Delegation

BRL-073 and FR-143 apply. In addition:
1. Delegation of approval is limited to document types the delegator can approve and to a maximum of 30 days (proposed, PENDING APPROVAL).
2. A delegate cannot be a user who created the document or who holds a conflicting permission for it.
3. The audit record names delegator, delegate and document (AUD-121).
4. A delegation ends when the delegator or the delegate is deactivated or loses the permission.

### 8A.8 Emergency access (break-glass)

| Item | Rule |
|---|---|
| Account | One sealed account per tenant, no password known to any single person, disabled by default |
| Release | Two different Super Admin users, each with step-up, with a reason (`identity.breakglass.release`) |
| Duration | Short and fixed. Proposed 2 hours, PENDING APPROVAL (OQ-036) |
| Permissions | Platform recovery only (reset administrators, restore access, disable a compromised user). No finance, procurement, inventory or sales permission. No approve, release, reverse, reopen or grant |
| Recording | Session recorded as audit events AUD-122 to AUD-124. Alert to all Super Admin users and the Auditor on release |
| Review | The Auditor reviews every use within 2 business days (`identity.breakglass.review`) |
| Closure | Credentials rotate after use. The account returns to disabled |

### 8A.9 Sensitive transaction restrictions

1. Attachments on submitted, approved or posted documents cannot be deleted. They can be voided by an Organization Admin with a reason (FR-147).
2. Posted records are never edited. Corrections are new documents (PRD principle 3).
3. A user cannot approve their own SoD override, role grant, controlled change or reopen request (FR-132).
4. Exports require read permission on the module and a module export permission (FR-148).
5. Service accounts never hold approve, release, reverse, reopen or grant permissions (FR-149).

### 8A.10 Controlled configuration keys

Controlled (maker-checker, FR-138): approval limits and chains, posting rules, numbering of posted documents, SoD rules, retention rules, credit limits, vendor bank details, payment hold period, step-up window, break-glass duration.
All other settings use `platform.settings.manage` with history.
The settings API rejects a direct change to a controlled key with CTRL-001 (see API Documentation).

### 8A.11 Resolved or clarified v0.1 ambiguities

| Ambiguity found by the audit | v0.2 rule |
|---|---|
| Which approve code applies | The decision endpoint requires both `workflow.approval.approve` and the document-type approve code. Holding only the generic code cannot approve a finance document |
| Employee role stacking | EM is added only to users with at least one T3 or T2 assignment. Auditor and Read-only users do not receive EM |
| Role names versus permissions in SoD | SoD is evaluated on permissions, across all assignments and all companies (BRL-074) |
| 403 versus 404 for scope | An unassigned company or an out-of-scope record returns the same 404 body. 403 is reserved for a missing permission inside the scope |
| Team and "unless widened" | PENDING APPROVAL (OQ-037): team means users sharing a department assignment, and widening needs a four-eyes grant |
| Shared master data versus company predicate | PENDING APPROVAL (OQ-037). Until decided, master data is company-owned and shared data is out of the MVP |
| Group and cross-company functions | Out of the MVP. FR-019 allows them only through explicit permission-gated functions that do not exist yet (recorded as a gap, not a defect) |
| Revocation timing | Access token validity is 5 minutes and every request checks the revocation list. The 15-minute figure of v0.1 is replaced (see Authentication and Authorization section 3, v0.2) |
| Chain limits versus settings | Limits live in approval chain steps. FR-027 settings hold only the thresholds that choose a chain, and both are controlled data |

### 8A.12 Checks on the v0.2 design (documentation checks)

The scripted checks are recorded in the Final Revalidation Report. They cover: no hard SoD pair inside one default role, Super Admin holds no business permission, Auditor holds no write permission, Read-only holds no write permission, every new permission code is used by an endpoint or an FR, and every FR-131 to FR-150 has a test case. Those results are not stated here because they are produced after this section is written.

## 9. Open points

OQ-013 (limits), OQ-026 (chained delegation), OQ-032 (SoD override policy and who may reopen a period), OQ-033 to OQ-037 (added in v0.2).
