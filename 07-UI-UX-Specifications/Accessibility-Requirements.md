# Accessibility Requirements

Document status: Draft v0.1 (Stage 3 part 2). Proposed. **Target: WCAG 2.2 level AA** for the web client (NFR-032). Level AAA is not required. The criteria numbers refer to WCAG 2.2 success criteria.
No accessibility audit, testing or assistive-technology run has been done. These are requirements, not results.
Related: [NFR](../03-Product-and-System-Requirements/NFR.md) (NFR-032, NFR-033), [Screen Specifications](Screen-Specifications.md), [Form Validation Rules](Form-Validation-Rules.md), [E2E Test Cases](../08-QA-and-Testing/E2E-Test-Cases.md).

## 1. Requirements

| ID | Principle | Topic | Requirement | WCAG 2.2 criteria | Pri |
|---|---|---|---|---|---|
| ACC-001 | Perceivable | Text alternatives | All non-text content that conveys meaning has a text alternative. Charts have a data table alternative. Icons used alone have an accessible name | 1.1.1 | P0 |
| ACC-002 | Perceivable | Structure and semantics | Pages use headings in order, landmarks (header, navigation, main, footer), real lists and real tables with header cells and captions. Layout does not rely on tables | 1.3.1, 2.4.1, 2.4.6 | P0 |
| ACC-003 | Perceivable | Form labels and instructions | Every control has a visible label tied to it programmatically. Required fields are stated in text. Help text is associated with the field. Autocomplete attributes on personal data fields | 1.3.1, 1.3.5, 3.3.2 | P0 |
| ACC-004 | Perceivable | Colour and contrast | Text contrast at least 4.5 to 1 (3 to 1 for large text). Interface component and chart contrast at least 3 to 1. Colour is never the only signal: status badges include text, charts use patterns or labels, required and error states have text | 1.4.1, 1.4.3, 1.4.11 | P0 |
| ACC-005 | Perceivable | Resize and reflow | Usable at 200 percent zoom and at 320 CSS pixel width without two-dimensional scrolling, except data tables and line grids that may scroll horizontally. Text spacing overrides do not hide content | 1.4.4, 1.4.10, 1.4.12 | P0 |
| ACC-006 | Perceivable | Orientation and input purpose | Works in portrait and landscape. Input purpose is programmatically determinable | 1.3.4, 1.3.5 | P1 |
| ACC-007 | Operable | Keyboard access | Every function works with a keyboard alone, including line grids, menus, dialogs, date pickers, drag-free alternatives and approval actions. No keyboard traps | 2.1.1, 2.1.2, 2.1.4 | P0 |
| ACC-008 | Operable | Focus | Focus is always visible, not hidden behind sticky headers or dialogs, and follows a logical order. Opening a dialog moves focus into it and closing returns it. Focus moves to the first error after failed submit | 2.4.3, 2.4.7, 2.4.11, 3.2.1 | P0 |
| ACC-009 | Operable | Skip and bypass | A skip-to-content link exists. Repeated navigation can be bypassed | 2.4.1 | P0 |
| ACC-010 | Operable | Page titles, links, purpose | Each page has a unique descriptive title. Link and button text is meaningful out of context. Current location is indicated in navigation | 2.4.2, 2.4.4, 2.4.8 | P0 |
| ACC-011 | Operable | Pointer and target size | Targets are at least 24 by 24 CSS pixels (WCAG 2.2 AA) and, as a product rule, 44 by 44 for touch. Actions with a path or drag gesture have a single-pointer alternative. Pointer actions can be cancelled | 2.5.1, 2.5.2, 2.5.7, 2.5.8 | P1 |
| ACC-012 | Operable | Time limits and session timeout | Idle session expiry (default 30 minutes) warns the user at least 2 minutes before, offers to extend, and preserves unsaved work where possible. Long background jobs are not time-limited for the user | 2.2.1, 2.2.2 | P0 |
| ACC-013 | Operable | Motion and flashing | No content flashes more than three times per second. Animations respect the reduced-motion setting | 2.3.1, 2.3.3 | P1 |
| ACC-014 | Understandable | Language | Page language and any change of language are marked. The interface language follows user preference | 3.1.1, 3.1.2 | P1 |
| ACC-015 | Understandable | Consistent navigation and identification | Navigation, search and help appear in the same place across pages. Components with the same function are named the same way. Help is available in a consistent place | 3.2.3, 3.2.4, 3.2.6 | P1 |
| ACC-016 | Understandable | Error identification and prevention | Errors are identified in text, linked from a summary, and suggest a fix. Financial and legal actions (post, release payment, close period, anonymise) are reversible, checked or confirmed. Previously entered data is not requested again within a process | 3.3.1, 3.3.3, 3.3.4, 3.3.7 | P0 |
| ACC-017 | Understandable | Accessible authentication | Sign-in does not require a cognitive test. Password managers and paste are allowed. Alternatives to one-time-code transcription exist where feasible (for example, authenticator approval) | 3.3.8 | P0 |
| ACC-018 | Robust | Names, roles, values and states | Custom widgets expose name, role, value and state to assistive technology using standard patterns. Status changes (save result, validation, new notification, approval count) are announced without moving focus | 4.1.2, 4.1.3 | P0 |
| ACC-019 | Robust | Parsing and compatibility | Markup is valid enough for assistive technology to parse. The web client works with current major browsers and screen readers (list TBD) | 4.1.2 | P1 |
| ACC-020 | Robust | Tables and data-dense screens | Data tables have header cells, sortable columns announce sort state, row actions have unique accessible names including the row identifier, selection state is announced, pagination controls are labelled | 1.3.1, 4.1.2 | P0 |
| ACC-021 | Robust | Documents and exports | Generated PDFs, emails and print views have structure, reading order, text alternatives and sufficient contrast. Spreadsheet exports have header rows | 1.3.1 | P1 |
| ACC-022 | Operable | Search and filters | Filter controls, saved filters and column choosers are keyboard operable and labelled. Result count changes are announced | 2.1.1, 4.1.3 | P1 |
| ACC-023 | Perceivable | Money, dates and numbers | Currency amounts are read with currency, negative values and signs are announced in words or with accessible text, dates are unambiguous in the user locale | 1.3.1, 3.1.1 | P1 |

Requirements: 23 (P0 14, P1 9).

## 2. Component rules

| Component | Rule |
|---|---|
| Navigation sidebar | Landmark with label. Current item marked. Collapsed state still exposes labels. Expand and collapse by keyboard |
| Company switcher | Labelled control. Change announces the new scope |
| Tables and line grids | ACC-020. Editing cells follows the grid pattern with documented keys. A non-grid alternative (form per line) exists on phone widths |
| Status badges | Text always present. Colour supports but does not carry meaning |
| Approval actions | Buttons have names such as "Approve purchase order PO-2026-0001". Reason field is required and described |
| Dialogs | Role dialog, labelled, focus trapped, Escape closes unless data loss, focus returns |
| Notifications | Live region for new notices. Approvals badge has text alternative with count |
| Date pickers | Typed entry always allowed. Calendar widget is keyboard operable |
| File upload | Button and drop zone are both keyboard operable. Progress and result announced |
| Charts on dashboard | Data table view toggle. Patterns and direct labels |
| Session timeout | ACC-012 |

## 3. Browser and assistive technology support

Current versions of the major desktop browsers and the mobile browsers on current phones and tablets (list TBD, NFR-033). Screen reader and browser combinations to test: to be chosen by the QA lead (at least one desktop reader per major operating system, one mobile reader). Voice control and screen magnification are checked manually.

## 4. Verification

| Method | When |
|---|---|
| Automated accessibility checks in component and E2E tests | Every build |
| Manual keyboard-only pass of the 14 user journeys | Each release candidate |
| Screen reader pass of journeys UJ-01, UJ-02, UJ-03, UJ-05, UJ-06, UJ-08 | Each release candidate |
| Contrast and zoom review of the design system | Before UI build and at release |
| Accessibility acceptance in UAT with at least one user of assistive technology, if available | UAT |

Written test cases that touch accessibility and UI quality: TC-0494 (NFR-032), TC-0215, TC-0222, TC-0496, TC-0497, TC-0551 (NFR-030, NFR-031), TC-0495 (NFR-033). Accessibility test depth is thin: one case covers NFR-032 as a whole (gap TG-04). More cases follow once the design exists.

## 5. Open points

| Point | Question |
|---|---|
| Design system and tokens (contrast values) | OQ-027 |
| Supported assistive technology list | Test plan |
| Languages and right-to-left support | OQ-011 |
| Legal accessibility duty for the target markets | OQ-010 |
