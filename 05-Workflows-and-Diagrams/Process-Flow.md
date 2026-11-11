# Process Flow Diagrams

Document status: Draft v0.1 (Stage 2 part 2). Proposed. Diagram syntax was checked with a Mermaid 10.9.1 parser (all diagram blocks in the package were re-rendered with Mermaid CLI 10.9.1 in Phase 2 (101 blocks, 0 errors)). The diagrams have **not been visually rendered or reviewed for layout**.
One flow per workflow in [Cross-Module Workflows](Cross-Module-Workflows.md) (WF-01 to WF-14). These flows include exception paths and replace the simpler sketches in the [To-Be Analysis](../02-Business-Analysis/To-Be-Analysis.md).
Post-MVP flows (WF-04, 05, 08, 09, 10, 11, 13, 14) are outlines.

## WF-01 Procure-to-Pay

```mermaid
flowchart TD
    A["Requester creates PR"] --> B["Submit PR"]
    B --> C{"PR approved?"}
    C -- "Rejected or returned" --> A
    C -- "Approved" --> D["Procurement creates PO for active vendor"]
    D --> E{"PO approved by value?"}
    E -- "Rejected" --> D
    E -- "Approved" --> F["PO sent to vendor"]
    F --> G["Warehouse posts GRN"]
    G --> H{"Full receipt?"}
    H -- "Partial" --> G2["PO stays Partly received"] --> G
    H -- "Full" --> I["Accountant enters vendor invoice"]
    G2 --> I
    G --> G3["Receipt reversal refused while an invoice matches it"]
    I --> J{"Duplicate invoice?"}
    J -- "Yes" --> J2["Refuse, or flag for override"] --> J3{"Finance Manager approves override?"}
    J3 -- "No" --> I
    J3 -- "Yes" --> K
    J -- "No" --> K{"Three-way match within tolerance?"}
    K -- "Yes" --> K1["System approves invoice, no human step"] --> L
    K -- "No" --> K2["Invoice Held for Finance Manager"] --> K3{"Finance Manager decision"}
    K3 -- "Approve variance" --> L["Post payable and ledger entry"]
    K3 -- "Reject" --> K4["Vendor asked for corrected invoice or credit note"] --> I
    L --> M["Accountant prepares payment and allocates"]
    M --> M1["Submit payment"] --> N{"Approved? second approver above limit"}
    N -- "Rejected" --> M
    N -- "Approved" --> O{"Releaser differs from preparer and approver, step-up done?"}
    O -- "No" --> O2["Refused DUTY-002 or STEP-001"] --> O
    O -- "Yes" --> O3{"First payment to new bank account inside hold?"}
    O3 -- "Yes" --> O4["Refused HOLD-001, wait for hold end"] --> O
    O3 -- "No" --> O5["Released: posted to bank clearing, instruction queued"]
    O5 --> P{"Bank outcome"}
    P -- "Rejected" --> P2["Payment Failed: posting reversed by system, invoice reopens"] --> M
    P -- "Accepted" --> P1["Payment Settled"] --> Q{"Fully paid?"}
    Q -- "No" --> Q2["Invoice Part-paid"] --> M
    Q -- "Yes" --> R["Invoice Paid, process complete"]
    F --> X["PO cancellation: remaining quantity cancelled"]
```

## WF-02 Order-to-Cash

```mermaid
flowchart TD
    A["Quotation created and sent"] --> B{"Customer accepts and quote valid?"}
    B -- "No or expired" --> A2["Revise or close"]
    B -- "Yes" --> C["Convert to sales order"]
    C --> D{"Credit limit OK?"}
    D -- "Exceeded" --> D2["Finance approval"]
    D2 -- "Rejected" --> D3["Order back to draft"]
    D2 -- "Approved" --> E
    D -- "OK" --> E{"Stock available?"}
    E -- "Short" --> E2["Partial reserve and backorder"] --> F
    E -- "Yes" --> F["Confirm order and reserve stock"]
    F --> G["Warehouse posts delivery"]
    G --> H{"Delivered in full?"}
    H -- "Partial" --> G
    H -- "Full" --> I["Accountant invoices delivered quantity"]
    I --> J["Post receivable and ledger entry"]
    J --> K["Customer pays"]
    K --> L{"Amount equals open?"}
    L -- "Less" --> L2["Part-paid"] --> K
    L -- "More" --> L3["Excess held as customer credit"] --> M
    L -- "Equal" --> M["Invoice Paid, complete"]
    F --> X["Order cancellation: release reservations"]
    G --> Y["Failed delivery: stock returns"]
```

## WF-03 Record-to-Report

```mermaid
flowchart TD
    A["Documents post from sub-ledgers"] --> L["General ledger"]
    B["Maker prepares manual journal"] --> C{"Balanced and period open?"}
    C -- "No" --> B
    C -- "Yes" --> D["Checker approves"]
    D -- "Rejected" --> B
    D -- "Approved" --> L
    L --> E["Period end: open close checklist"]
    E --> F{"Items outstanding?"}
    F -- "Yes" --> G["Post, cancel or carry forward"] --> E
    F -- "No" --> H{"Reconciliations pass?"}
    H -- "No" --> I["Investigate differences"] --> E
    H -- "Yes" --> J["Soft close then close period"]
    J --> K["Trial balance, income statement, balance sheet"]
    J --> Y{"Last period of year?"}
    Y -- "Yes" --> Z["Year-end close and opening balances"]
    J --> R["Reopen with reason and higher authority"] --> E
```

## WF-04 Hire-to-Retire (outline)

```mermaid
flowchart LR
    A["Job requisition"] --> B{"Approved?"} 
    B -- "No" --> A
    B -- "Yes" --> C["Recruit and select"] --> D["Offer"] --> E["Onboarding checklist"]
    E --> F["Employee record and user created"] --> G["Attendance, leave, payroll"]
    G --> H["Performance and development"] --> I{"Separation?"}
    I -- "No" --> G
    I -- "Yes" --> J["Offboarding clearance and access revoked"] --> K["Final settlement and record retained"]
```

## WF-05 Plan-to-Produce (outline)

```mermaid
flowchart LR
    A["Demand and forecast"] --> B["MRP run"] --> C["Planned orders"] --> D["Release work order"]
    D --> E["Issue materials"] --> F["Produce and track WIP"] --> G{"Quality inspection passed?"}
    G -- "No" --> H["Defect handling, rework or scrap"] --> F
    G -- "Yes" --> I["Receive finished goods"] --> J["Cost and close work order"]
```

## WF-06 Inventory-to-Fulfillment

```mermaid
flowchart TD
    A["Receipt via GRN or receipt document"] --> S[("Stock ledger")]
    B["Confirmed sales order"] --> C["Reserve stock"]
    C --> D{"Available?"}
    D -- "No" --> D2["Backorder"]
    D -- "Yes" --> S
    E["Transfer dispatched"] --> F["In transit"] --> G["Transfer received full or part"] --> S
    H["Delivery posted"] --> I{"Stock sufficient under row lock?"}
    I -- "No" --> I2["Refuse with available quantity"]
    I -- "Yes" --> S
    J["Count result or loss in transit"] --> K["Adjustment created with reason, impact shown"] --> K1["Creator submits"]
    K1 --> L{"Inventory Manager decision, not creator or submitter"}
    L -- "Rejected" --> K
    L -- "Approved" --> M{"Value above threshold?"}
    M -- "No" --> Z["System posts stock and ledger"]
    M -- "Yes" --> N{"Finance Manager decision"}
    N -- "Rejected" --> K
    N -- "Approved" --> Z
    Z --> S
    S --> V["Valuation run and ledger posting"]
    S --> R["Stock reports"]
```

## WF-07 Request-to-Approval

```mermaid
flowchart TD
    A["Requester submits document"] --> B{"Document valid?"}
    B -- "No" --> A
    B -- "Yes" --> C["Lock document and select chain"]
    C --> D{"Eligible approver exists?"}
    D -- "No" --> D2["Submit refused, tell administrator"]
    D -- "Yes" --> E["Notify approver, skip the creator"]
    E --> F{"Decision"}
    F -- "Approve" --> G{"More steps?"}
    G -- "Yes" --> E
    G -- "No" --> H["Document Approved, notify requester"]
    F -- "Reject" --> I["Rejected with reason"]
    F -- "Return" --> J["Returned for change"]
    I --> K["Requester revises"]
    J --> K
    K --> L["Resubmit, new round, history kept"] --> C
    E --> T{"Overdue?"}
    T -- "Yes" --> U["Escalate to next approver"] --> E
```

## WF-08 Expense-to-Reimbursement (outline)

```mermaid
flowchart LR
    A["Employee submits claim with receipts"] --> B{"Manager approves?"}
    B -- "No" --> C["Rejected, employee may resubmit"] --> A
    B -- "Yes" --> D{"Finance policy check passes?"}
    D -- "No" --> C
    D -- "Yes" --> E["Payable created"] --> F["Payment through payment engine"] --> G["Ledger posting and claim Paid"]
```

## WF-09 Asset Acquisition-to-Disposal (outline)

```mermaid
flowchart LR
    A["Capital PR and PO"] --> B["Receive and invoice"] --> C["Register asset"] --> D["Depreciation schedule runs"]
    D --> E{"Transfer, repair or dispose?"}
    E -- "Transfer" --> F["Update location and custodian"] --> D
    E -- "Dispose" --> G["Approve disposal"] --> H["Post gain or loss, retire asset"]
```

## WF-10 Lead-to-Customer (outline)

```mermaid
flowchart LR
    A["Lead captured"] --> B{"Qualified?"}
    B -- "No" --> L["Lost or nurture"]
    B -- "Yes" --> C["Opportunity"] --> D["Quotation"] --> E{"Won?"}
    E -- "No" --> L
    E -- "Yes" --> F["Convert to customer"] --> G["Order-to-Cash WF-02"]
```

## WF-11 Budget-to-Actual (outline)

```mermaid
flowchart LR
    A["Budget prepared"] --> B{"Approved?"}
    B -- "No" --> A
    B -- "Yes" --> C["Budget active"] --> D["Spend request PR, PO, invoice"]
    D --> E{"Within budget?"}
    E -- "Yes" --> F["Proceed"]
    E -- "No" --> G["Warn, block or route for approval"]
    F --> H["Actual versus budget report"]
    G --> H
    H --> I["Revise budget"] --> A
```

## WF-12 Return-to-Refund

```mermaid
flowchart TD
    A["Return recorded against delivery"] --> B{"Quantity within delivered and window OK?"}
    B -- "Above delivered" --> B2["Refused"]
    B -- "Outside window" --> B3{"Sales Manager decision"}
    B3 -- "Rejected" --> B4["Return Rejected, process ends"]
    B3 -- "Approved" --> C
    B -- "OK" --> C["Warehouse receives and inspects"]
    C --> D{"Disposition"}
    D -- "Restock" --> E["Stock in"]
    D -- "Scrap" --> F["Write off"]
    D -- "Return to vendor" --> G["Vendor return document in WF-01, separate debit note"]
    E --> H["Accountant creates credit note within invoiced amount"]
    F --> H
    G --> H
    H --> I{"Finance Manager decision"}
    I -- "Rejected" --> H
    I -- "Approved" --> I2["Credit note posted"]
    I2 --> J{"Customer already paid?"}
    J -- "No" --> K["Credit reduces open invoice"]
    J -- "Yes" --> L{"Refund or hold credit?"}
    L -- "Hold" --> M["Unallocated customer credit"]
    L -- "Refund" --> N{"Refund within amount received?"}
    N -- "No" --> N2["Refused"]
    N -- "Yes" --> O["Refund runs through the payment engine: prepare, approve, release by three users"]
    O --> P{"Bank outcome"}
    P -- "Rejected" --> Q["Refund Failed, posting reversed, credit stays held, new refund may be created"]
    P -- "Accepted" --> R["Refund Settled, process complete"]
```

## WF-13 Contract-to-Renewal (outline)

```mermaid
flowchart LR
    A["Draft contract"] --> B["Review and approve"] --> C["Active"] --> D["Track obligations and link POs"]
    D --> E{"Nearing expiry?"}
    E -- "No" --> D
    E -- "Yes" --> F{"Decision"}
    F -- "Renew" --> G["Renewed"] --> D
    F -- "Terminate" --> H["Terminated"]
    F -- "Lapse" --> I["Expired"]
```

## WF-14 Issue-to-Resolution (outline)

```mermaid
flowchart LR
    A["Ticket logged"] --> B["Categorise and assign"] --> C["SLA timer starts"] --> D["Work"] --> E{"Resolved?"}
    E -- "No" --> F{"SLA at risk?"}
    F -- "Yes" --> G["Escalate"] --> D
    F -- "No" --> D
    E -- "Yes" --> H["Requester confirms"] --> I{"Accepted?"}
    I -- "No" --> J["Reopen"] --> D
    I -- "Yes" --> K["Closed"]
```

## Phase 2 note (v0.2)

The process flows for WF-04, WF-05, WF-08, WF-09 and WF-13 are in the Lifecycles folder (`Lifecycles/WF-nn-*.md`). WF-01, WF-06 and WF-12 diagrams follow the [Authoritative Workflow Definitions](Authoritative-Workflow-Definitions.md).
