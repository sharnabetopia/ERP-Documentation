# Requirements Traceability Matrix (RTM)

Document status: **Draft v0.2 (Phase 2 remediation, regenerated 2026-11-11).** Sections 1 to 5 and 8 were rebuilt by script from the requirement documents and test case files using exact ID matching only (word boundaries, no substring matching). The v0.1 matrix is kept unchanged as evidence in `13-Remediation-Reports/evidence/RTM-v0.1-before-remediation.md`. Every Test Result is "Not Run" because no test has been executed. "Covered" means a test case is written and linked. It does not mean the requirement is verified.

Chain: Business Requirement, Functional Requirement, Non-Functional Requirement, Module, User Story, Use Case, API, Business Process (workflow), Test Case, Test Result.
IDs are defined in their own documents: [BRD](../02-Business-Analysis/BRD.md), [FRS](../03-Product-and-System-Requirements/FRS.md), [NFR](../03-Product-and-System-Requirements/NFR.md), [User Stories](../03-Product-and-System-Requirements/User-Stories.md), [Use Cases](../03-Product-and-System-Requirements/Use-Cases.md), [API Documentation](../04-System-Architecture/API-Documentation.md), [Workflows](../05-Workflows-and-Diagrams/Cross-Module-Workflows.md). Test case IDs use four digits.

## 1. Functional requirement trace (150 rows)

Columns: NFR lists the reasoned non-functional links in section 3A. TC lists test cases that name the FR directly, followed by "via" cases that reach the FR through a security requirement, database constraint or invariant whose own row names the FR. API TCs counts API-ID-named test cases for the FR's endpoints.

| FR | Title | BR | NFR | Module | US | UC | API | Workflow | TC | API TCs | Result |
|---|---|---|---|---|---|---|---|---|---|---|---|
| FR-001 | Sign-in | BR-004 | none | M-002 | US-001 | UC-001 | API-001 | none | TC-0001, TC-0002, TC-0003, TC-0482 ; via: TC-0193 | 19 | Not Run |
| FR-002 | Password policy | BR-004 | none | M-002 | US-002 | none | API-006 | none | TC-0004, TC-0005, TC-0006, TC-1035 ; via: TC-0194 | 18 | Not Run |
| FR-003 | Account lockout | BR-004 | none | M-002 | US-001 | UC-001 | API-001 | none | TC-0007, TC-0008 ; via: TC-0195 | 19 | Not Run |
| FR-004 | Session management | BR-004 | none | M-002 | US-003 | UC-001 | API-003, API-004, API-008, API-009 | none | TC-0009, TC-0010, TC-0011, TC-0680, TC-0681 ; via: TC-0200, TC-0201, TC-0204, TC-0793, TC-0818 | 19 | Not Run |
| FR-005 | Password reset | BR-004 | none | M-002 | US-002 | none | API-005, API-006 | none | TC-0012, TC-0013, TC-0014 ; via: TC-0196, TC-0197 | 18 | Not Run |
| FR-006 | Multi-factor authentication | BR-004 | none | M-002 | US-004 | UC-001 | API-002 | none | TC-0015, TC-0016 ; via: TC-0198 | 17 | Not Run |
| FR-007 | Single sign-on | BR-004 | none | M-002 | US-005 | UC-001 | API-007 | none | TC-0017, TC-0018, TC-0682 ; via: TC-0199 | 17 | Not Run |
| FR-008 | Invite user | BR-003 | none | M-003 | US-006 | UC-002 | API-010, API-011, API-012, API-013 | WF-04 | TC-0019, TC-0020, TC-0491, TC-0546, TC-0788, TC-0830 | 24 | Not Run |
| FR-009 | Deactivate user | BR-004, BR-049 | none | M-003 | US-007 | none | API-014 | WF-04 | TC-0021, TC-0022, TC-0491, TC-0793, TC-0818, TC-0819, TC-0827, TC-0832 ; via: TC-0202 | 17 | Not Run |
| FR-010 | Assign scope | BR-002, BR-003 | none | M-003, M-006 | US-006 | UC-002 | API-011, API-016 | none | TC-0023, TC-0491, TC-0683, TC-0684, TC-0788 | 21 | Not Run |
| FR-011 | Link user to employee | BR-049 | none | M-003, M-016 | US-006 | UC-002 | API-015 | WF-04 | TC-0024, TC-0788, TC-0804, TC-0830 | 18 | Not Run |
| FR-012 | Custom roles | BR-003 | NFR-016 | M-004 | US-008 | none | API-018, API-019, API-020, API-021 | none | TC-0025, TC-0026, TC-0491, TC-0546 | 23 | Not Run |
| FR-013 | Permission catalogue | BR-003 | NFR-016 | M-004 | US-008 | none | API-022 | none | TC-0027, TC-0685 | 17 | Not Run |
| FR-014 | Default roles | BR-003 | none | M-004 | US-008 | none | none | none | TC-0028, TC-0686 ; via: TC-0210, TC-1011 | 0 | Not Run |
| FR-015 | Scoped authorisation | BR-003 | NFR-015, NFR-016 | M-004 | US-009 | none | none | none | TC-0029, TC-0030, TC-0491, TC-0514, TC-0546, TC-0687, TC-0779, TC-0799, TC-0800, TC-0808, TC-0809, TC-0941, TC-0957, TC-0959 ; via: TC-0205, TC-0206, TC-0282, TC-0847, TC-0855, TC-0856, TC-1002, TC-1003, TC-1004, TC-1012 | 0 | Not Run |
| FR-016 | Temporary access | BR-003 | NFR-016 | M-004 | US-010 | UC-002 | API-016, API-017 | none | TC-0031, TC-0688 | 19 | Not Run |
| FR-017 | Segregation conflict check | BR-003, BR-021 | NFR-016 | M-004 | US-011 | UC-002 | API-016 | none | TC-0032, TC-0033, TC-1036 ; via: TC-0211 | 19 | Not Run |
| FR-018 | Company management | BR-001 | none | M-005 | US-012 | none | API-023, API-024, API-025 | none | TC-0034, TC-0035 | 21 | Not Run |
| FR-019 | Company isolation | BR-001 | NFR-018 | M-005 | US-009 | none | none | none | TC-0036, TC-0037, TC-0282, TC-0689, TC-0779, TC-0957, TC-0958 ; via: TC-0206, TC-0208, TC-0209, TC-0268, TC-0855, TC-0856, TC-1004 | 0 | Not Run |
| FR-020 | Branch and department tree | BR-002 | none | M-006 | US-013 | none | API-026, API-027, API-028, API-029, API-030 | none | TC-0038, TC-1037 | 22 | Not Run |
| FR-021 | Cost centres | BR-002 | none | M-006 | US-013 | none | API-031, API-032 | none | TC-0039, TC-0690 | 18 | Not Run |
| FR-022 | Directory search | BR-049 | NFR-004 | M-007 | US-015 | none | API-033 | none | TC-0040, TC-0691, TC-0796 | 16 | Not Run |
| FR-023 | Employee core record | BR-049 | none | M-016 | US-014 | none | API-033, API-034, API-035 | WF-04 | TC-0041, TC-0787, TC-0814, TC-0821, TC-0830 | 20 | Not Run |
| FR-024 | Reporting line | BR-049, BR-006 | none | M-016 | US-014 | none | API-034, API-035 | WF-04 | TC-0042, TC-0787, TC-0795, TC-0813 | 20 | Not Run |
| FR-025 | Numbering series | BR-012 | NFR-024 | M-008 | US-016 | none | API-036, API-037 | none | TC-0043, TC-0044, TC-0284, TC-0510, TC-0529, TC-0938 | 18 | Not Run |
| FR-026 | Print templates | BR-012 | none | M-008 | US-016 | none | API-163, API-164 | none | TC-0045, TC-0692 | 18 | Not Run |
| FR-027 | Policy settings | BR-006, BR-061 | none | M-008 | US-016 | none | API-038, API-039 | none | TC-0046, TC-0693 | 18 | Not Run |
| FR-028 | Master data maintenance | BR-008 | none | M-009 | US-017 | none | API-040, API-041 | none | TC-0047, TC-1038 | 19 | Not Run |
| FR-029 | Duplicate detection | BR-008 | none | M-009 | US-017 | none | API-041, API-102, API-128, API-146 | none | TC-0048, TC-0694 ; via: TC-0233, TC-0747 | 21 | Not Run |
| FR-030 | Audit capture | BR-005 | NFR-026 | M-010 | US-019 | none | none | none | TC-0049, TC-0281, TC-0508, TC-0547, TC-0695, TC-0743, TC-0826, TC-0833, TC-0983 ; via: TC-0213, TC-0238 | 0 | Not Run |
| FR-031 | Audit search and export | BR-005 | NFR-026 | M-010 | US-019 | none | API-051, API-052 | none | TC-0050, TC-0051, TC-0547, TC-0696 | 19 | Not Run |
| FR-032 | Audit resilience | BR-005 | NFR-003 | M-010 | US-019 | none | none | none | TC-0052, TC-0281, TC-0743, TC-0826, TC-0833, TC-0972, TC-0973, TC-0983 | 0 | Not Run |
| FR-033 | Attachments | BR-010 | none | M-011 | US-020 | none | API-042 | none | TC-0053, TC-0990, TC-1009, TC-1010 ; via: TC-0224, TC-0225 | 20 | Not Run |
| FR-034 | Attachment access | BR-010 | none | M-011 | US-020 | none | API-043 | none | TC-0054, TC-0697, TC-0938 ; via: TC-0226 | 18 | Not Run |
| FR-035 | In-app notifications | BR-007 | none | M-012 | US-021 | none | API-044, API-045 | none | TC-0055, TC-0550, TC-0698, TC-0993 ; via: TC-0203 | 16 | Not Run |
| FR-036 | Email notifications | BR-007 | none | M-012 | US-021 | none | API-193 | none | TC-0056, TC-1039 | 0 | Not Run |
| FR-037 | Notification retry | BR-007, BR-060 | NFR-008 | M-012 | US-021 | none | none | none | TC-0057, TC-0058, TC-0294, TC-0512, TC-0824, TC-0873, TC-0895, TC-0926, TC-0976, TC-1026, TC-1027 | 0 | Not Run |
| FR-038 | Notification preferences | BR-007 | none | M-012 | US-021 | none | API-046, API-165 | none | TC-0059, TC-0895 | 16 | Not Run |
| FR-039 | Approval chain definition | BR-006 | NFR-037 | M-013 | US-022 | UC-003 | API-058, API-059, API-060 | WF-07 | TC-0060, TC-0061, TC-0515 | 20 | Not Run |
| FR-040 | Submit for approval | BR-006 | none | M-013 | US-023 | UC-003 | none | WF-07 | TC-0062, TC-0291, TC-0699, TC-0785, TC-0830, TC-0835 | 0 | Not Run |
| FR-041 | Decide | BR-006 | none | M-013 | US-023 | UC-003 | API-062 | WF-07 | TC-0063, TC-0064, TC-0484, TC-0785, TC-0815, TC-0836, TC-0886, TC-0888 | 19 | Not Run |
| FR-042 | No self-approval | BR-006, BR-021 | none | M-013 | US-024 | UC-003 | API-062 | WF-07 | TC-0065, TC-0283, TC-0700, TC-0786, TC-0797, TC-0801, TC-0846, TC-0896, TC-0897, TC-0947, TC-0948, TC-0949, TC-0951, TC-0998 ; via: TC-0212 | 19 | Not Run |
| FR-043 | Authority limit | BR-006 | none | M-013 | US-023 | UC-003 | API-062 | WF-07 | TC-0066, TC-0701, TC-0785, TC-0798, TC-0817, TC-0823, TC-0887, TC-0915, TC-0928, TC-0935, TC-0950, TC-0951, TC-0966, TC-0987, TC-0989, TC-1020, TC-1028 | 19 | Not Run |
| FR-044 | Delegation | BR-006 | none | M-013 | US-025 | UC-003, UC-016 | API-063, API-064 | WF-07 | TC-0067, TC-0068, TC-0069, TC-0493, TC-0548, TC-0894 | 18 | Not Run |
| FR-045 | Escalation | BR-006 | none | M-013 | US-026 | UC-003 | none | WF-07 | TC-0070, TC-0493, TC-0548, TC-0702, TC-0820, TC-0828, TC-0920, TC-0923 | 0 | Not Run |
| FR-046 | Resubmission | BR-006 | none | M-013 | US-023 | UC-003 | none | WF-07 | TC-0071, TC-0484, TC-0703 | 0 | Not Run |
| FR-047 | Approval inbox | BR-006, BR-013 | none | M-013, M-001 | US-027 | UC-003 | API-061 | WF-07 | TC-0072, TC-0704, TC-0976 | 16 | Not Run |
| FR-048 | Import validate | BR-009 | none | M-014 | US-028 | UC-015 | API-047, API-049 | none | TC-0073, TC-0295, TC-0492, TC-0517, TC-0549, TC-0705, TC-0829 ; via: TC-0223 | 19 | Not Run |
| FR-049 | Import commit | BR-009 | none | M-014 | US-028 | UC-015 | API-048 | none | TC-0074, TC-0075, TC-0295, TC-0492, TC-0513, TC-0829 | 18 | Not Run |
| FR-050 | Export | BR-009, BR-005 | none | M-014 | US-029 | none | API-050, API-198 | none | TC-0076, TC-0077, TC-0517, TC-0549, TC-0706 ; via: TC-0223, TC-0237 | 18 | Not Run |
| FR-051 | Role dashboard | BR-013 | none | M-001 | US-030 | none | API-057, API-196 | none | TC-0078, TC-0550, TC-0707 | 16 | Not Run |
| FR-052 | Locale and time zone | BR-011 | none | M-015 | US-031 | none | API-165, API-166, API-195 | none | TC-0079, TC-0080 | 16 | Not Run |
| FR-053 | Chart of accounts | BR-015 | none | M-031 | US-032 | none | API-065, API-066, API-067 | WF-03 | TC-0089, TC-0090 ; via: TC-0267 | 21 | Not Run |
| FR-054 | COA template | BR-015 | none | M-031 | US-032 | none | API-068 | WF-03 | TC-0091, TC-0708 | 17 | Not Run |
| FR-055 | Account protection | BR-015 | none | M-031 | US-032 | none | API-067 | WF-03 | TC-0092, TC-1040 | 18 | Not Run |
| FR-056 | Fiscal year | BR-016 | none | M-044 | US-033 | none | API-071, API-197 | WF-03 | TC-0093, TC-1041 | 17 | Not Run |
| FR-057 | Periods | BR-016 | none | M-043 | US-033 | none | API-071, API-072 | WF-03 | TC-0094, TC-1042 | 18 | Not Run |
| FR-058 | Period states | BR-016 | none | M-043 | US-034 | UC-005 | API-073, API-074 | WF-03 | TC-0095, TC-0096, TC-0280, TC-0487, TC-0954, TC-0955, TC-0956 | 19 | Not Run |
| FR-059 | Period reopen | BR-016 | none | M-043 | US-034 | UC-005 | API-075 | WF-03 | TC-0097, TC-0098, TC-0488 ; via: TC-0214, TC-0280 | 17 | Not Run |
| FR-060 | Close checklist | BR-016 | none | M-045 | US-034 | UC-005 | API-074, API-076 | WF-03 | TC-0099, TC-0100, TC-0487, TC-0543 | 20 | Not Run |
| FR-061 | Year-end close | BR-016 | none | M-045 | US-035 | none | API-077 | WF-03 | TC-0101, TC-0102, TC-0285, TC-0488 | 19 | Not Run |
| FR-062 | Journal entry | BR-014 | none | M-035 | US-036 | UC-004 | API-078, API-079, API-080, API-081 | WF-03 | TC-0103, TC-0104, TC-0105, TC-0271, TC-0487, TC-0543 ; via: TC-0260 | 22 | Not Run |
| FR-063 | Journal approval | BR-021 | none | M-035 | US-037 | UC-004 | API-081 | WF-03 | TC-0106, TC-0107, TC-0487, TC-0543, TC-0709 | 19 | Not Run |
| FR-064 | Posting immutability | BR-014 | none | M-032, M-035 | US-038 | UC-004 | API-082 | WF-03 | TC-0108, TC-0508, TC-0710, TC-0778 ; via: TC-0207, TC-0261, TC-0742 | 18 | Not Run |
| FR-065 | Reversal | BR-014 | none | M-035 | US-038 | UC-004 | API-083 | WF-03 | TC-0109, TC-0110, TC-0962, TC-0974 | 19 | Not Run |
| FR-066 | Ledger inquiry | BR-019 | none | M-032 | US-039 | none | API-084 | WF-03 | TC-0111, TC-0711 | 17 | Not Run |
| FR-067 | Currencies and rates | BR-017 | none | M-041 | US-040 | none | API-085, API-086 | WF-03 | TC-0112, TC-1043 | 19 | Not Run |
| FR-068 | Currency conversion | BR-017 | none | M-041 | US-040 | none | none | WF-03 | TC-0113, TC-0114, TC-0292 | 0 | Not Run |
| FR-069 | Exchange difference | BR-017 | none | M-041 | US-040 | none | none | WF-03 | TC-0115, TC-0292, TC-0712, TC-0713 | 0 | Not Run |
| FR-070 | Tax codes | BR-018 | none | M-040 | US-041 | none | API-087, API-088 | WF-03 | TC-0116, TC-1044 | 18 | Not Run |
| FR-071 | Tax calculation | BR-018 | none | M-040 | US-041 | none | none | WF-03 | TC-0117, TC-0118, TC-1045 | 0 | Not Run |
| FR-072 | Bank accounts | BR-020 | none | M-036 | US-042 | none | API-089, API-090 | WF-03 | TC-0119, TC-1046 | 18 | Not Run |
| FR-073 | Allocation | BR-020, BR-045 | none | M-047, M-059, M-086 | US-043 | UC-009 | API-091, API-092 | WF-01, WF-02 | TC-0120, TC-0121, TC-0278, TC-0292, TC-0482, TC-0483, TC-0485, TC-0538, TC-0942, TC-0953 | 20 | Not Run |
| FR-074 | Advances and credit | BR-020, BR-045 | none | M-047 | US-043 | UC-009 | API-092 | WF-01, WF-02 | TC-0122, TC-0486, TC-0714 | 19 | Not Run |
| FR-075 | Payment approval | BR-030 | none | M-059, M-047 | US-044 | UC-009 | API-093, API-094 | WF-01 | TC-0123, TC-0288, TC-0482, TC-0538, TC-0715, TC-0754, TC-0755, TC-0756, TC-0757 | 20 | Not Run |
| FR-076 | Payment idempotency | BR-020 | NFR-025 | M-047 | US-044 | UC-009 | API-092, API-094 | WF-01 | TC-0124, TC-0125, TC-0516 ; via: TC-0232, TC-1031 | 22 | Not Run |
| FR-077 | Payment reversal | BR-020 | none | M-047 | US-045 | UC-009 | API-095 | WF-01 | TC-0126, TC-0288, TC-0716, TC-0755, TC-0962 | 19 | Not Run |
| FR-078 | Refund | BR-020, BR-044 | none | M-047, M-085 | US-045 | UC-012 | API-096 | WF-12 | TC-0127, TC-0293, TC-0490, TC-0542, TC-0764, TC-0765 | 18 | Not Run |
| FR-079 | Payable posting | BR-014, BR-028 | none | M-033 | US-047 | UC-008 | API-124 | WF-01 | TC-0128, TC-0271, TC-0286, TC-0482, TC-0717, TC-0936 | 19 | Not Run |
| FR-080 | Receivable posting | BR-014 | none | M-034 | US-047 | UC-011 | API-157 | WF-02 | TC-0129, TC-0271, TC-0287, TC-0485, TC-0718 | 18 | Not Run |
| FR-081 | Open items and ageing | BR-047 | none | M-033, M-034 | US-047 | none | API-097, API-162 | WF-02 | TC-0130, TC-0273, TC-0274, TC-0295, TC-0553, TC-0719 ; via: TC-0501 | 18 | Not Run |
| FR-082 | Trial balance | BR-019 | none | M-046 | US-046 | none | API-098 | WF-03 | TC-0131, TC-0272, TC-0487, TC-0544, TC-0720 | 17 | Not Run |
| FR-083 | Income statement | BR-019 | none | M-046 | US-046 | none | API-099 | WF-03 | TC-0132, TC-0544, TC-0721 | 17 | Not Run |
| FR-084 | Balance sheet | BR-019 | none | M-046 | US-046 | none | API-100 | WF-03 | TC-0133, TC-0487, TC-0544, TC-0722 | 17 | Not Run |
| FR-085 | Posting rules | BR-014 | none | M-030 | US-032 | none | API-069, API-070 | WF-03 | TC-0134, TC-0286, TC-0936, TC-0969 | 18 | Not Run |
| FR-086 | Vendor creation | BR-025 | none | M-054 | US-048 | none | API-101, API-102, API-103, API-104 | WF-01 | TC-0135, TC-0136, TC-0541 | 21 | Not Run |
| FR-087 | Vendor block | BR-025 | none | M-054 | US-048 | none | API-105 | WF-01 | TC-0137, TC-0723 | 17 | Not Run |
| FR-088 | Vendor bank change | BR-025 | none | M-054 | US-048 | none | API-106 | WF-01 | TC-0138, TC-0541, TC-0724, TC-0746 ; via: TC-0234 | 18 | Not Run |
| FR-089 | Requisition entry | BR-024 | none | M-049 | US-049 | UC-006 | API-107, API-108, API-111 | WF-01 | TC-0139, TC-0140, TC-0482, TC-0534, TC-0935 | 19 | Not Run |
| FR-090 | Requisition approval | BR-024 | none | M-050 | US-050 | UC-006 | API-109 | WF-01, WF-07 | TC-0141, TC-0291, TC-0482, TC-0484, TC-0534, TC-0935 | 17 | Not Run |
| FR-091 | Purchase order creation | BR-026 | none | M-053 | US-051 | UC-006 | API-110, API-112, API-113, API-118 | WF-01 | TC-0142, TC-0143, TC-0482, TC-0535, TC-0991, TC-1006, TC-1007, TC-1034 | 20 | Not Run |
| FR-092 | Purchase order approval | BR-026 | none | M-053 | US-052 | UC-006 | API-114 | WF-01, WF-07 | TC-0144, TC-0291, TC-0482, TC-0535, TC-0725, TC-1008 ; via: TC-0235, TC-0748 | 17 | Not Run |
| FR-093 | PO issue and revision | BR-026 | none | M-053 | US-053 | UC-006 | API-115, API-116 | WF-01 | TC-0145, TC-0726, TC-1034 | 19 | Not Run |
| FR-094 | PO cancellation | BR-026 | none | M-053 | US-053 | UC-006 | API-117 | WF-01 | TC-0146, TC-0147, TC-0483 | 17 | Not Run |
| FR-095 | Goods receipt | BR-027 | none | M-057 | US-054 | UC-007 | API-119 | WF-01 | TC-0148, TC-0149, TC-0286, TC-0291, TC-0482, TC-0483, TC-0536 | 20 | Not Run |
| FR-096 | Receipt reversal and return | BR-027 | none | M-057, M-073 | US-055 | UC-007 | API-120 | WF-01 | TC-0150, TC-0290, TC-0766 | 19 | Not Run |
| FR-097 | Purchase invoice entry | BR-028 | none | M-058 | US-056 | UC-008 | API-121 | WF-01 | TC-0151, TC-0482, TC-0750, TC-0936, TC-0945 | 18 | Not Run |
| FR-098 | Three-way match | BR-028 | none | M-058 | US-056 | UC-008 | API-122, API-123 | WF-01 | TC-0152, TC-0153, TC-0290, TC-0291, TC-0482, TC-0483, TC-0537, TC-0750, TC-0751, TC-0752, TC-0936 | 19 | Not Run |
| FR-099 | Duplicate invoice check | BR-029 | none | M-058 | US-057 | UC-008 | API-121 | WF-01 | TC-0154, TC-0155, TC-0290, TC-0483, TC-0537, TC-0747, TC-0753, TC-0904 ; via: TC-0233 | 18 | Not Run |
| FR-100 | Procurement reports | BR-058 | none | M-060 | US-058 | none | API-125, API-126 | WF-01 | TC-0156, TC-0727 | 17 | Not Run |
| FR-101 | Item master | BR-033 | none | M-061 | US-059 | none | API-127, API-128, API-129 | WF-06 | TC-0157, TC-0158 | 20 | Not Run |
| FR-102 | Categories | BR-033 | none | M-062 | US-059 | none | API-130, API-131 | WF-06 | TC-0159, TC-0728 | 18 | Not Run |
| FR-103 | Warehouses | BR-033, BR-002 | none | M-063 | US-060 | none | API-132, API-133 | WF-06 | TC-0160, TC-1047 | 18 | Not Run |
| FR-104 | Stock ledger | BR-034 | none | M-064, M-073 | US-061 | UC-007 | API-119, API-135 | WF-06 | TC-0161, TC-0162, TC-0276, TC-0286, TC-0287, TC-0293, TC-0508, TC-0729, TC-0861 ; via: TC-0261, TC-0742, TC-0778 | 21 | Not Run |
| FR-105 | Availability | BR-034, BR-036 | none | M-064 | US-061 | none | API-134 | WF-06 | TC-0163, TC-0730 | 17 | Not Run |
| FR-106 | Negative stock control | BR-035 | none | M-064 | US-062 | none | none | WF-06 | TC-0164, TC-0165, TC-0277, TC-0850, TC-0865 | 0 | Not Run |
| FR-107 | Reservation | BR-036 | none | M-065 | US-070 | UC-010 | API-152, API-189, API-190, API-191, API-192 | WF-06 | TC-0166, TC-0167, TC-0289, TC-0731 | 19 | Not Run |
| FR-108 | Transfer | BR-037 | none | M-066 | US-063 | UC-013 | API-136, API-137, API-138 | WF-06 | TC-0168, TC-0169, TC-0489, TC-0545 | 20 | Not Run |
| FR-109 | Adjustment | BR-038 | none | M-067 | US-064 | UC-014 | API-139, API-140 | WF-06 | TC-0170, TC-0171, TC-0489, TC-0545, TC-0758, TC-0759, TC-0760, TC-0761, TC-0842, TC-0852, TC-0853, TC-0854, TC-0864, TC-0876 | 19 | Not Run |
| FR-110 | Issue and receipt documents | BR-034 | none | M-073 | US-065 | none | API-141 | WF-06 | TC-0172, TC-0732, TC-0981 | 19 | Not Run |
| FR-111 | Valuation | BR-039 | none | M-072 | US-066 | none | API-142 | WF-06 | TC-0173, TC-0174, TC-0275, TC-0295, TC-0489, TC-0553, TC-0840, TC-0981 ; via: TC-0501 | 18 | Not Run |
| FR-112 | Inventory reports | BR-058 | none | M-075 | US-067 | none | API-143, API-144 | WF-06 | TC-0175, TC-0733 | 17 | Not Run |
| FR-113 | Stock concurrency | BR-035 | NFR-024 | M-064 | US-062 | none | none | WF-06 | TC-0176, TC-0277, TC-0511, TC-0529, TC-0734, TC-0749, TC-0867 | 0 | Not Run |
| FR-114 | Customer master | BR-041 | none | M-079 | US-068 | none | API-145, API-146, API-147 | WF-02 | TC-0177, TC-0178 | 19 | Not Run |
| FR-115 | Credit check | BR-046 | none | M-079, M-081 | US-070 | UC-010 | API-152 | WF-02 | TC-0179, TC-0486, TC-0749 | 19 | Not Run |
| FR-116 | Quotation | BR-042 | none | M-080 | US-069 | UC-010 | API-148, API-149 | WF-02 | TC-0180, TC-0485, TC-0539 | 18 | Not Run |
| FR-117 | Quote conversion | BR-042 | none | M-080, M-081 | US-069 | UC-010 | API-150 | WF-02 | TC-0181, TC-0485, TC-0735 | 18 | Not Run |
| FR-118 | Sales order | BR-042 | none | M-081 | US-070 | UC-010 | API-151, API-152, API-154 | WF-02 | TC-0182, TC-0183, TC-0289, TC-0485, TC-0486, TC-0539 | 22 | Not Run |
| FR-119 | Order cancellation | BR-043 | none | M-081 | US-071 | none | API-153 | WF-02 | TC-0184, TC-0289, TC-0486, TC-0736 | 17 | Not Run |
| FR-120 | Delivery | BR-043 | none | M-083 | US-072 | UC-011 | API-155 | WF-02 | TC-0185, TC-0186, TC-0279, TC-0287, TC-0485, TC-0486, TC-0540 | 19 | Not Run |
| FR-121 | Sales invoice | BR-043 | none | M-082 | US-073 | UC-011 | API-156 | WF-02 | TC-0187, TC-0188, TC-0279, TC-0485, TC-0540 | 18 | Not Run |
| FR-122 | Sales return | BR-044 | none | M-084 | US-074 | UC-012 | API-158, API-159 | WF-12 | TC-0189, TC-0190, TC-0293, TC-0490, TC-0542, TC-0762, TC-0763, TC-0766 | 19 | Not Run |
| FR-123 | Credit note | BR-044 | none | M-085 | US-075 | UC-012 | API-160 | WF-12 | TC-0191, TC-0293, TC-0490, TC-0542, TC-0762 | 18 | Not Run |
| FR-124 | Sales reporting | BR-047, BR-058 | none | M-088 | US-076 | none | API-161, API-162 | none | TC-0192, TC-0737 | 17 | Not Run |
| FR-125 | Optimistic concurrency | BR-014 | none | All | US-077 | none | API-013, API-080, API-103, API-108, API-113, API-129, API-147 | none | TC-0081, TC-0509, TC-0738, TC-0822, TC-1048 | 25 | Not Run |
| FR-126 | Report scope | BR-003, BR-058 | none | All | US-009 | none | none | none | TC-0082, TC-0282, TC-0739 ; via: TC-0779 | 0 | Not Run |
| FR-127 | Retention configuration | BR-059 | none | M-116 (basic) | US-078 | none | API-055 | none | TC-0083, TC-0084, TC-0552 | 17 | Not Run |
| FR-128 | Outbound integration queue | BR-060 | NFR-008 | M-012 | US-080 | none | API-053, API-054, API-194 | none | TC-0085, TC-0288, TC-0294, TC-0512, TC-0740, TC-0745, TC-0824, TC-0827, TC-1029 | 18 | Not Run |
| FR-129 | Personal data handling | BR-059 | none | M-116 (basic) | US-079 | none | API-056, API-199 | none | TC-0086, TC-0552, TC-0741 | 18 | Not Run |
| FR-130 | Configurable fields | BR-061 | none | M-008 | US-018 | none | API-167, API-168, API-169 | none | TC-0087, TC-0088 | 19 | Not Run |
| FR-131 | Role tiers | BR-003 | NFR-016 | M-004 | US-081, US-082 | none | API-016 | none | TC-0554, TC-0555, TC-0556, TC-0557, TC-0558, TC-0647, TC-0648, TC-0775, TC-0789 | 19 | Not Run |
| FR-132 | No self-grant | BR-003 | NFR-016 | M-004, M-003 | US-081 | none | API-016 | none | TC-0559, TC-0560, TC-0561, TC-0562, TC-0648, TC-0768, TC-0806 | 19 | Not Run |
| FR-133 | Four-eyes role grant | BR-003, BR-006 | NFR-016 | M-004, M-013 | US-083 | none | API-016, API-170, API-171, API-172 | WF-07 | TC-0567, TC-0568, TC-0569, TC-0570, TC-0571, TC-0572, TC-0573, TC-0574, TC-0575, TC-0647, TC-0768, TC-0775, TC-0789, TC-0807 | 19 | Not Run |
| FR-134 | Branch-limited assignment | BR-002, BR-003 | NFR-016 | M-004 | US-082 | none | API-016 | none | TC-0563, TC-0564, TC-0565, TC-0566, TC-0648, TC-0805 | 19 | Not Run |
| FR-135 | Payment duty separation | BR-020, BR-030 | NFR-016, NFR-026 | M-047, M-059 | US-084 | none | API-094, API-095 | WF-01, WF-12 | TC-0576, TC-0577, TC-0578, TC-0579, TC-0580, TC-0581, TC-0647, TC-0754, TC-0765, TC-0767, TC-0775, TC-0776, TC-0794, TC-0810, TC-0811, TC-0832, TC-0890, TC-0901 ; via: TC-0900 | 22 | Not Run |
| FR-136 | Journal duty separation | BR-021 | NFR-021, NFR-026 | M-035 | US-099 | none | API-083 | WF-03 | TC-0587, TC-0588, TC-0589, TC-0590, TC-0777, TC-1051 ; via: TC-0947 | 19 | Not Run |
| FR-137 | Release control | BR-020, BR-030 | NFR-016, NFR-025 | M-047 | US-084 | none | API-094, API-095 | WF-01, WF-12 | TC-0582, TC-0583, TC-0584, TC-0585, TC-0586, TC-0757, TC-0776, TC-0794, TC-0811, TC-0817, TC-0902, TC-0917, TC-0924, TC-0928 ; via: TC-0812 | 22 | Not Run |
| FR-138 | Controlled change workflow | BR-006, BR-021 | NFR-037, NFR-026 | M-013, M-008 | US-100 | none | API-039, API-181, API-182 | WF-07 | TC-0591, TC-0592, TC-0593, TC-0594, TC-0772, TC-0786, TC-0802, TC-0803, TC-0940, TC-0961, TC-0980, TC-0992, TC-1000, TC-1051 | 17 | Not Run |
| FR-139 | Vendor bank change control | BR-025 | NFR-016, NFR-026 | M-054 | US-085, US-086 | none | API-106, API-176, API-177, API-178 | WF-01 | TC-0595, TC-0596, TC-0597, TC-0598, TC-0599, TC-0600, TC-0601, TC-0602, TC-0603, TC-0769, TC-1014 | 18 | Not Run |
| FR-140 | Period reopen control | BR-016, BR-021 | NFR-026 | M-043 | US-087, US-088 | none | API-075, API-173, API-174, API-175 | WF-03 | TC-0604, TC-0605, TC-0606, TC-0607, TC-0608, TC-0609, TC-0610, TC-0770, TC-0960, TC-0975 | 17 | Not Run |
| FR-141 | Credit limit control | BR-046 | NFR-016 | M-079 | US-089, US-090 | none | API-147, API-179, API-180 | WF-02 | TC-0611, TC-0612, TC-0613, TC-0614, TC-0771 | 17 | Not Run |
| FR-142 | SoD override control | BR-003 | NFR-016, NFR-026 | M-004 | US-091 | none | API-183, API-184 | none | TC-0615, TC-0616, TC-0617, TC-0618, TC-0619, TC-0773, TC-0823 | 0 | Not Run |
| FR-143 | Delegation restrictions | BR-006 | NFR-016 | M-013 | US-101 | none | none | WF-07 | TC-0620, TC-0621, TC-0622, TC-0623, TC-0624, TC-0816, TC-0912, TC-1049 | 0 | Not Run |
| FR-144 | Emergency access | BR-003, BR-005 | NFR-026 | M-002, M-003 | US-092, US-093 | none | API-185, API-201 | none | TC-0625, TC-0626, TC-0627, TC-0628, TC-0629, TC-0774 | 0 | Not Run |
| FR-145 | Step-up for sensitive actions | BR-004 | NFR-016 | M-002 | US-083, US-084 | none | none | none | TC-0582, TC-0630, TC-0776, TC-0812, TC-0902 ; via: TC-0214, TC-0583, TC-0584, TC-0585, TC-0586, TC-0924 | 0 | Not Run |
| FR-146 | Permissions for remaining MVP modules | BR-003 | NFR-016 | M-001, M-012, M-015, M-065 | US-094 | none | API-189, API-190, API-191, API-192, API-193, API-194, API-195, API-196, API-197 | none | TC-0631, TC-0632, TC-0633, TC-0634, TC-0635 | 0 | Not Run |
| FR-147 | Attachment immutability | BR-010, BR-005 | NFR-026 | M-011 | US-095 | none | API-186 | none | TC-0636, TC-0637, TC-0638 ; via: TC-0908 | 0 | Not Run |
| FR-148 | Export control | BR-009, BR-003 | NFR-016 | M-014 | US-096 | none | API-050, API-198, API-199 | none | TC-0639, TC-0640, TC-0641 | 18 | Not Run |
| FR-149 | Service accounts and tokens | BR-004, BR-060 | NFR-014, NFR-016 | M-002 | US-097 | none | API-187, API-188, API-200 | none | TC-0642, TC-0643, TC-0644, TC-1050 | 0 | Not Run |
| FR-150 | Audit read separation | BR-005 | NFR-026 | M-010 | US-093, US-098 | none | API-051, API-202 | none | TC-0645, TC-0646 | 17 | Not Run |

## 2. Business requirement roll-up

| BR | Title | FRs | Distinct test cases | State |
|---|---|---|---|---|
| BR-001 | Multi-company support | FR-018, FR-019 | 9 | Covered by test cases (not executed) |
| BR-002 | Multi-branch operation | FR-010, FR-020, FR-021, FR-103, FR-134 | 17 | Covered by test cases (not executed) |
| BR-003 | Role and scope based access | FR-008, FR-010, FR-012, FR-013, FR-014, FR-015, FR-016, FR-017, FR-126, FR-131, FR-132, FR-133, FR-134, FR-142, FR-144, FR-146, FR-148 | 86 | Covered by test cases (not executed) |
| BR-004 | Secure sign-in | FR-001, FR-002, FR-003, FR-004, FR-005, FR-006, FR-007, FR-009, FR-145, FR-149 | 40 | Covered by test cases (not executed) |
| BR-005 | Complete audit trail | FR-030, FR-031, FR-032, FR-050, FR-144, FR-147, FR-150 | 31 | Covered by test cases (not executed) |
| BR-006 | Configurable approvals | FR-024, FR-027, FR-039, FR-040, FR-041, FR-042, FR-043, FR-044, FR-045, FR-046, FR-047, FR-133, FR-138, FR-143 | 103 | Covered by test cases (not executed) |
| BR-007 | Actionable notifications | FR-035, FR-036, FR-037, FR-038 | 18 | Covered by test cases (not executed) |
| BR-008 | Governed master data | FR-028, FR-029 | 4 | Covered by test cases (not executed) |
| BR-009 | Bulk import and export | FR-048, FR-049, FR-050, FR-148 | 16 | Covered by test cases (not executed) |
| BR-010 | Attachments | FR-033, FR-034, FR-147 | 10 | Covered by test cases (not executed) |
| BR-011 | Localisation | FR-052 | 2 | Covered by test cases (not executed) |
| BR-012 | Configurable numbering | FR-025, FR-026 | 8 | Covered by test cases (not executed) |
| BR-013 | Role-based dashboards | FR-047, FR-051 | 6 | Covered by test cases (not executed) |
| BR-014 | Double-entry ledger | FR-062, FR-064, FR-065, FR-079, FR-080, FR-085, FR-125 | 30 | Covered by test cases (not executed) |
| BR-015 | Configurable chart of accounts | FR-053, FR-054, FR-055 | 6 | Covered by test cases (not executed) |
| BR-016 | Period and year control | FR-056, FR-057, FR-058, FR-059, FR-060, FR-061, FR-140 | 30 | Covered by test cases (not executed) |
| BR-017 | Multi-currency | FR-067, FR-068, FR-069 | 8 | Covered by test cases (not executed) |
| BR-018 | Tax calculation | FR-070, FR-071 | 5 | Covered by test cases (not executed) |
| BR-019 | Financial statements | FR-066, FR-082, FR-083, FR-084 | 11 | Covered by test cases (not executed) |
| BR-020 | Payments and refunds | FR-072, FR-073, FR-074, FR-076, FR-077, FR-078, FR-135, FR-137 | 57 | Covered by test cases (not executed) |
| BR-021 | Maker-checker on journals | FR-017, FR-042, FR-063, FR-136, FR-138, FR-140 | 50 | Covered by test cases (not executed) |
| BR-022 | Bank reconciliation | none | 0 | No FR yet (post-MVP or covered by NFR) |
| BR-023 | Budgeting and control | none | 0 | No FR yet (post-MVP or covered by NFR) |
| BR-024 | Requisition and approval | FR-089, FR-090 | 8 | Covered by test cases (not executed) |
| BR-025 | Vendor master control | FR-086, FR-087, FR-088, FR-139 | 19 | Covered by test cases (not executed) |
| BR-026 | Purchase order approval | FR-091, FR-092, FR-093, FR-094 | 17 | Covered by test cases (not executed) |
| BR-027 | Goods receipt with partials | FR-095, FR-096 | 10 | Covered by test cases (not executed) |
| BR-028 | Three-way match | FR-079, FR-097, FR-098 | 17 | Covered by test cases (not executed) |
| BR-029 | Duplicate invoice prevention | FR-099 | 8 | Covered by test cases (not executed) |
| BR-030 | Approved vendor payments | FR-075, FR-135, FR-137 | 36 | Covered by test cases (not executed) |
| BR-031 | Sourcing by quotation | none | 0 | No FR yet (post-MVP or covered by NFR) |
| BR-032 | Contract management | none | 0 | No FR yet (post-MVP or covered by NFR) |
| BR-033 | Item and category master | FR-101, FR-102, FR-103 | 6 | Covered by test cases (not executed) |
| BR-034 | Stock ledger | FR-104, FR-105, FR-110 | 14 | Covered by test cases (not executed) |
| BR-035 | Negative stock control | FR-106, FR-113 | 11 | Covered by test cases (not executed) |
| BR-036 | Stock reservation | FR-105, FR-107 | 6 | Covered by test cases (not executed) |
| BR-037 | Transfers between locations | FR-108 | 4 | Covered by test cases (not executed) |
| BR-038 | Controlled adjustments | FR-109 | 14 | Covered by test cases (not executed) |
| BR-039 | Inventory valuation | FR-111 | 8 | Covered by test cases (not executed) |
| BR-040 | Batch, lot, serial and expiry | none | 0 | No FR yet (post-MVP or covered by NFR) |
| BR-041 | Customer master and terms | FR-114 | 2 | Covered by test cases (not executed) |
| BR-042 | Quote to invoice flow | FR-116, FR-117, FR-118 | 9 | Covered by test cases (not executed) |
| BR-043 | Partial delivery and invoicing | FR-119, FR-120, FR-121 | 12 | Covered by test cases (not executed) |
| BR-044 | Returns and credit notes | FR-078, FR-122, FR-123 | 12 | Covered by test cases (not executed) |
| BR-045 | Customer receipts | FR-073, FR-074 | 13 | Covered by test cases (not executed) |
| BR-046 | Credit limit control | FR-115, FR-141 | 8 | Covered by test cases (not executed) |
| BR-047 | Receivable ageing | FR-081, FR-124 | 8 | Covered by test cases (not executed) |
| BR-048 | Lead and opportunity pipeline | none | 0 | No FR yet (post-MVP or covered by NFR) |
| BR-049 | Single employee record | FR-009, FR-011, FR-022, FR-023, FR-024 | 22 | Covered by test cases (not executed) |
| BR-050 | Attendance, leave and payroll | none | 0 | No FR yet (post-MVP or covered by NFR) |
| BR-051 | Expense claims | none | 0 | No FR yet (post-MVP or covered by NFR) |
| BR-052 | Employee self-service | none | 0 | No FR yet (post-MVP or covered by NFR) |
| BR-053 | Project costing and billing | none | 0 | No FR yet (post-MVP or covered by NFR) |
| BR-054 | Asset lifecycle | none | 0 | No FR yet (post-MVP or covered by NFR) |
| BR-055 | Support and SLA tracking | none | 0 | No FR yet (post-MVP or covered by NFR) |
| BR-056 | External portals | none | 0 | No FR yet (post-MVP or covered by NFR) |
| BR-057 | Production and quality | none | 0 | No FR yet (post-MVP or covered by NFR) |
| BR-058 | Operational reporting | FR-100, FR-112, FR-124, FR-126 | 9 | Covered by test cases (not executed) |
| BR-059 | Data retention and privacy | FR-127, FR-129 | 5 | Covered by test cases (not executed) |
| BR-060 | External integration resilience | FR-037, FR-128, FR-149 | 21 | Covered by test cases (not executed) |
| BR-061 | Industry configurability | FR-027, FR-130 | 4 | Covered by test cases (not executed) |
| BR-062 | Service levels | none | 0 | No FR yet (post-MVP or covered by NFR) |
| BR-063 | Business intelligence | none | 0 | No FR yet (post-MVP or covered by NFR) |

## 3. Non-functional requirement trace

| NFR | Title | Target measurable (see NFR-Remediation-Report) | Test cases | FRs (section 3A) | Result |
|---|---|---|---|---|---|
| NFR-001 | Interactive response | 95th percentile under 2 s for reads and under 3 s for saves  | TC-0522 | none | Not Run |
| NFR-002 | Report response | 95th percentile under 10 s. Longer reports run in the backgr | TC-0523, TC-0744 | none | Not Run |
| NFR-003 | Posting throughput | Proposed: sustain 20 postings per second (invoice, receipt,  | TC-0524, TC-0649, TC-0650, TC-0972 | FR-032 | Not Run |
| NFR-004 | Search | 95th percentile under 2 s for 1 million rows per list | TC-0525 | FR-022 | Not Run |
| NFR-005 | Volume | Proposed: 100 companies, 2,000 concurrent users, 20 million  | TC-0526, TC-0651 | none | Not Run |
| NFR-006 | Growth | Adding a company or branch needs 0 code deployments and at m | TC-0498, TC-0652 | none | Not Run |
| NFR-007 | Uptime | 99.5 percent monthly for MVP (proposed). Final per OQ-028 | TC-0527 | none | Not Run |
| NFR-008 | Degraded mode | With email, bank adapter and gateway simulated down for 4 ho | TC-0527, TC-0653, TC-0745, TC-0824, TC-0926 | FR-037, FR-128 | Not Run |
| NFR-009 | RPO | 15 minutes proposed. Final per OQ-028 | TC-0528 | none | Not Run |
| NFR-010 | RTO | 4 hours proposed. Final per OQ-028 | TC-0528 | none | Not Run |
| NFR-011 | Backups | Daily full plus continuous log. Restore test each quarter | TC-0528 | none | Not Run |
| NFR-012 | Transport encryption | Current TLS version, no plain HTTP | TC-0219, TC-0227 | none | Not Run |
| NFR-013 | Encryption at rest | 256-bit security strength encryption at rest for databases,  | TC-0228, TC-0229, TC-0654, TC-0655 | none | Not Run |
| NFR-014 | Secrets | 0 secrets in source control history at release (full-history | TC-0230, TC-0656, TC-0657 | FR-149 | Not Run |
| NFR-015 | Web vulnerabilities | No open High findings at release | TC-0216, TC-0217, TC-0218, TC-0219, TC-0220, TC-0221, TC-0245 | FR-015 | Not Run |
| NFR-016 | Authorisation on every call | 100 percent of endpoints | TC-0205, TC-0243, TC-0518, TC-0899, TC-0910 | FR-012, FR-013, FR-015, FR-016, FR-017, FR-131, FR-132, FR-133, FR-134, FR-135, FR-137, FR-139, FR-141, FR-142, FR-143, FR-145, FR-146, FR-148, FR-149 | Not Run |
| NFR-017 | Rate limiting | Sign-in 5 failures per account per 15 minutes then lockout ( | TC-0236, TC-0658, TC-0659, TC-0660 | none | Not Run |
| NFR-018 | Tenant isolation | No cross-tenant access in isolation tests | TC-0499 | FR-019 | Not Run |
| NFR-019 | Dependency hygiene | No known Critical vulnerability at release | TC-0241 | none | Not Run |
| NFR-020 | Money precision | No floating-point money | TC-0500, TC-0889, TC-0918, TC-0928 | none | Not Run |
| NFR-021 | Transactions | 100 percent | TC-0531, TC-0868, TC-0870, TC-0921, TC-0922 | FR-136 | Not Run |
| NFR-022 | Referential integrity | Foreign keys on all references | TC-0266 | none | Not Run |
| NFR-023 | Reconciliation | Zero unexplained difference | TC-0501 | none | Not Run |
| NFR-024 | Safe concurrent use | Zero duplicate numbers, zero lost updates in load tests | TC-0529, TC-0867 | FR-025, FR-113 | Not Run |
| NFR-025 | Retry safety | Idempotency key supported on those endpoints | TC-0232, TC-0530, TC-0869, TC-0922, TC-0925 | FR-076, FR-137 | Not Run |
| NFR-026 | Audit completeness | 100 percent | TC-0502 | FR-030, FR-031, FR-135, FR-136, FR-138, FR-139, FR-140, FR-142, FR-144, FR-147, FR-150 | Not Run |
| NFR-027 | Audit retention | Audit records kept at least 7 years or the longest applicabl | TC-0502, TC-0661, TC-0662 | none | Not Run |
| NFR-028 | Logging | 100 percent of log lines carry a timestamp and correlation i | TC-0231, TC-0238, TC-0521, TC-0663 | none | Not Run |
| NFR-029 | Monitoring and alerts | Alerts fire for: error rate above 2 percent over 5 minutes,  | TC-0239, TC-0240, TC-0664, TC-0665 | none | Not Run |
| NFR-030 | Task efficiency | With at least 5 users per role, median create purchase requi | TC-0497, TC-0551, TC-0666 | none | Not Run |
| NFR-031 | Clear errors | 100 percent of error responses carry a code, a message namin | TC-0215, TC-0222, TC-0496, TC-0551, TC-0667 | none | Not Run |
| NFR-032 | Standard | Level AA | TC-0494 | none | Not Run |
| NFR-033 | Browsers and devices | Current and previous major versions of Chrome, Edge, Firefox | TC-0495, TC-0668, TC-0669 | none | Not Run |
| NFR-034 | Internationalisation | 100 percent of user-facing strings in resource files (scan f | TC-0503, TC-0670, TC-0671 | none | Not Run |
| NFR-035 | Modularity | 0 cyclic dependencies between modules. 0 cross-module access | TC-0504, TC-0672 | none | Not Run |
| NFR-036 | Test automation | Coverage targets in Test Strategy (Stage 3) | TC-0505 | none | Not Run |
| NFR-037 | Configuration over code | Approval chains, numbering formats, tolerances and tax codes | TC-0506, TC-0673 | FR-039, FR-138 | Not Run |
| NFR-038 | Environment parity | Staging and production are built from the same definitions.  | TC-0242, TC-0244, TC-0520, TC-0674 | none | Not Run |
| NFR-039 | Privacy | Personal data inventory covers 100 percent of personal field | TC-0231, TC-0675 | none | Not Run |
| NFR-040 | Retention | Retention period configurable per record type. Deletion insi | TC-0533, TC-0676 | none | Not Run |
| NFR-041 | Time handling | 0 timestamp columns stored in a non-UTC zone. Display is cor | TC-0519, TC-0532, TC-0677, TC-0678, TC-0889 | none | Not Run |
| NFR-042 | Archival | Closed-period data older than N years is archived with repor | TC-0507, TC-0679 | none | Not Run |

### 3A. FR to NFR relationship register

Every FR to NFR link with its reason. "Original" is the v0.1 link. The relationship was judged by requirement meaning and acceptance text, not by ID number. See also `13-Remediation-Reports/RTM-Correction-Report.md`.

| FR | NFR | Status | Original v0.1 link | Reason | Evidence |
|---|---|---|---|---|---|
| FR-012 | NFR-016 | Corrected | NFR-012, NFR-015 | Custom roles are only effective if every call checks authorisation server side. Transport encryption and web vulnerability classes are not what FR-012 specifies. | FRS FR-012 text; NFR-016 text; TC-0025, TC-0026 |
| FR-013 | NFR-016 | Corrected | NFR-013 | The permission catalogue is the list of codes that NFR-016 requires on every endpoint. Encryption at rest is unrelated. | FR-013 text; NFR-016; API conventions |
| FR-015 | NFR-015 | Verified valid | NFR-015 | Scoped authorisation includes object-level access (IDOR), which NFR-015 names. | TC-0206 object-level IDOR test names SEC-014 and US-009 |
| FR-015 | NFR-016 | Verified valid | NFR-016 | Scope is evaluated on every request. | NFR-016 target 100 percent of endpoints |
| FR-016 | NFR-016 | Verified valid | NFR-016 | Temporary grants are evaluated per request, so expiry takes effect at once. | FR-016 acceptance; NFR-016 |
| FR-017 | NFR-016 | Corrected | NFR-017 | Segregation checks run server side on every call. Rate limiting is unrelated. | FR-017 text; NFR-016; SOD rules |
| FR-019 | NFR-018 | Corrected | NFR-019 | Company isolation is the tenant isolation requirement. NFR-019 is dependency scanning. | FR-019 acceptance ("no read or write crosses companies"); NFR-018 text |
| FR-022 | NFR-004 | Corrected | NFR-022 | Directory search is a list search with a 95th percentile target. Referential integrity is unrelated. | FR-022 text; NFR-004 target |
| FR-025 | NFR-024 | Verified valid | NFR-024 | Numbering must not issue duplicates under concurrent use. | FR-025; NFR-024 "zero duplicate numbers"; TC-0284 |
| FR-030 | NFR-026 | Corrected | NFR-028 | Audit capture is the audit completeness requirement. NFR-028 is application logging. | FR-030 text; NFR-026 target 100 percent |
| FR-031 | NFR-026 | Corrected | NFR-031 | Audit search works on the audit record that NFR-026 protects. NFR-031 is user error messages. | FR-031 text; NFR-026 |
| FR-039 | NFR-037 | Corrected | NFR-028, NFR-039 | Approval chains are configuration, not code. Logging and privacy are unrelated. | FR-039 text; NFR-037 text |
| FR-076 | NFR-025 | Verified valid | NFR-025 | Payment idempotency is retry safety. | FR-076 text; NFR-025 target; TC-0124, TC-0125 |
| FR-113 | NFR-024 | Verified valid | NFR-024 | Stock concurrency is safe concurrent use. | FR-113 text; NFR-024; TC-0176 |
| FR-012 | none | Removed | NFR-012, NFR-015 (see above) | No second valid NFR found. Not forced. | Review of NFR list |
| FR-014 | none | Removed | NFR-014 | Default roles are seeded data. Secrets management does not apply. | FR-014 text |
| FR-015 | none | Removed | NFR-012 | Transport encryption is not specific to scoped authorisation. | FR-015 text |
| FR-025 | none | Removed | NFR-025 | Numbering is not an idempotent create endpoint. The number coincidence is the only link. | FR-025 text; NFR-025 |
| FR-028 | none | Removed | NFR-028, NFR-039 | Master data maintenance has no specific logging or privacy NFR beyond the platform ones. | FR-028 text |
| FR-029 | none | Removed | NFR-029 | Duplicate detection is a data rule, not monitoring. | FR-029 text |
| FR-038 | none | Removed | NFR-038 | Notification preferences have no portability aspect. | FR-038 text |
| FR-131 | NFR-016 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-016: server-side authorisation on every call | FR-131 text |
| FR-132 | NFR-016 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-016: server-side authorisation on every call | FR-132 text |
| FR-133 | NFR-016 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-016: server-side authorisation on every call | FR-133 text |
| FR-134 | NFR-016 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-016: server-side authorisation on every call | FR-134 text |
| FR-135 | NFR-016 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-016: server-side authorisation on every call | FR-135 text |
| FR-135 | NFR-026 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-026: audit completeness | FR-135 text |
| FR-136 | NFR-021 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-021: atomic commit of document and postings | FR-136 text |
| FR-136 | NFR-026 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-026: audit completeness | FR-136 text |
| FR-137 | NFR-016 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-016: server-side authorisation on every call | FR-137 text |
| FR-137 | NFR-025 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-025: retry safety on money-moving calls | FR-137 text |
| FR-138 | NFR-037 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-037: configuration without deployment | FR-138 text |
| FR-138 | NFR-026 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-026: audit completeness | FR-138 text |
| FR-139 | NFR-016 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-016: server-side authorisation on every call | FR-139 text |
| FR-139 | NFR-026 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-026: audit completeness | FR-139 text |
| FR-140 | NFR-026 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-026: audit completeness | FR-140 text |
| FR-141 | NFR-016 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-016: server-side authorisation on every call | FR-141 text |
| FR-142 | NFR-016 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-016: server-side authorisation on every call | FR-142 text |
| FR-142 | NFR-026 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-026: audit completeness | FR-142 text |
| FR-143 | NFR-016 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-016: server-side authorisation on every call | FR-143 text |
| FR-144 | NFR-026 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-026: audit completeness | FR-144 text |
| FR-145 | NFR-016 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-016: server-side authorisation on every call | FR-145 text |
| FR-146 | NFR-016 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-016: server-side authorisation on every call | FR-146 text |
| FR-147 | NFR-026 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-026: audit completeness | FR-147 text |
| FR-148 | NFR-016 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-016: server-side authorisation on every call | FR-148 text |
| FR-149 | NFR-014 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-014: secrets and token rotation | FR-149 text |
| FR-149 | NFR-016 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-016: server-side authorisation on every call | FR-149 text |
| FR-150 | NFR-026 | New in v0.2 | none | Authorisation hardening requirement relates to NFR-026: audit completeness | FR-150 text |

## 4. Technical requirement trace

| TR | Test cases | Result |
|---|---|---|
| TR-001 | TC-0500 | Not Run |
| TR-002 | TC-0208, TC-0499 | Not Run |
| TR-003 | TC-0508 | Not Run |
| TR-004 | TC-0509 | Not Run |
| TR-005 | TC-0510 | Not Run |
| TR-006 | TC-0511 | Not Run |
| TR-007 | TC-0512 | Not Run |
| TR-008 | TC-0513 | Not Run |
| TR-009 | TC-0514 | Not Run |
| TR-010 | TC-0515 | Not Run |
| TR-011 | TC-0516 | Not Run |
| TR-012 | TC-0226 | Not Run |
| TR-013 | TC-0296, TC-0297, TC-0298, TC-0299, TC-0300, TC-0301, TC-0302, TC-0303, TC-0304, TC-0305, TC-0306, TC-0307, TC-0308, TC-0309, TC-0310, TC-0311 | Not Run |
| TR-014 | TC-0517 | Not Run |
| TR-015 | TC-0518 | Not Run |
| TR-016 | TC-0519 | Not Run |
| TR-017 | TC-0520 | Not Run |
| TR-018 | TC-0521 | Not Run |

## 5. Security requirement, threat, invariant and database constraint trace

SEC rows show the FR named in the security requirement itself. A security requirement that names only an NFR or TR is a cross-cutting technical control and is traced to that NFR or TR, not to an FR (reason column).

| ID | FR named | Reason when no FR | Test cases | Result |
|---|---|---|---|---|
| SEC-001 | FR-001 | - | TC-0193 | Not Run |
| SEC-002 | FR-002 | - | TC-0194 | Not Run |
| SEC-003 | FR-003 | - | TC-0195 | Not Run |
| SEC-004 | FR-005 | - | TC-0196 | Not Run |
| SEC-005 | FR-005 | - | TC-0197 | Not Run |
| SEC-006 | FR-006 | - | TC-0198 | Not Run |
| SEC-007 | FR-007 | - | TC-0199 | Not Run |
| SEC-008 | FR-004 | - | TC-0200 | Not Run |
| SEC-009 | FR-004 | - | TC-0201, TC-0793, TC-0818 | Not Run |
| SEC-010 | FR-009 | - | TC-0202 | Not Run |
| SEC-011 | FR-035 | - | TC-0203 | Not Run |
| SEC-012 | FR-004 | - | TC-0204 | Not Run |
| SEC-013 | FR-015 | - | TC-0205, TC-0847, TC-1002, TC-1003, TC-1012 | Not Run |
| SEC-014 | FR-015, FR-019 | - | TC-0206, TC-0855, TC-0856, TC-1004 | Not Run |
| SEC-015 | FR-064 | - | TC-0207 | Not Run |
| SEC-016 | FR-019 | - | TC-0208, TC-0268 | Not Run |
| SEC-017 | FR-019 | - | TC-0209 | Not Run |
| SEC-018 | FR-014 | - | TC-0210, TC-1011 | Not Run |
| SEC-019 | FR-017 | - | TC-0211 | Not Run |
| SEC-020 | FR-042 | - | TC-0212, TC-0846, TC-0998 | Not Run |
| SEC-021 | FR-030 | - | TC-0213 | Not Run |
| SEC-022 | FR-059, FR-145 | - | TC-0214 | Not Run |
| SEC-023 | none | Cross-cutting control traced to NFR-031 | TC-0215 | Not Run |
| SEC-024 | none | Cross-cutting control traced to NFR-015 | TC-0216 | Not Run |
| SEC-025 | none | Cross-cutting control traced to NFR-015 | TC-0217 | Not Run |
| SEC-026 | none | Cross-cutting control traced to NFR-015 | TC-0218 | Not Run |
| SEC-027 | none | Cross-cutting control traced to NFR-012, NFR-015 | TC-0219 | Not Run |
| SEC-028 | none | Cross-cutting control traced to NFR-015 | TC-0220 | Not Run |
| SEC-029 | none | Cross-cutting control traced to NFR-015 | TC-0221 | Not Run |
| SEC-030 | none | Cross-cutting control traced to NFR-031 | TC-0222 | Not Run |
| SEC-031 | FR-048, FR-050 | - | TC-0223 | Not Run |
| SEC-032 | FR-033 | - | TC-0224, TC-1010 | Not Run |
| SEC-033 | FR-033 | - | TC-0225 | Not Run |
| SEC-034 | FR-034 | - | TC-0226 | Not Run |
| SEC-035 | none | Cross-cutting control traced to NFR-012 | TC-0227 | Not Run |
| SEC-036 | none | Cross-cutting control traced to NFR-013 | TC-0228 | Not Run |
| SEC-037 | none | Cross-cutting control traced to NFR-013 | TC-0229 | Not Run |
| SEC-038 | none | Cross-cutting control traced to NFR-014 | TC-0230 | Not Run |
| SEC-039 | none | Cross-cutting control traced to NFR-028, NFR-039 | TC-0231 | Not Run |
| SEC-040 | FR-076 | - | TC-0232, TC-1031 | Not Run |
| SEC-041 | FR-029, FR-099 | - | TC-0233, TC-0747 | Not Run |
| SEC-042 | FR-088 | - | TC-0234, TC-0746 | Not Run |
| SEC-043 | FR-092 | - | TC-0235, TC-0748 | Not Run |
| SEC-044 | none | Cross-cutting control traced to NFR-017 | TC-0236 | Not Run |
| SEC-045 | FR-050 | - | TC-0237 | Not Run |
| SEC-046 | FR-030 | - | TC-0238 | Not Run |
| SEC-047 | none | Cross-cutting control traced to NFR-029 | TC-0239 | Not Run |
| SEC-048 | none | Cross-cutting control traced to NFR-029 | TC-0240 | Not Run |
| SEC-049 | none | Cross-cutting control traced to NFR-019 | TC-0241 | Not Run |
| SEC-050 | none | Cross-cutting control traced to NFR-038 | TC-0242 | Not Run |
| SEC-051 | none | Cross-cutting control traced to NFR-016 | TC-0243 | Not Run |
| SEC-052 | none | Cross-cutting control traced to NFR-038 | TC-0244 | Not Run |
| SEC-053 | none | Cross-cutting control traced to NFR-015 | TC-0245 | Not Run |
| SEC-054 | FR-131 | - | TC-0554, TC-0555, TC-0556, TC-0557, TC-0558, TC-0647 | Not Run |
| SEC-055 | FR-132 | - | TC-0559, TC-0560, TC-0561, TC-0562, TC-0806 | Not Run |
| SEC-056 | FR-133 | - | TC-0567, TC-0568, TC-0569, TC-0570, TC-0571, TC-0572, TC-0573, TC-0574, TC-0575, TC-0789, TC-0807 | Not Run |
| SEC-057 | FR-134 | - | TC-0563, TC-0564, TC-0565, TC-0566, TC-0805 | Not Run |
| SEC-058 | FR-135 | - | TC-0576, TC-0577, TC-0578, TC-0579, TC-0580, TC-0581, TC-0754, TC-0900, TC-0901 | Not Run |
| SEC-059 | FR-136 | - | TC-0587, TC-0588, TC-0589, TC-0590, TC-0947 | Not Run |
| SEC-060 | FR-137, FR-145 | - | TC-0582, TC-0583, TC-0584, TC-0585, TC-0586, TC-0812, TC-0924 | Not Run |
| SEC-061 | FR-138 | - | TC-0591, TC-0592, TC-0593, TC-0594, TC-0802, TC-0803, TC-0961 | Not Run |
| SEC-062 | FR-139 | - | TC-0595, TC-0596, TC-0597, TC-0598, TC-0599, TC-0600, TC-0601, TC-0602, TC-0603 | Not Run |
| SEC-063 | FR-140 | - | TC-0604, TC-0605, TC-0606, TC-0607, TC-0608, TC-0609, TC-0610, TC-0960, TC-0975 | Not Run |
| SEC-064 | FR-141 | - | TC-0611, TC-0612, TC-0613, TC-0614 | Not Run |
| SEC-065 | FR-142 | - | TC-0615, TC-0616, TC-0617, TC-0618, TC-0619 | Not Run |
| SEC-066 | FR-143 | - | TC-0620, TC-0621, TC-0622, TC-0623, TC-0624, TC-0912, TC-1049 | Not Run |
| SEC-067 | FR-144 | - | TC-0625, TC-0626, TC-0627, TC-0628, TC-0629 | Not Run |
| SEC-068 | FR-145 | - | TC-0630, TC-0812, TC-0902 | Not Run |
| SEC-069 | FR-146 | - | TC-0631, TC-0632, TC-0633, TC-0634, TC-0635 | Not Run |
| SEC-070 | FR-147 | - | TC-0636, TC-0637, TC-0638, TC-0908 | Not Run |
| SEC-071 | FR-148 | - | TC-0639, TC-0640, TC-0641 | Not Run |
| SEC-072 | FR-149 | - | TC-0642, TC-0643, TC-0644, TC-1050 | Not Run |
| SEC-073 | FR-150 | - | TC-0645, TC-0646 | Not Run |

Threats. "Direct" means a test case names the threat. "Via SEC" means the threat row names mitigating security requirements that have test cases. A threat with neither is shown as none and counted as untested.

| THR | Direct test cases | Via SEC (mitigation tests) | Result |
|---|---|---|---|
| THR-001 | none | TC-0194, TC-0195, TC-0198, TC-0236 | Not Run |
| THR-002 | none | TC-0196 | Not Run |
| THR-003 | none | TC-0197 | Not Run |
| THR-004 | none | TC-0200, TC-0201, TC-0204, TC-0793, TC-0818 | Not Run |
| THR-005 | none | TC-0199 | Not Run |
| THR-006 | none | TC-0207 | Not Run |
| THR-007 | none | TC-0208, TC-0268 | Not Run |
| THR-008 | TC-0742 | none | Not Run |
| THR-009 | none | TC-0235, TC-0748 | Not Run |
| THR-010 | none | TC-0223 | Not Run |
| THR-011 | TC-0743 | none | Not Run |
| THR-012 | TC-0246 | none | Not Run |
| THR-013 | none | TC-0206, TC-0855, TC-0856, TC-1004 | Not Run |
| THR-014 | none | TC-0205, TC-0847, TC-1002, TC-1003, TC-1012 | Not Run |
| THR-015 | none | TC-0208, TC-0209, TC-0268 | Not Run |
| THR-016 | none | TC-0237 | Not Run |
| THR-017 | none | TC-0222, TC-0231 | Not Run |
| THR-018 | none | TC-0226 | Not Run |
| THR-019 | none | TC-0228 | Not Run |
| THR-020 | none | TC-0216 | Not Run |
| THR-021 | none | TC-0236 | Not Run |
| THR-022 | TC-0744 | none | Not Run |
| THR-023 | TC-0745 | none | Not Run |
| THR-024 | TC-0247 | none | Not Run |
| THR-025 | none | TC-0210, TC-0213, TC-0214, TC-1011 | Not Run |
| THR-026 | none | TC-0198, TC-0211, TC-0214 | Not Run |
| THR-027 | none | TC-0205, TC-0847, TC-1002, TC-1003, TC-1012 | Not Run |
| THR-028 | none | TC-0241, TC-0242 | Not Run |
| THR-029 | TC-0248 | TC-0239 | Not Run |
| THR-030 | none | TC-0221 | Not Run |
| THR-031 | TC-0249 | none | Not Run |
| THR-032 | TC-0746 | TC-0234 | Not Run |
| THR-033 | TC-0747 | TC-0233 | Not Run |
| THR-034 | TC-0748 | TC-0235 | Not Run |
| THR-035 | TC-0250 | TC-0238 | Not Run |
| THR-036 | TC-0251 | TC-0214 | Not Run |
| THR-037 | TC-0252 | none | Not Run |
| THR-038 | TC-0253 | TC-0211 | Not Run |
| THR-039 | TC-0254 | none | Not Run |
| THR-040 | TC-0255 | TC-0212, TC-0846, TC-0998 | Not Run |
| THR-041 | TC-0256 | TC-0232, TC-1031 | Not Run |
| THR-042 | TC-0749 | none | Not Run |
| THR-043 | TC-0257 | TC-0202, TC-0237, TC-0239 | Not Run |
| THR-044 | TC-0258 | TC-0243 | Not Run |
| THR-045 | none | TC-0554, TC-0555, TC-0556, TC-0557, TC-0558, TC-0559, TC-0560, TC-0561, TC-0562, TC-0563, TC-0564, TC-0565, TC-0566, TC-0647, TC-0805, TC-0806 | Not Run |
| THR-046 | none | TC-0559, TC-0560, TC-0561, TC-0562, TC-0567, TC-0568, TC-0569, TC-0570, TC-0571, TC-0572, TC-0573, TC-0574, TC-0575, TC-0789, TC-0806, TC-0807 | Not Run |
| THR-047 | none | TC-0576, TC-0577, TC-0578, TC-0579, TC-0580, TC-0581, TC-0582, TC-0583, TC-0584, TC-0585, TC-0586, TC-0754, TC-0812, TC-0900, TC-0901, TC-0924 | Not Run |
| THR-048 | none | TC-0587, TC-0588, TC-0589, TC-0590, TC-0947 | Not Run |
| THR-049 | none | TC-0595, TC-0596, TC-0597, TC-0598, TC-0599, TC-0600, TC-0601, TC-0602, TC-0603 | Not Run |
| THR-050 | none | TC-0604, TC-0605, TC-0606, TC-0607, TC-0608, TC-0609, TC-0610, TC-0960, TC-0975 | Not Run |
| THR-051 | none | TC-0611, TC-0612, TC-0613, TC-0614 | Not Run |
| THR-052 | none | TC-0615, TC-0616, TC-0617, TC-0618, TC-0619 | Not Run |
| THR-053 | none | TC-0620, TC-0621, TC-0622, TC-0623, TC-0624, TC-0912, TC-1049 | Not Run |
| THR-054 | none | TC-0625, TC-0626, TC-0627, TC-0628, TC-0629 | Not Run |
| THR-055 | none | TC-0567, TC-0568, TC-0569, TC-0570, TC-0571, TC-0572, TC-0573, TC-0574, TC-0575, TC-0789, TC-0807 | Not Run |
| THR-056 | none | TC-0636, TC-0637, TC-0638, TC-0908 | Not Run |
| THR-057 | none | TC-0642, TC-0643, TC-0644, TC-1050 | Not Run |
| THR-058 | none | TC-0201, TC-0202, TC-0793, TC-0818 | Not Run |
| THR-059 | none | TC-0645, TC-0646 | Not Run |

| ID | Test cases | Result |
|---|---|---|
| INV-01 | TC-0260, TC-0271 | Not Run |
| INV-02 | TC-0272 | Not Run |
| INV-03 | TC-0273, TC-0501 | Not Run |
| INV-04 | TC-0274 | Not Run |
| INV-05 | TC-0275, TC-0501 | Not Run |
| INV-06 | TC-0276 | Not Run |
| INV-07 | TC-0277 | Not Run |
| INV-08 | TC-0278 | Not Run |
| INV-09 | TC-0279 | Not Run |
| INV-10 | TC-0280 | Not Run |
| INV-11 | TC-0281 | Not Run |
| INV-12 | TC-0282, TC-0779 | Not Run |
| INV-13 | TC-0283 | Not Run |
| INV-14 | TC-0284 | Not Run |
| INV-15 | TC-0285 | Not Run |
| DBC-01 | TC-0259 | Not Run |
| DBC-02 | TC-0260 | Not Run |
| DBC-03 | TC-0261, TC-0742, TC-0778 | Not Run |
| DBC-04 | TC-0262 | Not Run |
| DBC-05 | TC-0263 | Not Run |
| DBC-06 | TC-0264 | Not Run |
| DBC-07 | TC-0265 | Not Run |
| DBC-08 | TC-0266 | Not Run |
| DBC-09 | TC-0267 | Not Run |
| DBC-10 | TC-0268 | Not Run |
| DBC-11 | TC-0269 | Not Run |
| DBC-12 | TC-0270 | Not Run |
| DBC-13 | TC-0767 | Not Run |
| DBC-14 | TC-0768 | Not Run |
| DBC-15 | TC-0769 | Not Run |
| DBC-16 | TC-0770 | Not Run |
| DBC-17 | TC-0771 | Not Run |
| DBC-18 | TC-0772 | Not Run |
| DBC-19 | TC-0773 | Not Run |
| DBC-20 | TC-0774 | Not Run |

## 6. Workflow, use case and module trace (carried from v0.1, see note)

### Workflows

| Workflow | Level of detail | Test cases | State |
|---|---|---|---|
| WF-01 | Full | 102 cases | Covered (not executed) |
| WF-02 | Full | 66 cases | Covered (not executed) |
| WF-03 | Full | 89 cases | Covered (not executed) |
| WF-04 | Outline | 23 cases | Covered (not executed) |
| WF-05 | Outline | none | No tests: outline workflow, post-MVP |
| WF-06 | Full | 66 cases | Covered (not executed) |
| WF-07 | Full | 45 cases | Covered (not executed) |
| WF-08 | Outline | none | No tests: outline workflow, post-MVP |
| WF-09 | Outline | none | No tests: outline workflow, post-MVP |
| WF-10 | Outline | none | No tests: outline workflow, post-MVP |
| WF-11 | Outline | none | No tests: outline workflow, post-MVP |
| WF-12 | Full | 18 cases | Covered (not executed) |
| WF-13 | Outline | none | No tests: outline workflow, post-MVP |
| WF-14 | Outline | none | No tests: outline workflow, post-MVP |

### Use cases

| UC | Name | FRs | Functional TCs | State |
|---|---|---|---|---|
| UC-001 | Sign in | 5 | 20 | Covered (not executed) |
| UC-002 | Create a user and assign scope | 5 | 13 | Covered (not executed) |
| UC-003 | Approve a document | 9 | 22 | Covered (not executed) |
| UC-004 | Create and post a manual journal | 4 | 16 | Covered (not executed) |
| UC-005 | Close a period | 3 | 12 | Covered (not executed) |
| UC-006 | Requisition to purchase order | 6 | 16 | Covered (not executed) |
| UC-007 | Receive goods (full or partial) | 3 | 16 | Covered (not executed) |
| UC-008 | Process a vendor invoice with three-way match | 4 | 15 | Covered (not executed) |
| UC-009 | Pay a vendor | 5 | 18 | Covered (not executed) |
| UC-010 | Confirm a sales order | 5 | 11 | Covered (not executed) |
| UC-011 | Deliver and invoice | 3 | 12 | Covered (not executed) |
| UC-012 | Return, credit note and refund | 3 | 8 | Covered (not executed) |
| UC-013 | Transfer stock between warehouses | 1 | 4 | Covered (not executed) |
| UC-014 | Adjust stock | 1 | 4 | Covered (not executed) |
| UC-015 | Import master or opening data | 2 | 9 | Covered (not executed) |
| UC-016 | Delegate approval authority | 1 | 5 | Covered (not executed) |

### MVP modules

| Module | Name | FRs | Functional TCs |
|---|---|---|---|
| M-001 | Dashboard & Analytics | 2 | 3 |
| M-002 | Authentication & Authorization | 7 | 29 |
| M-003 | User Management | 4 | 9 |
| M-004 | Role & Permission Management (RBAC) | 6 | 29 |
| M-005 | Organization & Company Management | 2 | 10 |
| M-006 | Branch & Department Management | 3 | 4 |
| M-007 | Employee Directory | 1 | 2 |
| M-008 | System Configuration | 4 | 10 |
| M-009 | Master Data Management | 2 | 7 |
| M-010 | Audit Logs & Activity Tracking | 3 | 12 |
| M-011 | Document Management | 2 | 5 |
| M-012 | Notification & Communication Management | 5 | 13 |
| M-013 | Workflow & Approval Engine | 9 | 22 |
| M-014 | Import & Export Management | 3 | 12 |
| M-015 | Localization & Language Settings | 1 | 2 |
| M-016 | Employee Management | 3 | 3 |
| M-030 | Finance Management (umbrella) | 1 | 2 |
| M-031 | Chart of Accounts | 3 | 5 |
| M-032 | General Ledger | 2 | 6 |
| M-033 | Accounts Payable | 2 | 9 |
| M-034 | Accounts Receivable | 2 | 9 |
| M-035 | Journal Entries | 4 | 16 |
| M-036 | Bank Account Management | 1 | 1 |
| M-040 | Tax & VAT Management | 2 | 3 |
| M-041 | Multi-Currency Management | 3 | 5 |
| M-043 | Financial Period Management | 3 | 10 |
| M-044 | Fiscal Year Management | 1 | 1 |
| M-045 | Financial Closing | 2 | 8 |
| M-046 | Financial Statements | 3 | 6 |
| M-047 | Payment & Refund Management | 6 | 22 |
| M-049 | Purchase Requisition (PR) | 1 | 4 |
| M-050 | Purchase Request Approval | 1 | 5 |
| M-053 | Purchase Order (PO) | 4 | 11 |
| M-054 | Vendor Management | 3 | 6 |
| M-057 | Goods Receipt Note (GRN) | 2 | 9 |
| M-058 | Purchase Invoice | 3 | 12 |
| M-059 | Vendor Payments | 2 | 11 |
| M-060 | Procurement Reports | 1 | 1 |
| M-061 | Product & Service Catalog | 1 | 2 |
| M-062 | Product Categories | 1 | 1 |
| M-063 | Warehouse Management | 1 | 1 |
| M-064 | Stock Management | 4 | 16 |
| M-065 | Stock Reservation | 1 | 3 |
| M-066 | Stock Transfer | 1 | 4 |
| M-067 | Stock Adjustment | 1 | 4 |
| M-072 | Inventory Valuation | 1 | 6 |
| M-073 | Goods Issue & Goods Receipt | 3 | 11 |
| M-075 | Inventory Reports | 1 | 1 |
| M-079 | Customer Management | 2 | 4 |
| M-080 | Quotation Management | 2 | 4 |
| M-081 | Sales Order Management | 4 | 9 |
| M-082 | Sales Invoice Management | 1 | 5 |
| M-083 | Delivery Management | 1 | 8 |
| M-084 | Sales Returns | 1 | 5 |
| M-085 | Credit Notes | 2 | 6 |
| M-086 | Customer Payments | 1 | 9 |
| M-088 | Sales Reports | 1 | 1 |
| M-116 | Data Governance | 2 | 4 |

### User stories

| US | FRs | Functional TCs | State |
|---|---|---|---|
| US-001 | FR-001, FR-003 | 8 | Covered (not executed) |
| US-002 | FR-002, FR-005 | 9 | Covered (not executed) |
| US-003 | FR-004 | 6 | Covered (not executed) |
| US-004 | FR-006 | 3 | Covered (not executed) |
| US-005 | FR-007 | 3 | Covered (not executed) |
| US-006 | FR-008, FR-010, FR-011 | 6 | Covered (not executed) |
| US-007 | FR-009 | 4 | Covered (not executed) |
| US-008 | FR-012, FR-013, FR-014 | 12 | Covered (not executed) |
| US-009 | FR-015, FR-019, FR-126 | 22 | Covered (not executed) |
| US-010 | FR-016 | 3 | Covered (not executed) |
| US-011 | FR-017 | 4 | Covered (not executed) |
| US-012 | FR-018 | 2 | Covered (not executed) |
| US-013 | FR-020, FR-021 | 2 | Covered (not executed) |
| US-014 | FR-023, FR-024 | 2 | Covered (not executed) |
| US-015 | FR-022 | 2 | Covered (not executed) |
| US-016 | FR-025, FR-026, FR-027 | 8 | Covered (not executed) |
| US-017 | FR-028, FR-029 | 7 | Covered (not executed) |
| US-018 | FR-130 | 2 | Covered (not executed) |
| US-019 | FR-030, FR-031, FR-032 | 12 | Covered (not executed) |
| US-020 | FR-033, FR-034 | 5 | Covered (not executed) |
| US-021 | FR-035, FR-036, FR-037, FR-038 | 11 | Covered (not executed) |
| US-022 | FR-039 | 4 | Covered (not executed) |
| US-023 | FR-040, FR-041, FR-043, FR-046 | 7 | Covered (not executed) |
| US-024 | FR-042 | 4 | Covered (not executed) |
| US-025 | FR-044 | 5 | Covered (not executed) |
| US-026 | FR-045 | 3 | Covered (not executed) |
| US-027 | FR-047 | 1 | Covered (not executed) |
| US-028 | FR-048, FR-049 | 9 | Covered (not executed) |
| US-029 | FR-050 | 6 | Covered (not executed) |
| US-030 | FR-051 | 2 | Covered (not executed) |
| US-031 | FR-052 | 2 | Covered (not executed) |
| US-032 | FR-053, FR-054, FR-055, FR-085 | 7 | Covered (not executed) |
| US-033 | FR-056, FR-057 | 2 | Covered (not executed) |
| US-034 | FR-058, FR-059, FR-060 | 12 | Covered (not executed) |
| US-035 | FR-061 | 4 | Covered (not executed) |
| US-036 | FR-062 | 7 | Covered (not executed) |
| US-037 | FR-063 | 4 | Covered (not executed) |
| US-038 | FR-064, FR-065 | 7 | Covered (not executed) |
| US-039 | FR-066 | 1 | Covered (not executed) |
| US-040 | FR-067, FR-068, FR-069 | 5 | Covered (not executed) |
| US-041 | FR-070, FR-071 | 3 | Covered (not executed) |
| US-042 | FR-072 | 1 | Covered (not executed) |
| US-043 | FR-073, FR-074 | 11 | Covered (not executed) |
| US-044 | FR-075, FR-076 | 8 | Covered (not executed) |
| US-045 | FR-077, FR-078 | 6 | Covered (not executed) |
| US-046 | FR-082, FR-083, FR-084 | 6 | Covered (not executed) |
| US-047 | FR-079, FR-080, FR-081 | 12 | Covered (not executed) |
| US-048 | FR-086, FR-087, FR-088 | 6 | Covered (not executed) |
| US-049 | FR-089 | 4 | Covered (not executed) |
| US-050 | FR-090 | 5 | Covered (not executed) |
| US-051 | FR-091 | 4 | Covered (not executed) |
| US-052 | FR-092 | 5 | Covered (not executed) |
| US-053 | FR-093, FR-094 | 4 | Covered (not executed) |
| US-054 | FR-095 | 7 | Covered (not executed) |
| US-055 | FR-096 | 2 | Covered (not executed) |
| US-056 | FR-097, FR-098 | 8 | Covered (not executed) |
| US-057 | FR-099 | 7 | Covered (not executed) |
| US-058 | FR-100 | 1 | Covered (not executed) |
| US-059 | FR-101, FR-102 | 3 | Covered (not executed) |
| US-060 | FR-103 | 1 | Covered (not executed) |
| US-061 | FR-104, FR-105 | 9 | Covered (not executed) |
| US-062 | FR-106, FR-113 | 7 | Covered (not executed) |
| US-063 | FR-108 | 4 | Covered (not executed) |
| US-064 | FR-109 | 4 | Covered (not executed) |
| US-065 | FR-110 | 1 | Covered (not executed) |
| US-066 | FR-111 | 6 | Covered (not executed) |
| US-067 | FR-112 | 1 | Covered (not executed) |
| US-068 | FR-114 | 2 | Covered (not executed) |
| US-069 | FR-116, FR-117 | 4 | Covered (not executed) |
| US-070 | FR-115, FR-118, FR-107 | 9 | Covered (not executed) |
| US-071 | FR-119 | 3 | Covered (not executed) |
| US-072 | FR-120 | 8 | Covered (not executed) |
| US-073 | FR-121 | 5 | Covered (not executed) |
| US-074 | FR-122 | 5 | Covered (not executed) |
| US-075 | FR-123 | 5 | Covered (not executed) |
| US-076 | FR-124 | 1 | Covered (not executed) |
| US-077 | FR-125 | 2 | Covered (not executed) |
| US-078 | FR-127 | 3 | Covered (not executed) |
| US-079 | FR-129 | 2 | Covered (not executed) |
| US-080 | FR-128 | 4 | Covered (not executed) |

## 7. API trace (carried from v0.1, see note)

An endpoint is counted as covered when at least one of these exists: a permission test for its permission code, an error-code test for one of its error codes, or a functional test for one of its FRs. Endpoints with `authenticated` or `public` permission rely on FR tests and error-code tests.

| API | Method and path | FRs | Permission TC | Error TCs | Functional TCs via FR | State |
|---|---|---|---|---|---|---|
| API-001 | POST /api/v1/auth/login | FR-001, FR-003 | n/a | TC-0439, TC-0440, TC-0441 | 8 | Covered (not executed) |
| API-002 | POST /api/v1/auth/mfa/verify | FR-006 | n/a | TC-0442 | 3 | Covered (not executed) |
| API-003 | POST /api/v1/auth/refresh | FR-004 | n/a | TC-0443 | 6 | Covered (not executed) |
| API-004 | POST /api/v1/auth/logout | FR-004 | n/a | none | 6 | Covered (not executed) |
| API-005 | POST /api/v1/auth/password/forgot | FR-005 | n/a | none | 5 | Covered (not executed) |
| API-006 | POST /api/v1/auth/password/reset | FR-002, FR-005 | n/a | TC-0444, TC-0481 | 9 | Covered (not executed) |
| API-007 | GET /api/v1/auth/sso/{provider}/callback | FR-007 | n/a | TC-0439 | 3 | Covered (not executed) |
| API-008 | GET /api/v1/auth/sessions | FR-004 | n/a | none | 6 | Covered (not executed) |
| API-009 | DELETE /api/v1/auth/sessions/{id} | FR-004 | TC-0346 | TC-0445 | 6 | Covered (not executed) |
| API-010 | GET /api/v1/users | FR-008 | TC-0350 | none | 4 | Covered (not executed) |
| API-011 | POST /api/v1/users | FR-008, FR-010 | TC-0348 | TC-0481, TC-0446, TC-0477 | 5 | Covered (not executed) |
| API-012 | GET /api/v1/users/{id} | FR-008 | TC-0350 | TC-0463 | 4 | Covered (not executed) |
| API-013 | PATCH /api/v1/users/{id} | FR-008, FR-125 | TC-0351 | TC-0447 | 6 | Covered (not executed) |
| API-014 | POST /api/v1/users/{id}/deactivate | FR-009 | TC-0349 | none | 4 | Covered (not executed) |
| API-015 | POST /api/v1/users/{id}/employee-link | FR-011 | TC-0347 | TC-0446 | 1 | Covered (not executed) |
| API-016 | POST /api/v1/users/{id}/role-assignments | FR-010, FR-016, FR-017 | TC-0341 | TC-0477, TC-0481 | 9 | Covered (not executed) |
| API-017 | DELETE /api/v1/users/{id}/role-assignments/{aid} | FR-016 | TC-0341 | none | 3 | Covered (not executed) |
| API-018 | GET /api/v1/roles | FR-012 | TC-0344 | none | 6 | Covered (not executed) |
| API-019 | POST /api/v1/roles | FR-012 | TC-0342 | TC-0481 | 6 | Covered (not executed) |
| API-020 | PATCH /api/v1/roles/{id} | FR-012 | TC-0345 | TC-0447, TC-0479 | 6 | Covered (not executed) |
| API-021 | POST /api/v1/roles/{id}/retire | FR-012 | TC-0343 | TC-0479 | 6 | Covered (not executed) |
| API-022 | GET /api/v1/permissions | FR-013 | TC-0344 | none | 3 | Covered (not executed) |
| API-023 | GET /api/v1/companies | FR-018 | n/a | none | 2 | Covered (not executed) |
| API-024 | POST /api/v1/companies | FR-018 | TC-0369 | TC-0481 | 2 | Covered (not executed) |
| API-025 | PATCH /api/v1/companies/{id} | FR-018 | TC-0370 | TC-0479, TC-0447 | 2 | Covered (not executed) |
| API-026 | GET /api/v1/branches | FR-020 | n/a | none | 1 | Covered (not executed) |
| API-027 | POST /api/v1/branches | FR-020 | TC-0367 | TC-0481 | 1 | Covered (not executed) |
| API-028 | PATCH /api/v1/branches/{id} | FR-020 | TC-0368 | TC-0479, TC-0447 | 1 | Covered (not executed) |
| API-029 | GET /api/v1/departments | FR-020 | n/a | none | 1 | Covered (not executed) |
| API-030 | POST /api/v1/departments | FR-020 | TC-0373 | TC-0481 | 1 | Covered (not executed) |
| API-031 | GET /api/v1/cost-centres | FR-021 | TC-0372 | none | 1 | Covered (not executed) |
| API-032 | POST /api/v1/cost-centres | FR-021 | TC-0371 | TC-0481 | 1 | Covered (not executed) |
| API-033 | GET /api/v1/employees | FR-022, FR-023 | n/a | none | 3 | Covered (not executed) |
| API-034 | POST /api/v1/employees | FR-023, FR-024 | TC-0374 | TC-0481, TC-0446 | 2 | Covered (not executed) |
| API-035 | PATCH /api/v1/employees/{id} | FR-023, FR-024 | TC-0375 | TC-0447, TC-0481 | 2 | Covered (not executed) |
| API-036 | GET /api/v1/number-series | FR-025 | TC-0389 | none | 6 | Covered (not executed) |
| API-037 | PUT /api/v1/number-series/{id} | FR-025 | TC-0388 | TC-0481 | 6 | Covered (not executed) |
| API-038 | GET /api/v1/settings | FR-027 | TC-0393 | none | 1 | Covered (not executed) |
| API-039 | PUT /api/v1/settings/{key} | FR-027 | TC-0392 | TC-0481 | 1 | Covered (not executed) |
| API-040 | GET /api/v1/master-data/{type} | FR-028 | TC-0387 | none | 3 | Covered (not executed) |
| API-041 | POST /api/v1/master-data/{type} | FR-028, FR-029 | TC-0386 | TC-0481, TC-0446 | 7 | Covered (not executed) |
| API-042 | POST /api/v1/attachments | FR-033 | TC-0376 | TC-0449, TC-0450, TC-0445 | 3 | Covered (not executed) |
| API-043 | GET /api/v1/attachments/{id}/download | FR-034 | n/a | TC-0445, TC-0463 | 2 | Covered (not executed) |
| API-044 | GET /api/v1/notifications | FR-035 | n/a | none | 3 | Covered (not executed) |
| API-045 | POST /api/v1/notifications/{id}/read | FR-035 | n/a | none | 3 | Covered (not executed) |
| API-046 | PUT /api/v1/notification-preferences | FR-038 | n/a | none | 3 | Covered (not executed) |
| API-047 | POST /api/v1/imports | FR-048 | TC-0383 | TC-0449, TC-0481 | 6 | Covered (not executed) |
| API-048 | POST /api/v1/imports/{id}/commit | FR-049 | TC-0383 | TC-0479, TC-0481 | 5 | Covered (not executed) |
| API-049 | GET /api/v1/imports/{id}/errors | FR-048 | TC-0384 | none | 6 | Covered (not executed) |
| API-050 | POST /api/v1/exports | FR-050 | TC-0382 | TC-0445 | 6 | Covered (not executed) |
| API-051 | GET /api/v1/audit-logs | FR-031 | TC-0378 | none | 5 | Covered (not executed) |
| API-052 | POST /api/v1/audit-logs/export | FR-031 | TC-0377 | TC-0445 | 5 | Covered (not executed) |
| API-053 | GET /api/v1/integration-queue | FR-128 | TC-0385 | none | 4 | Covered (not executed) |
| API-054 | POST /api/v1/integration-queue/{id}/retry | FR-128 | TC-0385 | TC-0479 | 4 | Covered (not executed) |
| API-055 | GET /api/v1/retention-policies | FR-127 | TC-0391 | none | 3 | Covered (not executed) |
| API-056 | POST /api/v1/privacy/anonymisation-requests | FR-129 | TC-0390 | TC-0469, TC-0479 | 2 | Covered (not executed) |
| API-057 | GET /api/v1/dashboard | FR-051 | n/a | none | 2 | Covered (not executed) |
| API-058 | GET /api/v1/approval-chains | FR-039 | TC-0436 | none | 4 | Covered (not executed) |
| API-059 | POST /api/v1/approval-chains | FR-039 | TC-0435 | TC-0481 | 4 | Covered (not executed) |
| API-060 | PATCH /api/v1/approval-chains/{id} | FR-039 | TC-0437 | TC-0447 | 4 | Covered (not executed) |
| API-061 | GET /api/v1/approvals | FR-047 | n/a | none | 1 | Covered (not executed) |
| API-062 | POST /api/v1/approvals/{id}/decisions | FR-041, FR-042, FR-043 | TC-0434 | TC-0478, TC-0481, TC-0479, TC-0445 | 8 | Covered (not executed) |
| API-063 | POST /api/v1/delegations | FR-044 | TC-0438 | TC-0481, TC-0478 | 5 | Covered (not executed) |
| API-064 | DELETE /api/v1/delegations/{id} | FR-044 | TC-0438 | none | 5 | Covered (not executed) |
| API-065 | GET /api/v1/accounts | FR-053 | TC-0314 | none | 3 | Covered (not executed) |
| API-066 | POST /api/v1/accounts | FR-053 | TC-0312 | TC-0481, TC-0446 | 3 | Covered (not executed) |
| API-067 | PATCH /api/v1/accounts/{id} | FR-053, FR-055 | TC-0315 | TC-0455, TC-0447 | 4 | Covered (not executed) |
| API-068 | POST /api/v1/accounts/template-load | FR-054 | TC-0313 | TC-0479 | 1 | Covered (not executed) |
| API-069 | GET /api/v1/posting-rules | FR-085 | TC-0337 | none | 2 | Covered (not executed) |
| API-070 | PUT /api/v1/posting-rules/{id} | FR-085 | TC-0336 | TC-0481 | 2 | Covered (not executed) |
| API-071 | POST /api/v1/fiscal-years | FR-056, FR-057 | TC-0333 | TC-0481 | 2 | Covered (not executed) |
| API-072 | GET /api/v1/fiscal-periods | FR-057 | TC-0335 | none | 1 | Covered (not executed) |
| API-073 | POST /api/v1/fiscal-periods/{id}/soft-close | FR-058 | TC-0334 | TC-0479 | 5 | Covered (not executed) |
| API-074 | POST /api/v1/fiscal-periods/{id}/close | FR-058, FR-060 | TC-0334 | TC-0453, TC-0454 | 8 | Covered (not executed) |
| API-075 | POST /api/v1/fiscal-periods/{id}/reopen | FR-059 | TC-0334 | TC-0481, TC-0479 | 4 | Covered (not executed) |
| API-076 | GET /api/v1/fiscal-periods/{id}/close-checklist | FR-060 | TC-0335 | none | 4 | Covered (not executed) |
| API-077 | POST /api/v1/fiscal-years/{id}/close | FR-061 | TC-0334 | TC-0453, TC-0454 | 4 | Covered (not executed) |
| API-078 | GET /api/v1/journals | FR-062 | TC-0323 | none | 7 | Covered (not executed) |
| API-079 | POST /api/v1/journals | FR-062 | TC-0321 | TC-0451, TC-0481 | 7 | Covered (not executed) |
| API-080 | PATCH /api/v1/journals/{id} | FR-062, FR-125 | TC-0325 | TC-0447, TC-0479 | 9 | Covered (not executed) |
| API-081 | POST /api/v1/journals/{id}/submit | FR-062, FR-063 | TC-0324 | TC-0451, TC-0452 | 9 | Covered (not executed) |
| API-082 | POST /api/v1/journals/{id}/post | FR-064 | TC-0320 | TC-0452, TC-0479 | 5 | Covered (not executed) |
| API-083 | POST /api/v1/journals/{id}/reverse | FR-065 | TC-0322 | TC-0452, TC-0480 | 2 | Covered (not executed) |
| API-084 | GET /api/v1/ledger-entries | FR-066 | TC-0326 | none | 1 | Covered (not executed) |
| API-085 | GET /api/v1/currency-rates | FR-067 | TC-0319 | none | 1 | Covered (not executed) |
| API-086 | POST /api/v1/currency-rates | FR-067 | TC-0318 | TC-0481, TC-0446 | 1 | Covered (not executed) |
| API-087 | GET /api/v1/tax-codes | FR-070 | TC-0340 | none | 1 | Covered (not executed) |
| API-088 | POST /api/v1/tax-codes | FR-070 | TC-0339 | TC-0481, TC-0446 | 1 | Covered (not executed) |
| API-089 | GET /api/v1/bank-accounts | FR-072 | TC-0317 | none | 1 | Covered (not executed) |
| API-090 | POST /api/v1/bank-accounts | FR-072 | TC-0316 | TC-0481, TC-0446 | 1 | Covered (not executed) |
| API-091 | GET /api/v1/payments | FR-073 | TC-0331 | none | 9 | Covered (not executed) |
| API-092 | POST /api/v1/payments | FR-073, FR-074, FR-076 | TC-0329 | TC-0456, TC-0481, TC-0458 | 15 | Covered (not executed) |
| API-093 | POST /api/v1/payments/{id}/submit | FR-075 | TC-0332 | TC-0479 | 4 | Covered (not executed) |
| API-094 | POST /api/v1/payments/{id}/release | FR-075, FR-076 | TC-0328 | TC-0478, TC-0479, TC-0448 | 8 | Covered (not executed) |
| API-095 | POST /api/v1/payments/{id}/reverse | FR-077 | TC-0330 | TC-0480, TC-0452 | 2 | Covered (not executed) |
| API-096 | POST /api/v1/refunds | FR-078 | TC-0329 | TC-0457, TC-0481 | 4 | Covered (not executed) |
| API-097 | GET /api/v1/open-items | FR-081 | TC-0327 | none | 5 | Covered (not executed) |
| API-098 | GET /api/v1/reports/trial-balance | FR-082 | TC-0338 | none | 4 | Covered (not executed) |
| API-099 | GET /api/v1/reports/income-statement | FR-083 | TC-0338 | none | 2 | Covered (not executed) |
| API-100 | GET /api/v1/reports/balance-sheet | FR-084 | TC-0338 | none | 3 | Covered (not executed) |
| API-101 | GET /api/v1/vendors | FR-086 | TC-0416 | none | 3 | Covered (not executed) |
| API-102 | POST /api/v1/vendors | FR-086, FR-029 | TC-0414 | TC-0481, TC-0446 | 7 | Covered (not executed) |
| API-103 | PATCH /api/v1/vendors/{id} | FR-086, FR-125 | TC-0417 | TC-0447 | 5 | Covered (not executed) |
| API-104 | POST /api/v1/vendors/{id}/activate | FR-086 | TC-0413 | TC-0478, TC-0479 | 3 | Covered (not executed) |
| API-105 | POST /api/v1/vendors/{id}/block | FR-087 | TC-0415 | none | 1 | Covered (not executed) |
| API-106 | POST /api/v1/vendors/{id}/bank-accounts | FR-088 | TC-0417 | TC-0478 | 3 | Covered (not executed) |
| API-107 | POST /api/v1/purchase-requisitions | FR-089 | TC-0408 | TC-0481 | 4 | Covered (not executed) |
| API-108 | PATCH /api/v1/purchase-requisitions/{id} | FR-089, FR-125 | TC-0411 | TC-0447, TC-0479 | 6 | Covered (not executed) |
| API-109 | POST /api/v1/purchase-requisitions/{id}/submit | FR-090 | TC-0410 | TC-0479 | 5 | Covered (not executed) |
| API-110 | POST /api/v1/purchase-requisitions/{id}/convert | FR-091 | TC-0403 | TC-0479, TC-0464 | 4 | Covered (not executed) |
| API-111 | GET /api/v1/purchase-requisitions | FR-089 | TC-0409 | none | 4 | Covered (not executed) |
| API-112 | POST /api/v1/purchase-orders | FR-091 | TC-0403 | TC-0481, TC-0464 | 4 | Covered (not executed) |
| API-113 | PATCH /api/v1/purchase-orders/{id} | FR-091, FR-125 | TC-0407 | TC-0447 | 6 | Covered (not executed) |
| API-114 | POST /api/v1/purchase-orders/{id}/submit | FR-092 | TC-0406 | TC-0479 | 5 | Covered (not executed) |
| API-115 | POST /api/v1/purchase-orders/{id}/send | FR-093 | TC-0404 | TC-0479, TC-0448 | 1 | Covered (not executed) |
| API-116 | POST /api/v1/purchase-orders/{id}/revise | FR-093 | TC-0407 | TC-0479 | 1 | Covered (not executed) |
| API-117 | POST /api/v1/purchase-orders/{id}/cancel | FR-094 | TC-0402 | TC-0479 | 3 | Covered (not executed) |
| API-118 | GET /api/v1/purchase-orders | FR-091 | TC-0405 | none | 4 | Covered (not executed) |
| API-119 | POST /api/v1/goods-receipts | FR-095, FR-104 | TC-0396 | TC-0465, TC-0460, TC-0466 | 14 | Covered (not executed) |
| API-120 | POST /api/v1/goods-receipts/{id}/reverse | FR-096 | TC-0397 | TC-0459, TC-0480 | 2 | Covered (not executed) |
| API-121 | POST /api/v1/purchase-invoices | FR-097, FR-099 | TC-0399 | TC-0467, TC-0481 | 9 | Covered (not executed) |
| API-122 | POST /api/v1/purchase-invoices/{id}/match | FR-098 | TC-0401 | TC-0468 | 7 | Covered (not executed) |
| API-123 | POST /api/v1/purchase-invoices/{id}/submit | FR-098 | TC-0400 | TC-0468, TC-0479 | 7 | Covered (not executed) |
| API-124 | POST /api/v1/purchase-invoices/{id}/post | FR-079 | TC-0398 | TC-0451, TC-0452, TC-0479 | 4 | Covered (not executed) |
| API-125 | GET /api/v1/reports/procurement/open-pos | FR-100 | TC-0412 | none | 1 | Covered (not executed) |
| API-126 | GET /api/v1/reports/procurement/spend | FR-100 | TC-0412 | none | 1 | Covered (not executed) |
| API-127 | GET /api/v1/items | FR-101 | TC-0358 | none | 2 | Covered (not executed) |
| API-128 | POST /api/v1/items | FR-101, FR-029 | TC-0357 | TC-0481, TC-0446 | 6 | Covered (not executed) |
| API-129 | PATCH /api/v1/items/{id} | FR-101, FR-125 | TC-0359 | TC-0462, TC-0447 | 4 | Covered (not executed) |
| API-130 | GET /api/v1/item-categories | FR-102 | TC-0355 | none | 1 | Covered (not executed) |
| API-131 | POST /api/v1/item-categories | FR-102 | TC-0354 | TC-0481 | 1 | Covered (not executed) |
| API-132 | GET /api/v1/warehouses | FR-103 | TC-0366 | none | 1 | Covered (not executed) |
| API-133 | POST /api/v1/warehouses | FR-103 | TC-0365 | TC-0481 | 1 | Covered (not executed) |
| API-134 | GET /api/v1/stock/balances | FR-105 | TC-0361 | none | 1 | Covered (not executed) |
| API-135 | GET /api/v1/stock/movements | FR-104 | TC-0361 | none | 8 | Covered (not executed) |
| API-136 | POST /api/v1/stock-transfers | FR-108 | TC-0362 | TC-0481 | 4 | Covered (not executed) |
| API-137 | POST /api/v1/stock-transfers/{id}/dispatch | FR-108 | TC-0363 | TC-0459, TC-0479 | 4 | Covered (not executed) |
| API-138 | POST /api/v1/stock-transfers/{id}/receive | FR-108 | TC-0363 | TC-0479, TC-0461 | 4 | Covered (not executed) |
| API-139 | POST /api/v1/stock-adjustments | FR-109 | TC-0352 | TC-0481 | 4 | Covered (not executed) |
| API-140 | POST /api/v1/stock-adjustments/{id}/submit | FR-109 | TC-0353 | TC-0459, TC-0479 | 4 | Covered (not executed) |
| API-141 | POST /api/v1/stock-documents | FR-110 | TC-0356 | TC-0459, TC-0452 | 1 | Covered (not executed) |
| API-142 | POST /api/v1/valuation/runs | FR-111 | TC-0364 | TC-0452, TC-0455 | 6 | Covered (not executed) |
| API-143 | GET /api/v1/reports/inventory/stock-on-hand | FR-112 | TC-0360 | none | 1 | Covered (not executed) |
| API-144 | GET /api/v1/reports/inventory/valuation | FR-112 | TC-0360 | none | 1 | Covered (not executed) |
| API-145 | GET /api/v1/customers | FR-114 | TC-0420 | none | 2 | Covered (not executed) |
| API-146 | POST /api/v1/customers | FR-114, FR-029 | TC-0419 | TC-0481, TC-0446 | 6 | Covered (not executed) |
| API-147 | PATCH /api/v1/customers/{id} | FR-114, FR-125 | TC-0421 | TC-0447 | 4 | Covered (not executed) |
| API-148 | POST /api/v1/quotations | FR-116 | TC-0429 | TC-0481 | 3 | Covered (not executed) |
| API-149 | POST /api/v1/quotations/{id}/send | FR-116 | TC-0430 | TC-0479 | 3 | Covered (not executed) |
| API-150 | POST /api/v1/quotations/{id}/convert | FR-117 | TC-0426 | TC-0470, TC-0479 | 2 | Covered (not executed) |
| API-151 | POST /api/v1/sales-orders | FR-118 | TC-0426 | TC-0481, TC-0471 | 6 | Covered (not executed) |
| API-152 | POST /api/v1/sales-orders/{id}/confirm | FR-115, FR-118, FR-107 | TC-0428 | TC-0472, TC-0459 | 9 | Covered (not executed) |
| API-153 | POST /api/v1/sales-orders/{id}/cancel | FR-119 | TC-0425 | TC-0479 | 3 | Covered (not executed) |
| API-154 | GET /api/v1/sales-orders | FR-118 | TC-0427 | none | 6 | Covered (not executed) |
| API-155 | POST /api/v1/deliveries | FR-120 | TC-0422 | TC-0473, TC-0459 | 8 | Covered (not executed) |
| API-156 | POST /api/v1/sales-invoices | FR-121 | TC-0424 | TC-0474, TC-0481 | 5 | Covered (not executed) |
| API-157 | POST /api/v1/sales-invoices/{id}/post | FR-080 | TC-0423 | TC-0451, TC-0452 | 4 | Covered (not executed) |
| API-158 | POST /api/v1/sales-returns | FR-122 | TC-0432 | TC-0475 | 5 | Covered (not executed) |
| API-159 | POST /api/v1/sales-returns/{id}/receive | FR-122 | TC-0433 | TC-0479 | 5 | Covered (not executed) |
| API-160 | POST /api/v1/credit-notes | FR-123 | TC-0418 | TC-0476, TC-0452 | 5 | Covered (not executed) |
| API-161 | GET /api/v1/reports/sales/summary | FR-124 | TC-0431 | none | 1 | Covered (not executed) |
| API-162 | GET /api/v1/reports/sales/receivable-ageing | FR-124, FR-081 | TC-0431 | none | 6 | Covered (not executed) |
| API-163 | GET /api/v1/document-templates | FR-026 | TC-0395 | none | 1 | Covered (not executed) |
| API-164 | PUT /api/v1/document-templates/{id} | FR-026 | TC-0394 | TC-0481, TC-0447 | 1 | Covered (not executed) |
| API-165 | GET /api/v1/me/preferences | FR-052, FR-038 | n/a | none | 5 | Covered (not executed) |
| API-166 | PUT /api/v1/me/preferences | FR-052 | n/a | TC-0481 | 2 | Covered (not executed) |
| API-167 | GET /api/v1/custom-fields | FR-130 | TC-0380 | none | 2 | Covered (not executed) |
| API-168 | POST /api/v1/custom-fields | FR-130 | TC-0379 | TC-0481, TC-0446 | 2 | Covered (not executed) |
| API-169 | PATCH /api/v1/custom-fields/{id} | FR-130 | TC-0381 | TC-0447, TC-0479 | 2 | Covered (not executed) |

Note: sections 6 and 7 are carried from v0.1 and were not regenerated in Phase 2. The additions made in v0.2 are summarised in section 9. Section 8 below is recomputed.

## 8. Coverage and gap analysis

Recomputed by script on 2026-11-11 after remediation. Original v0.1 figures are in the evidence copy.

### 8.1 Coverage figures

| Item | Total | With at least one test case | Coverage | Notes |
|---|---|---|---|---|
| Functional requirements (FR) | 150 | 150 | 100.0 percent | Direct FR ID only |
| Non-functional requirements (NFR) | 42 | 42 | 100.0 percent | |
| Technical requirements (TR) | 18 | 18 | 100.0 percent | |
| Security requirements (SEC) | 73 | 73 | 100.0 percent | |
| Threats (THR) | 59 | 59 | 100.0 percent | 21 with a test that names the threat, 38 only through mitigation SEC tests. Without any test: none |
| System invariants (INV) | 15 | 15 | 100.0 percent | |
| Database constraints (DBC) | 20 | 20 | 100.0 percent | |
| Business requirements (BR) | 63 | 47 | 74.6 percent | BRs with no FR are post-MVP, except BR-062 which is covered by NFRs |
| User stories (US) | 101 | 101 | 100.0 percent | Through their FRs |
| Test cases | 1038 | 0 executed | 0.0 percent executed | All Not Run |

### 8.2 Items requested by the QA brief

| Check | Result |
|---|---|
| FRs without a user story | 0 |
| FRs without a direct test case | 0 |
| FRs lacking a use case | 91 of 150 |
| FRs lacking an API endpoint | 17 of 150 |
| FRs with exactly one direct test case | 0 |
| NFR links in section 3A that are Removed or Corrected | 15 link changes against 23 original links |
| Test cases naming no FR, BR, NFR, TR, API or SEC | 42 |

### 8.3 Limits of these numbers

1. Coverage here is **traceability coverage**, not test results and not code coverage.
2. A test case existing for a requirement does not mean the case is adequate. 0 FRs have a single direct case (recomputed).
3. API coverage counts an endpoint as covered when its permission, an error code or an FR is tested. Detailed schema tests per endpoint wait for the full API schemas (PG-12).
4. NFR coverage is partial in substance: targets for NFR-001 to NFR-011 are placeholders, so those cases cannot pass or fail until numbers are agreed.
5. Test priority is inflated: P0 share is recomputed in the QA report. See Test Strategy section 5.
6. All test data is hypothetical until the business supplies reference sets (Finance sign-off).

### 8.4 Known tensions between requirements (not proven conflicts)

| Tension | Requirements | Needed decision |
|---|---|---|
| Hard SoD conflicts versus small companies with few staff | FR-017, BRL-057, BRL-058 | SoD override policy (OQ-032). Tests assume overrides are recorded and reported |
| Lock on submit versus edit after approval restarts approval | FR-040, FR-092 | Accepted: a submitted document is locked, an approved PO may be edited only by restarting approval. Confirm in review |
| Gap-free numbering versus throughput | FR-025, ADR-012 | OQ-031. Test TC for numbering accepts either policy |
| Negative stock default | FR-106, BRL-040 | OQ-020 |

### 8.5 Gaps to close

| ID | Gap | Action |
|---|---|---|
| TG-01 | No test has been executed | Execute per Test Plan |
| TG-02 | NFR numeric targets unknown | OQ-015, OQ-028 |
| TG-03 | 7 outline workflows and R2 to R4 modules have no tests | Write with each release |
| TG-04 | Test cases for UI behaviour beyond four cases | Extend now that the UI/UX specifications exist |
| TG-05 | No automated script exists for any case | Build after tool choice (OQ-024) |
| TG-06 | Cases written from Proposed rules; business may change rules | Review cases after rule approval |
| TG-07 | Priority inflation | Re-triage after cycle 1 |



## 9. v0.2 additions

- FR-131 to FR-150 (20 FRs), US-081 to US-098 (18 stories), API-170 to API-188 (19 endpoints), SEC-054 to SEC-073 (20), THR-045 to THR-059 (15), AUD-101 to AUD-127, BRL-066 to BRL-074, SOD-14 to SOD-32, TC-0554 onward.
- Findings fixed in the RTM: the substring match defect (14 same-number NFR links, 41 unsupported FR to test links), see the RTM Correction Report.
