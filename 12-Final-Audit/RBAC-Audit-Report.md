# RBAC and Security Audit Report

Audit date 2026-11-11. Related: [Documentation Audit](Documentation-Audit-Report.md), [Final Readiness](Final-Readiness-Assessment.md), [Critical Gaps](Critical-Gaps-and-Risks.md).
Scope: the 7 files in `06-RBAC-and-Security/`, checked against FRS, User Stories, Business Rules and the workflows.
Method: full read by a reviewing agent, then my own spot checks of the key claims (Permission-Matrix lines 36, 38, 51, 114 and SOD rows 146 to 157 were confirmed).
Anything not read in full is marked NOT VERIFIED. Permission-Matrix totals (211 codes) were recounted by the agent and matched.

## 1. Checklist result

| Item | Status | Evidence |
|---|---|---|
| All roles identified | PARTIAL | 18 roles in `RBAC-Matrix.md` section 2. Story roles with no definition: implementation lead (US-028), IT manager (US-080), privacy officer (US-079), compliance officer (US-078), data owner (US-017), checker (US-037), generic manager, approver, requester. "Planner" in `Cross-Module-Workflows.md` is undefined. HR Executive, Project Manager and Read-only have no story. |
| Every module has permission definitions | FAIL | No rows for M-001 Dashboard, M-012 Notifications, M-015 Localization, M-065 Stock Reservation. Partial for M-030, M-044, M-002, M-033, M-034, M-032, M-064, M-116. |
| CRUD mapped | PARTIAL | Delete exists only as `platform.attachment.delete`. "D: draft delete" in RBAC section 1 has no code. Post, reverse and release are folded into "manage". |
| Approve and reject | PASS | Paired, 15 each. Cancel for 6 areas only. |
| Organization (company) restriction | PASS | BRL-002, RBAC section 3, 4, Authentication-Authorization sections 4 to 6. |
| Branch restriction | PARTIAL | RBAC section 3 and layer 6. Branch Admin rule only. No document-level branch attribution rule. |
| Department restriction | PARTIAL | One rule: department manager sees own department and sub-units. |
| Record-level access | PARTIAL | `own_only` layer 7. "Team" and "unless widened" are undefined, and there are no own/team/all tiers. |
| Maker-checker | PARTIAL | BRL-008 and FR-042 (creator cannot approve), plus SOD-03, 06, 07, 09 at record level. Gaps in section 3 below. |
| Segregation of duties | PARTIAL | SOD-01 to SOD-13 (9 hard, 4 record-level). Several high-risk pairs missing. |
| Audit logging | PASS with gaps | AUD-001 to AUD-100, append-only, hash chain. |
| Authentication | PARTIAL | MFA, session, lockout, reset defined. SSO claim mapping, service accounts, API tokens, impersonation undefined. |
| Authorization | PASS with gaps | 9-step decision procedure. Contradictions in section 5. |
| Threats and mitigations | PARTIAL | THR-001 to THR-044 mapped to SEC. Missing topics in section 4. |

## 2. Privilege escalation risks demonstrable from the text

| # | Risk | Evidence |
|---|---|---|
| E1 | Branch Admin holds `identity.role.assign` and `identity.user.create`. The rule "roles at or below their own authority" has no authority ranking, so nothing bars assigning company-level roles such as Finance Manager, or self-assignment. | `Permission-Matrix.md` line 51 (OA, BA), `Authentication-Authorization.md` section 5 |
| E2 | Organization Admin can create users, assign roles and edit approval chains with no checker. SOD-08 and SOD-13 cover only `payment.approve` and `workflow.approval.approve`. OA can give itself Accountant, Inventory Manager, Sales Manager or Procurement Officer roles. | `RBAC-Matrix.md` section 5 |
| E3 | No permission code for SoD override or for editing the SoD list, yet OA approves overrides. US-011 lists "override by the same admin" with no rule. | `RBAC-Matrix.md` section 5, `User-Stories.md` US-011 |
| E4 | Super Admin may create custom roles but holds no business permissions, so it cannot build a business role without holding the permissions. US-008 ("access matches each company's practice") cannot be met. | `Authentication-Authorization.md` section 5 vs US-008 |
| E5 | Every staff user also holds the Employee role, which carries `proc.pr.create`, `proc.pr.submit`, `platform.attachment.delete`. That conflicts with SEC-018 and the section 8 check "Auditor holds only read". Whether Auditor and Read-only count as staff is undefined. | `RBAC-Matrix.md` sections 1 and 8 |
| E6 | `platform.attachment.delete` via Employee could remove evidence on posted or approved documents. No rule blocks it despite BRL-060. | `Permission-Matrix.md` line 114 |
| E7 | Two approve codes exist (`workflow.approval.approve` and document-specific ones). Which is checked is unstated. A custom role built from document-specific codes bypasses SOD-13. | Permission-Matrix header, `RBAC-Matrix.md` SOD-13 |
| E8 | Temporary access uses `identity.role.assign`. Section 8 requires an approver, RBAC section 7 does not. No maximum duration, no SoD check when grants stack. | FR-016, BRL-059 |
| E9 | Audit retention can be shortened: Super Admin holds `platform.retention.manage`. | `Permission-Matrix.md` |
| E10 | Revocation timing contradiction: a 15-minute access token, revocation checked on refresh (section 3), but SEC-009 says next request and SEC-010 within 60 seconds. | `Authentication-Authorization.md` section 3 vs `Security-Requirements.md` |

## 3. Missing or weak access-control rules

| Area | Gap |
|---|---|
| Journal posting | Finance Manager creates, approves and manages (reverses) journals. Only record-level SOD-03. |
| Payment release | No pair between release (`manage`) and approve. FM holds both. BRL-057 says no one both approves and pays. |
| Payable chain | FM holds `proc.invoice.approve` and `finance.payment.approve` on the same payable. No SoD. |
| Vendor bank change | RBAC section 6 says Finance Manager approves, but FM holds only `proc.vendor.read`. The only vendor approve code sits with Procurement Manager, who also updates. No code for bank change. SOD-02 pairs `vendor.update` with `payment.approve` only. |
| PO approval | Procurement Manager creates, updates and approves. Only the creator rule applies, so a non-creator editor may approve. |
| Bank accounts | SOD-10 and 11 cover create and update only. FM holds `bank.manage` and `payment.approve`. |
| Approval limits | SOD-12 covers `workflow.chain.update`. FM also holds `platform.settings.manage` (limits live in settings per FR-027) and `finance.postingrule.manage`. |
| Stock adjustment | Inventory Manager creates, submits, approves. SOD-07 is record-level only. |
| Period close and reopen | No maker-checker. Reopen approver is "higher than FM" but no such role exists (OQ-032). |
| Credit limit change | AUD-033 exists. No code, no checker. Sales roles hold `sales.customer.update`. |
| Export | One generic `platform.export.export`. No per-module export code, no rule tying export to read. No code for user export (SEC-022), unmask (AUD-074, DP-003), or subject export (AUD-081). |
| Group or cross-company | FR-019 and SEC-017 promise permission-gated group functions. No code or role exists. |
| Shared master data | US-017 shared reference data conflicts with the company predicate on every query (section 6). |
| Scope response | Unassigned company returns 403, records return 404. This leaks company existence. |
| SoD across assignments | Effective permissions are a union. Whether a conflict across company A and B counts is undefined. |
| Receipts | Vendor payments, customer receipts and refunds share `finance.payment`, so granting receipts grants payment authority. |
| Audit read split | `platform.audit.read` is flat: no security versus business split, no entity-type limit for Branch Admin and Finance Manager. |
| Audit events missing | API token and service account use, break-glass use, IdP mapping changes, SoD list edits. Report views and ledger inquiry are not audited. |
| Payroll SoD | Not defined. Deferred (payroll is R2). Not a defect for the MVP. |

## 4. Threat model gaps

Present: STRIDE threats THR-001 to 030 and ERP abuse threats THR-031 to 044, including IDOR, mass assignment, self-grant, CSV injection, tenant bypass, export leakage, webhooks, backups and assertion forgery.
Missing or weak:
- custom-role escalation and Super Admin to Org Admin chains
- SSO claim or email account-linking abuse (distinct from assertion forgery)
- file upload malware (THR-010 covers imports only)
- XSS, CSRF and clickjacking (SEC-025 to 027 are not mapped to a threat)
- service account and integration credential theft, break-glass abuse, SoD override abuse, temporary-grant abuse
- permission cache staleness, retention shortened by an insider
- SEC-022 step-up is P1 but THR-025, 026 and 036 depend on it as a mitigation

## 5. Authentication and authorization gaps

- SSO: claim-to-role mapping and just-in-time provisioning are not defined. US-005 has an "email changed at provider" edge with no rule.
- Service accounts: one line, no role model, no scopes, not among the 18 roles.
- User API tokens, impersonation and support access, admin-initiated reset, session revocation on reset, email-change reverification: not defined.
- Break-glass: named, but role, custody and audit events are undefined.
- MFA is mandatory for Super Admin, Organization Admin and payment-releasing finance roles only. MFA story US-004 is P1 while SEC-006 is P0.

## 6. Contradictions

- `RBAC-Matrix.md` section 6 and `Authentication-Authorization.md` section 7 put limits in chain steps. FR-027 puts thresholds in settings.
- Authentication-Authorization section 4 step 1 checks the session every request. Section 3 checks it on refresh only.
- US-004, 005, 010 priorities versus the security requirements and OQ-023.

Overall RBAC status: PARTIAL. The structure is strong. The access-control rules for the highest-risk actions are not yet safe to implement as written.
