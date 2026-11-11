# Definition of Ready (DoR)

Document status: Draft v0.1 (Stage 3 part 2). Proposed. A story enters a sprint only when every applicable item is true.
Related: [Definition of Done](Definition-of-Done.md), [Sprint Plan](Sprint-Plan.md), [User Stories](../03-Product-and-System-Requirements/User-Stories.md).

## 1. Story-level checklist

| # | Criterion | Evidence |
|---|---|---|
| 1 | The story has an ID, epic, module, role, "As a / I want / So that" text and priority | User Stories row |
| 2 | Linked FRs, BR and BRL are named and none is in a state "TBD" that blocks the work. If a TBD affects behaviour, a default is agreed and recorded | FRS, Business Rules, Open Questions |
| 3 | Acceptance criteria are in Given/When/Then and testable | Story row |
| 4 | Negative and edge cases are listed | Story row |
| 5 | Dependencies (stories, modules, APIs, data, decisions) are identified and either done or scheduled earlier | Sprint Plan |
| 6 | Related API items, permission codes and error codes are identified | API Documentation, Permission Matrix |
| 7 | Test cases exist for the linked FRs, or test design is part of the story | RTM |
| 8 | UI behaviour is described (screen group, states, validation messages), or the story has no UI | Screen Specifications, Form Validation Rules |
| 9 | Security and privacy impact is considered: new permission, personal data, audit event, threat model change | Security Requirements, Audit Trail |
| 10 | Data impact is known: new entity or migration, with owner | Database Design |
| 11 | Non-functional needs are stated if relevant: volume, performance, concurrency | NFR |
| 12 | The team has estimated the story and it fits in one sprint. Larger stories are split | Planning |
| 13 | The product owner or business owner has reviewed it and can answer questions during the sprint | Named person |
| 14 | Test data needed is identified | Test Data Management |

## 2. Epic or slice-level checklist

| # | Criterion |
|---|---|
| 1 | The slice in the Sprint Plan has its prerequisites done |
| 2 | Open questions that affect the slice have answers or approved defaults |
| 3 | Reference calculation sets (rounding, FX, valuation, allocation) are signed by Finance where the slice needs them |
| 4 | Stubs for external partners exist if the slice touches them |

## 3. Rules

1. A story that fails the DoR returns to refinement. It is not started "to save time".
2. The team may accept a story with a documented assumption if the business owner agrees in writing. The assumption is added to the BRD assumptions list.
3. Changes to requirements after a story is ready follow [Change Management](Change-Management.md).
