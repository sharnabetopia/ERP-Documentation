# Diagram Consistency Report

Phase 2 remediation, 2026-11-11. Related: [Remediation Progress](Remediation-Progress.md), [Final Revalidation Report](Final-Revalidation-Report.md), [Audit baseline](../12-Final-Audit/Final-Readiness-Assessment.md).

No stakeholder approval is claimed anywhere in this report. Values marked PENDING APPROVAL are recommendations.

## 1. Method

One authoritative definition was written for each contradicting process (`05-Workflows-and-Diagrams/Authoritative-Workflow-Definitions.md`, AWD-01 to AWD-04). Each definition was compared with the workflow table, business rules, user stories, API document, state lifecycle and test cases. The process flow, swimlane, sequence and state diagrams, the Data Dictionary status lists and the workflow tables were then changed to match. Every Mermaid block in the package was re-rendered with Mermaid CLI 10.9.1 after the change.

## 2. Contradictions resolved

| ID | Process | Contradiction found by the audit | Resolution | Status |
|---|---|---|---|---|
| D1, D2 | WF-01 variance and approval | Process flow looped a variance back to entry, swimlane sent it to Finance Manager approval. No-variance path posted with no approval | AWD-02: system approves within tolerance, Finance Manager decides a held invoice, rejection cancels | Closed |
| D3 | WF-06 adjustment approval | Process flow posted below threshold with no approval. Swimlane drew parallel branches. FR-109 said "above a threshold" | AWD-03: Inventory Manager always, Finance Manager above threshold. FR-109 clarified, PENDING APPROVAL (OQ-038) | Closed pending approval |
| D4 | WF-12 refund | Swimlane always refunded and retried a failed refund. Rejections not drawn. Vendor return merged into credit note | AWD-04: refund only when paid and chosen, Failed is final, both rejections drawn, vendor return separate | Closed |
| D5 | SEQ-11 | Wrong arrow, bank writing to database, refund through the Finance module | SEQ-11 rewritten through the payment engine and the bank adapter | Closed |
| D6 | State lifecycle | Data Dictionary terminal states disagreed with diagrams for five entities | Dictionary and diagrams aligned (requisition, purchase invoice, payment, journal, stock adjustment) | Closed for five entities |
| D8 | Payment and invoice | Ledger posted at release with no reversal on bank failure. No invoice reversal | Posting to bank clearing at release, system reversing entry on failure, Posted to Cancelled by approved reversal | Closed |
| SEQ-05 | Vendor payment release | No step-up, no second approver, no rejection branch, bank outcome written by the worker | Rewritten with duty checks, step-up, hold, and Settled or Failed recorded through the application | Closed |

## 3. Consistency assertions executed by script on the updated files

| Assertion | Result |
|---|---|
| P2P: process flow has no hold-to-entry loop (K2 --> I removed) | PASS |
| P2P: process flow, swimlane and sequence all show Finance Manager variance decision | PASS |
| P2P: invoice state Held to Approved by Finance Manager in state diagram and AWD | PASS |
| Payment: Settled and Failed states in state diagram, data dictionary and SEQ-05 | PASS |
| Payment: release by different user in process flow, swimlane, sequence and API | PASS |
| Payment: API release uses finance.payment.release | PASS |
| Adjustment: process flow has Inventory Manager decision before posting | PASS |
| Adjustment: swimlane routes above threshold to Finance Manager after Inventory Manager | PASS |
| Adjustment: state diagram has Rejected to Draft and dictionary says Rejected returns to Draft | PASS |
| Refund: process flow and swimlane make refund conditional on payment | PASS |
| Refund: SEQ-11 refund goes through payment engine | PASS |
| Refund: wrong-direction line removed from SEQ-11 | PASS |
| Cross-module workflow table mentions Settled and release by a different Finance Manager | PASS |
| FR-109 text no longer says only "above a threshold" | PASS |

These assertions are text checks for the presence or absence of agreed phrases and transitions. They are not a proof that two diagrams describe the same behaviour. They were supported by a side-by-side reading of the three processes.

## 4. Mermaid re-render

| Item | Value |
|---|---|
| Blocks in the package (excluding the archived audit) | 121 |
| Rendered without error | 121 |
| Empty blocks | 0 |
| Types | {'flowchart': 75, 'erDiagram': 6, 'sequenceDiagram': 17, 'stateDiagram-v2': 23} |

## 5. Not fixed (still open)

D7 sales order credit hold state, D9 SEQ-02 steps, D10 permission and audit steps in SEQ-03, 04, 06, 09, 10, D11 and D12 ERD cardinality, D13 DFD-1 balance, D14 WF-02 partial delivery paths, D15 exception paths for WF-03 and WF-07, D16 SEQ-01 branches, UML component diagram, C4 components, 19 undrawn ERD tables. See the register in the Remediation Progress file.
