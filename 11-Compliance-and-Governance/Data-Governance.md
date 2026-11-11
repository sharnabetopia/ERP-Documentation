# Data Governance

Document status: Draft v0.1 (Stage 3 part 2). Proposed. Basic governance capabilities (retention, privacy handling) are MVP under M-116 (basic). Full governance tooling is R3.
Related: [Data Protection](../06-RBAC-and-Security/Data-Protection.md), [Data Retention Policy](Data-Retention-Policy.md), [Privacy Requirements](Privacy-Requirements.md), [Database Design](../04-System-Architecture/Database-Design.md), BR-008, FR-028, FR-029.

## 1. Goals

1. Every important data item has an owner, a definition, a quality rule and a protection level.
2. Master data is created once, kept accurate, and changed under control.
3. Data is classified and handled according to its class.
4. Data quality is measured and improved.
5. Data lifecycle (creation, use, archival, disposal) follows policy.

## 2. Roles

| Role | Responsibility | Example holders (proposed) |
|---|---|---|
| Data governance council | Approves policies, resolves disputes, reviews metrics | Sponsor delegate, Finance Manager, IT manager, compliance officer |
| Data owner (per domain) | Accountable for the domain's quality, access and retention | Finance Manager (finance), Procurement Manager (vendors), Sales Manager (customers), Inventory Manager (items), HR Manager (employees) |
| Data steward | Maintains data day to day, resolves duplicates, approves changes | Accountants, procurement and sales administrators |
| Data custodian | Technical care: storage, backup, access mechanism | IT, database engineers |
| Data consumer | Uses data within permission | All users |
| Auditor | Independent review | Auditor role |

## 3. Data domains and owners

| Domain | Key entities | Owner role | Classification (see Data Protection) |
|---|---|---|---|
| Organisation | Company, branch, department, cost centre | Organization Admin | Internal |
| Identity and access | User, role, assignment | Security officer | Restricted |
| People (core) | Employee | HR Manager | Internal to Restricted |
| Finance | Accounts, ledger, periods, rates, tax codes, bank | Finance Manager | Restricted |
| Vendors and procurement | Vendor, PR, PO, GRN, purchase invoice | Procurement Manager | Confidential, bank data Restricted |
| Customers and sales | Customer, quotation, order, delivery, invoice | Sales Manager | Confidential |
| Items and inventory | Item, category, warehouse, stock ledger | Inventory Manager | Confidential |
| Platform | Settings, numbering, templates, chains | Organization Admin | Internal |
| Audit | Audit log | Auditor and security officer | Restricted |

## 4. Master data governance

| Rule | Source |
|---|---|
| One record per vendor, customer, item. Duplicate detection on creation | FR-029 |
| Records in use are deactivated, never deleted | BRL-015, FR-028 |
| Key field changes are audited (before and after) | FR-030 |
| Vendor creation and activation separate duties. Bank changes need independent approval | FR-086, FR-088 |
| Merge of duplicates is a controlled operation with audit (not specified in the FRS, gap DG-01) | |
| Reference data (units, currencies, tax codes, payment terms) changed by owners only | FR-028 |
| Import of master data validates first and audits the batch | FR-048, FR-049 |

## 5. Data quality

| Dimension | Rule or measure | Where enforced |
|---|---|---|
| Completeness | Required fields per entity | Form Validation Rules |
| Uniqueness | Codes, tax ids, invoice numbers | Database unique constraints, FR-029, FR-099 |
| Validity | Formats, ranges, reference integrity | Validation, foreign keys (NFR-022) |
| Consistency | Sub-ledgers equal control accounts. Stock equals movements | INV-03 to INV-06 |
| Timeliness | Rates and tax codes loaded before use | FR-068 |
| Accuracy | Reconciliations, reference data sets | Finance reference tests |

Quality measures reported monthly to the owners: duplicate candidates, incomplete records, failed validations, reconciliation differences, import error rates. Targets are TBD.

## 6. Classification and handling

Classes Public, Internal, Confidential, Restricted are defined in Data Protection section 1. Every entity and attribute in the data dictionary should carry a class. The current data dictionary lists entities and key attributes but does not yet carry a class per attribute (gap DG-02).

## 7. Lifecycle

| Stage | Control |
|---|---|
| Create | Validation, duplicate check, ownership by domain |
| Use | Scoped access, audit of changes, masked views |
| Share | Export controls, partner interfaces with minimum fields |
| Retain | Retention rules by record type |
| Archive | Closed-period data moved but still queryable (NFR-042) |
| Dispose | Controlled deletion or anonymisation after retention, with approval and audit |

## 8. Policies to approve

| ID | Policy | Status |
|---|---|---|
| GOV-01 | Data ownership and stewardship | Draft in this document |
| GOV-02 | Master data management | Draft in section 4 |
| GOV-03 | Data classification and handling | Draft in Data Protection |
| GOV-04 | Data retention and disposal | Draft, periods TBD |
| GOV-05 | Data quality | Draft in section 5, targets TBD |
| GOV-06 | Data sharing and integration | Draft in Integration Architecture |
| GOV-07 | Privacy and personal data | Draft in Privacy Requirements |

## 9. Tooling in releases

MVP: retention rules, legal hold, personal data export and anonymisation (FR-127, FR-129), duplicate detection, audit. R3: governance workflow, data catalogue, quality dashboards, merge tooling, policy attestations.

## 10. Gaps and open points

| ID | Item |
|---|---|
| DG-01 | Duplicate merge procedure not specified |
| DG-02 | No per-attribute classification in the data dictionary |
| DG-03 | Governance council membership and cadence not agreed |
| DG-04 | Quality targets not set |
| Questions | OQ-010, OQ-022, OQ-001 (ownership needs named people) |
