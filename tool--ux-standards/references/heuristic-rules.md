# Nielsen's 10 Usability Heuristics — Audit Companion

Expanded reference for each heuristic with specific checkpoints, common failure patterns, and severity guidelines. Use alongside the heuristic table in SKILL.md.

---

## NIE-01: Visibility of System Status

**Principle:** The system keeps the user informed about what's going on through timely and appropriate feedback.

### Checkpoints

| # | Check | Severity if missing |
|---|---|---|
| 01.1 | Loading indicators for all async operations | Major |
| 01.2 | Progress bars for operations >2 seconds | Major |
| 01.3 | Success confirmation after form submissions | Major |
| 01.4 | Error messages when operations fail | Critical |
| 01.5 | Active/selected state on navigation items | Minor |
| 01.6 | Character count for constrained inputs | Minor |
| 01.7 | Auto-save indicator when applicable | Advisory |
| 01.8 | Timestamps or "last updated" for data | Minor |
| 01.9 | Upload progress with percentage/file name | Major |
| 01.10 | Network/connection status indicator | Advisory |

### Common Failures
- Button clicked, nothing happens for 3 seconds, no visual feedback
- Form submitted but no confirmation — user clicks again, duplicates entry
- Background sync with no indicator — user thinks nothing is happening
- Stale data displayed without timestamp — user makes decisions on old data

---

## NIE-02: Match Between System and Real World

**Principle:** The system uses the user's language, familiar concepts, and natural task order.

### Checkpoints

| # | Check | Severity if missing |
|---|---|---|
| 02.1 | Labels use user vocabulary, not developer jargon | Major |
| 02.2 | Icons match real-world metaphors | Minor |
| 02.3 | Task flow follows real-world sequence | Major |
| 02.4 | Date/time/currency formats match user's locale | Minor |
| 02.5 | Error messages in plain language | Major |
| 02.6 | No raw codes, IDs, or technical strings exposed | Major |
| 02.7 | Metaphors are consistent (e.g., "cart" not sometimes "bag") | Minor |

### Common Failures
- "422 Unprocessable Entity" shown to end users
- "Epoch timestamp" instead of readable date
- Technical field names ("billing_address_line_1") as labels
- Flow that asks payment before items — wrong real-world order

---

## NIE-03: User Control and Freedom

**Principle:** Users need a clearly marked emergency exit to leave unwanted states without extended dialogue.

### Checkpoints

| # | Check | Severity if missing |
|---|---|---|
| 03.1 | Undo for destructive actions (delete, archive) | Critical |
| 03.2 | Cancel button on all modals and multi-step forms | Major |
| 03.3 | Back navigation works correctly (browser + in-app) | Major |
| 03.4 | Dismiss/close for all overlays, toasts, modals | Major |
| 03.5 | Draft saving for long forms | Minor |
| 03.6 | Confirmation dialog for irreversible actions | Critical |
| 03.7 | Easy path to return to home/starting state | Minor |

### Common Failures
- Delete with no undo and no confirmation
- Multi-step wizard with no back button
- Modal with no close button or Escape handler
- SPA navigation that breaks browser back button

---

## NIE-04: Consistency and Standards

**Principle:** Users should not have to wonder whether different words, situations, or actions mean the same thing.

### Checkpoints

| # | Check | Severity if missing |
|---|---|---|
| 04.1 | Same action = same appearance across all screens | Major |
| 04.2 | Platform conventions respected (close button position, nav patterns) | Major |
| 04.3 | Terminology consistent (no "save" vs "update" vs "apply" for same action) | Minor |
| 04.4 | Icon meanings don't change between contexts | Major |
| 04.5 | Interactive element styles consistent (buttons, links, inputs) | Major |
| 04.6 | Spacing and alignment follow the same scale | Minor |
| 04.7 | Color meanings consistent (red = error everywhere, not sometimes warning) | Major |

### Common Failures
- Primary button is blue on one page, green on another
- "Settings" in nav bar, "Preferences" in menu, "Options" in modal
- Clicking a card navigates on one screen, opens a modal on another
- Icons change meaning: star means "favourite" and also "premium"

---

## NIE-05: Error Prevention

**Principle:** Better than good error messages is a design that prevents problems in the first place.

### Checkpoints

| # | Check | Severity if missing |
|---|---|---|
| 05.1 | Confirmation before destructive/irreversible actions | Critical |
| 05.2 | Inline validation before form submission | Major |
| 05.3 | Autocomplete/suggestions to reduce input errors | Minor |
| 05.4 | Constraints prevent invalid states (date picker vs free text) | Major |
| 05.5 | Format hints for expected input ("MM/DD/YYYY") | Minor |
| 05.6 | Disabling actions that aren't available (greyed-out, with reason) | Minor |
| 05.7 | Confirming before leaving unsaved work | Major |

### Common Failures
- "Delete Account" with one click, no confirmation
- Date field accepts "January", "01", "1/1", "2025-01-01" — different parsing each time
- User can submit form with invalid email, error appears after server round-trip
- Navigating away from half-filled form with no warning

---

## NIE-06: Recognition Rather Than Recall

**Principle:** Make objects, actions, and options visible. The user should not have to remember information from one screen to another.

### Checkpoints

| # | Check | Severity if missing |
|---|---|---|
| 06.1 | Options visible, not hidden behind memory | Major |
| 06.2 | Recently used items accessible | Minor |
| 06.3 | Search with suggestions/autocomplete | Minor |
| 06.4 | Context preserved across navigation | Major |
| 06.5 | No blank states — always show guidance or examples | Major |
| 06.6 | Filter/sort selections visible and persistent | Minor |

### Common Failures
- Dropdown with 200 items and no search
- User selects filters, navigates to detail, comes back — filters reset
- Empty dashboard with no onboarding, guidance, or sample data
- Reference code from previous step needed but not shown on current step

---

## NIE-07: Flexibility and Efficiency of Use

**Principle:** Accelerators may speed up interaction for the expert user without impeding the novice.

### Checkpoints

| # | Check | Severity if missing |
|---|---|---|
| 07.1 | Keyboard shortcuts for frequent actions | Advisory |
| 07.2 | Expert users can bypass onboarding/tutorials | Minor |
| 07.3 | Settings/preferences persist across sessions | Minor |
| 07.4 | Batch operations for repetitive tasks | Minor |
| 07.5 | Customizable dashboard/view | Advisory |
| 07.6 | Power features progressive (don't overwhelm new users) | Advisory |

---

## NIE-08: Aesthetic and Minimalist Design

**Principle:** Every extra unit of information competes with relevant information and diminishes its relative visibility.

### Checkpoints

| # | Check | Severity if missing |
|---|---|---|
| 08.1 | Every visible element earns its space | Minor |
| 08.2 | No decoration without function | Advisory |
| 08.3 | Information hierarchy clear (primary, secondary, tertiary) | Major |
| 08.4 | No competing calls-to-action on same screen | Major |
| 08.5 | Whitespace used to create hierarchy, not just fill | Minor |

---

## NIE-09: Help Users Recognise, Diagnose, and Recover from Errors

**Principle:** Error messages should be expressed in plain language, precisely indicate the problem, and constructively suggest a solution.

### Checkpoints

| # | Check | Severity if missing |
|---|---|---|
| 09.1 | Errors in plain language (no codes, no jargon) | Critical |
| 09.2 | Error identifies which field/action failed | Critical |
| 09.3 | Error suggests how to fix it | Major |
| 09.4 | Error positioned near the source (inline, not just top-of-page) | Major |
| 09.5 | Error doesn't destroy user input (form data preserved) | Critical |

### Error Message Template
**Structure:** `[What went wrong] + [How to fix it]`

Good: "That email is already registered. Sign in instead?"
Bad: "Error: duplicate entry in users table"

Good: "Password must be at least 8 characters with one number"
Bad: "Invalid password"

---

## NIE-10: Help and Documentation

**Principle:** Even though it's better if the system can be used without documentation, help should be available.

### Checkpoints

| # | Check | Severity if missing |
|---|---|---|
| 10.1 | Contextual help/tooltips for complex features | Minor |
| 10.2 | Searchable help for complex products | Minor |
| 10.3 | Onboarding for first-time experiences | Advisory |
| 10.4 | Help is task-oriented, not feature-oriented | Advisory |
| 10.5 | Help is concise and to the point | Advisory |
