# System Architecture

Document status: Draft v0.1 (Stage 2). Provisional, see [ARD](ARD.md).
Diagrams here are the working set. The full diagram catalogue (swimlane, sequence, state, DFD, C4) is in [folder 05](../05-Workflows-and-Diagrams/Process-Flow.md).

## 1. System context

```mermaid
flowchart LR
    U[Business users<br/>18 roles] -->|HTTPS| ERP[(ERP platform)]
    A[Administrators] -->|HTTPS| ERP
    AU[Auditors] -->|HTTPS read only| ERP
    EX[External integrators] -->|HTTPS API| ERP
    ERP -->|email, SMS| NP[Notification providers]
    ERP <-->|OIDC| IDP[Identity provider]
    ERP <-->|files, API| BK[Banks and gateways]
    ERP -->|returns, e-invoices| TX[Tax authorities]
    LG[Legacy systems] -->|migration files| ERP
```

External parties are TBD except where marked in the [Business Context](../02-Business-Analysis/Business-Context.md).

## 2. Container view

```mermaid
flowchart TB
    subgraph Client
        WEB[Web client<br/>responsive, accessible]
    end
    subgraph Edge
        GW[Gateway and WAF<br/>TLS, rate limit, routing]
    end
    subgraph App[Application tier - stateless, scaled horizontally]
        API[ERP application<br/>modular monolith]
    end
    subgraph Workers[Background tier]
        WRK[Workers<br/>jobs, outbox publisher, imports, reports]
    end
    subgraph Data
        DB[(Relational database<br/>primary + standby)]
        RPL[(Read replica<br/>reports)]
        OBJ[(Object storage<br/>attachments, exports)]
        CCH[(Cache<br/>sessions, permissions)]
    end
    subgraph Ext[External]
        MAIL[Email and SMS]
        IDP[Identity provider]
        BANK[Bank and gateway adapters]
    end
    WEB --> GW --> API
    API --> DB
    API --> CCH
    API --> OBJ
    API -->|outbox rows| DB
    WRK --> DB
    WRK --> OBJ
    WRK --> MAIL
    WRK --> BANK
    API --> IDP
    DB --> RPL
    API -.reports.-> RPL
```

## 3. Component view of the application

```mermaid
flowchart TB
    subgraph Edge[API layer]
        CTL[Controllers and request validation]
        AUTHN[Authentication]
        POL[Policy component<br/>role + scope]
        IDEM[Idempotency handler]
    end
    subgraph Domain[Domain modules]
        ORG[Organisation]
        MD[Master data]
        FIN[Finance]
        PRC[Procurement]
        INV[Inventory]
        SAL[Sales]
    end
    subgraph Platform[Platform services]
        APR[Approval engine]
        NUM[Numbering]
        AUD[Audit writer]
        NTF[Notification service]
        IMP[Import and export]
        ATT[Attachments]
        CFG[Settings]
        EVT[Outbox]
    end
    CTL --> AUTHN --> POL --> IDEM
    IDEM --> PRC
    IDEM --> SAL
    IDEM --> INV
    IDEM --> FIN
    IDEM --> ORG
    PRC --> INV
    PRC --> FIN
    SAL --> INV
    SAL --> FIN
    INV --> FIN
    PRC --> APR
    SAL --> APR
    FIN --> APR
    INV --> APR
    Domain --> NUM
    Domain --> AUD
    Domain --> EVT
    EVT --> NTF
    Domain --> CFG
    Domain --> MD
```

## 4. High-level ERP module architecture

```mermaid
flowchart LR
    subgraph Core[Platform core - MVP]
        IAM[M-002..M-004 Identity and RBAC]
        ORG2[M-005..M-007, M-016 Organisation and people core]
        PLT[M-008..M-015 Platform services]
    end
    subgraph Fin[Finance - MVP]
        F1[M-030..M-036 Ledger, COA, journals, bank]
        F2[M-040..M-047 Tax, currency, periods, close, statements, payments]
    end
    subgraph Ops[Operations - MVP]
        P[M-049..M-060 Procure-to-Pay]
        I[M-061..M-075 Inventory]
        S[M-079..M-088 Order-to-Cash]
    end
    subgraph Later[Later releases]
        HR[HR depth R2]
        PRJ[Projects R3]
        MFG[Manufacturing R4]
        BI[BI R3]
    end
    Core --> Fin
    Core --> Ops
    Ops --> Fin
    Fin -.->|dimensions reserved| Later
    Ops -.->|stock and party data| Later
```

## 5. Request lifecycle (summary)

1. The client calls the API over TLS. The gateway applies rate limits.
2. The application authenticates the session or token, sets tenant and company context for the database session.
3. The policy component checks permission and scope for the action and for the target record.
4. The idempotency handler checks the key for money and stock operations.
5. The domain module runs the operation in one database transaction: validations, document change, stock or ledger rows, audit record, outbox event.
6. On commit, the response returns. Workers publish outbox events to notifications and integrations.
7. Failures return a standard error with a correlation id.

Detailed sequence diagrams per critical flow are in [Sequence Diagrams](../05-Workflows-and-Diagrams/Sequence-Diagrams.md).

## 6. Posting design for cross-module operations

| Operation | Rows written in one transaction |
|---|---|
| Goods receipt (GRN) | GRN header and lines, stock movements, PO line received quantity, ledger entry (stock and goods-received-not-invoiced, if configured), audit, outbox event |
| Purchase invoice posting | Invoice status, payable open item, ledger entry, match result, audit, outbox event |
| Vendor payment release | Payment, allocations, bank account entry, ledger entry, invoice status, idempotency record, audit, outbox event |
| Delivery | Delivery header and lines, stock movements, reservation consumption, ledger entries (cost of goods), order line delivered quantity, audit, outbox |
| Sales invoice posting | Invoice, receivable open item, tax lines, ledger entry, audit, outbox |
| Manual journal posting | Journal, ledger rows, approval link, audit, outbox |

If any row fails, none is committed (NFR-021).

## 7. Open points

| Point | Question |
|---|---|
| Whether branch scope is part of the tenancy key or only an access scope | OQ-019 and RBAC design |
| Reporting load on the primary | OQ-015 |
| Gateway product and web application firewall | OQ-016 |

## Phase 2 notes (v0.2)

1. Tenancy: the recommended model is ADR-017 (PENDING APPROVAL). Every container diagram in this document assumes tenant_id on every business row and database row-level security.
2. Authorisation: the policy component now also enforces role tiers, four-eyes grants, step-up and controlled-change routing (ADR-022, RBAC Matrix 8A).
3. The component view in section 3 omits Identity and Reporting. This is recorded as a known gap (Diagram Consistency Report, D13) and is not changed here.
