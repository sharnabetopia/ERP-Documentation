# Entity Relationship Diagrams

Document status: Draft v0.2 (Phase 2 remediation). Section 7 is new. Key relationships only. Full attribute lists are in [Database Design](Database-Design.md).
Cardinality: `||` exactly one, `o|` zero or one, `|{` one or many, `o{` zero or many.
Diagram syntax was checked with a Mermaid 10.9.1 parser (all diagram blocks in the package were re-rendered with Mermaid CLI 10.9.1 in Phase 2 (101 blocks, 0 errors)). The diagrams have **not been visually rendered or reviewed for layout**.

## 1. Identity, organisation and approvals

```mermaid
erDiagram
    TENANT ||--o{ COMPANY : owns
    TENANT ||--o{ USER_ACCOUNT : has
    TENANT ||--o{ ROLE : defines
    ROLE ||--o{ ROLE_PERMISSION : grants
    PERMISSION ||--o{ ROLE_PERMISSION : in
    USER_ACCOUNT ||--o{ USER_ROLE_SCOPE : assigned
    ROLE ||--o{ USER_ROLE_SCOPE : used_in
    COMPANY ||--o{ USER_ROLE_SCOPE : scoped_to
    COMPANY ||--o{ BRANCH : has
    COMPANY ||--o{ DEPARTMENT : has
    BRANCH ||--o{ BRANCH : parent_of
    COMPANY ||--o{ COST_CENTRE : has
    COMPANY ||--o{ EMPLOYEE : employs
    EMPLOYEE o|--o| USER_ACCOUNT : linked
    EMPLOYEE o|--o{ EMPLOYEE : manages
    USER_ACCOUNT ||--o{ SESSION : opens
    COMPANY ||--o{ APPROVAL_CHAIN : configures
    APPROVAL_CHAIN ||--|{ APPROVAL_CHAIN_STEP : has
    APPROVAL_INSTANCE ||--|{ APPROVAL_DECISION : records
    USER_ACCOUNT ||--o{ DELEGATION : gives
```

## 2. Finance

```mermaid
erDiagram
    COMPANY ||--o{ ACCOUNT : has
    ACCOUNT ||--o{ ACCOUNT : parent_of
    COMPANY ||--o{ FISCAL_YEAR : has
    FISCAL_YEAR ||--|{ FISCAL_PERIOD : divided_into
    FISCAL_PERIOD ||--o{ JOURNAL : contains
    JOURNAL ||--|{ JOURNAL_LINE : draft_lines
    JOURNAL ||--o{ LEDGER_ENTRY : posts
    ACCOUNT ||--o{ LEDGER_ENTRY : debited_credited
    JOURNAL o|--o| JOURNAL : reverses
    COMPANY ||--o{ BANK_ACCOUNT : holds
    ACCOUNT ||--o{ BANK_ACCOUNT : mapped_to
    BANK_ACCOUNT ||--o{ PAYMENT : used_by
    PAYMENT ||--o{ PAYMENT_ALLOCATION : allocates
    OPEN_ITEM ||--o{ PAYMENT_ALLOCATION : settled_by
    COMPANY ||--o{ TAX_CODE : defines
    COMPANY ||--o{ CURRENCY_RATE : keeps
    COMPANY ||--o{ POSTING_RULE : maps
```

## 3. Procure-to-Pay

```mermaid
erDiagram
    VENDOR ||--o{ VENDOR_BANK_ACCOUNT : has
    EMPLOYEE ||--o{ PURCHASE_REQUISITION : requests
    PURCHASE_REQUISITION ||--|{ PURCHASE_REQUISITION_LINE : has
    VENDOR ||--o{ PURCHASE_ORDER : receives
    PURCHASE_ORDER ||--|{ PURCHASE_ORDER_LINE : has
    PURCHASE_REQUISITION_LINE o|--o{ PURCHASE_ORDER_LINE : converted_to
    ITEM ||--o{ PURCHASE_ORDER_LINE : ordered
    PURCHASE_ORDER ||--o{ GOODS_RECEIPT : received_by
    GOODS_RECEIPT ||--|{ GOODS_RECEIPT_LINE : has
    PURCHASE_ORDER_LINE ||--o{ GOODS_RECEIPT_LINE : fulfilled_by
    VENDOR ||--o{ PURCHASE_INVOICE : bills
    PURCHASE_INVOICE ||--|{ PURCHASE_INVOICE_LINE : has
    PURCHASE_ORDER_LINE ||--o{ PURCHASE_INVOICE_LINE : matched
    GOODS_RECEIPT_LINE ||--o{ PURCHASE_INVOICE_LINE : matched
    PURCHASE_INVOICE ||--o{ INVOICE_MATCH_RESULT : evaluated
    PURCHASE_INVOICE ||--o| OPEN_ITEM : creates
```

## 4. Inventory

```mermaid
erDiagram
    ITEM_CATEGORY ||--o{ ITEM_CATEGORY : parent_of
    ITEM_CATEGORY ||--o{ ITEM : classifies
    ITEM ||--o{ ITEM_UOM_CONVERSION : converts
    BRANCH ||--o{ WAREHOUSE : hosts
    ITEM ||--o{ STOCK_MOVEMENT : moves
    WAREHOUSE ||--o{ STOCK_MOVEMENT : at
    ITEM ||--o{ STOCK_BALANCE : summarised
    WAREHOUSE ||--o{ STOCK_BALANCE : summarised
    STOCK_MOVEMENT ||--o{ COST_LAYER : creates
    STOCK_TRANSFER ||--|{ STOCK_TRANSFER_LINE : has
    WAREHOUSE ||--o{ STOCK_TRANSFER : from_to
    STOCK_ADJUSTMENT ||--|{ STOCK_ADJUSTMENT_LINE : has
    STOCK_DOCUMENT ||--|{ STOCK_DOCUMENT_LINE : has
    SALES_ORDER_LINE ||--o{ STOCK_RESERVATION : holds
    ITEM ||--o{ STOCK_RESERVATION : reserved
```

## 5. Order-to-Cash

```mermaid
erDiagram
    CUSTOMER ||--o{ CUSTOMER_CONTACT : has
    CUSTOMER ||--o{ QUOTATION : receives
    QUOTATION ||--|{ QUOTATION_LINE : has
    CUSTOMER ||--o{ SALES_ORDER : places
    QUOTATION o|--o| SALES_ORDER : converted_to
    SALES_ORDER ||--|{ SALES_ORDER_LINE : has
    SALES_ORDER ||--o{ DELIVERY : fulfilled_by
    DELIVERY ||--|{ DELIVERY_LINE : has
    SALES_ORDER_LINE ||--o{ DELIVERY_LINE : delivered
    CUSTOMER ||--o{ SALES_INVOICE : billed
    SALES_INVOICE ||--|{ SALES_INVOICE_LINE : has
    DELIVERY_LINE ||--o{ SALES_INVOICE_LINE : invoiced
    SALES_INVOICE ||--o| OPEN_ITEM : creates
    DELIVERY ||--o{ SALES_RETURN : returned_by
    SALES_RETURN ||--|{ SALES_RETURN_LINE : has
    SALES_INVOICE ||--o{ CREDIT_NOTE : credited_by
```

## 6. Cross-domain links

| From | To | Link |
|---|---|---|
| goods_receipt_line, delivery_line, stock_document_line, stock_adjustment_line | stock_movement | `source_type`, `source_id`, `source_line_id` |
| purchase_invoice, sales_invoice, payment, goods_receipt, delivery, credit_note | ledger_entry | `source_type`, `source_id` through the journal |
| Any approvable document | approval_instance | `doc_type`, `doc_id` |
| Any record | audit_log, attachment | `entity_type` or `owner_type` with id |
| Any document | outbox_event | event payload references the document |

These links are polymorphic references and need integrity checks in the domain layer, because a single foreign key cannot express them.

## 7. Authorisation hardening entities (v0.2)

Key relationships for the tables added in Database Design section 2.9.

```mermaid
erDiagram
    USER_ACCOUNT ||--o{ ROLE_GRANT_REQUEST : "grantor, grantee, approver"
    ROLE ||--o{ ROLE_GRANT_REQUEST : requested_role
    FISCAL_PERIOD ||--o{ REOPEN_REQUEST : reopened_by
    USER_ACCOUNT ||--o{ REOPEN_REQUEST : "requests, approves"
    VENDOR ||--o{ VENDOR_BANK_CHANGE : changes
    USER_ACCOUNT ||--o{ VENDOR_BANK_CHANGE : "requests, verifies, approves"
    CUSTOMER ||--o{ CREDIT_LIMIT_REQUEST : changes
    USER_ACCOUNT ||--o{ CREDIT_LIMIT_REQUEST : "requests, approves"
    USER_ACCOUNT ||--o{ CONFIG_CHANGE_REQUEST : "requests, approves"
    USER_ACCOUNT ||--o{ SOD_OVERRIDE : "subject, requests, approves"
    USER_ACCOUNT ||--o{ BREAK_GLASS_SESSION : "releases twice, reviews"
    USER_ACCOUNT ||--o{ SERVICE_ACCOUNT : "owns, approves"
    USER_ACCOUNT ||--o{ STEP_UP_EVENT : authenticates
    PAYMENT }o--o| USER_ACCOUNT : released_by
```
