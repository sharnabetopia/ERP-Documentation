# Authoritative Workflow Definitions

Document status: Draft v0.2 (Phase 2 remediation, 2026-11-11). Proposed. Items marked PENDING APPROVAL need a stakeholder decision.
Purpose: one definition of business behaviour for the three processes whose diagrams contradicted each other. Process flows, swimlanes, sequence diagrams, state diagrams, the workflow tables, the Data Dictionary and the API document were changed to match this file.
Where a source document disagreed, the resolution and its reason are in the table at the end of each definition.
Related: [Cross-Module Workflows](Cross-Module-Workflows.md), [Process Flow](Process-Flow.md), [Swimlane Diagrams](Swimlane-Diagrams.md), [Sequence Diagrams](Sequence-Diagrams.md), [State Transition Diagrams](State-Transition-Diagrams.md), [Data Dictionary](../04-System-Architecture/Data-Dictionary.md).

## AWD-01 Payment engine (vendor payment and customer refund)

Used by WF-01 step 9 to 11 and WF-12 step 7 and 8.

| Item | Definition |
|---|---|
| Actors | Accountant (prepare), Finance Manager approver, a different Finance Manager releaser, Bank (via adapter), System |
| Duties | Prepare, approve and release are three different users on one payment (BRL-068, FR-135, SOD-14 to SOD-16, DBC-13). Above limit A a second distinct approver is needed (FR-137). Limits are TBD (OQ-013, OQ-034) |
| Preconditions | Open invoice or credit, active bank account, no unverified bank change, open period |
| Release | Needs step-up (FR-145). Refused with DUTY-002, STEP-001 or HOLD-001 |
| Posting | At release the system posts the payment to a bank clearing account and queues the bank instruction. The ledger is therefore never ahead of a draft decision, and a bank rejection is corrected by a system reversing entry |
| Bank outcome | Accepted: Settled. Rejected: Failed, posting reversed by the system, invoice reopens or credit stays held |
| Retry | A Failed payment is final. A retry is a new payment with new approvals. Idempotency keys stop a double submit of one request, not a deliberate new payment. PENDING APPROVAL (proposal chosen for control) |
| Reversal | A Settled payment is reversed by a new document that restores open items. The requester and the approver of a reversal differ (FR-135) |
| Audit | AUD-106 to AUD-108, AUD-112 |
| API | API-092 to API-096. Release uses `finance.payment.release`. Reversal uses `finance.payment.reverse` |

States and allowed transitions:

| From | To | Condition |
|---|---|---|
| Draft | Submitted | Preparer submits |
| Draft, Submitted | Cancelled | Cancel |
| Submitted | Approved | Required approvers decide |
| Submitted | Rejected | Any approver rejects, reason required |
| Rejected | Draft | Revise |
| Approved | Released | Releaser differs from preparer and approver, step-up, no hold |
| Released | Settled | Bank accepts |
| Released | Failed | Bank rejects, posting reversed |
| Settled | Reversed | New reversing document approved by another user |

Terminal states: Failed, Cancelled, Reversed.

Conflicts resolved:

| Conflict found | Resolution |
|---|---|
| Release was authorised by `finance.payment.approve`, so the approver could release | Separate permission `finance.payment.release` with record-level SoD |
| Ledger posting at release with no reversal on bank failure (SEQ-05, state diagram) | Posting to bank clearing at release, system reversing entry on failure |
| Process flow "Failed, invoice reopens, back to prepare" vs state diagram "Failed to Draft" | Failed is final. A new payment is created |
| WF-12 "refund retries use the same idempotency key" | Applies to duplicate submits only |

## AWD-02 Purchase invoice approval and variance (WF-01 steps 6 to 8)

| Item | Definition |
|---|---|
| Actors | Accountant, Finance Manager, System |
| Entry | Accountant enters the invoice linked to PO and GRN (FR-097) |
| Duplicate check | Same vendor and number is blocked. An override is a recorded decision by a Finance Manager, never by the Accountant who entered it (FR-099) |
| Match | The system compares PO, GRN and invoice within tolerance (FR-098) |
| Within tolerance | The system approves the invoice. No person approves. The invoice posts to payables and the ledger |
| Variance | The invoice is Held. A Finance Manager approves the variance (then it posts) or rejects it (the vendor is asked for a corrected invoice or credit note, the invoice is Cancelled) |
| Posted invoice | Cannot be edited. A reversal before any payment is an approved Cancelled transition. After payment, the payment is reversed first (FR-077) |
| API | API-121 (create), API-122 (match), API-123 (submit), API-124 (post) |
| Audit | Match result, override, variance decision |

States: Draft, Submitted, Held, Approved, Posted, Part-paid, Paid, Cancelled.
Transitions: Draft to Submitted. Submitted to Approved (within tolerance). Submitted to Held (duplicate or variance). Held to Approved (Finance Manager approves). Held to Cancelled (rejected). Approved to Posted. Posted to Part-paid, Paid, Cancelled (approved reversal, no payment). Part-paid to Paid or Posted (payment reversed). Paid to Posted (payment reversed).

Conflicts resolved:

| Conflict found | Resolution |
|---|---|
| The process flow posted a no-variance invoice with no approval, while the state diagram and ACT-02 required Approved | Approval exists and is performed by the system when the match is within tolerance |
| The process flow sent a variance hold back to invoice entry, the swimlane sent it to Finance Manager approval | Variance goes to the Finance Manager. A rejection returns to the vendor for correction |
| The swimlane used a "variance approval" lane but the workflow table named Finance Manager only for payment | Both name the Finance Manager. The table now says so |
| Held to Submitted ("variance resolved") in the old state diagram | Replaced by Held to Approved or Cancelled so that a decision is always recorded |

## AWD-03 Inventory adjustment approval (WF-06 step 6)

| Item | Definition |
|---|---|
| Actors | Warehouse Staff or Inventory Manager (create), the creator or another user (submit), Inventory Manager (approve), Finance Manager (approve above threshold), System |
| Trigger | Count difference, loss in transit, damage |
| Create | Adjustment with reason. Financial impact shown before approval (FR-109) |
| Approve | Every adjustment needs Inventory Manager approval by a user who is neither creator nor submitter (SOD-07, SOD-31). Above the threshold a Finance Manager approves as well. A value equal to the threshold needs only the first level. Threshold is TBD (OQ-013) |
| Post | The system posts stock and ledger at final approval (BRL-038) |
| Reject | At any level. The adjustment returns to Draft |
| Audit | Both approvers named |

States: Draft, Submitted, Approved, Posted, Rejected (returns to Draft).

Conflicts resolved:

| Conflict found | Resolution |
|---|---|
| Process flow posted adjustments below the threshold with no approval | Every adjustment is approved by the Inventory Manager |
| FR-109 said "approval above a threshold" | Clarified in the FRS. PENDING APPROVAL (OQ-038) because it changes a v0.1 requirement |
| Swimlane drew the Inventory Manager and Finance Manager as parallel branches | They are sequential levels. Finance Manager only above the threshold |
| State diagram made Rejected terminal | Rejected returns to Draft, as for requisitions and journals |

## AWD-04 Return, credit note and refund (WF-12)

| Item | Definition |
|---|---|
| Actors | Sales Executive, Sales Manager, Warehouse Staff, Accountant, Finance Manager approver, Finance Manager releaser (different user), Bank |
| Return | Recorded against a delivery or invoice. Quantity at most delivered less earlier returns (BRL-051). Outside the window, the Sales Manager decides. A rejection ends the return |
| Receive | Warehouse receives and inspects. Disposition: restock, scrap, return to vendor |
| Vendor return | A vendor return opens in WF-01 (FR-096). Its debit note is separate. It does not change the customer credit note |
| Credit note | Accountant creates within invoiced amount less earlier credits (BRL-050). Finance Manager approves. A rejection returns it to Draft |
| Customer not paid | The credit reduces the open invoice |
| Customer paid | The user chooses hold (unallocated credit) or refund |
| Refund | Within the amount received (BRL-053). Runs through AWD-01 with three different users |
| Refund failure | Refund Failed, posting reversed, credit stays held. A new refund may be created |

Conflicts resolved:

| Conflict found | Resolution |
|---|---|
| The swimlane always created a refund after the credit note | Refund exists only when the customer already paid and chose refund |
| The swimlane retried a failed refund through the same step | Failed is final. A new refund is a new payment |
| Sales Manager and Finance Manager rejections were not drawn | Both drawn. Sales Manager rejection ends the return. Finance Manager rejection returns the credit note to Draft |
| The process flow merged vendor return into the credit-note path | Vendor return is a separate document in WF-01 |
| SEQ-11 had the worker sending instructions from the database to the bank, the bank writing to the database, and the refund going through the Finance module | Refund goes through the payment engine. The adapter talks to the bank. Results are recorded through the application |
