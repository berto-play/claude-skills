# The HARD-RULES gate (fail-closed)

Run before **any write — Create OR Update** (SKILL.md gate step) and re-confirm at verify. Every item
is binary. **Any FAIL → nothing is written.** No item is ever waved through. Each maps to a standing
HARD RULE. For a project **with seed tasks**, run this gate on the project; each seed task is gated by
`tool--notion-task`'s own gate.

Show the result as a table the operator sees at confirmation: `| # | Gate item | ✅/❌ |`

## The 11 items

**G1 — Template conformance.**
Built from the Project Brief template. Sections 01–09 present, in order, structure unchanged; the
"What is a Project?" reference block and the 🧭 "How to use" callout dropped; helper text replaced
with real content; the embedded Trackers block left intact.
*Update:* the template sections stay present + intact — never break, drop, or reorder one.
FAIL → rebuild/realign; never free-style a body.

**G2 — Property completion.**
MUST-set non-empty: Project Name, Status, Project Type, Phase, Owner, ROI Estimate. Every
WHEN-GROUNDED property is set or explicitly marked `— N/A (confirmed)`.
*Update:* change ONLY what was requested; never blank a MUST-set property by accident.
FAIL → fill it or confirm N/A. A filled brief never compensates for a blank property.

**G3 — Valid values only.**
Every select / status / phase value is an EXACT option from the live schema. Every relation points to
a real, existing record (verified by fetch). No invented option, no invented record, no guessed
select.
FAIL → use a real value or ASK.

**G4 — Icon (rendered) + clean title.**
A **meaningful emoji** is set for the project and actually renders — pass `icon: "🚪"` (or the fitting
emoji) directly. NOT the template's `paste_blue` placeholder. (If a built-in `icons/<name>` is ever
used, pass the full `https://www.notion.so/icons/<name>.svg` URL — the bare form stores as a broken
`/icons/...` path → NO icon.) The emoji lives ONLY in the icon — the Project Name has no emoji, glyph,
or prefix.
FAIL → set/replace the icon / strip the title.

**G5 — Creation date present.**
On create, Date Created (created_time) auto-stamps; confirm the property exists (it does). A project
without a creation date is invalid.

**G6 — Lean but deep.**
Each section answers its question honestly — no padding, no restating the title, no essay. Depth where
it earns it (the template's rule: *depth beats length*). Ticked checkboxes are truly answered, not
decorative.
FAIL → trim the padding or answer the section for real.

**G7 — Writes confined, nothing extra.**
The run writes ONLY the project and (when confirmed) its seed tasks — or the confirmed update and its
linked tasks. No Logs row, no other database, no other page, no schema edit. Anything else is a
separate, separately-approved action.
FAIL → drop the extra write.

**G8 — Permission to write.**
The operator has seen the exact change — the filled project (create) or the **before → after diff**
(update), plus the goal/ROI/GO line, any seed tasks, and this gate table — and given an explicit yes.
FAIL → do not write. Wait.

**G9 — No drift, or drift surfaced.**
Live schema, select/phase options, template sections, and template structure match the spec snapshot.
If they differ, the diff was surfaced and the build conforms to LIVE, not the stale snapshot.
FAIL → stop and surface the drift before building.

**G10 — Verify-after-act (post-write).**
After the write, re-fetch confirms at the user-visible layer: sections intact + filled, every changed
property set as intended, emoji icon renders, (create) Date Created stamped, seed tasks linked via
`Link to Project Hub`.
FAIL → fix the mismatch; verdict stays ⚠️ until it passes. Never ✅ on the API call's success alone.

**G11 — Goal alignment + ROI (the Strategy Gate). ← the project-defining item.**
The project links a real **Goal** it serves (verified in the Goals Tracker) OR the operator has given
an explicit, recorded exception; **ROI Estimate** is set; and the **Strategy Gate GO/NO-GO** one-liner
is recorded in the body. No goalless projects; no project without a stated return; no NO-GO written as
if it were GO.
FAIL → surface it and ASK before writing. This is the standing HARD RULE, not a suggestion.

## Verdict

- **All ✅** → write (after G8), then re-run G10 on the live page → ✅ + URL.
- **Any ❌ before write** → do not write; report the failing item(s) + the exact next action.
- **G10 ❌ after write** → ⚠️; state precisely what's wrong and fix it before declaring done.

The gate passing is the definition of done. The number of ✅ is a floor to clear, not a number to
chase. Never relax an item to make the table look complete — especially G11.
