# Software and System Requirements Specification (SRS)

Document status: Draft v0.2 (Phase 2: counts corrected). Proposed.
This SRS is an index and frame. The normative requirements are in the documents it links, so that each requirement exists once.

## 1. Purpose and scope

Describes the software system to be built for the MVP and how its requirements are organised. Scope: [MVP Strategy](../00-Project-Management/MVP-Strategy.md).

## 2. Overall description

| Item | Content |
|---|---|
| Product perspective | Web-based multi-company ERP. Integrates with email, identity provider, banks and gateways as confirmed (OQ-017). See [Business Context](../02-Business-Analysis/Business-Context.md). |
| Product functions | The 58 MVP modules in the [Module Inventory](../01-ERP-Module-Analysis/ERP-Module-Inventory.md). |
| User classes | The 18 default roles. See [Stakeholder Register](../00-Project-Management/Stakeholder-Register.md). |
| Operating environment | Browser clients, server-side application, relational database, object storage, background workers. Hosting TBD (OQ-016). |
| Constraints | [BRD](../02-Business-Analysis/BRD.md) section 9. |
| Assumptions and dependencies | BRD sections 8 and 10. |

## 3. Requirements index

| Requirement type | Where | Count |
|---|---|---|
| Business requirements | [BRD](../02-Business-Analysis/BRD.md) | 63 |
| Business rules | [Business Rules](../02-Business-Analysis/Business-Rules.md) | 74 (BRL-001 to BRL-065, BRL-066 to BRL-074 added in v0.2) |
| Functional requirements | [FRS](FRS.md) | 150 (FR-001 to FR-130, FR-131 to FR-150 added in v0.2) |
| Non-functional requirements | [NFR](NFR.md) | 42 |
| Technical requirements | [NFR](NFR.md) section 2 | 18 |
| User stories | [User Stories](User-Stories.md) | 98 |
| Use cases | [Use Cases](Use-Cases.md) | 16 |
| API requirements | [API Documentation](../04-System-Architecture/API-Documentation.md) | 202 |
| Security requirements | [Security Requirements](../06-RBAC-and-Security/Security-Requirements.md) | 73 |
| Test cases | [RTM](../08-QA-and-Testing/RTM.md) section 8 | see RTM (written, none executed) |

## 4. External interface requirements

| Interface | Requirement source |
|---|---|
| User interface | [UI/UX specifications](../07-UI-UX-Specifications/Screen-Specifications.md). Accessibility: NFR-032, [Accessibility Requirements](../07-UI-UX-Specifications/Accessibility-Requirements.md). |
| Software interfaces | [Integration Architecture](../04-System-Architecture/Integration-Architecture.md), [API Documentation](../04-System-Architecture/API-Documentation.md) |
| Communication | TLS only (NFR-012). |
| Hardware | None specified. |

## 5. System features

Each feature group maps to a section of the FRS:

| Feature group | FRS section | FR range |
|---|---|---|
| Platform and administration | 2 | FR-001 to FR-052 |
| Finance | 3 | FR-053 to FR-085 |
| Procure-to-Pay | 4 | FR-086 to FR-100 |
| Inventory | 5 | FR-101 to FR-113 |
| Order-to-Cash | 6 | FR-114 to FR-124 |
| Cross-cutting | 7 | FR-125 to FR-130 |
| Authorisation hardening (v0.2) | 7A | FR-131 to FR-150 |

## 6. Quality attributes

[NFR](NFR.md) sections 1 and 2.

## 7. Other requirements

Compliance and privacy: BR-059, NFR-039, NFR-040, with open points OQ-010 and OQ-022. See the compliance documents in folder 11.

## 8. Known gaps

| Gap | Plan |
|---|---|
| UI specification | Written as Draft in folder 07 (UI-UX-Specifications). Not reviewed |
| Numbers above were re-verified by script in the Phase 2 re-validation | Done, see 13-Remediation-Reports/Final-Revalidation-Report.md |
