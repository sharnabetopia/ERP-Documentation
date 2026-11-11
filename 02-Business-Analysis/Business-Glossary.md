# Business Glossary

Document status: Draft v0.1 (Stage 1). Terms are used with these meanings in every document.

| Term | Definition |
|---|---|
| Accrual | Recording income or expense when earned or incurred, not when cash moves. |
| Advance payment | Money received or paid before the related invoice exists. Held as unallocated credit. |
| Allocation | Matching a payment to one or more open invoices. |
| Approval chain | Ordered list of approvers for a document type, based on value and scope. |
| Audit trail | Immutable record of who changed what, when, and the before and after values. |
| Base currency | The currency in which a company keeps its books. |
| Batch / Lot | A group of units of one item produced or received together. |
| Branch | A physical or logical operating unit of a company. |
| BOM | Bill of Materials. List of components needed to make an item. |
| Chart of Accounts (COA) | Structured list of ledger accounts of a company. |
| Company | A legal entity with its own books. The primary data owner. |
| Control account | Ledger account maintained only by a sub-ledger, such as receivables or payables. |
| COGS | Cost of goods sold. |
| Credit note | Document that reduces an amount owed by a customer. |
| Debit note | Document that reduces an amount owed to a vendor, or increases a customer charge. |
| Delegation | Temporary transfer of approval authority to another user. |
| Department | A functional unit within a company or branch. |
| Document | A business transaction record with a number and status, such as a PO or invoice. |
| Escalation | Automatic routing of an overdue approval to a higher approver. |
| Fiscal year | The 12-month (or configured) accounting year. |
| GRN | Goods Receipt Note. Records goods received against a purchase order. |
| Idempotency | Repeating the same request has the same effect as sending it once. |
| In transit | Stock dispatched from one location and not yet received at another. |
| Journal entry | A balanced set of debits and credits posted to the ledger. |
| Maker-checker | Principle that the person who prepares a record cannot be the one who approves it. |
| Master data | Slowly changing shared records such as customers, vendors, items and accounts. |
| MRP | Material Requirements Planning. |
| Multi-tenancy | One software deployment serving several isolated customers. |
| PO | Purchase Order. |
| PR | Purchase Requisition. An internal request to buy. |
| Posting | Making a document's financial effect permanent in the ledger. |
| Reconciliation | Comparing two records (for example bank statement and ledger) and explaining differences. |
| Reservation | Holding stock for a specific order. |
| Reversal | A new entry that cancels the effect of an earlier posted entry. |
| RFQ | Request for Quotation. |
| Segregation of Duties (SoD) | Splitting conflicting duties across different people. |
| SLA | Service Level Agreement. |
| Stock ledger | Record of every stock movement per item and warehouse. |
| Sub-ledger | Detail ledger (for example per customer) that summarises into a control account. |
| Tenant | An isolated customer space in a multi-tenant system. See OQ-019 for how it relates to company. |
| Three-way match | Comparing PO, GRN and vendor invoice before payment. |
| Tolerance | Allowed difference before a mismatch is raised. |
| Valuation | Assigning a monetary cost to stock on hand. |
| WIP | Work in progress. |

## Process names

| Name | Meaning | Scope |
|---|---|---|
| P2P | Procure-to-Pay | Requisition to vendor payment |
| O2C | Order-to-Cash | Quotation to customer receipt |
| R2R | Record-to-Report | Transaction to financial statements |
| H2R | Hire-to-Retire | Recruitment to offboarding |
