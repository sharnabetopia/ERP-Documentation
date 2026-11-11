# Regression Test Plan

Document status: Draft v0.1 (Stage 3 part 1). Proposed. Suite sizes below are counted from the written test cases. No suite has run.
Related: [Test Strategy](Test-Strategy.md), [Test Plan](Test-Plan.md), [RTM](RTM.md).

## 1. Purpose

Show that new changes do not break existing behaviour, with special weight on financial integrity, authorisation and stock.

## 2. Suites

| Suite | Content | Size (cases) | Run when | Target duration |
|---|---|---|---|---|
| Smoke | P0 end-to-end journeys (E2E file) plus P0 positive sign-in cases | 13 | Every build deployed to Test | Minutes |
| Core regression | All P0 cases except API, performance and UAT | 260 | Every merge to the main branch, nightly | Under 1 hour (target, needs automation) |
| API regression | Full API file (contract, permission matrix, error codes) | 186 | Nightly and on API change | Under 30 minutes (target) |
| Full functional regression | All cases except performance and UAT | 521 | Each release candidate and weekly | Hours |
| Security regression | Security file and permission tests | 66 plus 127 permission cases | Each release candidate | |
| Performance regression | Performance and non-functional file | 12 | Each release candidate on Staging | |
| UAT regression | UAT scenarios impacted by change | up to 20 | Before release if scope touched | |

Sizes: smoke 13 (9 P0 end-to-end cases plus 4 P0 positive authentication cases), core 260, full functional 521 (553 total less 12 performance less 20 UAT). Counts will change as cases are added.

## 3. Selection rules

1. Every change runs smoke and the core suite for the modules it touches.
2. Changes to posting, tax, rounding, valuation, authorisation, approval or numbering run the full functional regression.
3. Changes to the permission catalogue or roles regenerate and run the permission matrix cases.
4. Changes to a database migration run the cross-module integration file and the database constraint cases (DBC-01 to DBC-12).
5. A hotfix runs smoke, the module core suite and the regression test for the defect.

## 4. Defect regression rule

Every defect fix adds a test that fails on the baseline commit and passes on the fix. The test is tagged with the defect ID and joins the core suite when the defect severity is Critical or High.

## 5. Maintenance

| Rule | Detail |
|---|---|
| Ownership | Each test case file has a named maintainer |
| Flaky tests | Quarantined within one day, fixed or removed within a week, never ignored |
| Review | After each release, remove obsolete cases, add new ones, re-triage priority (361 of 553 cases are P0, see Test Strategy section 5) |
| Traceability | Cases keep their TC ID. Changes are recorded against the requirement in the RTM |
| Generated cases | The permission and error-code cases are regenerated when the RBAC or API documents change |

## 6. Exit criteria for regression in a release

All smoke and core cases pass. No new Critical or High defects. Known failures are listed with approved exceptions. Performance regression within 10 percent of baseline (proposed).

## 7. Open points

Automation tool and pipeline durations (OQ-024), environment capacity for parallel runs, and baseline performance numbers (OQ-015).
