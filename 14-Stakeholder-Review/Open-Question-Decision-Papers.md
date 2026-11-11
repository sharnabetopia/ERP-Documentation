# Open Question Decision Papers

Phase 3, 2026-11-11. Status: **PENDING**. Every recommendation here is a proposal. No decision, approval, date or stakeholder name is recorded, because none exists in the evidence. Owners are roles taken from the Open Questions register or marked TBD.
One paper per build-blocking question. Each paper keeps the approval status PENDING and leaves the final decision field blank.
Related: [Stakeholder Decision Pack](Stakeholder-Decision-Pack.md), [Approval Register](Approval-Register.md), [Decision Dependency Matrix](Decision-Dependency-Matrix.md), [Open Questions](../02-Business-Analysis/Open-Questions.md).

| Paper | Question | Status |
|---|---|---|
| [OQ-001](#oq-001-project-sponsor) | Project Sponsor | PENDING |
| [OQ-003](#oq-003-mvp-scope) | MVP Scope | PENDING |
| [OQ-010](#oq-010-data-residency) | Data Residency | PENDING |
| [OQ-016](#oq-016-hosting-model) | Hosting Model | PENDING |
| [OQ-019](#oq-019-tenant-model) | Tenant Model | PENDING |
| [OQ-024](#oq-024-technology-stack) | Technology Stack | PENDING |
| [OQ-037](#oq-037-master-data-ownership) | Master Data Ownership | PENDING |

## OQ-001 Project Sponsor

| Field | Content |
|---|---|
| Approval status | **PENDING** |
| Decision date | TBD |
| Decision owner | Programme sponsor (TBD) |
| Approver | TBD (the sponsor approves their own appointment only through the organisation) |
| Final decision | _(blank until a stakeholder decides and the decision is documented)_ |

### 1. Current question and context

Who is the executive sponsor, and who can approve scope, priority and exceptions? Source: `02-Business-Analysis/Open-Questions.md` OQ-001. The Stakeholder Register lists "Executive sponsor" as role SH-01 with no named person.

### 2. Why the decision is required

Every PENDING item in this package needs an approver. Without a named sponsor, no scope (OQ-003), tenant model (OQ-019), budget (OQ-014) or security design (ADR-022) can be approved, and the package cannot leave NOT READY.

### 3. Options, with advantages and disadvantages

| Option | Description | Advantages | Disadvantages |
|---|---|---|---|
| A. One named executive sponsor | A single accountable person approves scope, priority and exceptions | Fast, clear accountability, matches RACI role SH-01 | Single point of delay. Personal availability risk |
| B. Sponsor plus steering group with delegated authority | Sponsor chairs a group (finance, IT, security, compliance) with written approval limits | Domain experts approve their own areas. Resilient | Slower. Needs a charter that says who decides when the group disagrees |
| C. Interim sponsor until a permanent one is named | A named person approves only low-risk items | Unblocks work | Cannot approve scope or security. Risk of rework at hand-over |

### 4. Impacts by area

| Area | Impact |
|---|---|
| Business | Decides who can commit budget and accept scope |
| Technical | None directly. Blocks ADR approval |
| Financial | Budget authority (OQ-014) attaches to this role |
| Security | Security design approval needs a business owner above the security officer |
| Operational | Defines the escalation path for exceptions (SoD overrides, scope change) |

### 5. Dependencies and risks

Dependencies: blocks OQ-003, OQ-014, OQ-004 and every item in `Approval-Register.md`. Risk: R-017 (capacity or budget) in `00-Project-Management/Risk-Register.md`. Risk of approving through an unauthorised person: items approved by someone without authority would need re-approval.

### 6. Recommended option and justification (a recommendation, not a decision)

**Option B**, because the package contains approvals that need specialist ownership (security officer for ADR-022, compliance officer for OQ-010, finance manager for NFR and limits) and one person is unlikely to be competent in all of them. The sponsor stays the final approver. This is a recommendation only.

### 7. Questions stakeholders must answer

1. Who is the executive sponsor (a name and role, supplied by the organisation)?
2. Which decisions can the sponsor delegate, to whom, and up to what limit?
3. What happens when owners disagree?
4. How is an approval recorded (signature, ticket, minute) so that the Approval Register can cite it?

### 8. Cross-references

`00-Project-Management/Stakeholder-Register.md` SH-01, `RACI-Matrix.md`, `Project-Charter.md`, `Risk-Register.md` R-017.

## OQ-003 MVP Scope

| Field | Content |
|---|---|
| Approval status | **PENDING** |
| Decision date | TBD |
| Decision owner | Programme sponsor (TBD) |
| Approver | TBD (OQ-001) |
| Final decision | _(blank until a stakeholder decides and the decision is documented)_ |

### 1. Current question and context

Is the proposed MVP of 58 modules accepted, or does capacity force a smaller first release? Source: OQ-003, `00-Project-Management/MVP-Strategy.md`. The Sprint Plan has no dates, velocity, team size or effort (OQ-014) and names no concrete fallback cut.

### 2. Why the decision is required

All FRs (FR-001 to FR-150), 1,038 test cases and the RTM are written for the 58-module scope. Scope decides what must be built first and what the first release can be tested against. The matrix in `MVP-Scope-Decision-Matrix.md` supports this decision.

### 3. Options, with advantages and disadvantages

| Option | Description | Advantages | Disadvantages |
|---|---|---|---|
| A. Accept all 58 modules as one MVP | Baseline in the documents | Whole buy-stock-sell-book cycle works. All documents already match | Largest effort. Capacity unknown (OQ-014) |
| B. Accept 58 modules in waves A to D with a pre-agreed cut line | Same scope, built in the Sprint Plan slices, with the 8 P1 modules (M-001, M-007, M-011, M-015, M-060, M-065, M-075, M-088) named as the first to defer | Keeps the full target. A smaller release is ready if capacity falls short | The cut line is a proposal. Deferred P1 modules reduce reporting and usability |
| C. Smaller MVP now (for example no Order-to-Cash) | Drop a business cycle | Cheaper first release | Fails the MVP completeness test in MVP-Strategy section 1. Breaks FR-115, FR-116 to FR-124 and 11 sales modules, and their tests |

### 4. Impacts by area

| Area | Impact |
|---|---|
| Business | A and B deliver a full operating cycle. C does not |
| Technical | Dependencies run in layers (`Module-Dependencies.md`). Removing a module may break a dependent one |
| Financial | Effort for A is unknown until OQ-014 is answered |
| Security | The authorisation hardening (FR-131 to FR-150) touches M-002, M-003, M-004, M-013, M-043, M-047, M-054, M-079 and is not optional for any option that keeps finance |
| Operational | Wave order decides training and cut-over sequence |

### 5. Dependencies and risks

Dependencies: OQ-001 (who approves), OQ-014 (budget, team, dates), OQ-004 (priorities), OQ-002 (industry). Risks: R-017. A scope change after approval invalidates RTM sections and test counts.

### 6. Recommended option and justification (a recommendation, not a decision)

**Option B**. It keeps the tested scope, and states the cut line in advance. The 8 P1 modules are the lowest documented priority (P1 in the Module Inventory), which is the only evidence available for the cut order. The sponsor must confirm or change the list.

### 7. Questions stakeholders must answer

1. Is the 58-module scope accepted?
2. What are the budget, team and target dates (OQ-014)?
3. If capacity falls short, which modules go first?
4. Which industry is first (OQ-002), and does it need a module outside the 58?

### 8. Cross-references

`MVP-Strategy.md`, `Sprint-Plan.md`, `ERP-Module-Inventory.md`, `Module-Dependencies.md`, `MVP-Scope-Decision-Matrix.md`, RTM section 8.

## OQ-010 Data Residency

| Field | Content |
|---|---|
| Approval status | **PENDING** |
| Decision date | TBD |
| Decision owner | Compliance officer (TBD) |
| Approver | TBD (sponsor, OQ-001) |
| Final decision | _(blank until a stakeholder decides and the decision is documented)_ |

### 1. Current question and context

Where must data be stored (country or region), and which privacy laws apply? Source: OQ-010. No jurisdiction is known (`11-Compliance-and-Governance`).

### 2. Why the decision is required

Residency decides the hosting region (ADR-020), what can be sent to external providers, retention (NFR-027, NFR-040, NFR-042), privacy export and anonymisation (NFR-039, FR-129) and whether per-tenant regions are needed (ADR-017 option D).

### 3. Options, with advantages and disadvantages

| Option | Description | Advantages | Disadvantages |
|---|---|---|---|
| A. Single fixed country or region | All tenants in one place | Simplest. Easy to evidence | May fail a customer with another rule |
| B. Region per tenant, chosen at onboarding | Design supports a region per tenant | Fits several jurisdictions. Matches ADR-017 option D | More operations. Backup and restore per region |
| C. No restriction | Choose the cheapest region | Cheapest | Unsafe until the legal position is known. Hard to reverse once data is stored |

### 4. Impacts by area

| Area | Impact |
|---|---|
| Business | Decides which customers can be served |
| Technical | Region per tenant needs per-tenant deployment or routing |
| Financial | A is cheapest to run. B costs more |
| Security | Cross-border transfers and provider access need controls (Data-Protection) |
| Operational | Incident and breach notification duties depend on the law |

### 5. Dependencies and risks

Dependencies: OQ-016 (hosting), OQ-022 (retention periods), OQ-019 (tenant model), OQ-005. Risk: choosing before the legal answer. The package cannot supply the legal answer.

### 6. Recommended option and justification (a recommendation, not a decision)

**No country or law is recommended**, because the evidence contains none. The recommended path is Option B as the design capability (already allowed by ADR-017), with the actual region fixed only after a written determination by the compliance officer or legal adviser.

### 7. Questions stakeholders must answer

1. Which countries will hold customers, employees and records?
2. Which privacy and financial record laws apply (a written legal determination)?
3. Are cross-border transfers allowed, and to which providers?
4. What are the breach notification duties?

### 8. Cross-references

`11-Compliance-and-Governance/Privacy-Requirements.md`, `Compliance-Requirements.md`, `Data-Retention-Policy.md`, NFR-039, NFR-040, FR-127, FR-129, ADR-020.

## OQ-016 Hosting Model

| Field | Content |
|---|---|
| Approval status | **PENDING** |
| Decision date | TBD |
| Decision owner | IT manager (TBD) |
| Approver | TBD (sponsor, OQ-001) |
| Final decision | _(blank until a stakeholder decides and the decision is documented)_ |

### 1. Current question and context

Which hosting model (public cloud, private cloud or on-premises, managed hosting) and which certifications are required? Source: OQ-016, ADR-020.

### 2. Why the decision is required

Infrastructure, backups, failover (NFR-007, NFR-009, NFR-010, NFR-011), network zones and the deployment plan cannot be built until the model is known.

### 3. Options, with advantages and disadvantages

| Option | Description | Advantages | Disadvantages |
|---|---|---|---|
| A. Public cloud with managed services | Provider-neutral containers on managed database and storage | Managed failover and backup. Fast start. Many regions | Provider dependence. Residency limited by available regions |
| B. Private cloud or on-premises | Own or leased hardware | Full control. Residency certain | Capital cost. Staff for failover, patching, backup |
| C. Managed hosting or hybrid | Outsourced operation | Control with less staff | Contract complexity. Mixed responsibilities |

### 4. Impacts by area

| Area | Impact |
|---|---|
| Business | Time to first environment |
| Technical | Design is provider-neutral (ADR-015, ADR-020), so all three are possible |
| Financial | A is operating cost. B is capital plus staff. Figures not available |
| Security | Shared-responsibility model in A. Physical control in B |
| Operational | A reduces operations work for RPO 15 minutes and RTO 4 hours (proposed) |

### 5. Dependencies and risks

Dependencies: OQ-010 (residency), OQ-015 (volumes), OQ-028 (service levels), OQ-024. Risk: residency rules may exclude a provider region.

### 6. Recommended option and justification (a recommendation, not a decision)

**Option A in a region that meets the OQ-010 answer, with the provider-neutral design kept**, as in ADR-020. If residency or policy forbids it, option B uses the same design with added failover work. Recommendation only.

### 7. Questions stakeholders must answer

1. Is there a preferred or mandated provider or data centre?
2. Which certifications are required (OQ-016)?
3. What availability, RPO and RTO are required (OQ-028)?
4. What budget applies to hosting?

### 8. Cross-references

ADR-015, ADR-020, `Infrastructure-Architecture.md`, `Deployment-Architecture.md`, `10-Deployment-and-Operations/`, NFR-007, NFR-009, NFR-010, NFR-011.

## OQ-019 Tenant Model

| Field | Content |
|---|---|
| Approval status | **PENDING** |
| Decision date | TBD |
| Decision owner | Architect and sponsor (names TBD) |
| Approver | TBD (sponsor, OQ-001) |
| Final decision | _(blank until a stakeholder decides and the decision is documented)_ |

### 1. Current question and context

Does each customer get its own isolated tenant holding many companies, or do all companies share one tenant? Source: OQ-019, ADR-017.

### 2. Why the decision is required

The schema carries `tenant_id` on every business row, and row-level security (SEC-016, NFR-018, TR-002) depends on the model. Adding it after build is a rewrite.

### 3. Options, with advantages and disadvantages

| Option | Description | Advantages | Disadvantages |
|---|---|---|---|
| A. Tenant per customer, many companies inside, shared schema with row-level security | ADR-002 and ADR-017 | Lowest cost. Single upgrade path. Group consolidation inside a tenant | A policy bug could reach all tenants. Per-tenant restore needs tooling |
| B. One pool, no tenant concept | All companies together | Simplest | Unsafe for more than one customer. Retrofit is a rewrite |
| C. Schema per tenant | Separate schema for each tenant | Smaller blast radius | Migrations multiply. Harder at scale |
| D. Database per tenant | Separate database for each tenant | Strongest isolation. Per-tenant region | Highest cost and operations |

### 4. Impacts by area

| Area | Impact |
|---|---|
| Business | Whether more than one customer can be served on one deployment |
| Technical | Schema, session context, cache keys and jobs all carry tenant context |
| Financial | A is cheapest, D most expensive |
| Security | Isolation tests (INV-12) and row-level security carry the risk in A |
| Operational | Restore and upgrade effort differ by option |

### 5. Dependencies and risks

Dependencies: OQ-005 (single customer or product), OQ-010 (residency), OQ-037 (master data ownership). Risk: wrong choice means rework (ARD risk list). Safe either way: add `tenant_id` now.

### 6. Recommended option and justification (a recommendation, not a decision)

**Option A, with `tenant_id` on every row from the first release and option D available for customers that need it** (ADR-017). It is correct even if only one customer ever exists. Recommendation only.

### 7. Questions stakeholders must answer

1. Will more than one customer share a deployment (OQ-005)?
2. Does any customer need a separate database or region?
3. Is group consolidation across companies of one customer required?

### 8. Cross-references

ADR-002, ADR-017, ADR-018, `Authentication-Authorization.md` section 6, FR-019, SEC-016, NFR-018, INV-12.

## OQ-024 Technology Stack

| Field | Content |
|---|---|
| Approval status | **PENDING** |
| Decision date | TBD |
| Decision owner | IT manager (TBD) |
| Approver | TBD (sponsor, OQ-001) |
| Final decision | _(blank until a stakeholder decides and the decision is documented)_ |

### 1. Current question and context

Are there technology constraints or preferences (languages, database, cloud, team skills)? Source: OQ-024, ADR-021.

### 2. Why the decision is required

No code can start without a language, framework and database. The database choice is constrained by requirements: deferred balance check at commit (DBC-02), row-level security (SEC-016), exact decimals (ADR-005) and point-in-time recovery (NFR-009).

### 3. Options, with advantages and disadvantages

| Option | Description | Advantages | Disadvantages |
|---|---|---|---|
| A. PostgreSQL | Database choice for requirements | Native row-level security, deferrable constraints, exact numeric, point-in-time recovery, no licence cost | Team skill needed |
| B. SQL Server | Alternative with native features | Row-level security, strong tooling | Licence cost. Deferred check needs a workaround |
| C. MySQL or MariaDB | Familiar to many | Low cost | No deferrable constraints or native row-level security, so DBC-02 and SEC-016 need application workarounds |
| D. Oracle | Alternative | Strong features | Licence and cost |
| Language and framework | Not recommended here | Depends on team skill and hiring | No evidence in the package |

### 4. Impacts by area

| Area | Impact |
|---|---|
| Business | Hiring and long-term maintenance |
| Technical | Options C and D change how DBC-02 and SEC-016 are met and tested |
| Financial | Licences and skills |
| Security | Row-level security is a core control (ADR-017) |
| Operational | Operations skills and tooling |

### 5. Dependencies and risks

Dependencies: OQ-016 (hosting), team skills (unknown), any mandated vendor. Risk: a mandated engine without deferrable constraints invalidates DBC-02 and its tests.

### 6. Recommended option and justification (a recommendation, not a decision)

**Database: option A or B** because only they meet DBC-02 and SEC-016 natively, A on cost (ADR-021). **No language or framework recommendation**, because the package has no data about the team. This is a recommendation only.

### 7. Questions stakeholders must answer

1. Is there an existing platform, licence or mandated vendor?
2. Which languages does the team know?
3. Who maintains the system after launch?
4. Any security standards that restrict components?

### 8. Cross-references

ADR-003, ADR-005, ADR-021, DBC-02, SEC-016, NFR-009, `Database-Design.md`.

## OQ-037 Master Data Ownership

| Field | Content |
|---|---|
| Approval status | **PENDING** |
| Decision date | TBD |
| Decision owner | Architect and sponsor (names TBD) |
| Approver | TBD (sponsor, OQ-001) |
| Final decision | _(blank until a stakeholder decides and the decision is documented)_ |

### 1. Current question and context

Is master data (vendors, customers, items, tax codes, units) company-owned or tenant-shared, and what does "team" mean for record-level scope? Source: OQ-037, RBAC Matrix 8A.11. Gap RF-022.

### 2. Why the decision is required

The schema depends on it. Authentication-Authorization section 6 puts a company predicate on every query, which conflicts with US-017 "shared reference data". Until decided, master data is treated as company-owned and sharing is outside the MVP.

### 3. Options, with advantages and disadvantages

| Option | Description | Advantages | Disadvantages |
|---|---|---|---|
| A. Company-owned master data | Every record belongs to one company | Simple. Matches FR-019 and the company predicate. Strong isolation | Duplicate vendors and items across companies. No group view |
| B. Tenant-shared master data | One record shared by all companies of a tenant | No duplicates. Group reporting | Conflicts with FR-019, SEC-017 and the company predicate. Needs group permission design |
| C. Hybrid: shared reference data (units, currencies), company-owned parties and items, optional sharing later | Mixed ownership | Fits real use. Allows later sharing | Needs a clear rule per entity and an owner field from the start |

### 4. Impacts by area

| Area | Impact |
|---|---|
| Business | Duplicate maintenance versus group visibility |
| Technical | Entity tables need an ownership scope column |
| Financial | Option B and C need group functions that are outside the MVP |
| Security | Sharing widens access. SEC-017 requires explicit, audited group functions |
| Operational | Data stewardship and de-duplication effort (FR-029) |

### 5. Dependencies and risks

Dependencies: OQ-019 (tenant model), OQ-005. Risk: choosing B or C without group permission codes (gap RF-021). Effects on M-009, M-054, M-061, M-062, M-079, FR-126 and the isolation tests.

### 6. Recommended option and justification (a recommendation, not a decision)

**Option A for the MVP with an ownership scope column in the design** so that C can be added later without a migration. It matches the interim rule already in RBAC 8A.11. Recommendation only.

### 7. Questions stakeholders must answer

1. Do the companies of one customer need to share vendors, customers or items?
2. Who owns shared reference data?
3. What does "team" mean for scope (shared department, reporting line)?

### 8. Cross-references

RBAC-Matrix 8A.11, `Authentication-Authorization.md` section 6, FR-019, FR-029, FR-126, SEC-017, US-017, M-009, ADR-017.
