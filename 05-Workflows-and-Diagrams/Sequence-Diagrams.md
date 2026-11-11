# Sequence Diagrams

Document status: Draft v0.1 (Stage 2 part 2). Proposed. Diagram syntax was checked with a Mermaid 10.9.1 parser (all diagram blocks in the package were re-rendered with Mermaid CLI 10.9.1 in Phase 2 (101 blocks, 0 errors)). The diagrams have **not been visually rendered or reviewed for layout**.
Participants use the components from [System Architecture](../04-System-Architecture/System-Architecture.md). Product names are not used because the stack is undecided (OQ-024).

Contents: SEQ-01 authentication, SEQ-02 RBAC authorization, SEQ-03 approval, SEQ-04 goods receipt, SEQ-05 payment release, SEQ-06 sales order confirmation, SEQ-07 API integration with outbox, SEQ-08 error handling and recovery, SEQ-09 period close, SEQ-10 stock transfer, SEQ-11 return, credit note and refund.

## SEQ-01 Authentication (with lockout, MFA and refresh)

```mermaid
sequenceDiagram
    autonumber
    actor U as User
    participant W as Web client
    participant G as Gateway
    participant A as Auth service
    participant D as Database
    participant I as Identity provider
    U->>W: Enter email and password
    W->>G: POST /auth/login
    G->>G: Rate limit check
    G->>A: Forward
    A->>D: Load user and lock state
    alt Account locked
        A-->>W: 423 AUTH-002
    else Credentials invalid
        A->>D: Increment failures, lock at limit
        A->>D: Audit AUD-002
        A-->>W: 401 AUTH-001, generic message
    else Credentials valid
        alt MFA required
            A-->>W: MFA challenge
            U->>W: Enter code
            W->>A: POST /auth/mfa/verify
            A->>A: Verify code
        end
        A->>D: Create session, audit AUD-001
        A-->>W: Access token and refresh token
    end
    Note over W,A: SSO alternative
    W->>I: Redirect for sign-in
    I-->>W: Code
    W->>A: GET /auth/sso/callback
    A->>I: Validate token, issuer, audience, nonce
    A->>D: Map subject to user, create session
    Note over W,A: Refresh
    W->>A: POST /auth/refresh with refresh token
    A->>D: Check session not revoked, rotate token
    alt Reuse of an old refresh token
        A->>D: Revoke session
        A-->>W: 401 AUTH-005
    else Valid
        A-->>W: New access token
    end
```

## SEQ-02 RBAC authorization of a request

```mermaid
sequenceDiagram
    autonumber
    participant C as Client
    participant G as Gateway
    participant P as Policy component
    participant S as Session store
    participant M as Domain module
    participant D as Database
    participant U as Audit
    C->>G: Request with access token
    G->>P: Authenticated call
    P->>S: Validate session, load assignments from cache
    alt Session invalid
        P-->>C: 401
    else Permission missing
        P->>U: Audit AUD-015
        P-->>C: 403 AUTHZ-001
    else Permission held
        P->>M: Call with tenant, company, scope context
        M->>D: Set session context, query with scope predicates
        alt Record outside scope
            D-->>M: No row
            M-->>C: 404 NOTF-001
        else Record in scope
            D-->>M: Row
            M->>P: Check state, authority, segregation rules
            alt Rule violated
                M-->>C: 409 or 403 with code
            else Allowed
                M->>D: Change, audit and outbox in one transaction
                M-->>C: Success
            end
        end
    end
```

## SEQ-03 Document approval with delegation and escalation

```mermaid
sequenceDiagram
    autonumber
    actor R as Requester
    participant M as Document module
    participant E as Approval engine
    participant N as Notification service
    actor A as Approver
    actor X as Delegate
    participant D as Database
    R->>M: Submit document
    M->>E: Start approval, document id and value
    E->>D: Select chain, create instance, lock document
    E->>E: Choose first eligible approver, skip creator
    E->>N: Notify approver
    alt Delegation active
        N-->>X: Notice to delegate
        X->>E: Decision with on-behalf-of approver
    else No delegation
        N-->>A: Notice
        A->>E: Decision
    end
    alt Approve and more steps
        E->>D: Record decision, move to next step
    else Final approve
        E->>D: Record decision, set Approved
        E->>M: Document approved event
        E->>N: Notify requester
    else Reject or return
        E->>D: Record decision with reason
        E->>N: Notify requester
    end
    Note over E,A: Overdue step triggers escalation to next approver
```

## SEQ-04 Goods receipt posting

```mermaid
sequenceDiagram
    autonumber
    actor W as Warehouse Staff
    participant API as Procurement module
    participant IN as Inventory module
    participant FN as Finance module
    participant NU as Numbering
    participant D as Database
    W->>API: POST goods-receipts with Idempotency-Key
    API->>D: Check idempotency record
    alt Key seen before
        D-->>API: Stored result
        API-->>W: Same response
    else New
        API->>D: Begin transaction
        API->>D: Lock PO lines, check open quantity and tolerance
        API->>NU: Next GRN number, locked counter
        API->>IN: Post stock movements
        IN->>D: Lock stock balance rows in order, insert movements, update balance
        API->>FN: Request ledger posting
        FN->>D: Check open period, insert ledger rows, balanced check
        API->>D: Update PO received quantity and status
        API->>D: Insert audit and outbox event
        API->>D: Store idempotency result
        API->>D: Commit
        API-->>W: 201 GRN
    end
    Note over API,D: Any failed step rolls back the whole transaction
```

## SEQ-05 Vendor payment release

```mermaid
sequenceDiagram
    autonumber
    actor AC as Accountant
    actor FM1 as Finance Manager approver
    actor FM2 as Finance Manager releaser
    participant P as Payment module
    participant PD as Policy component
    participant E as Approval engine
    participant FN as Finance module
    participant O as Outbox and worker
    participant B as Bank adapter
    participant D as Database
    AC->>P: Create payment with allocations, Idempotency-Key
    P->>PD: Authenticate, check permission and scope
    P->>D: Validate allocation within open amounts
    P-->>AC: Draft payment
    AC->>P: Submit
    P->>E: Approval request
    FM1->>E: Decide with step-up
    alt Rejected
        E-->>AC: Rejected with reason, back to Draft
    else Approved (second approver above limit)
        E-->>P: Approved
    end
    FM2->>P: Release with Idempotency-Key and step-up
    P->>PD: Check releaser differs from preparer and approver
    alt Same user or no step-up
        PD-->>FM2: DUTY-002 or STEP-001, audit written
    else First payment to new bank account inside hold
        P-->>FM2: HOLD-001
    else Allowed
        P->>D: Begin transaction
        P->>FN: Post payment to bank clearing and allocation entries
        P->>D: Set Released, outbox event with approved snapshot, audit with three users
        P->>D: Commit
        P-->>FM2: Released
    end
    O->>B: Send payment instruction
    alt Bank accepts
        B-->>O: Accepted
        O->>P: Record Settled through the application
    else Temporary failure
        B--xO: Error
        O->>O: Retry with backoff
    else Permanent rejection
        B-->>O: Rejected
        O->>P: Record Failed through the application
        P->>FN: Post reversing entry, reopen invoice, audit
        P-->>AC: Notify Finance
    end
```

## SEQ-06 Sales order confirmation with credit check and reservation

```mermaid
sequenceDiagram
    autonumber
    actor S as Sales Executive
    participant SO as Sales module
    participant FN as Finance module
    participant IN as Inventory module
    participant E as Approval engine
    participant D as Database
    S->>SO: POST sales-orders/{id}/confirm
    SO->>D: Begin transaction
    SO->>FN: Get customer exposure
    SO->>SO: Compare order value plus exposure with limit
    alt Credit exceeded
        SO->>E: Start credit approval
        SO->>D: Commit status pending
        SO-->>S: 409 SALES-003 with limit and exposure
    else Within limit
        SO->>IN: Reserve stock per line
        IN->>D: Lock balance rows, check available, insert reservation
        alt Shortage
            IN-->>SO: Partial reservation
        end
        SO->>D: Set Confirmed, audit, outbox
        SO->>D: Commit
        SO-->>S: Confirmed with reservation result
    end
```

## SEQ-07 API integration with outbox and retry

See also the outbound flow in [Integration Architecture](../04-System-Architecture/Integration-Architecture.md) section 3. The sequence below adds inbound webhook handling.

```mermaid
sequenceDiagram
    autonumber
    participant P as Partner
    participant G as Gateway
    participant W as Webhook handler
    participant D as Database
    participant Q as Worker
    P->>G: Webhook with signature and timestamp
    G->>W: Forward after rate limit
    W->>W: Verify signature and time window
    alt Invalid
        W-->>P: 401
        W->>D: Audit security event
    else Valid
        W->>D: Check message id already processed
        alt Duplicate
            W-->>P: 200 with stored result
        else New
            W->>D: Begin transaction, apply update, audit, outbox, store message id
            W->>D: Commit
            W-->>P: 200
            Q->>D: Publish follow-up events
        end
    end
```

## SEQ-08 Error handling and recovery

```mermaid
sequenceDiagram
    autonumber
    actor U as User
    participant A as Application
    participant D as Database
    participant L as Logging and alerts
    participant Q as Worker
    U->>A: Post a document
    A->>D: Begin transaction
    A->>D: Write document, ledger, stock, audit
    alt Validation or business rule failure
        A->>D: Rollback
        A-->>U: 4xx with code and fix hint, correlation id
    else Serialisation failure or deadlock
        A->>D: Rollback
        A->>A: Retry up to limit with backoff
        alt Retry succeeds
            A-->>U: Success
        else Retry exhausted
            A->>L: Error with correlation id
            A-->>U: 503, safe to retry with same Idempotency-Key
        end
    else Audit write fails
        A->>D: Rollback business change
        A->>L: Critical alert AUD-093
        A-->>U: 500 with correlation id
    else Database unavailable
        A->>L: Alert
        A-->>U: 503
        Note over A,D: Failover to standby. Clients retry with same key
    end
    Note over Q,D: Worker failure leaves outbox event Pending. Another worker claims it after lease expiry
```

## SEQ-09 Period close

```mermaid
sequenceDiagram
    autonumber
    actor FM as Finance Manager
    participant P as Period module
    participant R as Reconciliation checks
    participant D as Database
    participant U as Audit
    FM->>P: Request close checklist
    P->>D: Count unposted documents and pending approvals in period
    P-->>FM: Checklist with counts
    FM->>R: Run reconciliations
    R->>D: Compare AP, AR, stock valuation with control accounts
    R-->>FM: Result
    alt Items open or differences
        P-->>FM: 409 FIN-003 or FIN-004 with detail
    else Clean
        FM->>P: Close with Idempotency-Key
        P->>D: Begin transaction, set Closed, block posting
        P->>U: Audit AUD-050
        P->>D: Commit
        P-->>FM: Closed
    end
```

## SEQ-10 Stock transfer with in-transit and partial receipt

```mermaid
sequenceDiagram
    autonumber
    actor S as Source Warehouse Staff
    actor R as Destination Warehouse Staff
    participant T as Transfer service
    participant IN as Inventory module
    participant D as Database
    S->>T: POST stock-transfers/{id}/dispatch, Idempotency-Key
    T->>D: Begin transaction, check transfer Draft
    T->>IN: Reduce source stock
    IN->>D: Lock source balance row, check available
    alt Insufficient stock
        IN-->>T: STK-001
        T->>D: Rollback
        T-->>S: 409 with available quantity
    else Enough stock
        IN->>D: Insert transfer-out movement, add in-transit quantity
        T->>D: Set Dispatched, audit AUD-061, outbox event
        T->>D: Commit
        T-->>S: Dispatched
    end
    R->>T: POST stock-transfers/{id}/receive with quantity 6 of 10
    T->>D: Begin transaction, check received at most sent
    T->>IN: Increase destination, reduce in-transit by 6
    IN->>D: Lock destination balance, insert transfer-in movement
    T->>D: Set Partly received, audit, outbox
    T->>D: Commit
    T-->>R: Partly received, 4 remain in transit
    Note over S,R: Loss in transit is handled by an approved stock adjustment, not by editing the transfer
```

## SEQ-11 Return, credit note and refund

```mermaid
sequenceDiagram
    autonumber
    actor SE as Sales Executive
    actor SM as Sales Manager
    actor WS as Warehouse Staff
    actor AC as Accountant
    actor FM1 as Finance Manager approver
    actor FM2 as Finance Manager releaser
    participant RT as Return service
    participant IN as Inventory module
    participant FN as Finance module
    participant PY as Payment engine
    participant E as Approval engine
    participant B as Bank adapter
    participant D as Database
    SE->>RT: POST sales-returns against a delivery
    RT->>D: Check quantity at most delivered less earlier returns
    alt Above limit
        RT-->>SE: SALES-006
    else Outside window
        RT->>E: Approval request to Sales Manager
        SM->>E: Decide
        alt Rejected
            E-->>SE: Return Rejected
        else Approved
            E-->>RT: Approved
        end
    else Within limit
        RT->>D: Create return Requested
    end
    WS->>RT: POST sales-returns/{id}/receive with disposition
    RT->>IN: Restock, or scrap, or open vendor return in WF-01
    IN->>D: Insert stock movement
    AC->>FN: POST credit-notes within invoiced amount
    FN->>D: Check invoiced less earlier credits
    FN->>E: Approval request
    FM1->>E: Decide
    alt Rejected
        E-->>AC: Credit note back to Draft
    else Approved
        E->>FN: Approved
        FN->>D: Post credit note and ledger entry, audit
    end
    alt Invoice already paid and refund chosen
        AC->>PY: POST refunds with Idempotency-Key
        PY->>D: Check refund at most amount received
        PY->>E: Approval request
        FM1->>E: Decide with step-up
        FM2->>PY: Release with step-up, different user
        PY->>FN: Post refund to bank clearing
        PY->>D: Outbox event with approved snapshot
        PY->>B: Worker sends instruction
        alt Bank rejects
            B-->>PY: Rejected
            PY->>FN: Reverse posting, refund Failed, credit stays held
        else Bank accepts
            B-->>PY: Accepted
            PY->>D: Refund Settled
        end
    end
```

## Phase 2 note (v0.2)

The sequence diagrams for WF-04, WF-05, WF-08, WF-09 and WF-13 are in the Lifecycles folder (`Lifecycles/WF-nn-*.md`). WF-01, WF-06 and WF-12 diagrams follow the [Authoritative Workflow Definitions](Authoritative-Workflow-Definitions.md).
