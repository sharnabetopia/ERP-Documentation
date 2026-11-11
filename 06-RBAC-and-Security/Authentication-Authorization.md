# Authentication and Authorization Specification

Document status: Draft v0.2 (Phase 2 remediation, 2026-11-11). Sections 3 to 8 carry v0.2 corrections. Section 10 is new. Proposed. Values marked "default" are proposals.
Related: [Security Requirements](Security-Requirements.md), [RBAC Matrix](RBAC-Matrix.md), [ADR-006](../04-System-Architecture/Architecture-Decision-Records.md), [ADR-013](../04-System-Architecture/Architecture-Decision-Records.md), [Sequence Diagrams](../05-Workflows-and-Diagrams/Sequence-Diagrams.md) (authentication and authorization flows).

## 1. Evaluation of access control models

| Model | Strength | Weakness | Use in this design |
|---|---|---|---|
| RBAC | Simple, auditable, matches job roles | Role explosion when scope varies | Used for permissions (what actions) |
| ABAC | Expressive for scope and context | Harder to audit and test | Used for scope attributes (company, branch, department, owner, document status, value) |
| Relationship or ownership rules | Natural for "own records" | Needs care for team hierarchies | Used through `own_only` and reporting lines |
| Chosen | RBAC for permission plus attribute rules for scope (ADR-006) | | |

## 2. Identity and credentials

| Topic | Specification |
|---|---|
| Identifier | Email address, unique per tenant, case-insensitive (FR-008) |
| Password storage | Salted slow hash (SEC-001) |
| Password policy | Default 12 characters minimum, breached-password check, history 5 (SEC-002) |
| Lockout | Default 5 failures then 15 minutes (SEC-003) |
| MFA | Time-based codes plus recovery codes. Mandatory for Super Admin, Organization Admin and payment-releasing roles (SEC-006). Optional or mandatory for others per OQ-023 |
| SSO | OpenID Connect proposed (SEC-007). Local login can be disabled per company when SSO is enforced. Emergency access follows RBAC Matrix 8A.8 and FR-144 (sealed account, two-person release, expiry, no financial authority) |
| Provisioning | Invitation by an administrator. Activation link single use (SEC-005) |
| Deprovisioning | Deactivate user, revoke sessions, retain history (FR-009) |
| Service accounts | Named owner, narrow permission set, scoped expiring tokens, rotation, no interactive login. Never hold approve, release, reverse, reopen or grant permissions (FR-149, SEC-072). Created by an Organization Admin, approved by a Super Admin |

## 3. Session and token model

| Item | Default | Notes |
|---|---|---|
| Access token lifetime | 5 minutes (v0.2, was 15, PENDING APPROVAL) | Carries user id, tenant, session id. No permission list, to avoid stale rights |
| Refresh token | Bound to session, rotated on each use, 8 hours sliding | Reuse of a rotated token revokes the session |
| Idle timeout | 30 minutes | Configurable per company within limits |
| Absolute session lifetime | 12 hours | |
| Concurrent sessions | Allowed, listed to the user, revocable | Optional per-company limit |
| Revocation | Server-side session record and revocation list checked on every request (v0.2, was on refresh only). Revocation takes effect within 60 seconds (SEC-010) | Permission cache invalidated on role change (SEC-010) |
| Step-up authentication | Required within the last 5 minutes (proposed, PENDING APPROVAL) for the actions in RBAC Matrix 8A.6 (SEC-022, SEC-068) | A missing step-up returns `STEP-001` |
| Storage on client | Not readable by scripts where possible (SEC-012) | |

## 4. Authorisation decision procedure

For every request the policy component performs these steps in order. The first failure ends the request.

| Step | Check | Failure result |
|---|---|---|
| 1 | Session valid, user active, account not locked | 401 |
| 2 | Tenant and company context set from the session and the selected company is assigned to the user | 404 `NOTF-001`, same body as an unknown company (v0.2, was 403, avoids leaking company existence) |
| 3 | Endpoint permission code held through an active role assignment (not expired, not retired) | 403 `AUTHZ-001` |
| 4 | Scope: for a record, load its company, branch, department, owner. Match against the assignment that carries the permission | 404 `NOTF-001` |
| 5 | State rule: the action is allowed in the document's current status | 409 `STATE-001` |
| 6 | Business authority: for approvals, the approver holds authority for the value and is not the creator, or is a valid delegate | 403 `DUTY-002` |
| 7 | Segregation: hard conflict and record-level conflict checks, role tier and self-grant checks for grants | 409 `DUTY-001`, 403 `DUTY-002`, 403 `TIER-001`, 403 `SELF-001` |
| 8 | Step-up: recent authentication for privileged actions | 401 with step-up challenge |
| 9 | Execute. Audit written in the same transaction (ADR-011) | |

List and report queries apply step 4 as a filter in the query, not after loading rows (ADR-006), so counts and totals do not leak.

Refusals at steps 3, 4, 6 and 7 are audited. Repeated refusals raise an alert (SEC-047).

## 5. Role assignment model

A user can hold several role assignments. Each assignment has: role, company, optional branch, optional department, `own_only` flag, valid from and to, and a reason for temporary grants.

| Rule | Detail |
|---|---|
| Effective permissions | Union of permissions of active assignments that match the target record's scope |
| Deny by default | No assignment, no access (BRL-004) |
| No explicit deny rules in MVP | Keeps evaluation simple. Revisit if needed |
| Assignment limits | Governed by role tiers (RBAC Matrix 8A.1, 8A.2, BRL-066). A grantor can assign only a role of a lower authority tier, within their own scope, never to themselves. Branch Admin grants T3 and T4 only. Manager-tier, Auditor and approve/release/reverse roles need a second approver |
| Custom roles | Created from the permission catalogue by Super Admin. Tier is the highest tier of any permission inside (RBAC Matrix 8A.3). SoD is checked on permissions at creation and at assignment (BRL-074). v0.1 text "cannot include permissions the creator does not hold" is withdrawn because Super Admin holds no business permissions (audit E4). Replaced by: a custom role with approve, release, reverse, reopen or grant permissions needs four-eyes approval before use |
| System roles | The 18 defaults are read-only. Customisation is by copying to a custom role |

## 6. Multi-tenant data isolation

| Layer | Control |
|---|---|
| Session | Tenant and selected company are fixed at session start and when the user switches company |
| Application | Policy component adds tenant and company predicates to every query |
| Database | Row-level security bound to session variables (SEC-016) |
| Storage | Object keys are prefixed by tenant. Access only through authorised links |
| Cache | Keys include tenant and company |
| Jobs and events | Each job carries tenant and company context and runs under it |
| Tests | Isolation test matrix by role and scope runs in every release (INV-12) |

## 7. Approval authority and delegation (technical rules)

| Rule | Detail |
|---|---|
| Authority source | Approval chain steps hold a role and a value range. Value limits are company data (OQ-013) |
| Routing | First step whose range covers the document value, then subsequent steps in order |
| Creator exclusion | The creator is skipped as approver at every step (BRL-008) |
| Delegation | Delegate holds delegator's authority for listed types and dates. Delegate must hold the underlying permission and scope. Audit records both users |
| Escalation | An overdue step goes to the next authority (BRL-013) |
| Re-evaluation | A value or vendor change after approval restarts the chain (FR-092) |

## 8. Temporary access

Temporary assignments have start, end, reason and a named approver, and follow the same tier and four-eyes rules as permanent grants. Maximum duration 30 days (proposed, PENDING APPROVAL). SoD is checked when the grant stacks with existing roles. They expire automatically, appear in a report for the Auditor, and cannot be extended by the grantee.

## 9. Open points

| Point | Question |
|---|---|
| MFA and SSO scope, local login when SSO is enforced | OQ-023 |
| Chained delegation | OQ-026 |
| SoD override policy and who reopens a period | OQ-032, OQ-034 |
| Grant approver, payment limits, bank hold, break-glass, master data ownership | OQ-033 to OQ-037 |
| Token and session values | Confirm with the security officer |

## 10. Authorisation hardening (v0.2)

| Topic | Specification | Source |
|---|---|---|
| Grant request | A grant that needs a second approver is stored as Pending and has no effect. It is decided through the approval engine under `identity.role.grant.approve` | FR-133, BRL-067 |
| Tier check | Before any grant is stored, the policy component compares the target role tier with the grantor's highest tier and the target scope with the grantor's scope. Failure returns `TIER-001` | FR-131, FR-134 |
| Self-grant check | Any request whose grantee is the actor returns `SELF-001` and is audited (AUD-105) | FR-132 |
| Payment release | Step 7 checks the payment record: preparer, approver and releaser must differ. Step 8 requires step-up. Failure returns `DUTY-002` or `STEP-001` | FR-135, FR-137 |
| Controlled change | The settings and master data endpoints refuse a direct change to a controlled key and return `CTRL-001`. The change is made through a change request | FR-138 |
| Emergency access | Sealed account, two-person release, expiry, recorded actions, post-use review | FR-144, RBAC Matrix 8A.8 |
| Permission evaluation | Permissions come from role assignments checked per request. A custom role is evaluated on its permissions, not its name | BRL-074 |
