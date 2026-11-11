# NFR Remediation Report

Phase 2 remediation, 2026-11-11. Related: [Remediation Progress](Remediation-Progress.md), [Final Revalidation Report](Final-Revalidation-Report.md), [Audit baseline](../12-Final-Audit/Final-Readiness-Assessment.md).

No stakeholder approval is claimed anywhere in this report. Values marked PENDING APPROVAL are recommendations.

## 1. Result

| Measure | Before | After |
|---|---|---|
| NFRs | 42 | 42 |
| NFRs with a measurable target | 21 | 42 by script (a target counts as measurable when it holds a number, a named standard or an explicit rule) |
| NFRs flagged unmeasurable by script | 21 | 0  |
| NFRs with a test case | 42 | 42 |
| Measurable NFR test cases written in v0.2 | 0 | 31 (TC-0649 to TC-0679) |

## 2. How each of the 21 was handled

For every NFR the target in `NFR.md` section 1 was replaced and section 1A records the criterion, the test condition, the verification method, the value status and the test cases. The new target values are listed in section 1A of the NFR file.
Values are proposals. They are not final business targets, and each is marked PENDING APPROVAL with the question that governs it (OQ-010, OQ-011, OQ-015, OQ-022, OQ-028).

| NFR | Title | New measurable target (summary) | Governing question |
|---|---|---|---|
| NFR-003 | Posting throughput | Proposed: sustain 20 postings per second (invoice, receipt, payment mix) for 30 minutes with 95th percentile save under 3 s, 0 failed and 0 duplicate ... | OQ-015 |
| NFR-005 | Volume | Proposed: 100 companies, 2,000 concurrent users, 20 million ledger and stock rows per year, 5 years retained (100 million rows). NFR-001, NFR-002 and ... | OQ-015 |
| NFR-006 | Growth | Adding a company or branch needs 0 code deployments and at most 1 hour of administrator work. Adding a module changes 0 files in the public interface ... | none |
| NFR-008 | Degraded mode | With email, bank adapter and gateway simulated down for 4 hours: 100 percent of postings commit, outbox backlog returns to 0 within 30 minutes of reco... | none |
| NFR-013 | Encryption at rest | 256-bit security strength encryption at rest for databases, object storage and backups. Bank and tax identifiers also protected at field level. Keys i... | none |
| NFR-014 | Secrets | 0 secrets in source control history at release (full-history scan). Secrets rotated at least every 90 days. A quarterly rotation drill completes with ... | none |
| NFR-017 | Rate limiting | Sign-in 5 failures per account per 15 minutes then lockout (SEC-003). 20 attempts per address per minute then 429. Password reset 3 per account per ho... | OQ-015 |
| NFR-027 | Audit retention | Audit records kept at least 7 years or the longest applicable legal period if longer. Deletion before expiry refused in 100 percent of attempts | OQ-022 |
| NFR-028 | Logging | 100 percent of log lines carry a timestamp and correlation id. 0 occurrences of passwords, tokens or full bank numbers in a 7 day log scan. Logs kept ... | none |
| NFR-029 | Monitoring and alerts | Alerts fire for: error rate above 2 percent over 5 minutes, 95th percentile latency above twice the NFR-001 target over 10 minutes, outbox backlog old... | none |
| NFR-030 | Task efficiency | With at least 5 users per role, median create purchase requisition at most 8 steps and 3 minutes, receive goods at most 6 steps, post invoice at most ... | none |
| NFR-031 | Clear errors | 100 percent of error responses carry a code, a message naming the field or rule, and an action. 0 messages with only a generic text or a stack trace. ... | none |
| NFR-033 | Browsers and devices | Current and previous major versions of Chrome, Edge, Firefox and Safari on desktop. Layouts usable at widths 360, 768, 1024 and 1440 pixels. 0 blockin... | none |
| NFR-034 | Internationalisation | 100 percent of user-facing strings in resource files (scan finds 0 hard-coded strings). Launch in English. At least one non-Latin and one right-to-lef... | OQ-011 |
| NFR-035 | Modularity | 0 cyclic dependencies between modules. 0 cross-module access outside public interfaces. Both checked in the build and failing the build on violation | none |
| NFR-037 | Configuration over code | Approval chains, numbering formats, tolerances and tax codes change with 0 deployments. A change is effective within 5 minutes of approval | none |
| NFR-038 | Environment parity | Staging and production are built from the same definitions. An automated comparison shows 0 differences outside a declared list (size, secrets, URLs) | none |
| NFR-039 | Privacy | Personal data inventory covers 100 percent of personal fields. A subject export is produced within 24 hours of the request (the legal period depends o... | OQ-010 |
| NFR-040 | Retention | Retention period configurable per record type. Deletion inside retention refused in 100 percent of attempts. After expiry the archive job handles the ... | OQ-022 |
| NFR-041 | Time handling | 0 timestamp columns stored in a non-UTC zone. Display is correct for 5 time zones including a daylight saving change day. Posting dates are date-only ... | none |
| NFR-042 | Archival | Closed-period data older than N years is archived with reports and drill-down returning identical totals (difference 0.00). N is 7 as a placeholder | OQ-022 |

## 3. Assumptions behind the proposed values

1. Volumes follow the v0.1 design assumption in NFR-005 (100 companies, 2,000 concurrent users, 20 million rows a year). Throughput of 20 postings a second is more than 10 times the average implied by that volume.
2. Retention of 7 years is a placeholder for financial and audit records, not legal advice (OQ-022).
3. Rate limits and thresholds are common starting values and are not derived from this business.
4. Usability and cross-browser targets assume a staff user base on current desktop browsers.
5. Where a legal period decides the value (privacy export, retention), the design capability is fixed and the number is left PENDING APPROVAL.

## 4. Limits

- The "measurable" test is a text rule. It confirms a number, standard or rule exists. It does not confirm the number is right.
- No test has been run. Several tests need a staging environment that does not exist.
- The other 21 NFRs kept their v0.1 targets. Section 1A marks them Proposed, PENDING APPROVAL, except NFR-012, 016, 018, 020, 021, 022, 025, 026 and 032 which are fixed by a standard or design rule.
