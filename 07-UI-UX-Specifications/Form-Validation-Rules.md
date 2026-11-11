# Form Validation Rules

Document status: Draft v0.1 (Stage 3 part 2). Proposed. **Field lengths, ranges and message wording are proposals** to confirm with the data owners and in UX review. Business rules come from [Business Rules](../02-Business-Analysis/Business-Rules.md) and the [FRS](../03-Product-and-System-Requirements/FRS.md).
Related: [Screen Specifications](Screen-Specifications.md), [Data Dictionary](../04-System-Architecture/Data-Dictionary.md), [API Documentation](../04-System-Architecture/API-Documentation.md) (error codes), [Security Requirements](../06-RBAC-and-Security/Security-Requirements.md) (SEC-023).

## 1. Validation principles

1. The server is the authority (SEC-023). The client repeats checks only to help the user. Anything that passes the client and fails the server shows the server message.
2. Three layers: format and type checks, field-level business checks, document-level business checks. They run in that order.
3. Messages say what is wrong and how to fix it (NFR-031). They never reveal other users' data or internal details (SEC-030).
4. Errors appear next to the field, in a summary at the top with links, and are announced to assistive technology.
5. Required fields are marked in text, not by colour alone.
6. Inline validation runs on leaving a field. Submit runs all checks and moves focus to the first error.
7. Money, rates and quantities are exact decimals. Currency decides decimal places. Paste of formatted numbers is parsed by the user's locale (NFR-020).
8. Trim leading and trailing spaces on text. Compare codes and emails case-insensitively.
9. Unsupported characters are refused with the allowed set named. Free text is stored as typed and encoded on output (SEC-025).

## 2. Message catalogue

| ID | Message (proposed wording) | Trigger | API code |
|---|---|---|---|
| MSG-001 | This field is required. | Required field empty | VAL-001 |
| MSG-002 | Enter a valid {field}, for example {example}. | Wrong format (email, date, number, tax id) | VAL-001 |
| MSG-003 | Quantity must be greater than zero. | Quantity at or below zero | VAL-001 |
| MSG-004 | Amount cannot be negative. | Negative amount where not allowed | VAL-001 |
| MSG-005 | Use at most {n} characters. | Too long | VAL-001 |
| MSG-006 | Choose a date between {from} and {to}. | Date outside allowed range | VAL-001 |
| MSG-007 | The password does not meet the policy: {reasons}. | Password policy | VAL-001 |
| MSG-008 | The email or password is not correct. | Sign-in failure (never says which) | AUTH-001 |
| MSG-009 | This account is locked. Try again later or contact your administrator. | Lockout | AUTH-002 |
| MSG-010 | {field} is already in use. | Duplicate unique value | CONF-001 |
| MSG-011 | Someone else changed this record. Reload to see the latest version. | Version conflict | CONF-002 |
| MSG-012 | Enter a reason. | Reject, return, reopen, override, reversal without reason | VAL-001 |
| MSG-013 | You cannot approve or release something you prepared or created. | Self-approval, SoD | DUTY-002 |
| MSG-014 | This vendor is not active. Choose an active vendor. | Draft or blocked vendor | PROC-001 |
| MSG-015 | Quantity exceeds the open quantity of {open} plus the allowed tolerance. | Receipt too large | PROC-002 |
| MSG-016 | An invoice with this number already exists for this vendor. | Duplicate vendor invoice | PROC-004 |
| MSG-017 | The posting date is in a closed period. Choose a date in an open period. | Closed period | FIN-002 |
| MSG-018 | The allocated amount exceeds the open amount of {open}. | Over-allocation | FIN-006 |
| MSG-019 | Order value plus open balance exceeds the credit limit ({limit}, exposure {exposure}). Approval is required. Or: only {available} of {requested} could be reserved. | Credit or stock | SALES-003, STK-001 |
| MSG-020 | Return quantity exceeds what was delivered less earlier returns ({max}). | Over-return | SALES-006 |
| MSG-021 | The credit exceeds the invoiced amount less earlier credits ({max}). | Over-credit | SALES-007 |
| MSG-022 | The refund exceeds the amount received ({max}). | Over-refund | FIN-007 |
| MSG-023 | Debits and credits do not balance. Difference: {difference}. | Unbalanced journal | FIN-001 |
| MSG-024 | The period cannot be closed: {n} items need attention. | Close blocked | FIN-003, FIN-004 |
| MSG-025 | Not enough stock. Available: {available}. | Insufficient stock | STK-001 |
| MSG-026 | This role combination breaks a segregation-of-duties rule ({rule}). | SoD | DUTY-001 |
| MSG-027 | A delegate cannot delegate again. | Chained delegation (pending OQ-026) | STATE-001 |
| MSG-028 | No exchange rate exists for {currency} on {date}. Add a rate first. | Missing rate | FIN-005 |
| MSG-029 | This file type or size is not allowed. Allowed: {types}, up to {size}. | Upload | FILE-001, FILE-002 |
| MSG-030 | The record was not found. | Out of scope or non-existent (same message for both) | NOTF-001 |

## 3. Common field types

| Type | Rules | Messages |
|---|---|---|
| Code | 1 to 30 characters, letters, digits, hyphen, underscore, unique within its scope, stored upper case | MSG-001, MSG-010, MSG-005 |
| Name | 1 to 255 characters | MSG-001, MSG-005 |
| Email | Valid format, up to 254 characters, unique per tenant for users | MSG-002, MSG-010 |
| Phone | Digits, spaces, plus, hyphen, parentheses, 5 to 20 characters | MSG-002 |
| Date | Calendar date, within company's allowed range, document dates not later than allowed future days (setting) | MSG-006 |
| Money | Exact decimal, scale by currency (default 2, up to 4 stored), within plus or minus 999,999,999,999.9999 | MSG-004, MSG-002 |
| Quantity | Exact decimal up to 4 places, above zero for movements | MSG-003 |
| Percentage | 0 to 100 with up to 4 decimals | MSG-002 |
| Exchange rate | Above zero, up to 8 decimals | MSG-002, MSG-028 |
| Tax id | Format by country (TBD, OQ-007), unique per company for vendors and customers | MSG-002, MSG-010 |
| Narration and notes | Up to 4,000 characters | MSG-005 |
| Attachment | Type allow-list, size limit (default 10 MB, TBD), scan status clean | MSG-029 |
| Selection (vendor, customer, item) | Must be active and in scope | MSG-014, MSG-030 |

## 4. Form rules

Columns: R = required. Source names the requirement or rule. Lengths are proposals.

### FORM-01 Sign-in, password and reset

| Field | R | Rule | Message | Source |
|---|---|---|---|---|
| Email | Yes | Valid format | MSG-002 | FR-001 |
| Password (sign-in) | Yes | Non-empty. Failure never says which field is wrong | MSG-008, MSG-009 | FR-001, FR-003 |
| New password | Yes | At least 12 characters, not common or breached, not one of last 5 (defaults) | MSG-007 | FR-002, SEC-002 |
| Confirm password | Yes | Equals new password | MSG-007 | FR-002 |
| MFA code | Yes when required | 6 digits, not reused | MSG-008 | FR-006 |

### FORM-02 Invite user and assign role

| Field | R | Rule | Message | Source |
|---|---|---|---|---|
| Name | Yes | 1 to 255 characters | MSG-001 | FR-008 |
| Email | Yes | Valid, unique per tenant | MSG-002, MSG-010 | FR-008 |
| Company | Yes | One the admin administers | MSG-030 | FR-010 |
| Role | Yes | Not retired, admin may assign it | MSG-026 | FR-012, FR-017 |
| Branch, department | No | Belong to the company | MSG-030 | FR-010 |
| Valid from, valid to | If temporary | To after from, reason required | MSG-006, MSG-012 | FR-016 |
| Employee link | No | Employee not already linked | MSG-010 | FR-011 |

### FORM-03 Employee

| Field | R | Rule | Message | Source |
|---|---|---|---|---|
| Employee code | Yes | Unique per company | MSG-010 | FR-023 |
| Full name | Yes | 1 to 255 | MSG-001 | FR-023 |
| Branch, department | Yes | In scope | MSG-030 | FR-023 |
| Manager | No | No cycle | MSG-002 (cycle text) | FR-024 |

### FORM-04 Vendor

| Field | R | Rule | Message | Source |
|---|---|---|---|---|
| Code | Yes | Unique per company | MSG-010 | FR-086 |
| Name | Yes | 1 to 255 | MSG-001 | FR-086 |
| Tax id | By policy | Format by country, duplicate warning | MSG-002, MSG-010 | FR-029 |
| Payment terms | Yes | Active term | MSG-001 | FR-086 |
| Bank account | No | Format check, changes need independent approval | MSG-002 | FR-088 |

### FORM-05 Item

| Field | R | Rule | Message | Source |
|---|---|---|---|---|
| Code | Yes | Unique per company | MSG-010 | FR-101 |
| Name | Yes | 1 to 255 | MSG-001 | FR-101 |
| Type | Yes | Stock, non-stock, service | MSG-001 | FR-101 |
| Base unit | Yes | Active unit. Locked after movements | MSG-001 | FR-101 |
| Category | Yes | Active category with accounts | MSG-001 | FR-102 |

### FORM-06 Customer

| Field | R | Rule | Message | Source |
|---|---|---|---|---|
| Code, name | Yes | As vendor | MSG-001, MSG-010 | FR-114 |
| Payment terms | Yes | Active | MSG-001 | FR-114 |
| Credit limit | No | At least zero | MSG-004 | FR-115 |
| Tax id | By policy | Format and duplicate check | MSG-002 | FR-029 |

### FORM-07 Purchase requisition

| Field | R | Rule | Message | Source |
|---|---|---|---|---|
| Needed-by date | Yes | Today or later | MSG-006 | FR-089 |
| Department | Yes | In scope | MSG-030 | FR-089 |
| Line item or description | Yes | Active item or text for non-catalogue | MSG-001 | FR-089 |
| Line quantity | Yes | Above zero | MSG-003 | FR-089 |
| Justification | Yes above threshold (TBD) | Up to 4,000 | MSG-001 | FR-089 |

### FORM-08 Purchase order

| Field | R | Rule | Message | Source |
|---|---|---|---|---|
| Vendor | Yes | Active | MSG-014 | FR-091, BRL-032 |
| Order date | Yes | Within open dates | MSG-006 | FR-091 |
| Currency, rate | Yes | Rate exists | MSG-028 | FR-068 |
| Line quantity, price | Yes | Quantity above zero, price at least zero | MSG-003, MSG-004 | FR-091 |
| Tax code | Yes per line | Active at order date | MSG-001 | FR-071 |
| Delivery date | No | Not before order date | MSG-006 | FR-091 |

### FORM-09 Goods receipt

| Field | R | Rule | Message | Source |
|---|---|---|---|---|
| PO | Yes | Approved or sent, not cancelled | MSG-001 | FR-094, FR-095 |
| Warehouse | Yes | Active, in scope | MSG-001 | FR-095 |
| Received quantity | Yes per line | Zero or more, at most open plus tolerance | MSG-015 | FR-095, BRL-035 |
| Rejected quantity | No | At most received | MSG-003 | FR-095 |

### FORM-10 Purchase invoice

| Field | R | Rule | Message | Source |
|---|---|---|---|---|
| Vendor | Yes | Active | MSG-014 | FR-097 |
| Vendor invoice number | Yes | Unique per vendor after normalisation | MSG-016 | FR-099 |
| Invoice date, due date | Yes | Due not before invoice date | MSG-006 | FR-097 |
| Lines | Yes | Linked to PO and GRN lines, quantity within received | MSG-003 | FR-098 |
| Posting date | Yes | In an open period | MSG-017 | FR-058 |
| Totals | Auto | Equal lines plus tax within rounding | MSG-002 | FR-097 |

### FORM-11 Journal

| Field | R | Rule | Message | Source |
|---|---|---|---|---|
| Date | Yes | In an open period | MSG-017 | FR-058, FR-062 |
| Narration | Yes | 1 to 4,000 | MSG-001 | BRL-030 |
| Lines | Yes | At least two, each with one of debit or credit above zero | MSG-004 | FR-062 |
| Account | Yes | Postable, not control-only from manual source | MSG-002 | FR-053, BRL-019 |
| Balance | Auto | Debits equal credits in both currencies | MSG-023 | FR-062 |
| Rate | If foreign | Exists | MSG-028 | FR-068 |

### FORM-12 Payment and receipt

| Field | R | Rule | Message | Source |
|---|---|---|---|---|
| Direction, party | Yes | Party active | MSG-001 | FR-073 |
| Bank account | Yes | Active, currency matches | MSG-001 | FR-072 |
| Amount | Yes | Above zero | MSG-004 | FR-073 |
| Allocation per invoice | No | At most open amount | MSG-018 | FR-073 |
| Excess over allocation | Auto | Held as credit | none | FR-074 |
| Refund amount | Yes for refund | At most received | MSG-022 | FR-078 |

### FORM-13 Sales order and quotation

| Field | R | Rule | Message | Source |
|---|---|---|---|---|
| Customer | Yes | Active, not on hold | MSG-002 (status text) | FR-118 |
| Valid until (quote) | Yes | Not before today | MSG-006 | FR-116 |
| Line quantity, price | Yes | Quantity above zero | MSG-003 | FR-118 |
| Discount | No | 0 to 100 percent, approval above policy (TBD) | MSG-002 | FR-116 |
| Confirmation | Action | Credit and stock checks | MSG-019 | FR-115 |

### FORM-14 Delivery and sales invoice

| Field | R | Rule | Message | Source |
|---|---|---|---|---|
| Delivered quantity | Yes | At most ordered plus tolerance, at most available stock | MSG-025 | FR-120 |
| Invoiced quantity | Yes | At most delivered | MSG-003 | FR-121 |
| Return quantity | Yes | At most delivered less earlier returns | MSG-020 | FR-122 |
| Credit amount | Yes | At most invoiced less earlier credits | MSG-021 | FR-123 |

### FORM-15 Stock transfer and adjustment

| Field | R | Rule | Message | Source |
|---|---|---|---|---|
| From, to warehouse | Yes | Different and active | MSG-001 | FR-108 |
| Quantity | Yes | Above zero, at most available | MSG-025 | FR-108 |
| Counted quantity | Yes | Zero or more | MSG-004 | FR-109 |
| Reason | Yes | Selected from list or text | MSG-012 | FR-109 |

### FORM-16 Approval decision and delegation

| Field | R | Rule | Message | Source |
|---|---|---|---|---|
| Decision | Yes | Approve, reject, return | MSG-001 | FR-041 |
| Reason | Yes for reject and return | 1 to 4,000 | MSG-012 | BRL-010 |
| Approver | n/a | Not the creator | MSG-013 | FR-042 |
| Delegate | Yes | Holds the permission and scope, not creator, not chained | MSG-027, MSG-030 | FR-044 |
| Delegation dates | Yes | End after start | MSG-006 | FR-044 |

### FORM-17 Fiscal year, period close, rates, tax

| Field | R | Rule | Message | Source |
|---|---|---|---|---|
| Year start, end | Yes | No overlap | MSG-006 | FR-056 |
| Reopen reason | Yes | Non-empty, step-up authentication | MSG-012 | FR-059 |
| Rate date, rate | Yes | Rate above zero, unique per pair and date | MSG-002, MSG-010 | FR-067 |
| Tax rate | Yes | 0 to 100, effective dates not overlapping | MSG-006 | FR-070 |

### FORM-18 Import

| Field | R | Rule | Message | Source |
|---|---|---|---|---|
| File | Yes | Matches template, type and size | MSG-029 | FR-048 |
| Mode | Yes | All-or-nothing or valid rows only | MSG-001 | FR-049 |
| Row values | n/a | Per entity rules above, plus duplicate keys and scope | per row | FR-048 |

## 5. Coverage

| Item | Count |
|---|---|
| Messages | 30 (MSG-001 to MSG-030) |
| Forms specified | 18 |
| MVP screen groups that have a form | See [Screen Specifications](Screen-Specifications.md). Forms for settings, templates, custom fields, numbering and master data are not detailed here (gap UX-GAP-06) |

## 6. Open points

Country-specific formats (tax id, phone, address) wait for OQ-007 and OQ-010. Languages for messages wait for OQ-011. Discount and justification thresholds wait for OQ-013.
