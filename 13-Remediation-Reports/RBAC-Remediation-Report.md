# RBAC Remediation Report

Phase 2 remediation, 2026-11-11. Related: [Remediation Progress](Remediation-Progress.md), [Final Revalidation Report](Final-Revalidation-Report.md), [Audit baseline](../12-Final-Audit/Final-Readiness-Assessment.md).

No stakeholder approval is claimed anywhere in this report. Values marked PENDING APPROVAL are recommendations.

## 1. What changed

| Item | Before (v0.1) | After (v0.2) |
|---|---|---|
| Permission codes | 211 | 249 active, 5 superseded kept visible as Superseded |
| SoD pairs | 13 (SOD-01 to SOD-13) | 32 (SOD-01 to SOD-32), 17 hard and 15 record-level |
| Business rules for access | none specific | BRL-066 to BRL-074 |
| Functional requirements | none for these controls | FR-131 to FR-150 |
| Security requirements | 53 | 73 (SEC-054 to SEC-073) |
| Threats | 44 | 59 (THR-045 to THR-059) |
| Audit events | 100 | 127 (AUD-101 to AUD-127) |
| Database constraints | 12 | 20 (DBC-13 to DBC-20) |
| API endpoints | 169 | 188 (API-170 to API-188), 11 permission codes on existing endpoints changed |
| User stories | 80 | 98 (US-081 to US-098) |
| Negative security test cases | none for these controls | 95 in `Authorisation-Hardening-Test-Cases.md` (TC-0554 to TC-0648) |

## 2. Design in one view

Details are in `RBAC-Matrix.md` section 8A. This is a summary.

| Topic | Design | Where |
|---|---|---|
| Role hierarchy | Tiers T0 platform, T1 organisation, T2 manager, T3 operator, T4 basic, plus Oversight for the Auditor | 8A.1 |
| Role assignment restrictions | Grant only a lower tier, in own scope, never to self. Branch Admin grants T3 and T4 in own branches. Manager-tier, Auditor, approve, release and reverse roles need a Super Admin to approve | 8A.2 |
| Permission ownership | Maker and checker named per control area | 8A.3 |
| Separation of duties | SOD-14 to SOD-32 plus database constraints DBC-13 to DBC-20 | 5, 8A.4 |
| Maker-checker | Per transaction table: payment (three users), journal, PO, vendor, vendor bank (three users), stock adjustment, credit note, credit limit, period reopen, controlled configuration, role grant | 8A.4 |
| Approval limits and multi-level | Structure fixed, values TBD (OQ-013, OQ-034). Second approver above limit A. Third level PENDING APPROVAL | 8A.5 |
| Delegation | BRL-073: holds permission, SoD checked at use, no chains, no delegation of release, reverse, reopen, grant approval | 8A.7 |
| Sensitive transaction restrictions | Step-up list, attachment immutability, export control, service accounts barred from authority | 8A.6, 8A.9 |
| Audit logging | AUD-101 to AUD-127 | Audit-Trail 3A |
| Emergency access | Sealed account, two-person release, expiry, no financial authority, Auditor review | 8A.8 |

## 3. Audit findings and their status

| Finding | Status |
|---|---|
| RF-001 RBAC E1; Critical Gaps 1: Branch and Organization Admin could assign roles with no authority ranking | Closed pending approval |
| RF-002 RBAC E2: Org Admin could grant roles to self and had no checker | Closed pending approval |
| RF-003 RBAC E3: No code for SoD override or SoD list edit | Closed pending approval |
| RF-004 RBAC E4: Super Admin has no business permissions yet had to create business roles | Closed pending approval |
| RF-005 RBAC E5: Employee role stacked onto Auditor and Read-only users | Closed pending approval |
| RF-006 RBAC E6: Any staff user could delete attachments on approved documents | Closed pending approval |
| RF-007 RBAC E7: Two approve codes, unclear which governs | Closed pending approval |
| RF-008 RBAC E8: Temporary access had no approver rule or maximum | Closed pending approval |
| RF-009 RBAC E9: Super Admin could shorten audit retention | Closed pending approval |
| RF-010 RBAC E10: 15 minute token and revocation on refresh contradicted SEC-009 and SEC-010 | Closed pending approval |
| RF-011 Critical Gaps 1; RBAC section 3: Finance Manager could create, approve and release the same payment or journal | Closed pending approval |
| RF-012 RBAC section 3: No permission code or checker for vendor bank change | Closed pending approval |
| RF-013 RBAC section 3; Critical Gaps 1: Period reopen had no code and no approver | Closed pending approval |
| RF-014 RBAC section 3: No code or checker for credit limit change | Closed pending approval |
| RF-015 Critical Gaps 2; RBAC section 1: Four MVP modules had no permission rows | Partly closed |
| RF-016 RBAC section 3: High-risk SoD pairs missing (journal, payment, PO editor, bank, settings, stock adjustment, period) | Closed pending approval |
| RF-017 RBAC section 3: Events missing for break-glass, service accounts, overrides | Partly closed |
| RF-018 RBAC section 4: Threat gaps for escalation, collusion, bank diversion, delegation, break-glass, service accounts | Partly closed |
| RF-019 RBAC section 5: Service accounts, break-glass undefined | Partly closed |
| RF-020 RBAC section 3: Generic export, no user export or unmask code | Partly closed |
| RF-021 RBAC section 3: Group functions promised but no code | Open (deferred) |
| RF-022 RBAC section 3: Shared master data conflicts with company predicate | Open (stakeholder decision) |
| RF-023 RBAC section 3: 403 vs 404 leaks company existence | Closed (verified) |
| RF-024 RBAC section 1: Story roles with no RBAC role (9 names) | Open (deferred) |

## 4. Verification evidence (re-run by script on the updated files)

| Check | Result |
|---|---|
| Hard SoD pairs held together by one default role | 0 violation(s): none |
| Record-level SoD pairs that one role holds (the system refuses them on the same record) | 13 pair-role combinations, expected for FM, PM, IM, OA |
| Business permissions held by Super Admin | none |
| Auditor permissions that are not read, view, export or review | none |
| Read-only User permissions that are not read or view | none |
| API permission codes not found in the matrix | none |
| New permission codes not used by any endpoint | [] |
| New permission codes not mentioned in any other document | none |
| FR-131 to FR-150 with at least one test case | 20 of 20 |
| SEC-054 to SEC-073 with at least one test case | 20 of 20 |

Limits: these checks prove consistency inside the documents. They do not prove the design is sufficient. A security review by a person and a test run are still required. SoD pairs with a code that no default role holds were not tested against custom roles.

## 5. Remaining RBAC gaps (not fixed in Phase 2)

- Impersonation and support access, user API tokens, SSO claim mapping, admin-initiated password reset.
- Group and cross-company functions (declared out of MVP).
- Shared master data versus company predicate (OQ-037).
- Nine story role names with no RBAC role.
- Partial permission coverage for M-030, M-032, M-033, M-034, M-064 and M-116.
- Payroll SoD (R2).
- Threats for XSS, CSRF, SSO account linking and upload malware.
- Subject data export permission and IdP mapping change audit event.
- Receipts and payments still share the `finance.payment` family.
