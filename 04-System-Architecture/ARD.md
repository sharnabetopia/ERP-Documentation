# Architecture Requirements and Design Document (ARD)

Document status: Draft v0.2 (Phase 2 remediation, 2026-11-11). **Provisional.** The five foundational questions (OQ-005, OQ-015, OQ-016, OQ-019, OQ-024) now have a recommended decision in ADR-017 to ADR-021, each **PENDING APPROVAL**. Nothing is confirmed until a stakeholder approves it.
Related: [ADR](Architecture-Decision-Records.md), [System Architecture](System-Architecture.md), [NFR](../03-Product-and-System-Requirements/NFR.md).

## 1. Architecturally significant requirements

| Driver | Source | Consequence |
|---|---|---|
| Financial integrity: balanced, immutable, exact | BR-014, CON-01, CON-02, NFR-020, NFR-021 | Relational ACID store, append-only ledgers, exact decimals (ADR-003 to ADR-005) |
| Strict data isolation by company and scope | BR-001, BR-003, NFR-018 | Row-level isolation and central policy component (ADR-002, ADR-006) |
| Configurable approvals | BR-006 | Shared approval engine (ADR-007) |
| Complete, atomic audit | BR-005, NFR-026 | Audit in the business transaction (ADR-011) |
| Cross-module atomic operations (receipt, delivery, invoice) | BR-027, BR-042 | Modular monolith (ADR-001) |
| Safe retries and failures of external calls | BR-060, NFR-025 | Outbox, jobs, idempotency (ADR-008, ADR-009) |
| Concurrency on stock and numbering | BR-035, BR-012 | Locking strategy (ADR-012) |
| Growth in modules and industries | BR-061, NFR-006 | Module boundaries and configuration (ADR-001, FR-130) |
| Sizing and service levels unknown | OQ-015, OQ-028 | Design for horizontal scaling of the application tier. Targets are placeholders |

## 2. Principles

1. Correctness before speed. Money and stock operations are transactional.
2. One owner per data item. Modules access others through interfaces, not tables.
3. Deny by default.
4. Configuration before code.
5. Everything that matters is audited, and the audit cannot be bypassed.
6. Prefer the simplest deployment that meets the targets. Split only on evidence.
7. Every external dependency can fail without losing business events.

## 3. Capability evaluation (Phase 10 list)

Status values: Decided (provisional) means an ADR proposes it. Open means more input is needed.

| # | Capability | Approach for the MVP | ADR | Status |
|---|---|---|---|---|
| 1 | Multi-tenancy | Shared schema with row-level isolation. Option for dedicated databases | ADR-002 | Decided (provisional) |
| 2 | Multi-company and multi-branch | Company key on every row. Branch and department as scope dimensions | ADR-002, ADR-006 | Decided (provisional) |
| 3 | Multi-language and localisation | Externalised text, locale-aware formatting, UTC storage | NFR-034, NFR-041 | Open (languages, OQ-011) |
| 4 | Multi-currency and time zone | Transaction currency, rate and base amount on every financial row | ADR-005 | Decided (provisional) |
| 5 | Configurable approval engine | Chains as data | ADR-007 | Decided (provisional) |
| 6 | Dynamic workflow engine | Linear approval chains in MVP. Fuller workflow evaluated later | ADR-007 | Deferred |
| 7 | Central notification engine | Event-driven, templates, channels, retry | ADR-008 | Decided (provisional) |
| 8 | Background jobs | Worker processes with retry and dead-letter | ADR-008 | Decided (provisional) |
| 9 | Event-driven architecture | Internal domain events through an outbox. No external broker required for MVP | ADR-008 | Decided (provisional) |
| 10 | Real-time communication | Push for notifications only, polling fallback | ADR-014 | Decided (provisional) |
| 11 | Caching | Reference data and permission sets only | ADR-014 | Decided (provisional) |
| 12 | API versioning | Path versioning, backward compatible within a version | ADR-010 | Decided (provisional) |
| 13 | Rate limiting | Per user, per address, per tenant at the gateway and in the application | NFR-017 | Open (values) |
| 14 | Idempotency | Idempotency keys for money and stock operations | ADR-009 | Decided (provisional) |
| 15 | Centralised error handling | One error structure and code catalogue, correlation id | TR-018, API doc | Decided (provisional) |
| 16 | Logging and monitoring | Structured logs, metrics, traces, alerts | NFR-028, NFR-029 | Open (tooling) |
| 17 | Data synchronisation | Outbox-driven, at-least-once, idempotent consumers. Bank and gateway adapters pull and push | ADR-008 | Decided (provisional) |
| 18 | Integration retry | Exponential backoff, dead-letter, manual retry | ADR-008, FR-128 | Decided (provisional) |
| 19 | High availability | Multiple stateless application instances. Database with standby and automatic failover | Deployment doc | Open (RTO, OQ-028) |
| 20 | Scalability | Horizontal scale of application and workers. Vertical scale and read replicas for the database. Partition large tables | ADR-001 | Open (volumes, OQ-015) |
| 21 | Disaster recovery | Cross-zone or cross-region standby, restore tested | Deployment doc | Open (OQ-028) |
| 22 | Backup and restoration | Continuous log archiving plus daily full, encrypted, restore tested | NFR-011 | Decided (provisional) |
| 23 | Performance optimisation | Index design, summary tables, background reports, query review in CI | NFR-001 to NFR-004 | Open (volumes) |
| 24 | Data migration | Staged import with validation and reconciliation | [Data Migration Strategy](Data-Migration-Strategy.md) | Decided (provisional) |

## 4. Modular monolith versus microservices (summary)

| Criterion | Modular monolith | Microservices | Verdict for MVP |
|---|---|---|---|
| Atomic multi-module posting | Native | Needs sagas | Monolith |
| Team and ops capacity (unknown) | Low need | High need | Monolith |
| Independent scaling | Coarse | Fine | Not needed yet |
| Independent release | Coupled | Independent | Not needed yet |
| Consistency of financial data | Strong | Eventual between services | Monolith |
| Future extraction | Possible with boundaries | n/a | Keep boundaries |

Full reasoning is in ADR-001.

## 5. Module boundaries (proposed)

| Module group | Owns | May call |
|---|---|---|
| Identity and access | Users, roles, scopes, sessions | Audit |
| Organisation | Companies, branches, departments, employees core | Identity, Audit |
| Platform services | Numbering, settings, attachments, notifications, import and export, approvals | Organisation, Identity |
| Master data | Parties base, items, units, currencies, taxes | Organisation, Platform |
| Finance | Ledger, periods, journals, payments, AP, AR, tax, bank | Master data, Platform |
| Procurement | Requisitions, POs, GRN, vendor invoices | Master data, Inventory, Finance, Platform |
| Inventory | Warehouses, stock ledger, transfers, adjustments, valuation | Master data, Finance, Platform |
| Sales | Customers, quotations, orders, deliveries, invoices, returns | Master data, Inventory, Finance, Platform |
| Reporting | Read models and report definitions | Read access to others through views or replicas |

Rule: lower layers never call higher ones. Procurement and Sales call Inventory and Finance through their public interfaces. Finance does not know about Procurement or Sales documents except through posting requests and source references.

## 6. Risks and trade-offs

| Risk | Handling |
|---|---|
| Shared schema isolation bug leaks data | Row-level security in the database plus isolation test suite (R-005) |
| Monolith boundaries erode | Dependency rules checked in CI (NFR-035) |
| Strict numbering limits throughput | Decide gap policy early (OQ-031) |
| Summary tables drift from ledger | Reconciliation job and invariants INV-02 to INV-06 |
| Database becomes the bottleneck | Read replicas for reports, partitioning, archive (ADR-016) |
| Wrong tenancy choice | OQ-019 resolved before build. Design allows dedicated database per tenant |

## 6A. Foundational decision register (v0.2)

| Question | Recommended decision | ADR | Status | Blocks MVP build? |
|---|---|---|---|---|
| OQ-019 tenant model | Tenant per customer, many companies inside, shared schema with row-level security, tenant_id on every row | ADR-017 | PENDING APPROVAL | Yes for the schema. The recommendation is safe in both outcomes |
| OQ-005 single customer or product | Tenant-capable product, first deployed for one customer | ADR-018 | PENDING APPROVAL | No |
| OQ-015 sizing | Design to NFR-005 envelope, deploy at real size, prove with a load test | ADR-019 | PENDING APPROVAL | No for design, yes for infrastructure sizing |
| OQ-016 hosting | Provider-neutral containers, public cloud preferred in a region meeting OQ-010 | ADR-020 | PENDING APPROVAL | Yes for infrastructure, no for application design |
| OQ-024 technology | PostgreSQL or SQL Server for requirement-driven reasons. Language by team skill | ADR-021 | PENDING APPROVAL | Yes before build starts |

The authorisation design in ADR-022 is a sixth decision added by the audit.

## 7. What this document does not decide

Language, framework, cloud provider, broker, search engine and observability tooling. See the list at the end of the [ADR](Architecture-Decision-Records.md).
