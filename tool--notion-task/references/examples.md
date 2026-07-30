# Worked examples

Covers the three cases — **Create (+subtasks)**, **Update**, **Fallback** — plus the ask-path and
drift behavior that apply to all of them.

---

## Example 1 — Create (clean, single deliverable)

**Operator:** "Add a task to redesign the client onboarding flow, high priority, part of the client project, ~6h."

- **Build:** Task Name `Redesign client onboarding flow` · Status `Ready` · Area `🎨 Design` · Priority `High 🔥` · Owner `<operator>` · Link to Project Hub `[Client Project]` (verified) · Est. `6` · others N/A. Body mapped into the template sections; 📝 Subtasks → "None — single deliverable". Icon `🎫`.
- **Gate** 10/10 → **confirm** → **create** → **verify** by re-fetch.

> ✅ Created — [Redesign client onboarding flow](url) · Ready · 🎨 Design · High 🔥 · Est 6 · 🎫 · gate 10/10, verified live.

---

## Example 2 — Create + subtasks (multi-part)

**Operator:** "Create a task to launch the Q3 onboarding redesign — a few pieces: audit the current flow, redesign the screens, ship to staging. High priority, ~12h, client project."

- **Parent:** `Launch Q3 onboarding redesign` · Ready · 🎨 Design · High 🔥 · Project `[Client Project]` · Est `12` (default template, 🎫).
- **Multi-deliverable → propose 3 subtasks**, each from the **Sub-task template** (🎫), `Parent task` → the parent, Status `Ready`, Owner set:
  1. Audit current onboarding flow
  2. Redesign the screens
  3. Ship to staging
- **Confirm** the parent + all 3 subtasks + gate table (gate runs on each row). On yes → create the parent, then the 3 children (parent's `Sub-task` auto-fills from `Parent task`). **Verify** all 4 by re-fetch.

> ✅ Created parent + 3 subtasks linked. Each gate 10/10, verified.

---

## Example 3 — Update (task & sub-tasks)

**Operator** (pastes a task link): "Mark the 'Redesign the screens' subtask done, and push the parent's due date to next Monday."

- **Identify** the target from the URL; fetch it + its sub-tasks (don't guess).
- **Build change:** parent `Due Date` → `2026-06-22`; subtask "Redesign the screens" `Status` → `Completed`. Nothing else touched.
- **Gate (update):** valid Status option ✓, no MUST-set blanked ✓, template intact ✓.
- **Confirm — before → after diff:**

> Parent · Due Date: — → **2026-06-22**
> Subtask "Redesign the screens" · Status: In Progress → **Completed**

- On yes → `update_properties` on both rows → **verify** by re-fetch.

**Why:** updates change only what was asked, show the diff first, and never blank a required field.

---

## Example 4 — Fallback (the safety net)

- **Ambiguous:** "do the client onboarding task" → create a new one or update an existing one? → **ask which**; don't pick silently.
- **Not found:** "update the budget task" → no match in Tasks Tracker → say so, show the closest matches, offer to create it (→ Case A).
- **Out of scope:** "add a column to the Tracker" → that's a structural change → route to `tool--notion-architect`; don't do it here.

**Core:** ask / route / never guess-write.

---

## Carried behavior (all cases)

- **Ungroundable select** (Area/Priority not in the request) → ONE consolidated question; never guess. A wrong Area is worse than one question.
- **Drift** — if the live template/schema/icon differs from the spec snapshot, surface the diff, conform to **LIVE**, and flag the snapshot for update. Building on a stale snapshot is exactly the failure this skill prevents.
