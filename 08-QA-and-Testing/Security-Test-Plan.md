# Security Test Plan

Document status: Draft v0.1 (Stage 3 part 1). Proposed. The independent penetration test is not commissioned, and no security test has run.
Related: [Security Requirements](../06-RBAC-and-Security/Security-Requirements.md), [Threat Model](../06-RBAC-and-Security/Threat-Model.md), [Security Test Cases](test-cases/Security-Test-Cases.md), [API Test Cases](API-Test-Cases.md), [Test Strategy](Test-Strategy.md).

## 1. Objectives

1. Verify each security requirement SEC-001 to SEC-053.
2. Verify mitigations for the threats THR-001 to THR-044, including ERP fraud abuse cases.
3. Provide evidence for gate G4: no open High or Critical findings at release.

## 2. Scope and rules of engagement

| Item | Rule |
|---|---|
| Targets | Test and Staging environments only. **Never production and never third-party services** (partners are stubbed) |
| Data | Synthetic only |
| Authorisation | Written approval from the sponsor and IT manager before any active test |
| Intrusive tests | Denial-of-service style load only on Staging, at agreed times |
| Reporting | Findings logged with the [Defect Report Template](Defect-Report-Template.md) using security severity |
| Disclosure | Findings go only to the QA lead, security officer and architect until fixed |

## 3. Test activities

| Activity | Method | Covers | Cases |
|---|---|---|---|
| Requirement verification | Scripted cases | SEC-001 to SEC-053 | 53 in the security file |
| Abuse cases | Scripted scenarios | THR-012, 024, 029, 031, 035 to 041, 043, 044 | 13 |
| Authorisation matrix | Generated tests from the RBAC matrix | SEC-013, SEC-014, SEC-018, THR-013, THR-025, THR-027 | 127 permission cases in the [API Test Cases](API-Test-Cases.md) |
| Data isolation | Isolation matrix by role and scope | SEC-014, SEC-016, INV-12 | Cross-module and module cases |
| Static analysis | Code scanning in CI | SEC-023 to SEC-025, SEC-049 | Pipeline |
| Dependency and image scanning | CI scanners, software bill of materials | SEC-049, SEC-050 | Pipeline |
| Secret scanning | Repository, images, logs | SEC-038 | Pipeline |
| Dynamic scanning | Automated scanner against Staging | OWASP Top 10 categories | Per release |
| Configuration review | Checklist | SEC-027, SEC-028, SEC-035, SEC-051, SEC-052 | Per release |
| Penetration test | Independent party, black and grey box | All categories | Before MVP release and after major change (SEC-053) |
| Tabletop incident exercise | Scenario workshop | SEC-048 | Once before release |

## 4. OWASP Top 10 (2021) coverage plan

| Category | Main verification |
|---|---|
| A01 Broken access control | Permission matrix tests, IDOR tests, isolation matrix, mass assignment |
| A02 Cryptographic failures | TLS configuration scan, encryption inspection, field protection checks |
| A03 Injection | Injection payload sets on all inputs, formula injection, XSS output tests |
| A04 Insecure design | Abuse cases, SoD tests, idempotency and replay tests |
| A05 Security misconfiguration | Headers, CORS, error handling, baseline checks |
| A06 Vulnerable components | Dependency scan, malware scan of uploads |
| A07 Authentication failures | Lockout, reset, MFA, session, token tests |
| A08 Software and data integrity | Pipeline provenance, append-only checks, audit hash chain verification |
| A09 Logging and monitoring | Event logging and alert tests |
| A10 SSRF | Address allow-list tests |

## 5. Entry and exit

| Entry | Exit |
|---|---|
| Feature complete build on Staging, test data and stubs ready, approval in place | All P0 security cases executed, no open High or Critical findings, Medium findings have an accepted plan, penetration test report reviewed |

## 6. Severity of findings

Critical: remote exploit of data or money, cross-tenant access, authentication bypass. High: privilege escalation, injection with impact, sensitive data exposure. Medium: limited impact or needs unlikely conditions. Low: hardening gaps.
These definitions are proposals for security officer approval.

## 7. Retest and regression

Every fixed finding gets an automated regression case (see [Regression Test Plan](Regression-Test-Plan.md)). The security suite runs in full at each release candidate.

## 8. Tools

Tool selection waits for the stack decision (OQ-024). The plan requires: static analysis, dependency scanner, secret scanner, container image scanner, dynamic scanner, an intercepting proxy for manual tests, and a load tool for rate-limit tests.

## 9. Open points

OQ-023 (MFA and SSO scope), OQ-010 (privacy law affects data protection tests), OQ-015 and NFR-017 values for rate-limit tests, the approval and budget for the penetration test (OQ-014).
