# Risk Register

Document status: Draft v0.1 (Stage 1). Ratings are the BA's judgement and need review.
Likelihood (L) and Impact (I): 1 low to 5 high. Score = L x I.

| ID | Risk | Category | L | I | Score | Mitigation | Owner (role) | Linked |
|---|---|---|---|---|---|---|---|---|
| R-001 | Scope grows beyond the MVP as stakeholders ask for more modules. | Scope | 5 | 4 | 20 | Fixed MVP list, change control, release themes. | Programme manager | OQ-003 |
| R-002 | No sponsor or decision owner, so questions stay open. | Governance | 3 | 5 | 15 | Escalate OQ-001 first. | Programme manager | OQ-001 |
| R-003 | Requirements are generic and miss the real industry. | Requirements | 4 | 4 | 16 | Resolve OQ-002. Run As-Is workshops. | Business analyst | OQ-002 |
| R-004 | Financial logic errors (rounding, posting, closing) cause wrong books. | Technical | 3 | 5 | 15 | Exact decimals, posting rules documented, reconciliation tests, finance review. | Architect, Finance manager | CON-01 |
| R-005 | Cross-tenant or cross-branch data leak. | Security | 2 | 5 | 10 | Isolation model in ADR, data isolation test suite, security testing. | Security officer | BR-001, BR-003 |
| R-006 | Approval engine is too rigid or too complex for different companies. | Design | 3 | 4 | 12 | Configurable chains, prototype early with two scenarios. | Architect | BR-006 |
| R-007 | Payroll and tax rules unknown or change. | Compliance | 4 | 4 | 16 | Defer payroll to R2. Configurable rule tables. | HR manager, Finance manager | OQ-006, OQ-007 |
| R-008 | Data migration is underestimated. | Delivery | 4 | 4 | 16 | Early profiling, rehearsals, validate-only imports. | Implementation lead | OQ-012 |
| R-009 | Performance degrades with large ledgers and stock movements. | Technical | 3 | 4 | 12 | Volume assumptions, index and partition plan, performance tests. | Architect | OQ-015 |
| R-010 | Integration partners (banks, gateways) delay or change formats. | External | 3 | 3 | 9 | Adapter layer, retry, mock providers in test. | IT manager | OQ-017 |
| R-011 | Users resist the change, keep spreadsheets. | Adoption | 3 | 4 | 12 | Training, champions, UAT involvement. | Programme manager | None |
| R-012 | Documentation drifts out of sync across files. | Quality | 4 | 3 | 12 | Single ID registry, RTM, final audit. | Business analyst, QA lead | None |
| R-013 | Documents are mistaken for confirmed decisions. | Governance | 3 | 4 | 12 | Status labels (Proposed, TBD), open questions list. | Business analyst | None |
| R-014 | Compliance and privacy obligations discovered late. | Compliance | 3 | 5 | 15 | Resolve OQ-010 and OQ-022 early. | Compliance officer | OQ-010 |
| R-015 | Key people unavailable for workshops and UAT. | Resource | 3 | 3 | 9 | Schedule early, named backups. | Programme manager | OQ-029 |
| R-016 | Over-engineering (for example microservices) slows delivery. | Technical | 3 | 3 | 9 | Modular monolith evaluation in ADR, split only on evidence. | Architect | ASM-07 |
| R-017 | Capacity or budget cannot support the 58-module MVP. | Resource | 4 | 4 | 16 | Phase MVP milestones A to D, define a smaller fallback cut after capacity is known. | Programme manager | OQ-014 |

## Top risks by score

R-001 (20), R-003, R-007, R-008, R-017 (16 each), R-002, R-004, R-014 (15 each).
