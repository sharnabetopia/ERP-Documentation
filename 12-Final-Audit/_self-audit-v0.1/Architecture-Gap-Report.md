# Architecture Gap Report

Document status: Draft v0.1 (final audit, 2026-11-11). Self-review by the author, not an independent architecture review. Counts measured by script.
Related: [ARD](../04-System-Architecture/ARD.md), [ADR](../04-System-Architecture/Architecture-Decision-Records.md), [System Architecture](../04-System-Architecture/System-Architecture.md), [Database Design](../04-System-Architecture/Database-Design.md), [API Documentation](../04-System-Architecture/API-Documentation.md), [Documentation Audit Report](Documentation-Audit-Report.md).

## 1. Summary

1. The architecture is a **provisional proposal**. All 16 ADRs are Proposed. 16 of the 24 capabilities in the ARD are marked decided (provisional), the rest Open or Deferred.
2. The most consequential assumption is the tenancy model (ADR-002), which depends on OQ-005 and OQ-019.
3. No technology stack, hosting provider, broker, search engine or reporting store is chosen.
4. Consistency checks between architecture documents pass (section 3). That shows the documents agree with each other, not that the design is right.

## 2. Architecture decisions and what they depend on

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

ADRs depending on at least one open question: 13 of 16.
Most-referenced open questions: OQ-015 (2 ADRs), OQ-013 (2 ADRs), OQ-022 (2 ADRs), OQ-005 (1 ADRs), OQ-019 (1 ADRs), OQ-024 (1 ADRs).

### Capability status (ARD, 24 capabilities)

| Status | Count |
|---|---|
| Decided | 16 |
| Open | 7 |
| Deferred | 1 |

## 3. Consistency checks (script)

| Check | Result |
|---|---|
| All status-bearing entities in the data dictionary have a table in the database design | Yes (26 entities with status lists, 87 tables) |
| Workflow API dependencies exist in the API document | 54 references checked, 0 unmatched |
| API permission codes exist in the permission catalogue | All (checked when the RBAC matrix is generated: 132 codes used, 211 in catalogue) |
| Every MVP module appears in the dependency table | 58 of 58 |
| Every critical workflow has a swimlane and a sequence diagram | Yes (6 of 6) |
| Database constraints DBC-01 to DBC-12 have written tests | 12 of 12 |
| Architecture documents contain no broken links | 0 broken |

## 4. Gaps

Severity: High means it can invalidate the design or block the build. Medium means it limits quality. Low is polish.

| ID | Gap | Severity | Resolves with |
|---|---|---|---|
| AG-01 | Tenancy model (single customer, SaaS, shared or dedicated database) undecided | High | OQ-005, OQ-019 |
| AG-02 | Sizing unknown: users, companies, volumes, peak posting rate. Database partitioning, replica and summary-table design cannot be confirmed | High | OQ-015 |
| AG-03 | Technology stack, language and framework not chosen. Several TRs and ADRs assume features (row-level security, decimal types, locking) that the chosen stack must support | High | OQ-024, technology evaluation |
| AG-04 | Hosting, region and provider not chosen. HA, DR, residency and certificates depend on it | High | OQ-016, OQ-010, OQ-028 |
| AG-05 | Technology Stack Evaluation (document 35 in the brief) is not written. The brief lists it. The folder plan has no file for it | Medium | Write after OQ-024 inputs |
| AG-06 | Numbering policy (gap-free or not) affects throughput and locking design | Medium | OQ-031 |
| AG-07 | Data dictionary has no per-column definitions. API has no request and response schemas. No physical design | Medium | Physical design phase |
| AG-08 | Audit hash chain design and anchoring unreviewed | Medium | Architect and security review |
| AG-09 | Tenant-level backup and restore not designed (shared schema makes it hard) | Medium | OQ-019, Backup and Recovery |
| AG-10 | Reporting architecture (read replica vs warehouse), search engine, cache need not proven | Medium | Load tests, OQ-015 |
| AG-11 | Approval engine handles linear chains only. Parallel and conditional approvals would need a richer engine | Low | Revisit with real chains |
| AG-12 | C4 component views drawn for Finance and Procurement only. Inventory, Sales, Identity, Platform not drawn | Low | Before build |
| AG-13 | Multi-company group consolidation and inter-company transactions not designed | Medium | Business need, OQ-019 |
| AG-14 | Real-time communication limited to notifications. No design for scanners or offline warehouse use | Low | OQ-025 |
| AG-15 | Data-level threat model (per data flow) not done. Current model is per component | Medium | After physical design |
| AG-16 | No capacity or cost model | Medium | OQ-015, OQ-016 |
| AG-17 | Integration partners unknown, so adapter contracts are undefined | Medium | OQ-017 |
| AG-18 | Migration tooling and legacy sources unknown | Medium | OQ-012 |
| AG-19 | Diagrams not visually reviewed | Low | Review |

## 5. Open questions that gate architecture

| ID | Question | Owner |
|---|---|---|
| OQ-005 | Is this a single-customer system or a product sold to many customers (SaaS)? | Sponsor |
| OQ-010 | Where must data be stored (country or region) and which privacy laws apply? | Compliance officer |
| OQ-015 | Expected users, companies, branches and daily transaction volumes? | IT manager |
| OQ-016 | Hosting preference (public cloud provider, private, on-premises) and required certifications? | IT manager |
| OQ-019 | Does each customer get its own isolated tenant holding many companies, or do all companies share one tenant? | Architect, Sponsor |
| OQ-024 | Are there technology constraints or preferences (languages, database, cloud, team skills)? | IT manager |
| OQ-028 | What are the required service levels (availability, RPO, RTO, support hours)? | Sponsor, IT manager |
| OQ-031 | Must document numbers be gap-free (legal or audit rule), or may gaps exist after rollbacks or cancellations? | Finance manager, Compliance officer |

## 6. Recommendations

1. Hold an architecture decision workshop to answer OQ-005, OQ-015, OQ-016, OQ-019, OQ-024 and OQ-031. Update the ADRs from Proposed to Accepted or Replaced.
2. Write the Technology Stack Evaluation once constraints are known (AG-05).
3. Build a thin vertical prototype of the posting engine, approval engine and policy component with row-level isolation and test them against the invariants and the isolation matrix before committing to the stack.
4. Run an early load test of numbering, stock locking and ledger inserts at the sized volume (AG-02, AG-06).
5. Complete physical design, per-column dictionary and API schemas.
6. Have an independent architect review the package.
