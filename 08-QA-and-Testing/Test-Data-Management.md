# Test Data Management Plan

Document status: Draft v0.1 (Stage 3 part 1). Proposed.
Related: [Test Strategy](Test-Strategy.md), [Data Protection](../06-RBAC-and-Security/Data-Protection.md), [Data Migration Strategy](../04-System-Architecture/Data-Migration-Strategy.md).

## 1. Principles

1. Test data is synthetic or masked. Real personal and financial data never enters Test or Dev (SEC-039, DP-005).
2. Data is created by scripts and factories, so every environment can be rebuilt.
3. Each test creates the data it needs or uses a named baseline. Tests do not depend on each other.
4. Reference data sets with hand-calculated answers exist for money, tax, rounding, currency and valuation.
5. Data volumes for performance tests are generated at sized scale, not copied.

## 2. Baseline data sets

| Set | Content | Used by |
|---|---|---|
| BASE-TENANT | 2 tenants, 3 companies (A, B in tenant 1; C in tenant 2), base currencies USD and EUR, branches B1 and B2 per company, departments, cost centres | Isolation, scope tests |
| BASE-USERS | One user per default role per company (18 x 3), plus users with deliberately wrong scope and expired temporary grants, a locked user, an MFA user | Authentication, RBAC, API permission tests |
| BASE-COA | Chart of accounts with control accounts, rounding and exchange difference accounts, posting rules mapped | Finance tests |
| BASE-FY | Fiscal years with periods in states Open, Soft-closed, Closed, Future | Period tests |
| BASE-MASTER | 20 vendors (active, draft, blocked, duplicate tax id pair), 20 customers (credit limits, on hold), 100 items (stock, non-stock, service, categories), 4 warehouses | P2P, O2C, inventory |
| BASE-CHAINS | Approval chains for all approvable types with limit boundaries 5,000, 10,000, 50,000 | Approval tests |
| BASE-RATES | Rates for USD, EUR, and a third currency on dated points, one date deliberately missing | Currency tests |
| BASE-TAX | Tax codes: 15 percent exclusive, 15 percent inclusive, zero rate, exempt, with a rate change mid-year | Tax tests |

Names, codes and amounts follow a documented pattern so a failing record can be traced to its set.

## 3. Reference calculation sets (hand-verified by Finance)

| Set | Contents | Purpose |
|---|---|---|
| REF-ROUND | Prices 0.05, 0.15, 33.33, 99.99, quantities 1, 3, 7, discounts, line and document rounding | FR-071 |
| REF-FX | Documents at rates with 8 decimals, settlement at other rates | FR-068, FR-069 |
| REF-VAL | Receipts and issues in sequence with expected FIFO and weighted average costs (final methods per OQ-009) | FR-111 |
| REF-ALLOC | Invoices, partial payments, advances, credit notes, refunds with expected open amounts | FR-073 to FR-078 |
| REF-AGEING | Open items across age buckets | FR-081 |
| REF-CLOSE | A month of postings with known trial balance and statements | FR-082 to FR-084 |

Each set stores inputs and expected outputs in version control. Finance signs each set before it is used as oracle.

## 4. Data creation and refresh

| Need | Method |
|---|---|
| Baselines | Scripted seeding through the import module and API (also tests them) |
| Per-test data | Factories that call the API or domain layer |
| Volume | Generators producing ledger, stock and audit rows to the sized volume (OQ-015) |
| Refresh | Test environment rebuilt from scripts each night. Staging refreshed before each release cycle |
| Time control | A controllable clock for expiry, lockout, escalation and period tests |
| External providers | Stubs for mail, bank, gateway and identity provider with failure modes (timeout, error, duplicate callback) |

## 5. Masking of copied data

If production-derived data is ever needed (migration rehearsal on Staging only):

| Data | Treatment |
|---|---|
| Names, emails, phones, addresses | Replaced with generated values, consistently per person |
| Bank accounts, tax ids, national ids | Replaced by format-valid generated values |
| Free text and attachments | Removed or replaced |
| Amounts and dates | Kept, so totals and reconciliations still work |
| Audit actors | Replaced by generated users |

Masking is verified by a scan before data is released to testers.

## 6. Data for specific test types

| Type | Data need |
|---|---|
| Isolation | Identical-looking records in several companies so that leakage is visible |
| Concurrency | One scarce resource (last unit, last credit, one numbering series) and many actors |
| Performance | Sized volume and realistic distribution (many small documents, a few large) |
| Security | Malicious payload library, spoofed tokens, disguised files, malware test string |
| UAT | Production-like data, masked, agreed with business owners (OQ-029) |
| Migration rehearsal | Legacy extracts and reconciliation totals (OQ-012) |

## 7. Governance

| Item | Rule |
|---|---|
| Owner | QA lead |
| Access | Test environments hold no real personal data. Staging access limited to named testers |
| Retention | Test data deleted when an environment is rebuilt. Reference sets kept in version control |
| Changes | Any change to a reference set needs Finance review |

## 8. Open points

OQ-009 (valuation methods for REF-VAL), OQ-008 (currencies for BASE-RATES), OQ-007 (tax regimes for BASE-TAX), OQ-015 (volume), OQ-029 (UAT data).
