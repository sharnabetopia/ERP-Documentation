# Data Flow Diagrams

Document status: Draft v0.1 (Stage 2 part 2). Proposed. Diagram syntax was checked with a Mermaid 10.9.1 parser (all diagram blocks in the package were re-rendered with Mermaid CLI 10.9.1 in Phase 2 (101 blocks, 0 errors)). The diagrams have **not been visually rendered or reviewed for layout**.
Notation: rounded nodes are external entities, rectangles are processes, cylinders are data stores. Labels on arrows are data flows.

## DFD-0 Level 0 context

```mermaid
flowchart LR
    USR(["Internal users"]) -- "requests, approvals, documents" --> ERP["ERP system"]
    ERP -- "screens, reports, notifications" --> USR
    CUS(["Customers"]) -- "orders, payments" --> ERP
    ERP -- "quotes, invoices, statements" --> CUS
    VEN(["Vendors"]) -- "invoices, delivery notes" --> ERP
    ERP -- "purchase orders, remittances" --> VEN
    BNK(["Banks and gateways"]) -- "statements, payment results" --> ERP
    ERP -- "payment instructions" --> BNK
    IDP(["Identity provider"]) -- "identity assertions" --> ERP
    TAX(["Tax authorities"]) -- "rules and rates" --> ERP
    ERP -- "returns, reports" --> TAX
```

## DFD-1 Level 1 main processes and stores

```mermaid
flowchart TB
    U(["Users"])
    P1["1 Manage access and organisation"]
    P2["2 Approvals and notifications"]
    P3["3 Procure to pay"]
    P4["4 Order to cash"]
    P5["5 Inventory"]
    P6["6 Finance and reporting"]
    P7["7 Audit and logging"]
    D1[("Users, roles, org")]
    D2[("Master data")]
    D3[("Documents")]
    D4[("Stock ledger")]
    D5[("General ledger")]
    D6[("Audit log")]
    D7[("Approval instances")]
    U --> P1 --> D1
    U --> P3
    U --> P4
    P3 --> D3
    P4 --> D3
    P3 --> P5
    P4 --> P5
    P5 --> D4
    P3 --> P6
    P4 --> P6
    P5 --> P6
    P6 --> D5
    P3 --> P2
    P4 --> P2
    P6 --> P2
    P2 --> D7
    P2 --> U
    D2 --> P3
    D2 --> P4
    D2 --> P5
    P1 --> P7
    P3 --> P7
    P4 --> P7
    P5 --> P7
    P6 --> P7
    P2 --> P7
    P7 --> D6
```

## DFD-2a Level 2 Procure-to-Pay

```mermaid
flowchart LR
    EMP(["Requester"]) -- "PR data" --> A["3.1 Requisition"]
    A -- "PR" --> S1[("Documents")]
    A -- "approval request" --> AP["Approval engine"]
    AP -- "decision" --> A
    A -- "approved PR" --> B["3.2 Purchase order"]
    V[("Vendor master")] --> B
    I[("Item master")] --> B
    B -- "PO" --> S1
    B -- "PO document" --> VEN(["Vendor"])
    VEN -- "goods and delivery note" --> C["3.3 Goods receipt"]
    S1 -- "open PO lines" --> C
    C -- "movements" --> ST[("Stock ledger")]
    C -- "GRN" --> S1
    VEN -- "invoice" --> D["3.4 Invoice and match"]
    S1 -- "PO and GRN" --> D
    D -- "approved invoice" --> E["3.5 Payables posting"]
    E -- "entries" --> GL[("General ledger")]
    E -- "open item" --> OI[("Open items")]
    OI --> F["3.6 Payment"]
    F -- "payment, allocations" --> GL
    F -- "instruction" --> BNK(["Bank"])
    F -- "remittance" --> VEN
```

## DFD-2b Level 2 Order-to-Cash

```mermaid
flowchart LR
    SE(["Sales Executive"]) -- "quote data" --> A["4.1 Quotation"]
    CM[("Customer master")] --> A
    A -- "quote" --> CUS(["Customer"])
    CUS -- "acceptance" --> B["4.2 Sales order"]
    B -- "credit query" --> FIN["Receivables"]
    B -- "reserve request" --> ST[("Stock ledger")]
    B -- "order" --> S1[("Documents")]
    S1 --> C["4.3 Delivery"]
    C -- "issue movements" --> ST
    C -- "cost entries" --> GL[("General ledger")]
    C --> D["4.4 Invoicing"]
    D -- "receivable entry" --> GL
    D -- "invoice" --> CUS
    D -- "open item" --> OI[("Open items")]
    CUS -- "payment" --> E["4.5 Receipt"]
    OI --> E
    E -- "entries" --> GL
```

## DFD-2c Level 2 Record-to-Report

```mermaid
flowchart LR
    SUBS[("Sub-ledger postings")] --> A["6.1 Posting engine"]
    MJ(["Accountant"]) -- "manual journal" --> B["6.2 Journals and approval"]
    B --> A
    RATE[("Currency rates")] --> A
    TAXC[("Tax codes")] --> A
    PER[("Periods")] -- "open or closed" --> A
    A --> GL[("General ledger")]
    GL --> C["6.3 Balances and summaries"]
    C --> SUM[("Account balances")]
    SUM --> D["6.4 Period close"]
    D -- "state" --> PER
    SUM --> E["6.5 Financial statements"]
    E --> FM(["Finance Manager and Auditor"])
```

## DFD-3 Data migration flow

Detailed steps are in [Data Migration Strategy](../04-System-Architecture/Data-Migration-Strategy.md).

```mermaid
flowchart LR
    LEG[("Legacy systems")] --> EX["Extract to files"]
    EX --> STG[("Staging files")]
    STG --> CL["Cleanse and map"]
    CL --> VAL["Validate-only load"]
    VAL -- "errors" --> CL
    VAL -- "clean" --> LD["Commit through import module"]
    LD --> ERP[("ERP database")]
    ERP --> REC["Reconcile counts, balances, stock"]
    LEG --> REC
    REC -- "mismatch" --> CL
    REC -- "match" --> SIGN(["Business sign-off"])
```

## DFD-4 Data synchronisation flow

```mermaid
flowchart LR
    DOM["Domain modules"] -- "business change and event, one transaction" --> DB[("Database with outbox")]
    DB --> WRK["Worker claims pending events"]
    WRK -- "adapter call with reference" --> EXT(["Partner"])
    EXT -- "result or webhook" --> IN["Inbound handler"]
    IN -- "idempotent update" --> DB
    WRK -- "failure" --> RT["Retry with backoff"]
    RT --> WRK
    RT -- "limit reached" --> DL[("Dead-letter, visible to admin")]
    DL -- "manual retry or cancel" --> WRK
    EXT2(["Rate source"]) -- "scheduled pull" --> RATE[("Currency rates")]
    RATE --> DOM
```

## Data stores summary

| Store | Owner module | Contents |
|---|---|---|
| Users, roles, org | Identity and Organisation | Accounts, roles, assignments, companies, branches |
| Master data | Master data | Vendors, customers, items, units, taxes, currencies |
| Documents | Procurement, Sales, Inventory | PR, PO, GRN, invoices, orders, deliveries |
| Stock ledger | Inventory | Append-only stock movements and balances |
| General ledger | Finance | Append-only ledger rows and summaries |
| Open items | Finance | AP and AR items and allocations |
| Approval instances | Platform | Chains, instances, decisions |
| Audit log | Platform | Append-only audit records |
