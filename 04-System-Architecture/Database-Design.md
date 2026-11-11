# Database Design (Logical)

Document status: Draft v0.2 (Phase 2 remediation, 2026-11-11). Section 2.9 and constraints DBC-13 to DBC-20 are new. Logical design for the MVP. Provisional (ADR-002, ADR-003).
Names are proposed. No physical DDL is produced. The design is product-neutral except where marked.
Related: [ERD](ERD.md), [Data Dictionary](Data-Dictionary.md), [ADR](Architecture-Decision-Records.md).

## 1. Conventions

| Topic | Convention |
|---|---|
| Keys | Surrogate primary key `id` (UUID, time-ordered) on every table. Business numbers (document numbers, codes) are separate unique columns. |
| Tenancy | `tenant_id` on every table except global catalogues. `company_id` on every company-owned table. Row-level security on both (ADR-002). |
| Audit columns | `created_at`, `created_by`, `updated_at`, `updated_by` on all mutable tables. Timestamps in UTC. |
| Versioning | `version` integer on mutable documents for optimistic locking (FR-125). |
| Soft deletion | Masters carry `is_active` and `deactivated_at`. Documents carry `status` and are never deleted. Append-only tables have no deletion. |
| Money | `numeric(19,4)` for amounts, `numeric(18,8)` for rates, `numeric(18,4)` for quantities. Currency code stored beside every amount (ADR-005). |
| Dates | Business dates (posting, document, due) are `date`. Event times are `timestamp with time zone`. |
| Status | Status stored as a constrained code. Allowed values are in the [Data Dictionary](Data-Dictionary.md). |
| Names | snake_case, singular table names. Foreign key column `<entity>_id`. |
| Referential integrity | Every reference is a foreign key. `on delete restrict` is the default. |
| Currency of a row | `txn_currency`, `fx_rate`, `base_amount` on all financial documents and ledger rows. |

## 2. Entity catalogue

Key: PK primary key, FK foreign key, UQ unique constraint, IX index. Tenancy: T tenant key, C company key, G global (no tenant).
"Del" is the deletion style: A append-only, S soft (inactive), D document status (never deleted).

### 2.1 Identity and access

| Entity | Purpose | Key attributes | FK | UQ | IX | Ten. | Del |
|---|---|---|---|---|---|---|---|
| tenant | Customer space (see OQ-019) | name, status | | name | | G | S |
| user_account | A person who signs in | email, display_name, password_hash, mfa_enabled, status, last_login_at, failed_attempts, locked_until | tenant, employee (nullable) | (tenant, lower(email)); employee_id where not null | status | T | S |
| role | Role definition | code, name, is_system, status | tenant | (tenant, code) | | T | S |
| permission | Catalogue entry | code (`module.submodule.action`), module, action | | code | module | G | S |
| role_permission | Permissions of a role | role, permission | role, permission | (role, permission) | | T | A |
| user_role_scope | Role assigned to a user with scope and validity | user, role, company, branch (nullable), department (nullable), own_only, valid_from, valid_to, reason | user, role, company, branch, department | (user, role, company, branch, department) | (user), valid_to | T | D |
| session | Server-side session | user, issued_at, last_seen_at, expires_at, revoked_at, client_info | user | | (user), expires_at | T | A |
| credential_token | Reset and activation tokens | user, purpose, token_hash, expires_at, used_at | user | token_hash | expires_at | T | A |
| sod_conflict | Conflicting permission pairs | permission_a, permission_b, mode (block or warn) | permission | (a, b) | | T | S |

### 2.2 Organisation

| Entity | Purpose | Key attributes | FK | UQ | IX | Ten. | Del |
|---|---|---|---|---|---|---|---|
| company | Legal entity | code, legal_name, country, base_currency, tax_id, status | tenant, currency | (tenant, code) | | T | S |
| branch | Operating unit | code, name, parent, status | company, parent branch | (company, code) | parent | C | S |
| department | Functional unit | code, name, parent, status | company, parent department | (company, code) | parent | C | S |
| cost_centre | Reporting dimension | code, name, branch, department, status | company, branch, department | (company, code) | | C | S |
| employee | Core people record | employee_code, full_name, job_title, branch, department, manager, status, hire_date | company, branch, department, employee (manager) | (company, employee_code) | manager, department | C | S |

### 2.3 Platform services

| Entity | Purpose | Key attributes | FK | UQ | IX | Ten. | Del |
|---|---|---|---|---|---|---|---|
| number_series | Document numbering | doc_type, fiscal_year, prefix, format, next_value, allow_gaps | company | (company, doc_type, fiscal_year) | | C | S |
| setting | Policy values with history | key, value, valid_from, changed_by | company | (company, key, valid_from) | | C | A |
| unit_of_measure | Units | code, name | | (tenant, code) | | T | S |
| currency | Currency list | code (ISO), name, decimals | | code | | G | S |
| payment_term | Terms | code, days, discount_pct | | (company, code) | | C | S |
| attachment | File link | owner_type, owner_id, file_key, file_name, size, content_type, checksum, scan_status | company | | (owner_type, owner_id) | C | D |
| notification | In-app notice | user, type, ref_type, ref_id, read_at, created_at | user | | (user, read_at) | T | S |
| notification_template | Template | code, channel, language, subject, body | | (tenant, code, channel, language) | | T | S |
| outbox_event | Reliable events | event_type, payload, status, attempts, next_attempt_at, trace_id | | | (status, next_attempt_at) | T | A |
| idempotency_record | Stored operation outcome | key, endpoint, request_hash, response_ref, expires_at | user | (tenant, key, endpoint) | expires_at | T | A |
| import_batch | Import run | template, mode, status, row_count, error_count | company, user | | status | C | D |
| import_error | Row errors | batch, row_number, field, message | import_batch | | batch | C | A |
| audit_log | Immutable history | actor, action, entity_type, entity_id, company, occurred_at, before, after, reason, trace_id | user | | (entity_type, entity_id), occurred_at, actor | T | A |
| retention_policy | Retention rules | record_type, years, legal_hold | | (tenant, record_type) | | T | S |
| custom_field_def | Configured fields | entity_type, code, data_type, required, options | company | (company, entity_type, code) | | C | S |
| custom_field_value | Values | def, entity_id, value | custom_field_def | (def, entity_id) | | C | S |

### 2.4 Approval engine

| Entity | Purpose | Key attributes | FK | UQ | IX | Ten. | Del |
|---|---|---|---|---|---|---|---|
| approval_chain | Chain definition | doc_type, name, min_value, max_value, branch, department, status | company | | (company, doc_type) | C | S |
| approval_chain_step | Ordered step | chain, sequence, approver_role, approver_user (nullable), timeout_hours | approval_chain, role, user | (chain, sequence) | | C | S |
| approval_instance | One approval run | doc_type, doc_id, round_no, status, submitted_by, submitted_at, value, currency | company, user | (doc_type, doc_id, round_no) | status, doc_id | C | D |
| approval_decision | Decision per step | instance, step, decided_by, on_behalf_of, decision, reason, decided_at | approval_instance, user | | instance | C | A |
| delegation | Authority transfer | from_user, to_user, doc_types, valid_from, valid_to, status | user | | (to_user, valid_to) | C | D |

### 2.5 Finance

| Entity | Purpose | Key attributes | FK | UQ | IX | Ten. | Del |
|---|---|---|---|---|---|---|---|
| account | Chart of accounts | code, name, type, normal_balance, parent, is_postable, is_control, control_for, currency (nullable), status | company, account (parent) | (company, code) | parent, type | C | S |
| fiscal_year | Year | name, start_date, end_date, status | company | (company, name) | | C | D |
| fiscal_period | Period | fiscal_year, number, start_date, end_date, status | fiscal_year | (fiscal_year, number) | (company, start_date, end_date) | C | D |
| currency_rate | Dated rate | from_currency, to_currency, rate_date, rate, source | company, currency | (company, from, to, rate_date) | | C | S |
| tax_code | Tax definition | code, rate, inclusive, account_collected, account_paid, valid_from, valid_to | company, account | (company, code, valid_from) | | C | S |
| posting_rule | Document type to accounts | doc_type, item_category (nullable), debit_account, credit_account, role | company, account, item_category | (company, doc_type, item_category, role) | | C | S |
| journal | Journal header | number, journal_date, period, source_type, source_id, narration, status, txn_currency, fx_rate, reverses (nullable) | company, fiscal_period, journal | (company, number) | (source_type, source_id), status | C | D |
| journal_line | Draft lines | journal, line_no, account, debit, credit, cost_centre, branch, department, party_type, party_id | journal, account | (journal, line_no) | account | C | D |
| ledger_entry | Posted general ledger rows | journal, line_no, posting_date, period, account, debit, credit, txn_currency, fx_rate, base_debit, base_credit, branch, department, cost_centre, party_type, party_id, source_type, source_id | company, journal, account, period | (journal, line_no) | (account, posting_date), (company, period), (party_type, party_id), (source_type, source_id) | C | A |
| account_balance | Summary per account and period | account, period, branch, department, debit_total, credit_total | account, period | (account, period, branch, department) | | C | S (rebuildable) |
| bank_account | Bank or cash account | code, bank_name, account_no, currency, ledger_account, status | company, account, currency | (company, code), (company, bank_name, account_no) | | C | S |
| payment | Payment or receipt | number, direction, party_type, party_id, bank_account, payment_date, amount, txn_currency, fx_rate, base_amount, unallocated_amount, status, reverses | company, bank_account | (company, number) | (party_type, party_id), status | C | D |
| payment_allocation | Allocation to open item | payment, open_item, amount, base_amount, fx_difference | payment, open_item | (payment, open_item) | open_item | C | A |
| open_item | AP and AR items | party_type, party_id, source_type, source_id, document_date, due_date, amount, open_amount, txn_currency, status | company | (source_type, source_id, installment_no) | (party_type, party_id, status), due_date | C | D |

### 2.6 Procurement

| Entity | Purpose | Key attributes | FK | UQ | IX | Ten. | Del |
|---|---|---|---|---|---|---|---|
| vendor | Vendor master | code, name, tax_id, payment_term, currency, status | company | (company, code), (company, tax_id) where not null | name | C | S |
| vendor_bank_account | Vendor bank details | vendor, bank_name, account_no_encrypted, status, approved_by | vendor | (vendor, account_hash) | | C | S |
| purchase_requisition | PR header | number, requester, department, needed_by, status, justification | company, employee, department | (company, number) | status | C | D |
| purchase_requisition_line | PR line | pr, line_no, item, description, quantity, unit, estimated_price, converted_qty | pr, item | (pr, line_no) | | C | D |
| purchase_order | PO header | number, vendor, order_date, currency, fx_rate, total, tax_total, status, revision_no | company, vendor | (company, number, revision_no) | (vendor), status | C | D |
| purchase_order_line | PO line | po, line_no, item, quantity, unit, price, tax_code, received_qty, invoiced_qty, pr_line | po, item, tax_code, pr_line | (po, line_no) | | C | D |
| goods_receipt | GRN header | number, po, warehouse, receipt_date, status, reverses | company, po, warehouse | (company, number) | po | C | D |
| goods_receipt_line | GRN line | grn, line_no, po_line, item, quantity, rejected_qty | grn, po_line, item | (grn, line_no) | | C | D |
| purchase_invoice | Vendor invoice | internal_no, vendor_invoice_no, vendor, invoice_date, due_date, currency, fx_rate, total, tax_total, match_status, status | company, vendor | (company, internal_no), (company, vendor, vendor_invoice_no) | status, due_date | C | D |
| purchase_invoice_line | Line | invoice, line_no, po_line, grn_line, item, quantity, price, tax_code, amount | invoice | (invoice, line_no) | | C | D |
| invoice_match_result | Match outcome | invoice, line, qty_variance, price_variance, within_tolerance, decided_by | purchase_invoice | | invoice | C | A |

### 2.7 Inventory

| Entity | Purpose | Key attributes | FK | UQ | IX | Ten. | Del |
|---|---|---|---|---|---|---|---|
| item_category | Category tree | code, name, parent, inventory_account, cogs_account, valuation_method | company, account | (company, code) | parent | C | S |
| item | Item or service | code, name, type, base_unit, category, valuation_method, is_stocked, status | company, item_category, unit_of_measure | (company, code) | category, name | C | S |
| item_uom_conversion | Unit conversion | item, unit, factor | item | (item, unit) | | C | S |
| warehouse | Stock location | code, name, branch, status | company, branch | (company, code) | | C | S |
| stock_movement | Stock ledger (append only) | item, warehouse, movement_date, qty_in, qty_out, unit_cost, value, source_type, source_id, source_line_id, movement_type | company, item, warehouse | | (item, warehouse, movement_date), (source_type, source_id) | C | A |
| stock_balance | Summary per item and warehouse | item, warehouse, on_hand, reserved, in_transit, version | item, warehouse | (item, warehouse) | | C | S (rebuildable) |
| stock_reservation | Hold for an order | order_line, item, warehouse, quantity, status, expires_at | sales_order_line, item, warehouse | | (item, warehouse, status) | C | D |
| stock_transfer | Transfer header | number, from_warehouse, to_warehouse, status | company, warehouse | (company, number) | status | C | D |
| stock_transfer_line | Line | transfer, item, qty_sent, qty_received | stock_transfer, item | (transfer, item) | | C | D |
| stock_adjustment | Adjustment header | number, warehouse, reason, status, total_value | company, warehouse | (company, number) | status | C | D |
| stock_adjustment_line | Line | adjustment, item, system_qty, counted_qty, difference, unit_cost | stock_adjustment, item | (adjustment, item) | | C | D |
| stock_document | Goods issue and receipt without PO | number, type, warehouse, cost_centre, status | company, warehouse | (company, number) | | C | D |
| stock_document_line | Line | document, item, quantity, unit_cost | stock_document, item | | | C | D |
| cost_layer | Cost layers for FIFO or average | item, warehouse, receipt_movement, remaining_qty, unit_cost | item, stock_movement | | (item, warehouse, remaining_qty) | C | S |

### 2.8 Sales

| Entity | Purpose | Key attributes | FK | UQ | IX | Ten. | Del |
|---|---|---|---|---|---|---|---|
| customer | Customer master | code, name, tax_id, payment_term, credit_limit, currency, status | company | (company, code) | name | C | S |
| customer_contact | Contacts | customer, name, email, phone | customer | | customer | C | S |
| quotation | Quote header | number, customer, quote_date, valid_until, status, total | company, customer | (company, number) | status | C | D |
| quotation_line | Line | quotation, line_no, item, quantity, price, discount_pct, tax_code | quotation, item | (quotation, line_no) | | C | D |
| sales_order | Order header | number, customer, order_date, currency, fx_rate, total, status, quotation | company, customer, quotation | (company, number) | (customer), status | C | D |
| sales_order_line | Line | order, line_no, item, quantity, price, tax_code, reserved_qty, delivered_qty, invoiced_qty | sales_order, item | (order, line_no) | | C | D |
| delivery | Delivery header | number, order, warehouse, delivery_date, status | company, sales_order, warehouse | (company, number) | order | C | D |
| delivery_line | Line | delivery, order_line, item, quantity | delivery, sales_order_line | (delivery, order_line) | | C | D |
| sales_invoice | Invoice | number, customer, invoice_date, due_date, currency, fx_rate, total, tax_total, status | company, customer | (company, number) | (customer), status | C | D |
| sales_invoice_line | Line | invoice, line_no, delivery_line, item, quantity, price, tax_code, amount | sales_invoice | (invoice, line_no) | | C | D |
| sales_return | Return | number, delivery, customer, status, disposition | company, delivery | (company, number) | | C | D |
| sales_return_line | Line | return, delivery_line, item, quantity, disposition | sales_return | | | C | D |
| credit_note | Credit note | number, invoice, customer, amount, reason, status | company, sales_invoice | (company, number) | invoice | C | D |

Entity count in this catalogue: 87 in section 2.1 to 2.8 (counted by script on 2026-11-11) plus 9 in section 2.9. Draft count only, since R2 to R4 entities are not modelled.

### 2.9 Authorisation hardening (v0.2)

New tables for FR-131 to FR-150. Existing tables gain these columns: `role.tier`, `role_assignment.approval_status`, `payment.released_by`, `payment.release_step_up_at`, `payment.hold_until`, `vendor_bank_account.status`, `customer.credit_limit` (written only by an approved request).

| Entity | Purpose | Key attributes | FK | UQ | IX | Ten. | Del |
|---|---|---|---|---|---|---|---|
| role_grant_request | Pending grant needing a second approver | grantor, grantee, role, scope, tier, status, decided_by, decided_at, reason | company, user_account (3), role | (company, id) | grantee, status | C | A |
| reopen_request | Request to reopen a closed period | period, requested_by, reason, status, approved_by, window_start, window_end | company, fiscal_period, user_account (2) |  | period, status | C | A |
| vendor_bank_change | Request, verification and approval of vendor bank details | vendor, requested_by, verified_by, approved_by, new_account (field protected), status, hold_until | company, vendor, user_account (3) |  | vendor, status | C | A |
| credit_limit_request | Credit limit change request | customer, old_limit, new_limit, requested_by, approved_by, status | company, customer, user_account (2) |  | customer, status | C | A |
| config_change_request | Controlled configuration change | key, old_value, new_value, requested_by, approved_by, status | company, user_account (2) |  | key, status | C | A |
| sod_override | Time-limited SoD override | user, rule, requested_by, approved_by, valid_from, valid_to, reason | company, user_account (3) |  | user, valid_to | C | A |
| break_glass_session | Emergency access session | released_by_1, released_by_2, reason, expires_at, closed_at, reviewed_by | user_account (3) |  | expires_at | T | A |
| service_account | Integration identity | name, owner, scopes, status, approved_by, token_expiry | company, user_account (2) | (company, name) | status | C | S |
| step_up_event | Recent re-authentication record | user, session, authenticated_at, method | user_account |  | user, authenticated_at | T | A |

Entity count after v0.2: 96 (87 in v0.1 plus 9).

## 3. Database-enforced constraints

| ID | Constraint | Enforces |
|---|---|---|
| DBC-01 | `ledger_entry`: debit and credit are not negative, exactly one is greater than zero. | BRL-016 |
| DBC-02 | A journal is accepted for posting only if the sum of base debits equals the sum of base credits (deferred check at commit). | INV-01 |
| DBC-03 | `ledger_entry`, `stock_movement`, `audit_log`: update and delete revoked from the application role. | FR-064, FR-104, BRL-007 |
| DBC-04 | `ledger_entry.posting_date` must fall in an open or soft-closed period at insert time, checked in the posting transaction. | BRL-018 |
| DBC-05 | `payment_allocation` total per open item does not exceed its amount. | BRL-026 |
| DBC-06 | `stock_balance.on_hand` may not be below zero when the company policy forbids negative stock. Checked in the posting transaction under row lock. | BRL-040 |
| DBC-07 | `purchase_invoice`: unique (company, vendor, vendor_invoice_no) after normalisation. | BRL-037 |
| DBC-08 | Foreign keys on every reference with `restrict`. | NFR-022 |
| DBC-09 | `account`: a parent must be in the same company. A posting to a non-postable account is refused. | FR-053 |
| DBC-10 | Row-level security: every statement runs with tenant and company context. | ADR-002 |
| DBC-11 | Approval: a decision row where `decided_by` equals the document creator is refused. | BRL-008 |
| DBC-12 | `sales_order_line`: delivered_qty is at most quantity plus tolerance. `credit_note.amount` at most the invoice less earlier credits. Checked in the transaction. | BRL-048, BRL-050 |

Constraints that need reading other rows are enforced in the transaction by the domain module and by integrity checks (INV-xx), not only by declarative constraints.
| DBC-13 | `payment`: `released_by` differs from `created_by` and from `approved_by` (check constraint). | BRL-068, FR-135 |
| DBC-14 | `role_grant_request`: `grantee` differs from `grantor`, and `decided_by` differs from `grantor` and `grantee`. | BRL-066, BRL-067, FR-132, FR-133 |
| DBC-15 | `vendor_bank_change`: `requested_by`, `verified_by` and `approved_by` are three different users. | BRL-070, FR-139 |
| DBC-16 | `reopen_request`: `approved_by` differs from `requested_by`. | BRL-071, FR-140 |
| DBC-17 | `credit_limit_request`: `approved_by` differs from `requested_by`. | BRL-070, FR-141 |
| DBC-18 | `config_change_request`: `approved_by` differs from `requested_by`. | BRL-070, FR-138 |
| DBC-19 | `sod_override`: `approved_by` differs from `requested_by` and from `user`. | FR-142 |
| DBC-20 | `break_glass_session`: `released_by_1` differs from `released_by_2`. | BRL-072, FR-144 |

## 4. Indexing and performance notes

1. Every foreign key column has an index unless it is the leading column of a unique index.
2. Ledger and stock ledger are range-partitioned by posting date (monthly or yearly, decided by volume, OQ-015).
3. Audit log is partitioned by month and archived by retention policy.
4. Summary tables (`account_balance`, `stock_balance`) are updated in the posting transaction and can be rebuilt from the ledgers. A job verifies them nightly.
5. Open-item and status lists use partial indexes on non-closed statuses.
6. Reports use a read replica where the lag is acceptable.

## 5. Concurrency design

| Case | Mechanism |
|---|---|
| Two users edit one document | `version` check, conflict on mismatch |
| Two postings issue the last unit | Row lock on `stock_balance` in fixed item-warehouse order |
| Concurrent numbering | Lock on `number_series` row, or sequence if gaps are allowed (OQ-031) |
| Double payment click | Idempotency record unique on (tenant, key, endpoint) |
| Approval decided twice | Unique (instance, step) decision and status check |

## 6. Retention and archival

| Data | Rule |
|---|---|
| Ledger, journals, invoices, payments | Retained for the legal period (OQ-022). Archive closed years when older than the configured age. Archive stays queryable |
| Audit log | Retain at least as long as financial records |
| Sessions, tokens, idempotency records | Short lifetime, purged by job |
| Outbox | Purged after successful publication and a grace period |
| Personal data | Anonymised on request where law allows. Ledger rows keep party keys, not names (FR-129) |

## 7. Not yet modelled

| Area | When |
|---|---|
| R2 to R4 entities (payroll, budgets, bank statements, projects, manufacturing) | With each release. MVP reserves branch, department, cost_centre and project-ready dimension columns on ledger rows |
| Multi-company group consolidation | After OQ-019 and group reporting needs |
| Physical design (types per engine, tablespaces, partition ranges) | After OQ-015, OQ-024 |
