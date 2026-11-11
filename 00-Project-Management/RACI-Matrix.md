# RACI Matrix

Document status: Draft v0.1 (Stage 1). Roles only. Names TBD.
R responsible, A accountable (one per row), C consulted, I informed.

Columns: SP sponsor, PgM programme manager, BA business analyst, SA solution architect, FIN finance manager, PRO procurement manager, INV inventory manager, SAL sales manager, HR HR manager, SEC security officer, IT IT manager, CMP compliance officer, QA QA lead, DEV development lead.

## 1. Documentation phase

| Activity | SP | PgM | BA | SA | FIN | PRO | INV | SAL | HR | SEC | IT | CMP | QA | DEV |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Approve charter and scope | A | R | C | C | C | C | C | C | C | I | C | I | I | I |
| Elicit business requirements | I | A | R | C | C | C | C | C | C | C | C | C | C | I |
| Approve BRD and MVP scope | A | R | R | C | C | C | C | C | C | C | C | C | C | I |
| Prioritise modules | A | C | R | C | C | C | C | C | C | I | I | I | I | I |
| Define business rules | I | I | R | C | A | C | C | C | C | C | I | C | C | I |
| Answer open questions | A | R | R | C | C | C | C | C | C | C | C | C | I | I |
| Write functional requirements and stories | I | C | A/R | C | C | C | C | C | C | C | I | I | C | C |
| Design architecture and ADRs | I | C | C | A/R | I | I | I | I | I | C | C | C | C | C |
| Design RBAC and security | I | I | C | R | C | C | C | C | C | A | C | C | C | C |
| Design database and API | I | I | C | A/R | C | C | C | C | C | C | C | I | C | R |
| Write test strategy and cases | I | C | C | C | C | C | C | C | C | C | I | I | A/R | C |
| Documentation audit | I | A | C | C | I | I | I | I | I | C | I | C | R | I |

## 2. Delivery phase (preview)

| Activity | SP | PgM | BA | SA | QA | DEV | IT | SEC |
|---|---|---|---|---|---|---|---|---|
| Sprint planning | I | A | C | C | C | R | I | I |
| Change request decision | A | R | C | C | C | C | I | C |
| Release approval | A | R | C | C | R | C | C | C |
| UAT sign-off | A | R | C | I | R | I | I | I |
| Production deployment | I | A | I | C | C | R | R | C |
| Incident response | I | C | I | C | I | R | A | R |

Each row has exactly one A, except rows marked A/R where the BA, architect or QA lead both owns and performs the work.
