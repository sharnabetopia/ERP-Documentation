# Enterprise ERP Documentation Package

Business analysis, architecture and QA documentation for a configurable, multi-company, multi-branch ERP.
This package contains documentation only. No application code.

Current state: **Stages 1 and 2 written as Draft** (business analysis, requirements, architecture, workflows, diagrams, RBAC and security). All 107 planned files are written as Draft, including QA (553 test cases, none executed), UI/UX, delivery, operations, compliance and a final self-audit. **Verdict of the audit: ready for stakeholder review, not ready to build.** See [Final-Readiness-Assessment](12-Final-Audit/Final-Readiness-Assessment.md). See [Documentation-Progress](00-Project-Management/Documentation-Progress.md) for exact status per file.
Phase 2 remediation (2026-11-11) addressed the independent audit findings. See [Remediation-Progress](13-Remediation-Reports/Remediation-Progress.md) and [Final-Revalidation-Report](13-Remediation-Reports/Final-Revalidation-Report.md).
Everything is **Proposed** until a sponsor confirms it. Unknowns are tracked in [Open Questions](02-Business-Analysis/Open-Questions.md).

## Start here

| If you want | Read |
|---|---|
| The MVP in one page | [MVP-Strategy](00-Project-Management/MVP-Strategy.md) |
| All 120 modules with priority and release | [ERP-Module-Inventory](01-ERP-Module-Analysis/ERP-Module-Inventory.md) |
| Business requirements (BR-001 to BR-063) | [BRD](02-Business-Analysis/BRD.md) |
| Business rules (BRL-001 to BRL-065) | [Business-Rules](02-Business-Analysis/Business-Rules.md) |
| Functional requirements (FR-001 to FR-130) | [FRS](03-Product-and-System-Requirements/FRS.md) |
| User stories and use cases | [User-Stories](03-Product-and-System-Requirements/User-Stories.md), [Use-Cases](03-Product-and-System-Requirements/Use-Cases.md) |
| Architecture decisions (provisional) | [ADR](04-System-Architecture/Architecture-Decision-Records.md) |
| Proposed API design | [API-Documentation](04-System-Architecture/API-Documentation.md) |
| Who can do what | [RBAC-Matrix](06-RBAC-and-Security/RBAC-Matrix.md) |
| End-to-end processes | [Cross-Module-Workflows](05-Workflows-and-Diagrams/Cross-Module-Workflows.md) |
| Audit result and readiness | [Final-Readiness-Assessment](12-Final-Audit/Final-Readiness-Assessment.md), [Documentation-Audit-Report](12-Final-Audit/Documentation-Audit-Report.md), [Phase 2 re-validation](13-Remediation-Reports/Final-Revalidation-Report.md) |
| Test cases and traceability | [RTM](08-QA-and-Testing/RTM.md), [Test-Plan](08-QA-and-Testing/Test-Plan.md) |
| Threats and security requirements | [Threat-Model](06-RBAC-and-Security/Threat-Model.md), [Security-Requirements](06-RBAC-and-Security/Security-Requirements.md) |
| What is still unknown | [Open-Questions](02-Business-Analysis/Open-Questions.md) |
| Risks | [Risk-Register](00-Project-Management/Risk-Register.md) |

## Folder map

| Folder | Content | Status |
|---|---|---|
| 00-Project-Management | Charter, scope, roadmap, MVP, stakeholders, RACI, risks, progress | Draft |
| 01-ERP-Module-Analysis | Inventory, features, dependencies, prioritisation | Draft (features partial) |
| 02-Business-Analysis | BRD, rules, glossary, context, As-Is, To-Be, gaps, questions | Draft (As-Is partial) |
| 03-Product-and-System-Requirements | PRD, SRS, FRS, NFR, stories, use cases, acceptance criteria | Draft (MVP only) |
| 04-System-Architecture | ARD, ADRs, database, ERD, dictionary, API, integration, deployment, infrastructure, migration | Draft, provisional (dictionary partial) |
| 05-Workflows-and-Diagrams | 14 workflows, process, swimlane, sequence, activity, state, data flow, C4 | Draft (8 workflows are outlines) |
| 06-RBAC-and-Security | RBAC, permissions, security, threat model, privacy, audit | Draft, provisional |
| 07-UI-UX-Specifications | Navigation, 47 screen groups, 14 journeys, 18 forms, accessibility | Draft |
| 08-QA-and-Testing | Strategy, plans, 553 test cases, RTM, UAT | Draft. No test executed |
| 09-Development-and-Delivery | Guidelines, standards, DoR, DoD, sprint slices, release, change, CI/CD | Draft |
| 10-Deployment-and-Operations | Deployment, environments, monitoring, backup, DR, SLA, maintenance | Draft (no SLA agreed) |
| 11-Compliance-and-Governance | Compliance, governance, retention, privacy, audit | Draft (no jurisdiction known) |
| 12-Final-Audit | Independent audit reports (10) and the archived authors' self-audit in `_self-audit-v0.1` | Final (audit baseline, not edited in Phase 2) |
| 13-Remediation-Reports | Phase 2 remediation register, correction reports, re-validation and readiness comparison | Draft (Phase 2) |
| 14-Stakeholder-Review | Decision papers, approval register, deferred findings plan, MVP scope matrix. All items PENDING | Draft (Phase 3) |

## ID conventions

| Prefix | Meaning | Prefix | Meaning |
|---|---|---|---|
| M- | Module | BR- | Business requirement |
| BRL- | Business rule | FR- | Functional requirement |
| NFR- | Non-functional requirement | TR- | Technical requirement |
| API- | API requirement | US- | User story |
| UC- | Use case | TC- | Test case |
| OQ- | Open question | R- | Risk |
| ASM-, CON-, DEP- | Assumption, constraint, dependency | PRB-, OBJ-, GAP- | Problem, objective, gap |
| ADR- | Architecture decision | DBC- | Database constraint |
| INV- | System invariant | WF- | Workflow |
| SEC- | Security requirement | THR-, AST- | Threat, asset |
| AUD- | Audit event | SOD- | Segregation of duties rule |
| DP- | Data protection requirement | SEQ-, ACT-, DFD- | Sequence, activity, data flow diagram |
| TC- | Test case (four digits, TC-0001 onward) | DEF- | Defect |
| SCR-, UJ-, MSG-, ACC- | Screen group, journey, message, accessibility requirement | MON-, MNT-, CS- | Alert, maintenance task, coding standard |
| COM-, PRV-, AUR-, RP- | Compliance, privacy, audit requirement, retention type | AG-, TG-, PG- | Architecture, test, documentation gap |

## Conventions

No em dashes in prose. Diagrams use Mermaid. Cross-references use relative links.
