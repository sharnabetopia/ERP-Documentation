# HR Test Cases

Document status: Draft v0.1 (Stage 3 part 1). Proposed. Test cases are written, **none has been executed**. Every Actual Result is blank and every status is Not Run.
MVP covers only the employee core record, directory and reporting line (FR-022 to FR-024). Test cases for attendance, leave, payroll, self-service and other HR modules (R2 and R3) are written with those releases. Priority P0 to P2 follows the [Test Strategy](../Test-Strategy.md) definitions. Severity is the impact if the test fails. Steps are separated by line breaks.

Test cases in this file: 3. IDs run from TC-0040 to TC-0042 (IDs are shared across all files, so ranges may be non-contiguous).

| TC ID | Requirement ID | Story ID | Module | Submodule | Scenario | Title | Description | Preconditions | Test Data | Execution Steps | Expected Result | Actual Result | Priority | Severity | Test Type | Execution Status |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| TC-0040 | FR-022, BR-049 | US-015 | M-007 | Directory | Permission | Restricted fields are hidden in the directory | Permission test of "Directory search": The system shall let a user search the employee directory and see an organisation chart limited to fields their role permits. | Employee with sensitive field populated | viewer role Sales Executive | 1. Search directory | Name, job, branch shown; restricted fields absent in UI and API |  | P1 | High | Permission | Not Run |
| TC-0041 | FR-023, BR-049 | US-014 | M-016 | Employee record | Negative | Duplicate employee code refused | Negative test of "Employee core record": The system shall maintain employee code, name, job, branch, department, manager and status. | Employee code E001 exists | E001 in same company; E001 in another company | 1. Create both | Same company refused; other company accepted |  | P0 | Medium | Negative | Not Run |
| TC-0042 | FR-024, BR-049, BR-006 | US-014 | M-016 | Reporting line | Negative | Manager cycle refused | Negative test of "Reporting line": The system shall record a manager per employee and refuse cycles. | A manages B | set B as manager of A | 1. Save | Refused |  | P0 | Medium | Negative | Not Run |

