# The HARD-RULES gate (fail-closed)

Run before **any write — Create OR Update** (SKILL.md gate step) and re-confirm at verify. Every
item is binary. **Any FAIL → nothing is written.** No item is ever waved through. Each maps to a
standing HARD RULE. For a task **with subtasks**, run the gate on the parent **and each subtask**.

Show the result as a table the operator sees at confirmation: `| # | Gate item | ✅/❌ |`

## The 10 items

**G1 — Template conformance.**
Built from the correct template (parent task → default template; subtask → Sub-task template). All
sections present, in order, structure unchanged; placeholder helper text replaced with real content.
*Update:* the template sections stay present + intact — never break, drop, or reorder one.
FAIL → rebuild/realign; never free-style a body.

**G2 — Property completion.**
MUST-set non-empty: Task Name, Status, Area, Priority, Owner (subtasks additionally: `Parent task`).
Every WHEN-GROUNDED property is set or explicitly marked `— N/A (confirmed)`.
*Update:* change ONLY what was requested; never blank a MUST-set property by accident.
FAIL → fill it or confirm N/A. A filled body never compensates for a blank property.

**G3 — Valid values only.**
Every select/status value is an EXACT option from the live schema. Every relation points to a real,
existing record (verified by fetch). No invented option, no invented record, no guessed select.
FAIL → use a real value or ASK.

**G4 — Icon (rendered) + clean title.**
Icon matches the live template's icon (currently `🎫`) AND actually renders — pass `icon: "🎫"`
directly. (If a built-in `icons/<name>` is ever used, pass the full `https://www.notion.so/icons/<name>.svg`
URL — the bare form stores as a broken relative path `/icons/...` → NO icon; verify no leading
slash.) The emoji lives ONLY in the icon — the Task Name has no emoji, glyph, or prefix.
FAIL → set the icon / strip the title.

**G5 — Creation date present.**
On create, Date Created (created_time) auto-stamps; confirm the property exists (it does). A task
without a creation date is invalid.

**G6 — Lean, not a canvas.**
Section bodies are terse work-unit content, not essays. No padding, no restating the title. Value
lives in properties + the row.
FAIL → trim.

**G7 — Writes confined, nothing extra.**
The run writes ONLY the task and (when confirmed) its subtasks — or the confirmed update and its
sub-tasks. No Logs row, no other database, no other page, no schema edit. Anything else is a
separate, separately-approved action.
FAIL → drop the extra write.

**G8 — Permission to write.**
The operator has seen the exact change — the filled task (create) or the **before → after diff**
(update), plus any subtasks and this gate table — and given an explicit yes.
FAIL → do not write. Wait.

**G9 — No drift, or drift surfaced.**
Live schema, select options, template sections, and template icon match the spec snapshot. If they
differ, the diff was surfaced and the build conforms to LIVE, not the stale snapshot.
FAIL → stop and surface the drift before building.

**G10 — Verify-after-act (post-write).**
After the write, re-fetch confirms at the user-visible layer: sections intact + filled, every
changed property set as intended, icon `🎫`, (create) Date Created stamped, subtasks linked via
`Parent task`.
FAIL → fix the mismatch; verdict stays ⚠️ until it passes. Never ✅ on the API call's success alone.

## Verdict

- **All ✅** → write (after G8), then re-run G10 on the live page(s) → ✅ + URL(s).
- **Any ❌ before write** → do not write; report the failing item(s) + the exact next action.
- **G10 ❌ after write** → ⚠️; state precisely what's wrong and fix it before declaring done.

The gate passing is the definition of done. The number of ✅ is a floor to clear, not a number to
chase. Never relax an item to make the table look complete.
