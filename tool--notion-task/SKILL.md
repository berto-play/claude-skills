---
name: tool--notion-task
description: >
  Creates, updates, and breaks down tasks in the Notion Tasks Tracker with deterministic, repeatable
  accuracy. Three cases: CREATE a task (+ subtasks if needed), UPDATE an existing task & its
  sub-tasks, and a FALLBACK for anything ambiguous or unforeseen. Activates on "create a task",
  "new ticket", "add a task", "file a ticket", "make a Notion task", "update task", "edit task",
  "change/close/reschedule task", "mark task done", "add a subtask", "break this into subtasks".
  Fetches the live Tasks Tracker schema + templates at runtime, conforms exactly, fills every
  applicable property, runs a fail-closed HARD-RULES gate (template conformance, property
  completion, valid select values only, correct 🎫 icon, no invented data), shows the change for
  confirmation, writes, then verifies by re-fetch. Never writes without confirmation; when unsure,
  it asks.
metadata:
  node_type: skill
  type: capability
  category: safety-critical
classification: trusted-input
type: capability
last_reviewed: 2026-07-28
---

# Notion Task — Create · Update · Subtasks

Turns a freeform request into the right change in the **Tasks Tracker** — every time. Reliability
does not come from being careful; it comes from a **fail-closed gate**: nothing is written unless
every check passes, and every write is verified by re-fetch afterward.

This is a reconciliation loop (the model behind `tool--automation-architect`): define correct →
read live state → build → check → confirm → write → verify. The loop is fixed across all cases.

## The principle that makes it consistent

**Fetch live. Conform to live. Fail closed.**

- Never trust a hardcoded snapshot of the schema or templates. Fetch the live Tasks Tracker data
  source + the relevant template(s) at the start of every run and conform to *that*.
- The snapshot in [references/tasks-tracker-spec.md](references/tasks-tracker-spec.md) is a
  **tripwire**, not the source of truth — if live differs, that is drift; flag it before building.
- Accuracy comes from **refusing to write** a non-conforming change, not from hoping the draft is
  right. A blocked write is the gate working, not the skill failing.

## Routing — pick the case from the request

| Request looks like | Case |
|---|---|
| "create / add / new task", "file a ticket", or multi-part work to break down | **A — Create (+ subtasks)** |
| "update / edit / change / close / reschedule / mark done", "add a subtask to X", or names an existing task to modify | **B — Update (task & sub-tasks)** |
| create-vs-update unclear · task not found · another DB / structural change · bulk / odd input · anything unforeseen | **C — Fallback** |

When A vs B is genuinely unclear, that itself routes to **C** — ask, don't guess.

## Canonical target — the only place this writes

| Thing | Value |
|---|---|
| Database | ✅ Tasks Tracker — `24015c23-aec3-807b-b2f3-c7a7b9024258` |
| Data source (parent) | `collection://24015c23-aec3-800a-972d-000b2157c22a` |
| Parent-task template | "Task Tracker Template Main" — `24015c23-aec3-80e7-b792-dff1a28f5ca4` |
| Sub-task template | "Sub-tasker Template" — `37d15c23-aec3-80e4-914d-f00fbc64466e` (lean: 🎯 Objective · ✅ Done when) |
| Icon (standard) | `🎫` — pass `icon: "🎫"` directly *(built-in `icons/<name>` icons must be the full `https://www.notion.so/icons/<name>.svg` URL — the bare form stores broken)* |

Full property schema, select options, fill policy, and the create/update/subtask mechanics:
[references/tasks-tracker-spec.md](references/tasks-tracker-spec.md).

## Shared contract — every case, every write

Fetch live → conform → **fail-closed GATE** ([references/validation-gate.md](references/validation-gate.md))
→ **confirm with the operator** → write → **verify by re-fetch**. The gate is binary; any FAIL
blocks the write. The skill may write a task **and its subtasks** in one run — but each row is
individually gated and the whole set is confirmed before any write. Nothing else is ever written.

## Case A — Create (+ subtasks if needed)

1. **Capture** — deliverable, done-conditions, area, urgency, dates, links, parent, project/goal. Don't invent; note what's missing.
2. **Observe** — `notion-fetch` the data source (schema + options + `default_page_template`) and the parent-task template.
3. **Drift check** — compare live vs the spec snapshot; if different, surface the diff and conform to LIVE.
4. **Build** — create from the parent-task template (`template_id`, `icon: "🎫"`), fill every applicable property (policy below), then fill the body sections (drop placeholder helper text; never add/remove/reorder sections). Lean, not a canvas.
5. **Subtasks (if the task is multi-deliverable)** — propose a short subtask list. On confirm, create each as a child row from the **Sub-task template** (🎫), set `Parent task` → the new task (parent's `Sub-task` auto-fills). Each subtask is gated: Task Name, Status, Owner, Parent task required; keep it tiny.
6. **Gate** → **Confirm** (show filled task + any subtasks + gate table) → **Create** → **Verify** by re-fetch. ✅ only after the live page confirms.

## Case B — Update (task & sub-tasks)

1. **Identify the target** — operator pastes a Notion **link** (most reliable) OR names the task → `notion-search` and **show the match(es) to confirm**. Never guess which row.
2. **Observe** — `notion-fetch` the row: current properties, body, and its linked sub-tasks.
3. **Build the change** — apply ONLY the requested edits: properties (`update_properties`) and/or body sections (`update_content`), keeping template conformance — never break a section, never blank a MUST-set property by accident. For sub-tasks: add (Case A subtask flow), edit, mark done (Status), or reschedule.
4. **Gate (update mode)** — changed select/status values are valid (no invented options); MUST-set properties stay complete; icon stays 🎫; template structure intact; relations point to real records.
5. **Confirm** — show a **before → after diff** of every field and section that changes (and which sub-tasks). Wait for an explicit yes.
6. **Write → Verify** — apply, then re-fetch and confirm each change landed at the user-visible layer. Never claim done on the API's success alone.

## Case C — Fallback (the safety net)

For anything the two cases don't cleanly cover. **Never produces a silent or guessed write.**

- **Ambiguous** (create vs update, or which task) → ask ONE clarifying question; don't pick silently.
- **Not found** → say so, show the closest matches, ask which — or offer to create it (→ Case A).
- **Out of scope** → name the right home and stop: another database or structural Notion change → `tool--notion-architect`; not a task at all → decline cleanly.
- **Unforeseen** → state what's unclear, propose the safest interpretation, and confirm before any write.

Core rule: **when uncertain, ASK; never invent; never write without confirmation.**

## Property fill policy (summary — full table in spec reference)

| Tier | Properties | Rule |
|---|---|---|
| **MUST set** (FAIL if blank) | Task Name, Status, Area, Priority, Owner | Always a correct value. Subtasks additionally MUST set `Parent task`. If Area/Priority aren't groundable, ASK — never guess a select. |
| **Set when grounded** (blank only if confirmed N/A) | Due Date, Link to Goals Tracker, Link to Project Hub, Parent task, Sprint, Notes / Links, Task Time Est. (hrs) | Set if grounded; blank only when truly N/A and confirmed — never a skip. |
| **Leave blank at creation** | Sub-task, Time Spent (hrs) | Children/time added later (or via Case B). |
| **Never set** (auto / readOnly) | Date Created, Last Edited, Task Progress | System-managed; setting them errors. |

**Status default rule:** `Unscoped` if done-conditions unclear · `Ready` if scoped + actionable · `Next Up` if imminent. Only the 8 live options are valid.

## Completion assist — help fill EVERY property accurately

On any "fill everything" / "all properties" request, do **not** fall back to N/A. For each
WHEN-GROUNDED property: fetch the related data source (Goals `collection://24015c23-aec3-8064-a984-000b9ac3d427`,
Project Hub `collection://24015c23-aec3-807f-bfd0-000be29a5fd5`, Sprint `collection://37215c23-aec3-8055-8977-000bfd73537d`),
present the **real matching records** to choose. Resolve Owner via `notion-get-users`. For ungrounded
selects/dates, ASK. **Never invent a value to dodge a blank** — offer real choices; a blank survives
only as an explicit "N/A (confirmed)."

## Examples

Worked runs — create, create-with-subtasks, update, and fallback — are in
[references/examples.md](references/examples.md).

Inline (create): *"Add a task to redesign the client onboarding flow, high priority, ~6h."* →
Task Name `Redesign client onboarding flow` · Status `Ready` · Area `🎨 Design` · Priority
`High 🔥` · Owner `<operator>` · Est. `6` → gate → confirm → create → verify → ✅ + URL.

## Degrees of freedom

- **Tight (no latitude):** target DB, template conformance, 🎫 icon, valid-values-only, the gate,
  confirm-before-write, verify-after, and "ask when uncertain." These are the contract.
- **Loose (judgment):** body wording, Status/Area/Priority inference from context, how to phrase the
  clarifying question, how to group subtasks, how much context to include.

## Output style

- Lead with the filled task / the before→after diff and the gate table; then the one question if needed; then confirm.
- Plain English, short scannable lines. No persuasion, no filler.
- Completion Report after every write: a status table + a one-line verdict + the page URL.

## Boundaries

- **Writes ONLY to the Tasks Tracker** — a task and (when confirmed) its subtasks, or an update to a
  task and its sub-tasks. Each row gated; the whole set confirmed before any write. No Logs row, no
  other database, no other page.
- Project-level work (a deliverable with phases, its own brief, a goal it ladders to) → `tool--notion-project`. Do not create a project as a task; the two databases are separate and a misfiled project loses its phase tracking.
- Another DB or structural Notion change → `tool--notion-architect`.
- Building or auditing this skill → `tool--sota-skill-qa`. Verification rigor → `tool--rule-model-brutal-truth`. Repeatable-automation design → `tool--automation-architect`.
