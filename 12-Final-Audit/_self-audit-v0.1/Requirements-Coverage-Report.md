# Requirements Coverage Report

Document status: Draft v0.1 (final audit, 2026-11-11). Measured by script. "Covered" means linked, not verified. No test has run.
Related: [RTM](../08-QA-and-Testing/RTM.md), [Documentation Audit Report](Documentation-Audit-Report.md), [FRS](../03-Product-and-System-Requirements/FRS.md).

## 1. Functional requirements by area

| Area | FRs | P0 | With BRL | With user story | With use case | With API endpoint | With written TC |
|---|---|---|---|---|---|---|---|
| Platform and administration (FR-001 to FR-052) | 52 | 40 | 37 | 52 | 21 | 42 | 52 |
| Finance (FR-053 to FR-085) | 33 | 31 | 24 | 33 | 15 | 30 | 33 |
| Procure-to-Pay (FR-086 to FR-100) | 15 | 14 | 11 | 15 | 11 | 15 | 15 |
| Inventory (FR-101 to FR-113) | 13 | 11 | 12 | 13 | 4 | 11 | 13 |
| Order-to-Cash (FR-114 to FR-124) | 11 | 9 | 8 | 11 | 8 | 11 | 11 |
| Cross-cutting (FR-125 to FR-130) | 6 | 4 | 5 | 6 | 0 | 5 | 6 |
| **Total** | **130** | **109** | **97** | **130** | **59** | **114** | **130** |

## 2. Business requirements

| State | Count |
|---|---|
| Covered by test cases (not executed) | 47 |
| No FR yet (post-MVP) | 16 |

Total BRs 63. BRs with no FR: BR-022, BR-023, BR-031, BR-032, BR-040, BR-048, BR-050, BR-051, BR-052, BR-053, BR-054, BR-055, BR-056, BR-057, BR-062, BR-063. BR-062 (service levels) is covered by NFRs. BR-063 and others are R3 or later.

## 3. Other requirement families

| Family | Total | With written test | Note |
|---|---|---|---|
| NFR | 42 | 42 | Targets for NFR-001 to NFR-011 are placeholders |
| TR | 18 | 18 |  |
| SEC | 53 | 53 |  |
| THR | 44 | 13 | 13 named directly, the rest through mitigations |
| INV | 15 | 15 |  |
| DBC | 12 | 12 |  |

## 4. Modules by release

| Release | Modules | Requirements written | Coverage |
|---|---|---|---|
| MVP | 58 | BR, FR, stories, tests | 58 modules reached by at least one FR |
| R2 | 20 | BR only for some | 0 FRs, 0 tests |
| R3 | 25 | BR only for some | 0 FRs, 0 tests |
| R4 | 17 | BR-057 only | 0 FRs, 0 tests |

Requirement coverage for R2 to R4 modules is therefore 0 percent at FR level. This is by design: they are written before each release.

## 5. Traceability chain completeness (functional requirements)

| Link in chain | FRs with the link | Percent |
|---|---|---|
| Business requirement | 130 | 100.0 percent |
| User story | 130 | 100.0 percent |
| Use case | 59 | 45.4 percent |
| API endpoint | 114 | 87.7 percent |
| Business rule | 97 | 74.6 percent |
| Written test case | 130 | 100.0 percent |
| Executed passing test | 0 | 0.0 percent |

## 6. Gaps

| Gap | Count | Action |
|---|---|---|
| FRs with no use case | 71 | Use cases cover critical flows only. Acceptable if stories suffice. Review |
| FRs with no API endpoint | 16 | Internal behaviours (authorisation, audit, calculation). Verified through other endpoints |
| FRs with no business rule | 33 | Add rules or record that none applies |
| BRs with no business rule | 22 | Review (PG-01) |
| FRs with exactly one test case | 27 | Review depth for P0 ones |
| Requirements with an executed passing test | 130 FRs have none | Execute tests |
