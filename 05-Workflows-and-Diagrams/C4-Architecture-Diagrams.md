# C4 Architecture Diagrams

Document status: Draft v0.1 (Stage 2 part 2). Provisional, as the architecture is ([ARD](../04-System-Architecture/ARD.md)). Diagram syntax was checked with a Mermaid 10.9.1 parser (all diagram blocks in the package were re-rendered with Mermaid CLI 10.9.1 in Phase 2 (101 blocks, 0 errors)). The diagrams have **not been visually rendered or reviewed for layout**.
Mermaid's C4 syntax is experimental, so these diagrams use flowcharts with C4 naming: person, system, container, component.
Level 1 and the first container and component views already exist in [System Architecture](../04-System-Architecture/System-Architecture.md) sections 1 to 3. They are not repeated. This file adds the system context with roles, the container view with technologies marked as open, and component views of the Finance and Procurement modules.

## Level 1 System context (with roles)

```mermaid
flowchart TB
    FIN(["Finance staff<br/>Finance Manager, Accountant"])
    OPS(["Operations staff<br/>Procurement, Warehouse, Sales"])
    ADM(["Administrators<br/>Super Admin, Org Admin, Branch Admin"])
    AUD(["Auditor, Read-only User"])
    EMP(["Employees and approvers"])
    ERP["ERP platform<br/>Multi-company, multi-branch ERP"]
    IDP["Identity provider<br/>TBD, OQ-023"]
    MAIL["Email and SMS provider<br/>TBD, OQ-017"]
    BANK["Banks and payment gateways<br/>TBD, OQ-017"]
    TAX["Tax authorities<br/>TBD, OQ-007"]
    FIN -- "post, approve, close, report" --> ERP
    OPS -- "requisitions, orders, stock, delivery" --> ERP
    ADM -- "users, roles, structure, settings" --> ERP
    AUD -- "read, export audit and reports" --> ERP
    EMP -- "requests and approvals" --> ERP
    ERP -- "authenticate via OIDC" --> IDP
    ERP -- "send notifications" --> MAIL
    ERP -- "payment files and results" --> BANK
    ERP -- "returns and e-invoices, not MVP" --> TAX
```

## Level 2 Containers (technology open)

```mermaid
flowchart TB
    U(["Users"]) --> WEB["Web client<br/>Responsive single-page or server-rendered app, TBD"]
    WEB -- "HTTPS JSON /api/v1" --> GW["Gateway and WAF<br/>TLS, rate limiting"]
    GW --> APP["ERP application<br/>Modular monolith, stateless, TBD language"]
    APP -- "SQL, row-level security" --> DB[("Relational database<br/>TBD, PostgreSQL proposed")]
    APP -- "private links" --> OBJ[("Object storage<br/>Attachments and exports")]
    APP -- "sessions, permissions" --> CCH[("Cache<br/>Optional")]
    WRK["Workers<br/>Jobs, outbox publisher, imports, reports"] -- "SQL" --> DB
    WRK --> OBJ
    WRK -- "adapters" --> EXT["External providers"]
    DB -- "replication" --> RPL[("Read replica")]
    APP -- "report queries" --> RPL
    APP -- "OIDC" --> IDP["Identity provider"]
    SEC["Secret and key store"] -.-> APP
    SEC -.-> WRK
    MON["Monitoring and logging"] -.-> APP
    MON -.-> WRK
```

## Level 3 Components: Finance module

```mermaid
flowchart TB
    API["Finance API endpoints"] --> POL["Policy and scope check"]
    POL --> JRN["Journal service<br/>draft, submit, post, reverse"]
    POL --> PAY["Payment service<br/>create, allocate, release, reverse"]
    POL --> PRD["Period service<br/>open, close, reopen, checklist"]
    POL --> COA["Chart of accounts service"]
    POL --> RPT["Reporting service<br/>trial balance, statements"]
    PST["Posting engine<br/>balance check, period check, rules, rounding"]
    JRN --> PST
    PAY --> PST
    SUB["Posting requests from Procurement, Inventory, Sales"] --> PST
    PST --> RUL["Posting rules and account mapping"]
    PST --> CUR["Currency and rate service"]
    PST --> TAX["Tax service"]
    PST --> LED[("Ledger entries, append-only")]
    PST --> SUM[("Account balances")]
    PAY --> OI[("Open items and allocations")]
    PRD --> PER[("Fiscal periods")]
    RPT --> SUM
    RPT --> LED
    JRN --> APR["Approval engine interface"]
    PAY --> APR
    PST --> AUD["Audit writer"]
    PST --> OUT["Outbox"]
```

## Level 3 Components: Procurement module

```mermaid
flowchart TB
    API["Procurement API endpoints"] --> POL["Policy and scope check"]
    POL --> VND["Vendor service<br/>activate, block, bank change approval"]
    POL --> PRQ["Requisition service"]
    POL --> PO["Purchase order service<br/>create, revise, cancel, send"]
    POL --> GRN["Goods receipt service<br/>partial, reverse"]
    POL --> INV["Invoice service<br/>entry, duplicate check, match"]
    PRQ --> APR["Approval engine interface"]
    PO --> APR
    INV --> APR
    PO --> PRQ
    GRN --> PO
    INV --> GRN
    INV --> MTH["Match engine<br/>tolerance rules from settings"]
    GRN --> IFI["Inventory interface<br/>post movements"]
    INV --> IFF["Finance interface<br/>request posting"]
    PO --> NUM["Numbering"]
    GRN --> NUM
    VND --> MD["Master data interface"]
    PO --> AUD["Audit writer"]
    GRN --> AUD
    INV --> AUD
    PO --> OUT["Outbox"]
```

## Notes

1. Component views for Inventory, Sales, Identity and Platform follow the same shape and are not drawn yet.
2. Boundaries follow the module rules in [ARD](../04-System-Architecture/ARD.md) section 5. Procurement reaches Finance and Inventory only through interfaces.
3. Technology labels are provisional (ADR-003, OQ-024).
