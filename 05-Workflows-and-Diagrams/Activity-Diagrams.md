# Activity Diagrams

Document status: Draft v0.1 (Stage 2 part 2). Proposed. Diagram syntax was checked with a Mermaid 10.9.1 parser (all diagram blocks in the package were re-rendered with Mermaid CLI 10.9.1 in Phase 2 (101 blocks, 0 errors)). The diagrams have **not been visually rendered or reviewed for layout**.
Activity diagrams show the detailed logic inside a single operation or sub-process. Start and end are marked by rounded nodes. Decisions are diamonds. Parallel steps are shown by two arrows out of one node.

## ACT-01 Approval workflow and escalation

Covers the approval workflow and the escalation flow (FR-039 to FR-047).

```mermaid
flowchart TD
    S(["Submit"]) --> V["Validate document"]
    V --> C["Select chain by type, value, branch, department"]
    C --> E{"Chain found with eligible approver?"}
    E -- "No" --> X(["Refuse submit, notify administrator"])
    E -- "Yes" --> L["Lock document, create instance"]
    L --> P["Pick next step approver, skip creator"]
    P --> D{"Active delegation for this approver?"}
    D -- "Yes" --> D1["Route to delegate, record on-behalf-of"]
    D -- "No" --> D2["Route to approver"]
    D1 --> N["Notify"]
    D2 --> N
    N --> W{"Decision before timeout?"}
    W -- "No" --> T["Escalate to next authority, notify both"]
    T --> M{"Another authority exists?"}
    M -- "No" --> M1["Alert administrator, keep pending"]
    M -- "Yes" --> P
    W -- "Approve" --> A{"Value above this approver's limit?"}
    A -- "Yes" --> P
    A -- "No" --> F{"More steps?"}
    F -- "Yes" --> P
    F -- "No" --> OK(["Approved, unlock for next action, notify"])
    W -- "Reject" --> R["Require reason, set Rejected"]
    W -- "Return" --> R2["Require reason, set Returned"]
    R --> END1(["Notify requester"])
    R2 --> END1
```

## ACT-02 Three-way match and duplicate check

```mermaid
flowchart TD
    S(["Invoice entered"]) --> N["Normalise vendor invoice number"]
    N --> D{"Same vendor and number, or same vendor, date and amount, already exists?"}
    D -- "Yes" --> D1["Refuse or flag, reason required for override"]
    D1 --> OV{"Override approved?"}
    OV -- "No" --> E1(["Stop"])
    OV -- "Yes" --> M
    D -- "No" --> M["Load PO lines and GRN lines"]
    M --> G{"GRN exists for invoiced lines?"}
    G -- "No" --> H1["Hold, wait for receipt"]
    G -- "Yes" --> Q{"Invoiced quantity at most received quantity?"}
    Q -- "No" --> H2["Quantity variance, hold"]
    Q -- "Yes" --> P{"Price within tolerance of PO?"}
    P -- "No" --> H3["Price variance, hold"]
    P -- "Yes" --> T{"Tax matches expected?"}
    T -- "No" --> H4["Tax variance, hold"]
    T -- "Yes" --> OK(["Match passed, send for approval"])
    H1 --> HE(["Held"])
    H2 --> HE
    H3 --> HE
    H4 --> HE
```

## ACT-03 Stock availability, reservation and negative stock prevention

```mermaid
flowchart TD
    S(["Issue or reserve request"]) --> L["Start transaction, lock balance rows in item then warehouse order"]
    L --> A["Compute available = on hand minus reserved"]
    A --> Q{"Requested at most available?"}
    Q -- "Yes" --> OK["Insert movement or reservation, update balance"]
    OK --> C(["Commit"])
    Q -- "No" --> R{"Request is a reservation?"}
    R -- "Yes" --> R1["Reserve what is available, record backorder"]
    R1 --> C
    R -- "No, an issue" --> P{"Policy allows negative stock for this item?"}
    P -- "Yes" --> N["Allow, audit AUD-063"] --> OK
    P -- "No" --> X["Rollback"] --> E(["Refuse STK-001 with available quantity"])
```

## ACT-04 Payment creation and allocation

```mermaid
flowchart TD
    S(["Create payment"]) --> K{"Idempotency key seen?"}
    K -- "Yes" --> K1(["Return stored result"])
    K -- "No" --> B{"Bank account active and currency matches?"}
    B -- "No" --> E1(["Refuse"])
    B -- "Yes" --> R{"Rate available for payment date?"}
    R -- "No" --> E2(["Refuse FIN rate missing"])
    R -- "Yes" --> L["For each allocation check amount at most open amount"]
    L --> O{"All allocations valid?"}
    O -- "No" --> E3(["Refuse FIN-006"])
    O -- "Yes" --> U{"Allocated total less than payment amount?"}
    U -- "Yes" --> U1["Remainder becomes unallocated credit"]
    U -- "No" --> D
    U1 --> D["Save Draft"]
    D --> AP["Submit, approve, release"]
    AP --> X["Post ledger, compute exchange difference per allocation"]
    X --> OKK(["Open items updated, status Part-paid or Paid"])
```

## ACT-05 Period close checklist

```mermaid
flowchart TD
    S(["Start close"]) --> A["List unposted documents in period"]
    S --> B["List pending approvals touching period"]
    S --> C["Run AP, AR and stock reconciliations"]
    A --> D{"Any?"}
    B --> E{"Any?"}
    C --> F{"Differences?"}
    D -- "Yes" --> D1["Post, cancel or carry forward"] --> J
    E -- "Yes" --> E1["Decide or cancel"] --> J
    F -- "Yes" --> F1["Investigate and adjust by journal"] --> J
    D -- "No" --> J["Re-run checklist"]
    E -- "No" --> J
    F -- "No" --> J
    J --> G{"All clear?"}
    G -- "No" --> S
    G -- "Yes" --> H["Soft close optional"]
    H --> I["Close period, block posting, audit"]
    I --> Z(["Statements final"])
```

## ACT-06 Import validate and commit

```mermaid
flowchart TD
    S(["Upload file"]) --> T{"Template and file type correct?"}
    T -- "No" --> E1(["Reject file"])
    T -- "Yes" --> V["Validate each row: type, required, reference, duplicate key, scope"]
    V --> R["Row error report"]
    R --> M{"Errors?"}
    M -- "Yes, user fixes file" --> S
    M -- "No" --> C{"Commit mode"}
    C -- "All or nothing" --> A["One transaction for all rows"]
    C -- "Valid rows only" --> B["Commit valid rows, report skipped rows"]
    A --> Z{"Any failure?"}
    Z -- "Yes" --> RB["Rollback all"] --> E2(["Failed, nothing saved"])
    Z -- "No" --> OK(["Committed, audited"])
    B --> OK
```

## ACT-07 Employee lifecycle (MVP core and later releases)

```mermaid
flowchart TD
    S(["Hire approved"]) --> A["HR creates employee record"]
    A --> B["Link or create user, assign roles and scope"]
    B --> C["Onboarding checklist R3"]
    C --> D["Active employment: attendance, leave, payroll R2"]
    D --> E{"Event"}
    E -- "Transfer" --> F["Update branch, department, manager, review role scope"] --> D
    E -- "Manager change" --> G["Re-route pending approvals"] --> D
    E -- "Leave of absence" --> H["Status On leave, delegation"] --> D
    E -- "Separation" --> I["Offboarding clearance R3"]
    I --> J["Deactivate user, revoke sessions within 60 seconds"]
    J --> K["Reassign pending approvals and owned records"]
    K --> L(["Status Separated, history retained"])
```

## ACT-08 Credit note and refund limits

```mermaid
flowchart TD
    S(["Credit note requested"]) --> I{"Linked to a posted invoice?"}
    I -- "No" --> E1(["Refuse"])
    I -- "Yes" --> L["Invoiced amount minus earlier credits"]
    L --> Q{"Requested at most remaining?"}
    Q -- "No" --> E2(["Refuse SALES-007"])
    Q -- "Yes" --> A["Approval by Finance Manager"]
    A --> P["Post credit note and ledger entry"]
    P --> O{"Invoice already paid?"}
    O -- "No" --> R1["Reduce open amount"]
    O -- "Yes" --> C["Create customer credit"]
    C --> D{"Refund requested?"}
    D -- "No" --> Z(["Credit held"])
    D -- "Yes" --> F{"Refund at most amount received?"}
    F -- "No" --> E3(["Refuse FIN-007"])
    F -- "Yes" --> G(["Approve and release refund"])
```
