# Open Questions

Document status: Living document. Started Stage 1. v0.2 (Phase 2): OQ-033 to OQ-038 added and a classification section added at the end. No question was answered, so every Status stays Open. v0.2 (Phase 2): OQ-033 to OQ-038 added and a classification section added at the end. No question was answered, so every Status stays Open.
Nothing in the package is confirmed until the question that governs it is answered here.
Owner column is a proposed role.

| ID | Question | Why it matters | Blocks | Proposed owner | Status |
|---|---|---|---|---|---|
| OQ-001 | Who is the executive sponsor and who can approve scope, priority and exceptions? | Every sign-off and priority call. | All approvals | Programme sponsor | Open |
| OQ-002 | Which industry or industries will use the first release? Is there a named first customer? | Decides which industry modules (manufacturing, projects, POS, and others) move forward. | M-089 to M-109, section 5 of inventory | Sponsor, Product manager | Open |
| OQ-003 | Is the proposed MVP (58 modules) accepted, or does capacity force a smaller first release? | Defines release 1. | MVP-Strategy | Sponsor | Open |
| OQ-004 | Are the P0, P1, P2 assignments accepted? | Drives release plan. | Module-Prioritization | Sponsor, Product manager | Open |
| OQ-005 | Is this a single-customer system or a product sold to many customers (SaaS)? | Tenancy, pricing, support model and release process. | ARD, ADR on tenancy | Sponsor | Open |
| OQ-006 | Which countries and payroll rules apply (tax slabs, social contribution, pay cycles)? | Payroll cannot be specified without them. | M-023, M-024 | HR manager | Open |
| OQ-007 | Which tax regimes apply (VAT, GST, withholding)? Are tax-inclusive prices used? | Invoice and ledger correctness. | M-040 | Finance manager | Open |
| OQ-008 | What are the base currencies, which foreign currencies are used, and what is the exchange-rate source? | Multi-currency design. | M-041 | Finance manager | Open |
| OQ-009 | Which inventory valuation methods are required (FIFO, weighted average, standard cost)? | Ledger postings and COGS. | M-072 | Finance manager | Open |
| OQ-010 | Where must data be stored (country or region) and which privacy laws apply? | Hosting, DPA, consent. | M-116, deployment | Compliance officer | Open |
| OQ-011 | Which languages and scripts (including right-to-left) are needed at launch? | UI and document templates. | M-015 | Product manager | Open |
| OQ-012 | What systems hold data today, how much, and how far back must history be migrated? | Migration scope and effort. | Data migration strategy | Implementation lead | Open |
| OQ-013 | What approval thresholds and authority limits apply per document type? | Approval engine defaults. | M-013 | Finance manager | Open |
| OQ-014 | What are the budget, team size, and target dates? | Sprint plan and risk. | Roadmap, Sprint-Plan | Sponsor | Open |
| OQ-015 | Expected users, companies, branches and daily transaction volumes? | Sizing, architecture choice. | NFR, ADR on modular monolith | IT manager | Open |
| OQ-016 | Hosting preference (public cloud provider, private, on-premises) and required certifications? | Infrastructure design. | Deployment architecture | IT manager | Open |
| OQ-017 | Which external systems must integrate (banks, payment gateways, email, SMS, e-invoicing, e-commerce)? | Integration scope. | Integration architecture | IT manager | Open |
| OQ-018 | Which accounting framework applies (IFRS, local GAAP) and is there a standard chart of accounts? | Statement structure. | M-031, M-046 | Finance manager | Open |
| OQ-019 | Does each customer get its own isolated tenant holding many companies, or do all companies share one tenant? | Data isolation model. | ARD | Architect, Sponsor | Open |
| OQ-020 | Is negative stock ever allowed? By item, warehouse or company? | Stock rule BRL-040. | M-064 | Inventory manager | Open |
| OQ-021 | What is the fiscal calendar (start month, 4-4-5 weeks, and so on)? | Period setup. | M-043, M-044 | Finance manager | Open |
| OQ-022 | What legal retention periods apply per record type? | Archival and deletion rules. | Data-Retention-Policy | Compliance officer | Open |
| OQ-023 | Is multi-factor authentication mandatory? Is single sign-on with a corporate identity provider required? | Authentication design. | M-002 | Security officer | Open |
| OQ-024 | Are there technology constraints or preferences (languages, database, cloud, team skills)? | Stack evaluation. | Technology stack evaluation | IT manager | Open |
| OQ-025 | Are native mobile apps needed, or is responsive web enough? Is offline use needed (warehouse)? | Client architecture. | UI/UX, ARD | Product manager | Open |
| OQ-026 | Should approval delegation allow chains of delegation, and for how long? | BRL on delegation. | M-013 | Finance manager | Open |
| OQ-027 | Are there existing branding, document templates and legal entity details to reuse? | Print and PDF output. | M-008 | Product manager | Open |
| OQ-028 | What are the required service levels (availability, RPO, RTO, support hours)? | NFR and operations. | NFR, DR plan | Sponsor, IT manager | Open |
| OQ-029 | Who performs UAT for each business area and are they available? | UAT plan. | UAT-Test-Cases | Programme manager | Open |
| OQ-030 | Is an existing system to be retired, and when? | Cut-over plan. | Data migration, roadmap | Sponsor | Open |
| OQ-031 | Must document numbers be gap-free (legal or audit rule), or may gaps exist after rollbacks or cancellations? | Numbering design and posting throughput (ADR-012). | M-008, FR-025 | Finance manager, Compliance officer | Open |
| OQ-032 | In small companies one person may need two conflicting duties. Is a recorded, time-limited segregation-of-duties override acceptable, and who approves it? Which role may reopen a closed period (a higher authority than the Finance Manager)? | RBAC SoD policy and period reopen (BRL-022). | RBAC-Matrix, FR-017, FR-059 | Sponsor, Finance manager, Security officer | Open |
| OQ-033 | May a second Organization Admin of another company approve role grants instead of a Super Admin? | Grant approval workload and who can approve. | RBAC-Matrix 8A.2 | Security officer, Sponsor | Open |
| OQ-034 | What are the payment value limits A and B, is there a role above Finance Manager (for example Finance Director), and may the Organization Admin approve a period reopen? | Multi-level approval and reopen approver. | RBAC-Matrix 8A.4, 8A.5 | Sponsor, Finance manager | Open |
| OQ-035 | What hold period applies to the first payment to a changed vendor bank account, and how is out-of-band verification done? | Fraud control strength against operating cost. | FR-139 | Finance manager, Security officer | Open |
| OQ-036 | What duration, custody and notification apply to break-glass access? | Emergency recovery against abuse risk. | FR-144 | Security officer | Open |
| OQ-037 | Is master data company-owned or tenant-shared, and what does "team" scope mean? | Isolation model and record-level rules. | RBAC-Matrix 8A.11 | Architect, Sponsor | Open |
| OQ-038 | Do all stock adjustments need approval, or only those above a threshold? The v0.1 FR-109 said above a threshold. The workflow table and SOD-07 imply all. | Inventory control strength against workload. | FR-109, AWD-03 | Inventory manager, Finance manager | Open |

## How questions are closed

1. The owner answers in writing.
2. The BA records the decision, date and approver here.
3. Affected requirements change from Proposed to Confirmed in the BRD.
4. The change is noted in [Documentation-Progress](../00-Project-Management/Documentation-Progress.md).

## Phase 2 classification (v0.2, 2026-11-11)

Each question is classified from the evidence in the package. A "Proposed solution" is a recommendation in this documentation. It is not a decision and not an approval. No question counts as resolved, because resolving needs a named stakeholder.

| Classification | ID | Effect on MVP | Evidence or note |
|---|---|---|---|
| Requires business stakeholder decision | OQ-001 | Blocks MVP build start | No one can approve any document in this package until a sponsor is named. |
| Requires business stakeholder decision | OQ-002 | Does not block MVP | Affects R4 industry modules only. |
| Requires business stakeholder decision | OQ-003 | Blocks MVP build start | Release 1 scope is not confirmed. MVP-Strategy proposes 58 modules. |
| Requires business stakeholder decision | OQ-004 | Blocks MVP configuration or go-live, not build | P0 to P2 assignments are proposals in Module-Prioritization. |
| Proposed solution awaiting approval | OQ-005 | Does not block MVP | ADR-018 recommends a tenant-capable product deployed first for one customer. PENDING APPROVAL. |
| Requires business stakeholder decision | OQ-006 | Does not block MVP | Payroll is R2. |
| Requires business stakeholder decision | OQ-007 | Blocks MVP configuration or go-live, not build | Tax codes and rules are configuration, but the MVP tax FRs cannot be accepted without the regime. |
| Requires business stakeholder decision | OQ-008 | Blocks MVP configuration or go-live, not build | Currency list and rate source are configuration. |
| Requires business stakeholder decision | OQ-009 | Blocks MVP configuration or go-live, not build | FR-111 valuation method is configurable, ADR-005 and the design allow any. A first method must still be chosen before UAT. |
| Requires business stakeholder decision | OQ-010 | Blocks MVP build start | Residency drives hosting (ADR-020), retention and privacy. Compliance officer decision. |
| Requires business stakeholder decision | OQ-011 | Does not block MVP | English at launch is the working assumption (NFR-034). Further languages are configuration. |
| Requires business stakeholder decision | OQ-012 | Blocks MVP configuration or go-live, not build | Migration scope is needed before the migration plan, not before build. |
| Requires business stakeholder decision | OQ-013 | Blocks MVP configuration or go-live, not build | Approval limit values are company data. The structure is defined in RBAC Matrix 8A.5. |
| Requires business stakeholder decision | OQ-014 | Blocks MVP configuration or go-live, not build | Planning input. No effect on documentation consistency. |
| Proposed solution awaiting approval | OQ-015 | Blocks MVP configuration or go-live, not build | ADR-019 proposes the NFR-005 envelope as the design target. IT manager must supply real volumes. |
| Proposed solution awaiting approval | OQ-016 | Blocks MVP build start | ADR-020 recommends provider-neutral design with public cloud preferred. Infrastructure cannot be built until decided. |
| Requires technical stakeholder decision | OQ-017 | Blocks MVP configuration or go-live, not build | Integration partners are adapters. No MVP function needs a named partner except email (ASM-05). |
| Requires business stakeholder decision | OQ-018 | Blocks MVP configuration or go-live, not build | FR-054 template content depends on it. |
| Proposed solution awaiting approval | OQ-019 | Blocks MVP build start | ADR-017 recommends tenant per customer with shared schema and row-level security. The schema needs tenant_id either way. PENDING APPROVAL. |
| Proposed solution awaiting approval | OQ-020 | Blocks MVP configuration or go-live, not build | BRL-040 default is refuse negative stock. Awaiting approval. |
| Requires business stakeholder decision | OQ-021 | Blocks MVP configuration or go-live, not build | Fiscal calendar is configuration of M-043 and M-044. |
| Requires business stakeholder decision | OQ-022 | Blocks MVP configuration or go-live, not build | Retention values are placeholders (NFR-027, NFR-040, NFR-042). Legal advice needed. |
| Proposed solution awaiting approval | OQ-023 | Blocks MVP configuration or go-live, not build | Authentication-Authorization proposes MFA mandatory for privileged roles and OpenID Connect for SSO. Security officer must confirm. |
| Requires technical stakeholder decision | OQ-024 | Blocks MVP build start | ADR-021 gives criteria and a database recommendation, not a language. The team must choose. |
| Proposed solution awaiting approval | OQ-025 | Does not block MVP | Responsive web is assumed (NFR-033). Native apps and offline use are not in the MVP. |
| Proposed solution awaiting approval | OQ-026 | Does not block MVP | BRL-073 and FR-143 propose no chained delegation and a 30 day maximum. Awaiting approval. |
| Requires business stakeholder decision | OQ-027 | Does not block MVP | Print template content. |
| Proposed solution awaiting approval | OQ-028 | Blocks MVP configuration or go-live, not build | NFR-007, NFR-009 and NFR-010 propose 99.5 percent, 15 minutes and 4 hours. Sponsor must confirm. |
| Requires business stakeholder decision | OQ-029 | Does not block MVP | Blocks UAT scheduling, not build. |
| Requires business stakeholder decision | OQ-030 | Does not block MVP | Affects migration timing. |
| Requires business stakeholder decision | OQ-031 | Does not block MVP | ADR-012 and the numbering tests accept either policy. |
| Proposed solution awaiting approval | OQ-032 | Blocks MVP configuration or go-live, not build | FR-142 proposes override requested by an Organization Admin and approved by a Super Admin. The reopen approver moved to OQ-034. Awaiting approval. |
| Proposed solution awaiting approval | OQ-033 | Blocks MVP configuration or go-live, not build | RBAC Matrix 8A.2 proposes a Super Admin as grant approver. |
| Requires business stakeholder decision | OQ-034 | Blocks MVP configuration or go-live, not build | Payment limits, a role above Finance Manager and the reopen approver are business choices. |
| Proposed solution awaiting approval | OQ-035 | Blocks MVP configuration or go-live, not build | FR-139 proposes a 24 hour hold. Awaiting approval. |
| Proposed solution awaiting approval | OQ-036 | Blocks MVP configuration or go-live, not build | RBAC Matrix 8A.8 proposes 2 hours. Awaiting approval. |
| Requires business stakeholder decision | OQ-037 | Blocks MVP build start | Master data ownership changes the schema (company-owned or tenant-shared). Needs a decision with ADR-017. |
| Requires business stakeholder decision | OQ-038 | Blocks MVP configuration or go-live, not build | Whether all adjustments need approval. AWD-03 proposes yes. |

Totals: 38 questions. Proposed solution awaiting approval 13. Requires business stakeholder decision 23. Requires technical stakeholder decision 2. Resolved 0. No longer applicable 0.
Blocking build start: OQ-001, OQ-003, OQ-010, OQ-016, OQ-019, OQ-024, OQ-037.
