# Coding Standards

Document status: Draft v0.1 (Stage 3 part 2). Proposed. **Language-neutral rules.** Language-specific style guides and linters are chosen after OQ-024 and added as an appendix. Where a rule names a tool class (formatter, linter) any equivalent tool in the chosen stack applies.
Related: [Development Guidelines](Development-Guidelines.md), [Definition of Done](Definition-of-Done.md).

## 1. General

| ID | Rule |
|---|---|
| CS-001 | One formatter and one linter configuration for the whole codebase, enforced in CI. No style debates in review |
| CS-002 | Names state business meaning using the [glossary](../02-Business-Analysis/Business-Glossary.md) terms (purchase order, not "PO doc"). No abbreviations except those in the glossary |
| CS-003 | Functions do one thing. Prefer small functions and early returns. Avoid deep nesting |
| CS-004 | No dead code, no commented-out code, no unused dependencies |
| CS-005 | Public interfaces and business rules carry a short comment naming the rule or requirement ID (for example BRL-036). Do not comment what the code already says |
| CS-006 | No hard-coded business values (tolerances, limits, tax rates, account codes, thresholds). They are configuration (NFR-037) |
| CS-007 | Static analysis findings of severity high are fixed before merge |

## 2. Money, quantity and numbers

| ID | Rule |
|---|---|
| CS-010 | Exact decimal type for all money, rate and quantity values. Never float or double (NFR-020) |
| CS-011 | Every amount has a currency. Arithmetic on amounts of different currencies is a compile-time or runtime error |
| CS-012 | Rounding only through the shared rounding component |
| CS-013 | Division by zero and overflow are handled explicitly |
| CS-014 | Comparisons of money use exact equality on decimals, never tolerance comparisons |

## 3. Data access

| ID | Rule |
|---|---|
| CS-020 | Parameterised queries only. No string-built SQL (SEC-024) |
| CS-021 | Every query for tenant data runs under the tenant and company context. A query without context fails (SEC-016) |
| CS-022 | Select only needed columns. No unbounded queries. Lists are paginated |
| CS-023 | No query inside loops where a set query works. Review query plans for hot paths |
| CS-024 | Transaction boundaries are explicit and short. Lock order is documented and followed (ADR-012) |
| CS-025 | Append-only tables have no update or delete code paths |

## 4. Errors and results

| ID | Rule |
|---|---|
| CS-030 | Business rule failures return a catalogue error code (API Documentation section 4), not a generic exception |
| CS-031 | Never swallow exceptions. Log with correlation id and rethrow or convert |
| CS-032 | Error messages to users use the message catalogue (MSG-nnn). Internal detail stays in logs |
| CS-033 | Fail fast on invalid state. Do not continue with partial data |

## 5. Security

| ID | Rule |
|---|---|
| CS-040 | Authorise in the server for every operation. Check permission and scope through the policy component (SEC-013, SEC-014) |
| CS-041 | Validate all input at the boundary. Reject unknown fields on write endpoints (SEC-015) |
| CS-042 | Encode output for its context. Never build markup from user input |
| CS-043 | No secrets in code, config files in source control, logs or test data (SEC-038) |
| CS-044 | Use approved cryptographic libraries. No custom cryptography |
| CS-045 | Mask Restricted fields in logs and exports (SEC-037) |

## 6. Concurrency

| ID | Rule |
|---|---|
| CS-050 | Mutable records carry a version and updates check it (TR-004) |
| CS-051 | Money and stock operations accept idempotency keys (ADR-009) |
| CS-052 | Shared mutable state is avoided. Application instances are stateless |
| CS-053 | Background jobs are idempotent and safe to run twice |

## 7. API

| ID | Rule |
|---|---|
| CS-060 | Follow API conventions: versioned path, plural resource nouns, action sub-resources for lifecycle, standard error body |
| CS-061 | Each endpoint declares its permission, rate class and idempotency requirement in one place that also feeds documentation |
| CS-062 | Backward compatible changes only within a version. Deprecations announced in release notes |

## 8. Tests

| ID | Rule |
|---|---|
| CS-070 | A test names the requirement or rule it verifies |
| CS-071 | Unit tests touch no database, network or external process |
| CS-072 | Tests are deterministic. Time and randomness are injected |
| CS-073 | Reference data sets (rounding, FX, valuation, allocation) are shared with Finance sign-off |
| CS-074 | No test depends on another test's data or order |

## 9. Logging

| ID | Rule |
|---|---|
| CS-080 | Structured logs, one correlation id per request across logs, events and audit |
| CS-081 | No personal data or secrets. Log ids, not content |
| CS-082 | Log level meaning: error needs attention, warning is unexpected but handled, info is business-significant, debug is off in production |

## 10. Prose, naming and files

Documentation and comments use plain language and avoid em dashes. File and module names follow the module list. One module per directory tree. Tests live beside the module they verify.

## 11. Enforcement

Rules CS-001, CS-004, CS-007, CS-020, CS-043 and dependency boundaries are automated in CI. Others are checked in review using the pull request checklist in the [Definition of Done](Definition-of-Done.md).

## 12. Open points

Language-specific appendix after OQ-024. Static analysis rule set and thresholds to be agreed by the architect.
