# Compliance Requirements

Document status: Draft v0.1 (Stage 3 part 2). Proposed. **No jurisdiction, industry or customer is known** (OQ-002, OQ-010), so no specific law or standard is asserted to apply. This document defines compliance *capabilities* the ERP must offer and the questions that decide which obligations are in force. It is not legal advice. A legal or compliance specialist must confirm applicability.
Related: BR-059, [Security Requirements](../06-RBAC-and-Security/Security-Requirements.md), [Audit Trail](../06-RBAC-and-Security/Audit-Trail.md), [Data Protection](../06-RBAC-and-Security/Data-Protection.md), [Privacy Requirements](Privacy-Requirements.md), [Data Retention Policy](Data-Retention-Policy.md), [Audit Requirements](Audit-Requirements.md), [Data Governance](Data-Governance.md).

## 1. How obligations are determined

| Question | Decides | Open question |
|---|---|---|
| Which countries do customers operate in? | Tax, accounting, labour, privacy laws | OQ-010, OQ-007, OQ-006 |
| Which industry? | Industry regulation (for example food, pharma, health, finance) | OQ-002 |
| Is the customer listed or audited? | Financial reporting and internal control expectations | OQ-018 |
| Does the ERP process personal data? | Privacy law obligations | OQ-010 |
| Is payment card data handled? | Card industry standard scope. The design avoids storing card data (payment gateway hosted pages proposed) | OQ-017 |
| Is e-invoicing or real-time tax reporting mandated? | Integration scope | OQ-007, OQ-017 |

Until answered, the ERP is designed so compliance is **configurable and evidenced**: retention rules, tax setup, numbering, audit, access controls, privacy handling and reporting.

## 2. Compliance domains and capability requirements

Each requirement is a capability, linked to existing requirements. Status shows whether the capability is specified in this package. "Specified" does not mean implemented or verified.

| ID | Domain | Capability required | Realised by | Status |
|---|---|---|---|---|
| COM-001 | Financial reporting | Complete, traceable, balanced books with period control and immutable postings | FR-053 to FR-065, FR-082 to FR-084, INV-01 to INV-04 | Specified |
| COM-002 | Financial reporting | Accounting framework configuration (chart, statements) | FR-053, FR-054, OQ-018 | Partly, framework TBD |
| COM-003 | Financial reporting | Audit-ready period close and year-end with reopen control | FR-058 to FR-061 | Specified |
| COM-004 | Tax | Configurable tax codes, effective dates, rounding, tax reports | FR-070, FR-071, OQ-007 | Partly, regimes TBD. Tax return reports not in MVP |
| COM-005 | Tax | Sequential, unique, legally formatted document numbers | FR-025, OQ-031 | Specified, gap-free policy TBD |
| COM-006 | Tax | E-invoicing and statutory submissions | None | Not in MVP (OQ-017) |
| COM-007 | Internal control | Segregation of duties, maker-checker, approval authority | FR-017, FR-042 to FR-047, RBAC Matrix section 5 | Specified |
| COM-008 | Internal control | Complete audit trail, tamper evidence | FR-030 to FR-032, Audit Trail | Specified, hash chain unreviewed |
| COM-009 | Internal control | Periodic access review evidence | Maintenance SOP MNT-11, access change audit events | Specified |
| COM-010 | Information security | Controls aligned to a recognised framework, for example an information security management standard | Security Requirements SEC-001 to SEC-053 | Mapping to a named standard not done (OQ-016) |
| COM-011 | Information security | Independent security testing before release and after major change | SEC-053, Security Test Plan | Specified |
| COM-012 | Information security | Incident response and breach handling | SEC-048, SLA and Support | Outline, runbook not written |
| COM-013 | Privacy | Data minimisation, access limits, masking, subject rights support, anonymisation | Privacy Requirements, FR-127, FR-129 | Specified, law TBD |
| COM-014 | Privacy | Records of processing, provider agreements, cross-border controls | Privacy Requirements | Process only |
| COM-015 | Records management | Retention by record type, legal hold, controlled disposal | FR-127, Data Retention Policy | Specified, periods TBD |
| COM-016 | Business continuity | Backup, restore, DR with tested RPO and RTO | NFR-009 to NFR-011, Backup and Recovery, Disaster Recovery | Specified, values TBD |
| COM-017 | Payroll and labour (R2) | Statutory payroll, leave and records rules | M-023 and related | Not in MVP (OQ-006) |
| COM-018 | Industry specific | Batch traceability, expiry, quality records, validation | M-068 to M-070, M-103 | Not in MVP (OQ-002) |
| COM-019 | Accessibility | Legal accessibility duties where they apply | Accessibility Requirements | Specified (WCAG 2.2 AA), legal duty TBD |
| COM-020 | Vendor and third-party | Due diligence and agreements for providers and partners | Privacy Requirements, Integration Architecture | Process only |
| COM-021 | Change and release control | Controlled, traceable changes and releases | Change Management, Release Management | Specified |
| COM-022 | Data governance | Ownership, quality, classification | Data Governance | Specified |

## 3. Compliance matrix by artefact

| Evidence the ERP must produce | Source |
|---|---|
| Trial balance, statements, ledger detail with drill-down | Finance reports |
| Audit trail export by entity, actor, date | Audit Trail section 7 |
| Access review report, SoD override report | Audit reports |
| Approval history of any document | Approval engine |
| Retention and legal hold register | Data Retention Policy |
| Subject rights request log and anonymisation records (AUD-080, AUD-081) | Privacy Requirements |
| Backup and restore test records | Backup and Recovery |
| Change and release records | Release and Change Management |
| Security test reports and findings | Security Test Plan |
| Incident log and reviews | SLA and Support |

## 4. Responsibilities

| Role | Responsibility |
|---|---|
| Sponsor | Accepts compliance risk, funds controls |
| Compliance and privacy officer (role TBD) | Determines applicable obligations, approves retention and privacy rules |
| Finance Manager | Financial and tax compliance owner |
| Security officer | Security control owner |
| IT manager | Operational control owner |
| Auditor | Independent assurance |
| Architect | Ensures design supports the controls |

## 5. Gaps

| ID | Gap | Action |
|---|---|---|
| CG-01 | No jurisdiction or industry confirmed, so applicable obligations are unknown | Answer OQ-002, OQ-010 |
| CG-02 | No mapping to a named control framework (for example for security or internal control) | After the customer names one (OQ-016) |
| CG-03 | Tax filings, e-invoicing and payroll statutory outputs are outside the MVP | Plan per jurisdiction |
| CG-04 | Legal review of this package has not happened | Before build and before go-live |
| CG-05 | Compliance monitoring and regulatory change process not defined | Define with the compliance officer |

## 6. Open points

OQ-002, OQ-006, OQ-007, OQ-010, OQ-016, OQ-017, OQ-018, OQ-022, OQ-031.
