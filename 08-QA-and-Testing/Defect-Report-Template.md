# Defect Report Template

Document status: Draft v0.1 (Stage 3 part 1). Template. No defects exist because no test has run.
Related: [Test Strategy](Test-Strategy.md) section 5 and section 9.

## 1. Template

Copy this table for each defect. Fields marked * are required at logging.

| Field | Entry |
|---|---|
| Defect ID* | DEF-0001 (sequential) |
| Title* | Short, specific. Say what fails |
| Reported by* / date* | |
| Environment and build* | Test, Staging, build id, configuration, data set |
| Module and submodule* | Module ID (M-nnn) and name |
| Related requirement* | FR, NFR, SEC, INV or other ID |
| Related test case | TC ID, or "found during exploration" |
| Related user story, use case, API | |
| Type* | Functional, Data integrity, Security, Performance, Usability, Accessibility, Compatibility, Documentation, Environment |
| Severity* | Critical, High, Medium, Low (definitions in Test Strategy) |
| Priority* | P0, P1, P2 (business urgency) |
| Preconditions* | State, user role and scope, data used |
| Steps to reproduce* | Numbered, complete, from a known state |
| Expected result* | Reference the requirement or acceptance criterion |
| Actual result* | What happened, with exact messages and codes |
| Evidence | Screenshots, logs, correlation id, request and response, audit record, database query results (masked) |
| Frequency | Always, intermittent (x of y), once |
| Workaround | |
| Impact | Who and what is affected, money or stock affected, data corrupted |
| Suspected cause | Optional. File and line if known |
| Security note | If a vulnerability: restrict visibility, do not post details in open channels |
| Status | New, Triaged, In progress, Fixed, Ready for retest, Verified, Closed, Reopened, Rejected, Deferred |
| Assigned to | |
| Fix build and notes | |
| Regression test added | Test case ID that fails on the baseline and passes on the fix |
| Retest result and date | |
| Triage decision | By, date, reason (especially for Rejected and Deferred) |

## 2. Severity and priority guide

| Severity | Example in this ERP |
|---|---|
| Critical | Unbalanced ledger posted. Stock goes negative against policy. User sees another company's data. Payment released twice. Authentication bypass |
| High | Approval cannot be completed. Period close fails. Wrong tax on invoice. Report total wrong. Permission too wide |
| Medium | Filter fails on one list. Message unclear. Export column missing. Workaround exists |
| Low | Typo, alignment, minor cosmetic |

Priority is set by the business and QA lead. A Low severity defect can be P0 if it blocks a release demonstration. A Critical defect is never lower than P0.

## 3. Lifecycle

New, then Triaged (severity and priority confirmed, duplicates linked), then In progress, then Fixed, then Ready for retest, then Verified and Closed. A failed retest moves it to Reopened. Rejected means not a defect (reason recorded). Deferred needs a named approver and a target release.

## 4. Triage rules

1. Critical defects are triaged within the same day and stop the affected test cycle.
2. Duplicates are linked to the first report, never deleted.
3. A defect without reproducible steps is returned to the reporter.
4. Each fix must name the regression test before the defect can close.
5. Security defects follow the Security Test Plan disclosure rule.

## 5. Metrics (reported per cycle)

Open by severity, found versus fixed, mean time to fix, reopen rate, defects by module, defects by requirement. Metrics are reported only from logged data.

## 6. Example (illustrative, not a real defect)

| Field | Entry |
|---|---|
| Defect ID | DEF-EXAMPLE |
| Title | Goods receipt accepts 106 against open quantity 100 with 5 percent tolerance |
| Related requirement and test | FR-095, a boundary test case for receipt tolerance |
| Severity and priority | High, P0 |
| Steps | 1. Create PO for 100. 2. Post GRN for 100. 3. Post second GRN for 6. |
| Expected | Second GRN refused PROC-002 |
| Actual | Accepted, PO status Received |
