# Swimlane Diagrams

Document status: Draft v0.1 (Stage 2 part 2). Proposed. Diagram syntax was checked with a Mermaid 10.9.1 parser (all diagram blocks in the package were re-rendered with Mermaid CLI 10.9.1 in Phase 2 (101 blocks, 0 errors)). The diagrams have **not been visually rendered or reviewed for layout**.
Mermaid has no native swimlane type. Each lane is a subgraph named after the actor or system. Arrows show hand-offs between lanes.
Six critical MVP workflows are covered: WF-01, WF-02, WF-03, WF-06, WF-07, WF-12. Steps match [Cross-Module Workflows](Cross-Module-Workflows.md).

## WF-01 Procure-to-Pay

```mermaid
flowchart LR
    subgraph REQ["Requester"]
        R1["Create PR"]
        R2["Revise if rejected"]
    end
    subgraph APR["Approvers"]
        A1["Approve PR"]
        A2["Approve PO"]
    end
    subgraph PRC["Procurement"]
        P1["Create PO"]
        P2["Send PO to vendor"]
    end
    subgraph WH["Warehouse"]
        W1["Post GRN"]
    end
    subgraph ACC["Accountant"]
        C1["Enter invoice"]
        C2["Post payable"]
        C3["Prepare and submit payment"]
    end
    subgraph FIN["Finance Manager approver"]
        F1["Decide variance or duplicate override"]
        F2["Approve payment"]
    end
    subgraph REL["Finance Manager releaser, different user"]
        F3["Release payment with step-up"]
    end
    subgraph SYS["System"]
        S1["Duplicate check"]
        S2["Three-way match"]
        S5["Approve invoice within tolerance"]
        S3["Ledger and stock postings"]
        S6["Reverse posting and reopen invoice"]
        S4["Notifications"]
    end
    subgraph BNK["Bank"]
        B1["Execute payment"]
    end
    R1 --> A1
    A1 -- "rejected" --> R2 --> A1
    A1 -- "approved" --> P1 --> A2
    A2 --> P2 --> W1
    W1 --> S3
    W1 --> C1 --> S1 --> S2
    S2 -- "within tolerance" --> S5 --> C2
    S2 -- "variance" --> F1
    F1 -- "approve" --> C2
    F1 -- "reject" --> C1
    C2 --> S3
    C2 --> C3 --> F2 --> F3 --> S3
    F3 --> B1
    B1 -- "accepted" --> S3
    B1 -- "failed" --> S6 --> C3
    S3 --> S4
```

## WF-02 Order-to-Cash

```mermaid
flowchart LR
    subgraph SAL["Sales Executive"]
        E1["Quote"]
        E2["Create and confirm order"]
    end
    subgraph SMG["Sales Manager"]
        M1["Approve discount if needed"]
    end
    subgraph FIN["Finance Manager"]
        F1["Approve credit breach"]
    end
    subgraph WH["Warehouse"]
        W1["Post delivery"]
    end
    subgraph ACC["Accountant"]
        C1["Invoice delivered quantity"]
        C2["Record and allocate receipt"]
    end
    subgraph CUS["Customer"]
        U1["Accept quote"]
        U2["Pay"]
    end
    subgraph SYS["System"]
        S1["Credit check"]
        S2["Reserve stock"]
        S3["Ledger and stock postings"]
    end
    E1 --> M1 --> U1 --> E2 --> S1
    S1 -- "exceeded" --> F1 --> S2
    S1 -- "ok" --> S2
    S2 --> W1 --> S3
    W1 --> C1 --> S3
    C1 --> U2 --> C2 --> S3
```

## WF-03 Record-to-Report

```mermaid
flowchart LR
    subgraph ACC["Accountant"]
        A1["Prepare journal"]
        A2["Resolve close items"]
    end
    subgraph FIN["Finance Manager"]
        F1["Approve journal"]
        F2["Run checklist and reconcile"]
        F3["Close period"]
        F4["Review statements"]
    end
    subgraph SUB["Sub-ledgers"]
        L1["Post AP, AR, stock, payments"]
    end
    subgraph SYS["System"]
        S1["Validate balance and period"]
        S2["General ledger"]
        S3["Checklist and reconciliation checks"]
        S4["Statements"]
    end
    subgraph AUD["Auditor"]
        U1["Review audit trail and statements"]
    end
    L1 --> S2
    A1 --> S1 --> F1 --> S2
    S2 --> F2 --> S3
    S3 -- "items open" --> A2 --> S3
    S3 -- "clean" --> F3 --> S4 --> F4 --> U1
```

## WF-06 Inventory-to-Fulfillment

```mermaid
flowchart LR
    subgraph WH["Warehouse Staff"]
        W1["Receive goods"]
        W2["Dispatch transfer"]
        W3["Deliver order"]
        W4["Count stock and create adjustment"]
    end
    subgraph DST["Destination warehouse"]
        D1["Receive transfer"]
    end
    subgraph INV["Inventory Manager"]
        I1["Submit adjustment"]
        I2["Approve, not the creator or submitter"]
    end
    subgraph FIN["Finance Manager"]
        F1["Approve above threshold"]
        F2["Run valuation"]
    end
    subgraph SYS["System"]
        S1["Stock ledger and balances"]
        S2["Lock and availability check"]
        S3["Ledger postings"]
        S4["Post adjustment"]
    end
    W1 --> S1
    W2 --> S2 --> S1
    S1 --> D1 --> S1
    W3 --> S2
    W4 --> I1 --> I2
    I2 -- "rejected" --> W4
    I2 -- "at or below threshold" --> S4
    I2 -- "above threshold" --> F1
    F1 -- "rejected" --> W4
    F1 -- "approved" --> S4
    S4 --> S1
    S1 --> S3
    F2 --> S3
```

## WF-07 Request-to-Approval

```mermaid
flowchart LR
    subgraph REQ["Requester"]
        Q1["Submit document"]
        Q2["Revise and resubmit"]
    end
    subgraph ENG["Approval engine"]
        E1["Lock, select chain"]
        E2["Route to eligible approver"]
        E3["Escalate if overdue"]
        E4["Record decision"]
    end
    subgraph APR["Approver or delegate"]
        A1["Decide"]
    end
    subgraph NTF["Notification service"]
        N1["Notify approver"]
        N2["Notify requester"]
    end
    subgraph AUD["Audit"]
        U1["Write audit record"]
    end
    Q1 --> E1 --> E2 --> N1 --> A1
    E2 --> E3 --> E2
    A1 --> E4 --> U1
    E4 -- "approved, more steps" --> E2
    E4 -- "final approval" --> N2
    E4 -- "rejected or returned" --> N2 --> Q2 --> Q1
```

## WF-12 Return-to-Refund

```mermaid
flowchart LR
    subgraph SAL["Sales Executive"]
        S1["Record return"]
    end
    subgraph SMG["Sales Manager"]
        M1["Decide outside window"]
    end
    subgraph WH["Warehouse"]
        W1["Receive and inspect"]
    end
    subgraph ACC["Accountant"]
        C1["Create credit note"]
        C2["Prepare refund if customer paid and refund chosen"]
    end
    subgraph FIN["Finance Manager approver"]
        F1["Decide credit note"]
        F2["Approve refund"]
    end
    subgraph REL["Finance Manager releaser, different user"]
        F3["Release refund with step-up"]
    end
    subgraph SYS["System"]
        Y1["Check quantity and amount limits"]
        Y2["Stock movement"]
        Y3["Ledger postings"]
        Y4["Reverse posting, keep credit held"]
    end
    subgraph BNK["Bank"]
        B1["Execute refund"]
    end
    S1 --> Y1
    Y1 -- "outside window" --> M1
    M1 -- "approved" --> W1
    M1 -- "rejected" --> S1
    Y1 -- "ok" --> W1
    W1 --> Y2 --> C1 --> F1
    F1 -- "approved" --> Y3
    F1 -- "rejected" --> C1
    Y3 -- "customer paid, refund chosen" --> C2 --> F2 --> F3 --> B1
    B1 -- "failed" --> Y4
    B1 -- "accepted" --> Y3
```

## Notes

1. Lanes show responsibility, not organisational reporting. A user may hold more than one role, but segregation rules still apply (RBAC Matrix section 5).
2. Where a diagram shows one System lane, the individual system steps are in the [Sequence Diagrams](Sequence-Diagrams.md).

## Phase 2 note (v0.2)

The swimlanes for WF-04, WF-05, WF-08, WF-09 and WF-13 are in the Lifecycles folder (`Lifecycles/WF-nn-*.md`). WF-01, WF-06 and WF-12 diagrams follow the [Authoritative Workflow Definitions](Authoritative-Workflow-Definitions.md).
