# Architecture Decision Records (ADR)

Document status: Draft v0.2 (Phase 2 remediation, 2026-11-11). **All ADRs are Proposed and provisional.** ADR-017 to ADR-022 are new and marked PENDING APPROVAL because each needs a stakeholder decision that this documentation cannot make.
No ADR is accepted until a named architect and the sponsor approve it.
Several depend on unanswered questions. Each ADR states what would change it.

Related: [ARD](ARD.md), [NFR](../03-Product-and-System-Requirements/NFR.md), [Open Questions](../02-Business-Analysis/Open-Questions.md).

| ADR | Title | Status | Depends on |
|---|---|---|---|
| ADR-001 | Modular monolith for the MVP | Proposed | OQ-015 |
| ADR-002 | Multi-tenancy by shared schema with row-level isolation | Proposed | OQ-005, OQ-019 |
| ADR-003 | Relational database as system of record | Proposed | OQ-024 |
| ADR-004 | Append-only double-entry ledger and stock ledger | Proposed | None |
| ADR-005 | Exact decimal money and rate handling | Proposed | OQ-008 |
| ADR-006 | Central authorisation: role plus scope attributes | Proposed | OQ-013 |
| ADR-007 | Shared configuration-driven approval engine | Proposed | OQ-013, OQ-026 |
| ADR-008 | Transactional outbox and background jobs | Proposed | None |
| ADR-009 | Idempotency keys on money and stock operations | Proposed | None |
| ADR-010 | REST over JSON with versioned API | Proposed | OQ-017 |
| ADR-011 | Audit written in the business transaction | Proposed | OQ-022 |
| ADR-012 | Concurrency control strategy | Proposed | OQ-031 |
| ADR-013 | Authentication and session model | Proposed | OQ-023 |
| ADR-014 | Caching and real-time notification scope | Proposed | OQ-015 |
| ADR-015 | Containerised deployment, orchestrator open | Proposed | OQ-016 |
| ADR-016 | Soft deletion, archival and retention | Proposed | OQ-022 |
| ADR-017 | Tenant model (answers OQ-019) | PENDING APPROVAL | OQ-019, OQ-005 |
| ADR-018 | Delivery model: single customer or product | PENDING APPROVAL | OQ-005 |
| ADR-019 | Sizing envelope | PENDING APPROVAL | OQ-015 |
| ADR-020 | Hosting model | PENDING APPROVAL | OQ-016, OQ-010 |
| ADR-021 | Technology constraints and database engine | PENDING APPROVAL | OQ-024 |
| ADR-022 | Authorisation tiers, four-eyes grants, controlled changes | PENDING APPROVAL | OQ-033, OQ-034 |

## ADR-001 Modular monolith for the MVP

| Item | Content |
|---|---|
| Context | 58 MVP modules share master data, approvals, ledger and stock. Most business operations span several modules and need one atomic transaction (for example, a goods receipt updates stock, the PO and the ledger). The team size and volumes are unknown (OQ-014, OQ-015). |
| Options | A. Modular monolith with strict module boundaries. B. Microservices per domain. C. Monolith without boundaries. |
| Decision | A. One deployable application, modules with explicit public interfaces, one database with module-owned schemas or table groups. |
| Rationale | Atomic cross-module transactions are simple. Operations and testing are simpler. Boundaries keep a later split possible. Microservices add distributed transactions and operations cost that the MVP does not justify without evidence of scale needs. |
| Consequences | One scaling unit for the application tier. Discipline is needed to prevent boundary erosion (enforced by dependency checks, NFR-035). Long-running work moves to background workers from the same codebase. |
| Revisit when | Volumes exceed what horizontal scaling of the monolith serves, or independent release cadence per domain becomes necessary (for example, manufacturing at R4). Candidates to extract first: notifications, reporting, integrations. |

## ADR-002 Multi-tenancy by shared schema with row-level isolation

| Item | Content |
|---|---|
| Context | The ERP is multi-company. It may also serve several customers (SaaS), which is not decided (OQ-005, OQ-019). Cross-company leakage is a severe risk (R-005). |
| Options | A. Shared database and schema, tenant and company keys on every row, row-level security policies. B. Schema per tenant. C. Database per tenant. |
| Decision | A as the default. Every business table carries `tenant_id` and `company_id`. The database enforces isolation through row-level security bound to the session context. The design keeps B and C possible for large or regulated tenants. |
| Rationale | Lowest operating cost and simplest upgrades. Database-level isolation protects against application bugs (NFR-018, TR-002). |
| Consequences | All queries run in a session with tenant and company context set. Cross-company group reports go through an explicit, audited function. Noisy-neighbour risk is managed by quotas and indexing. Backup and restore per tenant needs extra tooling. |
| Revisit when | OQ-019 is answered. If one customer needs a separate database for legal reasons, option C applies to that customer. |

## ADR-003 Relational database as system of record

| Item | Content |
|---|---|
| Context | Financial integrity, transactions and exact decimals are mandatory (TR-001). |
| Options | A. PostgreSQL. B. Another relational database (MySQL family, SQL Server, Oracle). C. Document store. |
| Decision | A relational database with ACID transactions, foreign keys, exact decimals, row-level locking and row-level security. PostgreSQL is the proposed product because it provides all of these natively. |
| Rationale | Document stores weaken referential integrity. PostgreSQL features match ADR-002 and ADR-012. |
| Consequences | Product choice is provisional pending OQ-024 and the technology evaluation (Stage 3 document). The logical design in [Database Design](Database-Design.md) is product-neutral except where it says otherwise. |
| Revisit when | The customer mandates another engine or the team has no capacity to run PostgreSQL. |

## ADR-004 Append-only double-entry ledger and stock ledger

| Item | Content |
|---|---|
| Context | Posted history must be trustworthy (CON-02, BRL-017, BRL-039). |
| Decision | The general ledger and the stock ledger are append-only tables. Corrections are reversing rows that reference the original. Balances are derived from rows, and may be cached in summary tables updated in the same transaction. The database denies update and delete on these tables to the application role. |
| Consequences | Reporting reads aggregates. Summary tables must be reconcilable to rows at any time (INV-02 to INV-06). Reversals double the row count for corrected items. |
| Revisit when | Never for immutability. Revisit summary table design if performance targets are missed. |

## ADR-005 Exact decimal money and rate handling

| Item | Content |
|---|---|
| Decision | Amounts are stored as exact decimals with a currency code, never as binary floating point. Proposed precision: amounts with 4 decimal places stored, displayed with the currency scale. Exchange rates with 8 decimal places. Each document stores transaction amount, rate, and base amount. Rounding follows the company rule (BRL-020, BRL-025). Rounding differences post to a configured rounding account. |
| Consequences | Application code uses a decimal type end to end, including in JSON (amounts as strings). Tests include rounding reference sets. |
| Revisit when | OQ-008 reveals currencies with unusual scales (for example, three decimals) or high-precision needs. |

## ADR-006 Central authorisation: role plus scope attributes

| Item | Content |
|---|---|
| Context | The requirements need role permissions plus company, branch, department and ownership limits (FR-015). |
| Options | A. Pure RBAC. B. Pure ABAC. C. RBAC for permissions with attribute-based scope rules. |
| Decision | C. Permission strings follow `module.submodule.action` (the 16 actions in FR-013). A scope assignment (company, branch, department, own-records) is evaluated with the record's attributes by one policy component used by every endpoint and every report query. Default is deny. Approval limits are attributes of the approval chain, not roles. |
| Consequences | A single place to test and audit. Scope filters are applied in queries (not after loading rows), so lists and counts do not leak. A permission cache must be invalidated on role change. |
| Revisit when | Policies need dynamic attributes beyond the scope model (for example, time or device), which would move toward fuller ABAC. |

## ADR-007 Shared configuration-driven approval engine

| Item | Content |
|---|---|
| Decision | One approval engine serves all documents. A chain definition is data: document type, conditions (value ranges, company, branch, department), ordered steps (role or user), timeouts, escalation. An approval instance records each decision append-only. Documents call the engine through an interface. |
| Consequences | New approvable documents need only registration and a chain, not new workflow code. The engine must evaluate conditions deterministically so tests can predict routing. Delegation and escalation are engine features, not module features. |
| Revisit when | Needs exceed linear chains (parallel approvals, conditional branches). A BPMN-style engine would then be evaluated. Chained delegation per OQ-026. |

## ADR-008 Transactional outbox and background jobs

| Item | Content |
|---|---|
| Decision | Domain events are written to an outbox table in the same transaction as the business change. A worker publishes them to subscribers (notifications, integrations, projections). Jobs run in workers with retry, backoff and a dead-letter state visible to authorised users. |
| Rationale | A committed business event is never lost if email or a bank is down (BR-060, BRL-062). |
| Consequences | Delivery is at least once. Consumers must be idempotent (ADR-009). The outbox needs cleanup. A message broker is optional in MVP because the database can host the queue. |
| Revisit when | Throughput exceeds what the database-backed queue handles. |

## ADR-009 Idempotency keys on money and stock operations

| Item | Content |
|---|---|
| Decision | Endpoints that post payments, receipts, deliveries, invoices and stock changes accept an `Idempotency-Key` header. The key, request fingerprint and outcome are stored for a retention window (proposed 24 hours, TBD). A repeat with the same key and body returns the stored result. A repeat with a different body is refused. |
| Consequences | Safe client and integration retries (FR-076, NFR-025). Storage and expiry job required. |

## ADR-010 REST over JSON with versioned API

| Item | Content |
|---|---|
| Options | A. REST. B. GraphQL. C. RPC. |
| Decision | A. Resource-oriented REST with JSON, versioned in the path (`/api/v1`). Pagination, filtering and sorting follow one convention. Errors use one structure with stable codes. |
| Rationale | Familiar to integrators, simple to rate limit and cache, easy to test. Reporting needs are met by dedicated report endpoints. |
| Consequences | Over-fetching is handled by sparse field selection where needed. |
| Revisit when | UI needs flexible cross-module queries that REST serves poorly. |

## ADR-011 Audit written in the business transaction

| Item | Content |
|---|---|
| Decision | The audit record is created in the same transaction as the change (FR-032). Audit tables are append-only and partitioned by time. Sensitive field values are masked or hashed where law requires. Sign-in and permission events use the same mechanism. |
| Consequences | Audit write failure rolls back the business change. Volume is high and needs partitioning and archival (ADR-016). |

## ADR-012 Concurrency control strategy

| Item | Content |
|---|---|
| Decision | Mutable documents use optimistic locking by version number (FR-125). Stock balances use row-level locks on item and warehouse during posting, taken in a fixed order to avoid deadlocks (FR-113). Document numbers come from a per-series counter row locked in the posting transaction, or a database sequence where gaps are allowed (FR-025). Transactions are short and retried on serialisation failure. |
| Consequences | Strict gap-free numbering serialises posting per series. Whether gaps are allowed is a policy question (OQ-031). |

## ADR-013 Authentication and session model

| Item | Content |
|---|---|
| Decision | Local credentials with a strong password hash, optional MFA (time-based codes), and OpenID Connect for SSO when required. Short-lived access tokens with refresh tokens bound to a server-side session record so sessions can be revoked at once (FR-004). Tokens are not stored in places scripts can read. Cross-site request protection applies to cookie-based flows. |
| Consequences | A session store is needed. Token lifetimes are configurable (values TBD). |
| Revisit when | OQ-023 is answered (SAML needs, mandatory MFA). |

## ADR-014 Caching and real-time notification scope

| Item | Content |
|---|---|
| Decision | Cache only reference data and permission sets, keyed by tenant, with explicit invalidation. Never cache financial balances or stock availability beyond the transaction (TR-015). In-app notifications use server push over a standard web channel where available, with polling as fallback. |
| Consequences | An in-memory store is introduced when measured need exists, not before. Real-time is limited to notifications and approval counts in the MVP. |

## ADR-015 Containerised deployment, orchestrator open

| Item | Content |
|---|---|
| Decision | The application, workers and supporting services are packaged as containers, built once and promoted across environments, with infrastructure as code. The choice between a managed container service, Kubernetes, or virtual machines is deferred to OQ-016. |
| Consequences | Environment parity (NFR-038). Infrastructure detail in [Infrastructure Architecture](Infrastructure-Architecture.md) is vendor-neutral. |

## ADR-016 Soft deletion, archival and retention

| Item | Content |
|---|---|
| Decision | Master records are deactivated, not deleted (BRL-015). Transactions are cancelled or reversed, not deleted. Retention periods are configuration (FR-127). Closed-period data older than a configured age moves to archive storage while remaining queryable by reports and drill-down (NFR-042). Personal data is anonymised without breaking ledger totals (FR-129). |
| Consequences | Queries must filter inactive records explicitly. Archive design must keep keys stable. Periods are TBD (OQ-022). |


## ADR-017 Tenant model (answers OQ-019)

| Item | Content |
|---|---|
| Status | **PENDING APPROVAL** (recommendation only, depends on OQ-019 and OQ-005) |
| Context | The ERP is multi-company. It is not yet known whether one customer runs many companies in one isolated tenant, or all companies share one tenant. Cross-customer leakage is a severe risk (R-005, THR-015). |
| Decision (recommended) | Option A as the default, with `tenant_id` on every business row from the first release, even when only one customer exists. Option D is available for any customer with a legal or contractual need. Option B is rejected. |
| Rationale | The cost of adding `tenant_id` now is small. The cost of adding it after build is a rewrite. Database-enforced isolation protects against application bugs (NFR-018, TR-002). |
| Assumptions | Tenants have different data residency needs only if OQ-010 says so. The first customer accepts shared infrastructure. A policy-test suite runs in every release. |
| Impact if not approved or changed | If the sponsor chooses a single private deployment with no SaaS, tenant count is one and nothing in the design changes. If a customer requires a separate database, option D applies to that customer and deployment effort rises. |
| Revisit when | OQ-005 or OQ-019 is answered, or the first regulated customer appears. |

Options considered:

| Option | Advantages | Disadvantages | Security | Scalability | Operations | Data isolation |
|---|---|---|---|---|---|---|
| A. Tenant per customer, many companies inside, shared schema with row-level security (current ADR-002) | Lowest cost. One upgrade path. Database enforces isolation. Group consolidation stays inside a tenant | Noisy neighbours. Per-tenant restore needs tooling. One bug in policy reaches all tenants | Strong with RLS and tests (INV-12). Policy mistakes affect everyone | Good. Partition by tenant | Low | Logical, enforced by the database |
| B. No tenant concept, all companies in one pool | Simplest | Unsafe for more than one customer. Retrofit of tenant_id is a rewrite | Weak across customers | Good | Lowest | Company only |
| C. Schema per tenant | Better blast radius. Per-tenant restore | Migrations multiply. Connection pooling harder | Good | Medium (thousands of schemas strain catalogs) | High | Strong logical |
| D. Database per tenant | Strongest isolation. Per-tenant residency and restore | Highest cost and operations. Cross-tenant reporting needs a warehouse | Strongest | Scales by adding databases | Highest | Physical |

## ADR-018 Delivery model: single customer or product (answers OQ-005)

| Item | Content |
|---|---|
| Status | **PENDING APPROVAL** (recommendation only, depends on OQ-005) |
| Context | The sponsor has not said whether the ERP is for one customer or sold to many. This changes onboarding automation, support model and release process. |
| Decision (recommended) | Option C: build as a tenant-capable product, deploy first for one customer. |
| Rationale | The extra cost is small and the decision can be deferred without rework. It avoids choosing for the sponsor. |
| Assumptions | The sponsor wants the option to sell later. No billing or self-service onboarding is in the MVP. |
| Impact if not approved or changed | If the sponsor says single customer only, drop provisioning tooling and keep tenant_id for safety. If SaaS, add billing, provisioning and tenant-level monitoring before release. |
| Revisit when | OQ-005 is answered. |

Options considered:

| Option | Advantages | Disadvantages | Security | Scalability | Operations | Data isolation |
|---|---|---|---|---|---|---|
| A. Single-customer private deployment | Simplest operations. Custom fit | Rework needed to sell later. Upgrades are per site | Strong, single owner | Limited by one deployment | Low at first, rising per site | Physical |
| B. Multi-customer SaaS from day one | Efficient to scale. One release train | Needs tenant provisioning, billing, support tooling, stronger isolation proof | Must prove isolation | Best | High | Logical (ADR-017) |
| C. Product that can run either way | Keeps both options. Only extra cost is tenant_id and a provisioning script | Slightly more design discipline | Same as B | Same as B | Medium | Per deployment |

## ADR-019 Sizing envelope (answers OQ-015)

| Item | Content |
|---|---|
| Status | **PENDING APPROVAL** (recommendation only, depends on OQ-015) |
| Context | Users, companies, branches and volumes are unknown. NFR-005 holds a design assumption of 100 companies, 2,000 concurrent users and 20 million ledger rows per year. |
| Decision (recommended) | Design to option C. Deploy at the size the first customer needs. Prove the design with a load test at option C volumes on staging (NFR-005). |
| Rationale | Design headroom is cheap for stateless tiers and expensive to retrofit in the data model. The modular monolith (ADR-001) scales out. |
| Assumptions | The NFR-005 numbers are the starting assumptions, not confirmed business numbers. Report load is lower than posting load. |
| Impact if not approved or changed | If real volumes are far smaller, deploy smaller and skip replicas. If larger, partitioning and a reporting store become necessary earlier. |
| Revisit when | IT manager supplies volumes. |

Options considered:

| Option | Advantages | Disadvantages | Security | Scalability | Operations | Data isolation |
|---|---|---|---|---|---|---|
| A. Small design target (up to 5 companies, 100 users) | Cheap | Needs rework to grow | Same | Low | Low | Same |
| B. Medium target (up to 25 companies, 500 users) | Balanced | May need rework for large customers | Same | Medium | Medium | Same |
| C. Large target (NFR-005 assumption: 100 companies, 2,000 concurrent users, 20 million rows per year) | Headroom without redesign. Stateless tiers scale out | Over-built for a small first customer if deployed at full size | Same | High | Medium. Size deployment to need | Same |

## ADR-020 Hosting model (answers OQ-016)

| Item | Content |
|---|---|
| Status | **PENDING APPROVAL** (recommendation only, depends on OQ-016, OQ-010) |
| Context | Hosting preference, region and certifications are unknown. |
| Decision (recommended) | Design provider-neutral: containers, infrastructure as code, standard relational database. Prefer option A in a region that satisfies OQ-010. Option B is supported by the same design. |
| Rationale | Provider-neutral design keeps the choice open. Managed failover reduces the operational risk of NFR-009 and NFR-010. |
| Assumptions | No certification is mandatory until OQ-010 and OQ-016 are answered. |
| Impact if not approved or changed | If residency forces on-premises, add database failover tooling and a larger operations team to the plan. |
| Revisit when | OQ-010 or OQ-016 is answered. |

Options considered:

| Option | Advantages | Disadvantages | Security | Scalability | Operations | Data isolation |
|---|---|---|---|---|---|---|
| A. Public cloud with managed services | Fast, managed failover and backups, regions | Provider dependence. Residency depends on region availability | Strong baseline, shared responsibility | High | Low to medium | Logical plus account separation |
| B. Private cloud or on-premises | Full control. Residency certain | Capital cost. Staff needed for failover, backup and patching | Depends on team | Limited by hardware | High | Physical |
| C. Managed hosting or hybrid | Control with some outsourcing | Contract complexity | Mixed | Medium | Medium | Mixed |

## ADR-021 Technology constraints and database engine (answers OQ-024)

| Item | Content |
|---|---|
| Status | **PENDING APPROVAL** (recommendation only, depends on OQ-024) |
| Context | No language or database preference is known. Some requirements constrain the database: deferred balance check at commit (DBC-02), row-level security (SEC-016), exact decimals (ADR-005), point-in-time recovery (NFR-009). |
| Decision (recommended) | Database: option A or B because they meet DBC-02 and SEC-016 natively. Option A is recommended on cost. Application language and framework: no recommendation. Choose by team skill, hiring market and the listed constraints. |
| Rationale | The recommendation is limited to the requirement-driven constraints. A language decision needs facts about the team. |
| Assumptions | No existing licence or mandated vendor. |
| Impact if not approved or changed | If a mandated engine lacks deferrable constraints or RLS, DBC-02 and SEC-016 must be redesigned and re-tested. |
| Revisit when | OQ-024 is answered. |

Options considered:

| Option | Advantages | Disadvantages | Security | Scalability | Operations | Data isolation |
|---|---|---|---|---|---|---|
| A. PostgreSQL | Row-level security, deferrable constraints, exact numeric, point-in-time recovery, no licence cost | Team skill needed | Strong | High with replicas and partitioning | Medium | RLS native |
| B. SQL Server | Row-level security, strong tooling | Licence cost. Deferred constraint check needs a design workaround | Strong | High | Medium | RLS native |
| C. MySQL or MariaDB | Familiar to many teams | No deferrable constraints and no native row-level security, so DBC-02 and SEC-016 need application workarounds | Weaker | Medium | Low | Application only |
| D. Oracle | Strong features | Cost and licence | Strong | High | High | VPD |

## ADR-022 Authorisation tiers, four-eyes grants and controlled changes

| Item | Content |
|---|---|
| Status | **PENDING APPROVAL** by the security officer and sponsor (depends on OQ-033, OQ-034) |
| Context | The RBAC audit found that Branch Admin and Organization Admin could assign roles with no authority ranking, and that Finance Manager could approve and release the same payment. See `RBAC-Matrix.md` section 8A. |
| Decision (recommended) | Role tiers T0 to T4 plus Oversight. A grantor grants only lower tiers in own scope. Manager-tier, Auditor and approve, release, reverse roles need a second approver. Payment prepare, approve and release are three users. Controlled data changes use request and approval. Sealed emergency account with two-person release. |
| Rationale | Enforces least privilege and separation of duties in the policy component (ADR-006) with checks in the database for the payment and request tables (DBC-13 to DBC-20). |
| Assumptions | A company can staff two Finance Managers, or accepts the SoD override. At least two Super Admin users exist. |
| Impact if not approved | Escalation and self-approval paths remain (RBAC audit E1 to E10). |
| Revisit when | OQ-033 to OQ-037 are answered. |

Options considered:

| Option | Advantages | Disadvantages | Security | Scalability | Operations | Data isolation |
|---|---|---|---|---|---|---|
| A. Tiers plus four-eyes plus controlled changes (recommended) | Closes all audit findings. Auditable | More steps. Needs two people in some roles | Strong | No effect | Medium (staffing) | Unchanged |
| B. Rely on SoD pairs only | Simple | Does not stop role grants above authority | Weak for grants | No effect | Low | Unchanged |
| C. External approval tool | Separation from the ERP | Integration and cost. Second system to secure | Medium | No effect | High | Unchanged |

Decisions in ADR-006, ADR-007 and ADR-013 are updated by this ADR: access token lifetime 5 minutes (proposed), per-request revocation, step-up window 5 minutes (proposed).

## Decisions not yet made

| Topic | Needed input |
|---|---|
| Programming language and framework | OQ-024. ADR-021 gives criteria, not a choice |
| Cloud provider and region | OQ-016, OQ-010 |
| Message broker versus database queue | OQ-015 |
| Search engine | Only if search targets are not met on the database |
| Reporting store (replica or warehouse) | OQ-015 and report volumes |
| Gap-free numbering policy | OQ-031 |
