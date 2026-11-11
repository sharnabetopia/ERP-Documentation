# Security Requirements

Document status: Draft v0.2 (Phase 2 remediation). v0.1 (Stage 2 part 2). Proposed. Numeric values (lengths, lifetimes, limits) are proposed defaults to confirm with the security officer (OQ-023) and are marked "default".
IDs: `SEC-nnn`. Each links to the functional, non-functional or business requirement it refines, and to the OWASP Top 10 (2021) category where one applies.
Related: [Authentication and Authorization](Authentication-Authorization.md), [Threat Model](Threat-Model.md), [Data Protection](Data-Protection.md), [Audit Trail](Audit-Trail.md), [NFR](../03-Product-and-System-Requirements/NFR.md).

Verification: T test, I inspection, A analysis, D demonstration.

## 1. Authentication and session

| ID | Requirement | Linked | OWASP | Pri | Verif. |
|---|---|---|---|---|---|
| SEC-001 | Passwords shall be stored only as salted hashes made with a current, deliberately slow password hashing function. No reversible storage. | FR-001 | A02, A07 | P0 | I, T |
| SEC-002 | Password policy default: minimum 12 characters, no composition tricks required, checked against a list of common and breached passwords, history of last 5, no forced rotation unless compromise is suspected. | FR-002 | A07 | P0 | T |
| SEC-003 | Lockout default: 5 consecutive failures lock the account for 15 minutes, with progressive delay. An administrator can unlock. Lockout state is per account and per source address. | FR-003, BRL-005 | A07 | P0 | T |
| SEC-004 | Sign-in, reset and activation responses shall not reveal whether an account exists. | FR-005 | A07 | P0 | T |
| SEC-005 | Reset and activation tokens shall be random, single-use, stored hashed, and expire (default 30 minutes for reset, 7 days for activation). | FR-005 | A07 | P0 | T |
| SEC-006 | MFA shall be available with time-based codes and recovery codes, and enforceable per company. It shall be mandatory for Super Admin, Organization Admin and Finance roles that release payments. | FR-006 | A07 | P0 for privileged roles, P1 otherwise | T |
| SEC-007 | SSO shall use a standard protocol (OpenID Connect proposed). The system shall validate issuer, audience, signature, expiry and nonce, and map identities by a stable subject id, not by email alone. | FR-007 | A07 | P1 | T |
| SEC-008 | Access tokens shall be short-lived (default 15 minutes). Refresh tokens shall be bound to a server-side session, rotated on use, and revoked on reuse detection. | FR-004 | A07 | P0 | T |
| SEC-009 | Idle session timeout default 30 minutes. Absolute session lifetime default 12 hours. Sessions are revocable by the user and by administrators, and revocation takes effect on the next request. | FR-004, BRL-006 | A07 | P0 | T |
| SEC-010 | Deactivating a user or removing a role shall invalidate sessions and cached permissions within 60 seconds (default). | FR-009, BRL-055 | A01 | P0 | T |
| SEC-011 | Sign-in from a new device or location and password or MFA changes shall notify the user. | FR-035 | A07 | P1 | T |
| SEC-012 | Tokens and session identifiers shall not be placed in URLs, logs or referrer headers. Cookies shall be secure, HTTP-only and same-site restricted where cookies are used. | FR-004 | A02, A05 | P0 | I, T |

## 2. Authorisation and tenant isolation

| ID | Requirement | Linked | OWASP | Pri | Verif. |
|---|---|---|---|---|---|
| SEC-013 | Every API call and every report query shall be authorised on the server for permission and scope. Client-side hiding is not a control. | FR-015, NFR-016 | A01 | P0 | T, I |
| SEC-014 | Access to any record by identifier shall check that the record is inside the caller's scope (object-level authorisation). A guessed identifier of another company or branch shall return not found. | FR-015, FR-019 | A01 | P0 | T |
| SEC-015 | The system shall prevent mass assignment: only documented fields are accepted on create and update, and fields such as company, status, amounts of posted documents and approval state cannot be set by the client directly. | FR-064 | A01, A04 | P0 | T |
| SEC-016 | Tenant and company isolation shall be enforced in the database through row-level policies bound to the session context, in addition to application checks. | FR-019, TR-002, ADR-002 | A01 | P0 | T |
| SEC-017 | Cross-company group functions shall be explicit, permission-gated and audited. | FR-019 | A01 | P0 | T |
| SEC-018 | Default roles shall follow least privilege. Super Admin shall hold no business transaction rights. Auditor shall hold read, export and view only. | FR-014 | A01 | P0 | I |
| SEC-019 | Hard segregation-of-duties conflicts shall be blocked. Record-level conflicts shall be blocked on the same record. Overrides need approval and are reported to the Auditor. | FR-017, BRL-057, BRL-058 | A04 | P0 | T |
| SEC-020 | A user shall not approve their own document, including through delegation. | FR-042, BRL-008 | A04 | P0 | T |
| SEC-021 | Changes to roles, scope, delegation, approval chains and SoD overrides shall be audited and notify the Auditor-designated recipients. | FR-030 | A09 | P0 | T |
| SEC-022 | Privileged actions (period reopen, role grant, chain change, user export, settings change) shall require recent authentication (step-up) within a short window (default 5 minutes, proposed, PENDING APPROVAL). v0.2: raised from P1 to P0 and extended by SEC-068. | FR-059, FR-145 | A07 | P0 | T |

## 3. Input handling and web security

| ID | Requirement | Linked | OWASP | Pri | Verif. |
|---|---|---|---|---|---|
| SEC-023 | All input shall be validated on the server for type, length, range and format. Business validation runs after schema validation. | NFR-031 | A03 | P0 | T |
| SEC-024 | Database access shall use parameterised statements only. Dynamic filter and sort fields shall come from an allow-list. | NFR-015 | A03 | P0 | T, I |
| SEC-025 | Output shall be encoded for its context to prevent cross-site scripting. User-supplied rich text, if allowed, is sanitised with an allow-list. | NFR-015 | A03 | P0 | T |
| SEC-026 | State-changing requests using cookies shall be protected against cross-site request forgery. | NFR-015 | A01 | P0 | T |
| SEC-027 | The web client shall send a restrictive content security policy, frame protection, content-type protection and strict transport security. | NFR-012, NFR-015 | A05 | P0 | T |
| SEC-028 | Cross-origin access shall be limited to an allow-list of origins. | NFR-015 | A05 | P0 | T |
| SEC-029 | Server-side requests to user-supplied addresses (webhooks, imports by link) shall be blocked unless allow-listed, to prevent server-side request forgery. | NFR-015 | A10 | P0 | T |
| SEC-030 | Error responses shall not expose stack traces, queries, internal paths or configuration. | NFR-031 | A05 | P0 | T |
| SEC-031 | Imported files and spreadsheet exports shall neutralise formula injection (cells that start with formula characters). | FR-048, FR-050 | A03 | P0 | T |

## 4. Files and attachments

| ID | Requirement | Linked | OWASP | Pri | Verif. |
|---|---|---|---|---|---|
| SEC-032 | Uploads shall be limited by type allow-list (checked by content, not only extension) and by size (default 10 MB, TBD). | FR-033 | A04 | P1 | T |
| SEC-033 | Uploads shall be scanned for malware before they become available. Files with scan status other than clean are not downloadable. | FR-033 | A06 | P1 | T |
| SEC-034 | Files shall be stored outside the web root in private storage under generated names, served only through authorised, short-lived links (default 5 minutes). | FR-034, TR-012 | A01 | P1 | T |

## 5. Data protection (detail in [Data Protection](Data-Protection.md))

| ID | Requirement | Linked | OWASP | Pri | Verif. |
|---|---|---|---|---|---|
| SEC-035 | All network traffic shall use current TLS. Plain HTTP shall redirect or be refused. Internal service links shall also be encrypted. | NFR-012 | A02 | P0 | T |
| SEC-036 | Data at rest shall be encrypted in databases, object storage and backups, with keys in a managed key store and rotated. | NFR-013 | A02 | P0 | I |
| SEC-037 | Restricted fields (bank account numbers, tax ids, national ids, salary in R2) shall have field-level protection and shall be masked in lists, logs and exports unless the role is explicitly entitled. | NFR-013 | A02 | P0 | T |
| SEC-038 | Secrets shall be held in a secret store, never in source code, images or logs, and rotated on schedule and on suspected exposure. | NFR-014 | A02, A05 | P0 | I |
| SEC-039 | Personal data in logs, audit records and non-production environments shall be minimised or masked. | NFR-039, NFR-028 | A09 | P0 | I |

## 6. Business-logic abuse

| ID | Requirement | Linked | OWASP | Pri | Verif. |
|---|---|---|---|---|---|
| SEC-040 | Money and stock endpoints shall be idempotent and safe against replay and double submission. | FR-076, NFR-025 | A04 | P0 | T |
| SEC-041 | Duplicate vendor invoices, duplicate vendors (same tax id) and duplicate payments shall be detected and blocked or flagged. | FR-029, FR-099 | A04 | P0 | T |
| SEC-042 | Changes to vendor bank details shall need independent approval and shall trigger a notification to the Finance Manager. | FR-088, BRL-033 | A04 | P0 | T |
| SEC-043 | Approval value shall be re-evaluated when a document changes after approval. Splitting orders to stay under limits shall be detectable by a report of same-vendor same-day orders. | FR-092, UC-006 | A04 | P1 | T, A |
| SEC-044 | Rate limits shall apply per user, per address and per tenant on sign-in, reset, search, export and write endpoints. Values TBD. | NFR-017 | A04, A07 | P0 | T |
| SEC-045 | Exports and bulk reads above a threshold shall be audited and may require approval. Threshold TBD. | FR-050 | A01 | P1 | T |

## 7. Logging, monitoring and response

| ID | Requirement | Linked | OWASP | Pri | Verif. |
|---|---|---|---|---|---|
| SEC-046 | Security events (sign-in success and failure, lockout, MFA failure, permission refusal, privilege change, export, SoD override) shall be logged with actor, source, time and correlation id. | FR-030, NFR-028 | A09 | P0 | T |
| SEC-047 | Alerts shall fire on repeated failures, impossible travel (where detectable), privilege escalation, mass export, audit log write failure and integration credential failure. | NFR-029 | A09 | P0 | D |
| SEC-048 | An incident response procedure shall exist with named roles, severity levels and notification duties. Detail in Stage 3 operations documents. | NFR-029 | A09 | P0 | I |

## 8. Platform and supply chain

| ID | Requirement | Linked | OWASP | Pri | Verif. |
|---|---|---|---|---|---|
| SEC-049 | Third-party libraries and base images shall be scanned in the build. Builds fail on known Critical vulnerabilities. A software bill of materials is kept. | NFR-019 | A06, A08 | P0 | T |
| SEC-050 | Build artifacts shall be produced by the pipeline from reviewed code and signed or verifiably traceable. | NFR-038 | A08 | P1 | I |
| SEC-051 | Databases and internal services shall not be reachable from the internet. Administrative access shall use MFA and be logged. | NFR-016 | A05 | P0 | I, T |
| SEC-052 | Security configuration baselines (hardened images, default accounts removed, unused features off) shall be defined and checked. | NFR-038 | A05 | P0 | I |
| SEC-053 | Independent security testing (penetration test) shall be carried out before MVP release and after major change. | NFR-015 | All | P0 | D |

## 8A. Authorisation hardening (v0.2)

SEC-054 to SEC-073 close the RBAC audit findings. Each names the FR it supports.

| ID | Requirement | FR / NFR | OWASP | Pri | Verif. |
|---|---|---|---|---|---|
| SEC-054 | The server shall refuse a role grant when the role tier is not strictly lower than the grantor's highest tier, whatever the client sends. | FR-131 | A01 | P0 | T |
| SEC-055 | The server shall refuse any grant, scope widening, grant approval or override that targets the acting user. | FR-132 | A01 | P0 | T |
| SEC-056 | Grants of Manager-tier, Auditor and approve/release/reverse roles, and scope widening, shall stay Pending until a second authorised user approves. The approver shall not be the grantor, grantee or creator of the user. | FR-133 | A01 | P0 | T |
| SEC-057 | Branch-limited administrators shall grant only T3 and T4 roles inside their own branches. The check shall use the target record's scope, not a client value. | FR-134 | A01 | P0 | T |
| SEC-058 | The server shall enforce, on the payment record, that prepare, approve and release are done by different users, and that a reversal is approved by a user other than the requester. | FR-135 | A04 | P0 | T |
| SEC-059 | The server shall refuse approval of a journal or reversal by its creator or requester and shall post only by system action at final approval. | FR-136 | A04 | P0 | T |
| SEC-060 | Payment release and reversal shall need step-up authentication and, above the configured value, two different approvers. | FR-137, FR-145 | A07 | P0 | T |
| SEC-061 | Controlled data shall change only through request and approval by different users. The old value shall stay active until approval. The approver shall not hold the authority being changed. | FR-138 | A04 | P0 | T |
| SEC-062 | A vendor bank change shall need request, out-of-band verification and approval by three different users. No payment shall go to the new account before approval. The first payment shall wait for the hold period. | FR-139 | A04 | P0 | T |
| SEC-063 | A closed period shall reopen only after an approved request with reason, shall close again automatically, and every posting during the window shall be audited. | FR-140 | A04 | P0 | T |
| SEC-064 | A credit limit shall change only through request and approval. The field shall be absent from ordinary customer updates. | FR-141 | A04 | P0 | T |
| SEC-065 | An SoD override shall be requested and approved by different administrators, shall be time-limited, and shall be reported to the Auditor. | FR-142 | A01 | P0 | T |
| SEC-066 | Delegation shall follow BRL-073. SoD shall be checked at delegation and at use. Chains shall be refused. | FR-143 | A01 | P0 | T |
| SEC-067 | Emergency access shall use a sealed account released by two users, expiring, without financial authority, fully audited and reviewed by the Auditor. | FR-144 | A07 | P0 | T, I |
| SEC-068 | Sensitive actions shall require authentication within the step-up window (proposed 5 minutes). A missing or expired step-up shall return STEP-001. | FR-145 | A07 | P0 | T |
| SEC-069 | Permission codes shall exist and be enforced for M-001, M-012, M-015 and M-065. Each endpoint of those modules shall name one code. | FR-146 | A01 | P0 | T, I |
| SEC-070 | An attachment on a submitted, approved or posted document shall not be deletable. Voiding shall need a reason and an administrator and shall keep the file. | FR-147 | A08 | P1 | T |
| SEC-071 | An export shall check read permission on the module as well as the export permission. User and bulk exports shall need step-up. Every export shall be audited. | FR-148 | A01 | P0 | T |
| SEC-072 | Service accounts shall have a named owner, narrow scopes, expiring tokens and rotation, and shall never hold approve, release, reverse, reopen or grant permissions. | FR-149 | A07 | P0 | T, I |
| SEC-073 | Security events and business events shall be readable under separate permissions. Audit reads and exports shall be audited. | FR-150 | A09 | P1 | T |

## 9. Summary

| Item | Count |
|---|---|
| Security requirements | 73 (SEC-001 to SEC-053 from v0.1, SEC-054 to SEC-073 added in v0.2) |
| P0 | 63 (SEC-022 raised from P1 in v0.2 because other mitigations depend on it) |
| P1 | 10 |

## 10. Open points

| Point | Question |
|---|---|
| MFA mandatory or optional, SSO protocol | OQ-023 |
| SoD overrides | OQ-032 |
| Data residency and breach notification duties | OQ-010 |
| Rate limit and file size values | OQ-015, NFR-017 |
