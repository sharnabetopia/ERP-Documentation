# Development Guidelines

Document status: Draft v0.1 (Stage 3 part 2). Proposed. **Technology-neutral.** The language, framework and tools are not chosen (OQ-024), so rules are stated as practices to apply in whichever stack is selected.
Related: [Coding Standards](Coding-Standards.md), [Definition of Done](Definition-of-Done.md), [CI/CD Strategy](CICD-Strategy.md), [ARD](../04-System-Architecture/ARD.md), [ADR](../04-System-Architecture/Architecture-Decision-Records.md).

## 1. Working principles

1. Requirements are the source of truth. Work starts from a story that meets the Definition of Ready and names its FRs, BRLs and API items.
2. Correctness of money and stock comes first. Speed and elegance come after.
3. Small, reviewed, tested changes merged often.
4. Security and audit are part of every story, not a later phase.
5. The architecture decisions (ADR-001 to ADR-016) are binding until changed through an ADR. They are provisional until OQ-005, OQ-015, OQ-016, OQ-019, OQ-024 are answered.

## 2. Repository and branching

| Topic | Guideline |
|---|---|
| Repositories | One repository for the modular monolith and workers. Infrastructure code in a separate repository or folder with its own review |
| Branching | Short-lived branches from the main branch. Names include the story ID. Releases tagged from the main branch. Long-lived release branches only for supported versions after the first production release |
| Commits | Small, one logical change, message states what and why and the story ID. Commits are attributed to the person who authored them |
| Pull requests | Link the story and requirements. Describe behaviour change, test evidence, migration and rollback. Keep under a size the reviewer can read in one sitting |
| Reviews | At least one reviewer who is not the author. A second reviewer for changes to posting, authorisation, approval, numbering, stock locking, migrations or security settings |
| Protected branch | No direct pushes. Required checks must pass (see CI/CD) |

## 3. Module structure and boundaries

1. Follow the modules and call rules in ARD section 5. Lower layers never call higher ones. Procurement and Sales call Finance and Inventory through public interfaces only.
2. A module owns its tables. No other module reads or writes them directly (NFR-035). Cross-module reads use interfaces or read models.
3. Dependency rules are checked in the build. A violation fails the build.
4. Shared code is limited to platform services (numbering, audit, approval, notification, outbox, policy) and small utilities. No shared "everything" library.
5. Each public interface is documented and versioned inside the codebase.

## 3. Money, quantities and time

| Rule | Detail |
|---|---|
| Decimal types only for money, rates and quantities | Never binary floating point (NFR-020). JSON carries them as strings |
| Rounding | One shared rounding component following company rules (BRL-020, BRL-025). No ad hoc rounding in modules |
| Currency | Amounts always travel with a currency code. Base amount computed at posting and stored |
| Time | Store instants in UTC. Business dates are dates, not instants (NFR-041). Never use local server time |
| Clock | Code reads time through an injectable clock so tests can control it |

## 4. Transactions and consistency

1. A business operation is one database transaction: document change, ledger or stock rows, audit record, outbox event (ADR-008, ADR-011).
2. Never call an external service inside a transaction. Use the outbox.
3. Stock and numbering use the locking rules in ADR-012, taking locks in the documented order.
4. Retry only on serialisation failures and deadlocks, with limits. Never retry blindly on business errors.
5. Money and stock endpoints require an idempotency key (ADR-009).
6. Posted records are never updated or deleted. Corrections are new records (ADR-004).

## 5. Security practices (see SEC-001 to SEC-053)

| Practice | Rule |
|---|---|
| Authorisation | Every endpoint declares its permission code and passes through the policy component (SEC-013) |
| Input | Validate on the server, parameterised queries only, allow-listed sort and filter fields (SEC-023, SEC-024) |
| Output | Encode for context (SEC-025). No internal details in errors (SEC-030) |
| Secrets | Only from the secret store. Never in code, tests, logs or images (SEC-038) |
| Personal data | Minimise, mask in logs and exports (SEC-037, SEC-039) |
| Dependencies | Add a dependency only with a reason, licence check and scan. Pin versions |
| Threat model | New features update the Threat Model if they change a trust boundary |

## 6. API and data conventions

Follow the [API Documentation](../04-System-Architecture/API-Documentation.md) conventions: versioned path, standard error body, pagination, optimistic concurrency, idempotency. Database changes follow the [Database Design](../04-System-Architecture/Database-Design.md): tenant and company keys, audit columns, soft deletion for masters, foreign keys everywhere.

## 7. Migrations

1. Every schema change is a versioned migration reviewed with the code.
2. Backward compatible in two steps (expand, then contract), so old and new code run together during rolling deployment (Deployment Architecture).
3. Migrations are tested on a copy with sized data for duration and locking.
4. No migration changes posted ledger or stock rows. Data fixes are new corrective records.

## 8. Logging and errors

Structured logs with correlation id (TR-018). Levels used consistently. No secrets or Restricted data. Business errors return the catalogue code. Unexpected errors are logged with context and return a generic message with the correlation id.

## 9. Testing practice

Unit tests with no database, network or external process. Integration tests with real database and stubbed partners. Every defect fix adds a failing-then-passing test. Posting, tax, rounding, valuation, authorisation, approval, numbering and stock locking must have thorough tests (see [Test Strategy](../08-QA-and-Testing/Test-Strategy.md) for coverage targets). Tests are deterministic and independent.

## 10. Documentation in code

Public interfaces, non-obvious business rules and decisions are documented next to the code with the BRL or ADR ID. Requirements documents are updated in the same change when behaviour changes. Do not duplicate requirements in code comments.

## 11. Configuration and feature flags

Environment differences are configuration only (TR-017). Feature flags hide unfinished work and switch modules per tenant. Flags have an owner and a removal date.

## 12. Onboarding checklist for a developer

Read the README, ADRs, ARD, Database Design, API conventions, RBAC Matrix and these guidelines. Run the project locally with test data. Complete one small reviewed change before touching posting or authorisation code.
