---
name: tool--notion-project
description: >
  Creates, updates, and scopes projects in the Notion Project Hub with deterministic, repeatable
  accuracy. Three cases: CREATE a project (+ seed tasks if needed), UPDATE an existing project & its
  linked tasks, and a FALLBACK for anything ambiguous or unforeseen. Activates on "create a project",
  "new project", "start a project", "spin up a project", "write a project brief", "update project",
  "edit project", "change/close/kill/reschedule project", "move project to a phase",
  "mark project done", "scope this project". Fetches the live Project Hub
  schema + Project Brief template at runtime, conforms exactly, fills every applicable property, runs
  a fail-closed HARD-RULES gate (template conformance, property completion, valid select values only,
  meaningful emoji icon, GOAL-ALIGNMENT + ROI check, no invented data), shows the change for
  confirmation, writes, then verifies by re-fetch. Never writes without confirmation; when unsure, it
  asks. No goalless projects.
metadata:
  node_type: skill
  type: capability
  category: safety-critical
  last_reviewed: 2026-07-06
classification: trusted-input
last_reviewed: 2026-07-06
---

# Notion Project — Create · Update · Scope

Turns a freeform request into the right change in the **Project Hub** — every time. Reliability does
not come from being careful; it comes from a **fail-closed gate**: nothing is written unless every
check passes, and every write is verified by re-fetch afterward.

This is the sibling of [`tool--notion-task`](../tool--notion-task/SKILL.md). Same reconciliation loop
(the model behind `tool--automation-architect`): define correct → read live state → build → check →
confirm → write → verify. Same fixed loop across all cases. The difference is the target (**Project
Hub**, not Tasks Tracker), the richer artifact (a **strategy brief**, not a work ticket), and one
extra non-negotiable gate: **goal alignment + ROI**.

## The principle that makes it consistent

**Fetch live. Conform to live. Fail closed.**

- Never trust a hardcoded snapshot of the schema or template. Fetch the live Project Hub data source +
  the Project Brief template at the start of every run and conform to *that*.
- The snapshot in [references/project-hub-spec.md](references/project-hub-spec.md) is a **tripwire**,
  not the source of truth — if live differs, that is drift; flag it before building.
- Accuracy comes from **refusing to write** a non-conforming change, not from hoping the draft is
  right. A blocked write is the gate working, not the skill failing.

## The project-defining rule — no goalless projects

A project is bigger than a task, so it earns a bigger bar. Per the operator's standing HARD RULE,
**every project is vetted against a goal (ROI + alignment) before any work.** This is the template's
own **Strategy Gate (GO / NO-GO)** and it is a fail-closed gate item here (G11). A project with no
linked Goal and no recorded GO decision does not get written — the skill asks first. Never spin up a
project just because it was requested; confirm it's worth resources.

## Routing — pick the case from the request

| Request looks like | Case |
|---|---|
| "create / add / new / start a project", "write a project brief", or a body of work that needs planning + multiple tasks | **A — Create (+ seed tasks)** |
| "update / edit / change / close / kill / reschedule project", "move X to Build", "mark project done", or names an existing project to modify | **B — Update (project & linked tasks)** |
| create-vs-update unclear · project not found · task-sized (not a project) · another DB / structural change · bulk / odd input · anything unforeseen | **C — Fallback** |

When A vs B is genuinely unclear, that itself routes to **C** — ask, don't guess. When the work is
really a single deliverable (task-sized), route to `tool--notion-task`, not here.

## Canonical target — the only place this writes

| Thing | Value |
|---|---|
| Database | 🗂️ Project Hub — `24015c23-aec3-80f7-9aad-c83ceca8cdc6` |
| Data source (parent) | `collection://24015c23-aec3-807f-bfd0-000be29a5fd5` |
| Default template | "Project Brief Template Main" — `24015c23-aec3-803a-b7f3-cab866b318a7` |
| Icon | A **meaningful emoji** chosen for the project (e.g. 🚪, 🚀, 🎨) — pass the emoji directly. NOT the template's `paste_blue` placeholder. Propose one; confirm with the operator. |
| Seed tasks (optional) | Created in the **Tasks Tracker** via the `tool--notion-task` mechanism, each with `Link to Project Hub` → this project. |

Full property schema, select options, fill policy, template structure, and the create/update
mechanics: [references/project-hub-spec.md](references/project-hub-spec.md).

## Shared contract — every case, every write

Fetch live → conform → **fail-closed GATE** ([references/validation-gate.md](references/validation-gate.md))
→ **confirm with the operator** → write → **verify by re-fetch**. The gate is binary; any FAIL blocks
the write. The skill may write a project **and seed its tasks** in one run — but the project is gated,
each task is individually gated (per `tool--notion-task`), and the whole set is confirmed before any
write. Nothing else is ever written.

## Case A — Create (+ seed tasks if needed)

1. **Capture** — purpose/outcome, audience, scope (in/out), value/leverage, success + ROI, phase, type, timeframe, links, goal it serves. Don't invent; note what's missing.
2. **Observe** — `notion-fetch` the data source (schema + options + `default_page_template`) and the Project Brief template.
3. **Drift check** — compare live vs the spec snapshot; if different, surface the diff and conform to LIVE.
4. **Goal-alignment vet (G11)** — identify the Goal this project serves (fetch the Goals DS, show real matches), set **ROI Estimate**, and record the **Strategy Gate GO/NO-GO** one-liner. If no goal fits, ASK before proceeding — don't create a goalless project.
5. **Build** — create from the Project Brief template (`template_id`, meaningful emoji `icon`), fill every applicable property (policy below), then fill the body sections 01–09 + the Strategy Gate (drop placeholder helper text and the "How to use" callout; never add/remove/reorder sections; tick the checkboxes you can honestly answer). Lean but deep.
6. **Seed tasks (if the project has obvious first work)** — propose a short task list. On confirm, create each via `tool--notion-task` (Tasks Tracker, 🎫), setting `Link to Project Hub` → the new project. Each task is gated by that skill. Keep the set small — the project brief holds the plan.
7. **Gate** → **Confirm** (show filled project + goal/ROI/GO + any seed tasks + gate table) → **Create** → **Verify** by re-fetch. ✅ only after the live page confirms.

## Case B — Update (project & linked tasks)

1. **Identify the target** — operator pastes a Notion **link** (most reliable) OR names the project → `notion-search` and **show the match(es) to confirm**. Never guess which row.
2. **Observe** — `notion-fetch` the row: current properties, body, and its linked tasks.
3. **Build the change** — apply ONLY the requested edits: properties (`update_properties`) and/or body sections (`update_content`), keeping template conformance — never break a section, never blank a MUST-set property by accident. For linked tasks: add (via `tool--notion-task`), edit, mark done, or reschedule.
4. **Gate (update mode)** — changed select/status/phase values are valid (no invented options); MUST-set properties stay complete; a meaningful emoji icon stays; template structure intact; relations point to real records; goal link + ROI not silently dropped.
5. **Confirm** — show a **before → after diff** of every field and section that changes (and which tasks). Wait for an explicit yes.
6. **Write → Verify** — apply, then re-fetch and confirm each change landed at the user-visible layer. Never claim done on the API's success alone.

## Case C — Fallback (the safety net)

For anything the two cases don't cleanly cover. **Never produces a silent or guessed write.**

- **Ambiguous** (create vs update, or which project) → ask ONE clarifying question; don't pick silently.
- **Not found** → say so, show the closest matches, ask which — or offer to create it (→ Case A).
- **Task-sized, not a project** → say so and route to `tool--notion-task`; a single deliverable is a task.
- **Out of scope** → name the right home and stop: another database or structural Notion change → `tool--notion-architect`; not a project at all → decline cleanly.
- **No fitting goal** → surface it; propose creating/adjusting a Goal first, or confirm an explicit exception. Don't write a goalless project silently.
- **Unforeseen** → state what's unclear, propose the safest interpretation, and confirm before any write.

Core rule: **when uncertain, ASK; never invent; never write without confirmation.**

## Property fill policy (summary — full table in spec reference)

| Tier | Properties | Rule |
|---|---|---|
| **MUST set** (FAIL if blank) | Project Name, Status, Project Type, Phase, Owner, ROI Estimate | Always a correct value. `ROI Estimate` is MUST because of the goal-alignment rule. If Type/Phase aren't groundable, ASK — never guess a select. |
| **Vet + set** (the G11 rule) | Link to Goals Tracker | Link the real Goal this project serves. Blank survives ONLY as an explicit, confirmed exception recorded in the Strategy Gate — never a silent skip. |
| **Set when grounded** (blank only if confirmed N/A) | Urgency, Effort, Quarter, Timeframe, Client, Local Path, Notes / Links, Link to Tasks Tracker, Blocked by, Blocking | Set if grounded; blank only when truly N/A and confirmed. Urgency + Effort + ROI feed the Priority Score — set them when known. |
| **Never set** (auto / readOnly) | Date Created, Last Edited, Priority Score, Progress, Time Est. (hrs), Time Spent (hrs), Est. (d/w/mo), Spent (d/w/mo) | System-managed; setting them errors. |

**Status vs Phase:** Status is the workflow state (`Unscoped` → … → `Completed`/`Killed`). Phase is the
lifecycle (`Idea` → `Discovery` → … → `Maintain` → `Archived`). A new project is typically Status
`Unscoped`/`Ready` and Phase `Idea`/`Discovery`. Only the live options are valid.

## Completion assist — help fill EVERY property accurately

On any "fill everything" / "all properties" request, do **not** fall back to N/A. For each WHEN-GROUNDED
property: fetch the related data source (Goals `collection://24015c23-aec3-8064-a984-000b9ac3d427`,
Tasks `collection://24015c23-aec3-800a-972d-000b2157c22a`), present the **real matching records** to
choose. Resolve Owner/Client via `notion-get-users`. For ungrounded selects/dates, ASK. **Never invent
a value to dodge a blank** — offer real choices; a blank survives only as an explicit "N/A (confirmed)."

## Examples

Worked runs — create, create-with-seed-tasks, update, goal-alignment ask, and fallback — are in
[references/examples.md](references/examples.md).

Inline (create): *"Start a project to build a macOS menu-bar Notion capture app, product, ~1 week."* →
Project Name `FrontDoor` · Status `Ready` · Type `🚀 Product / App` · Phase `Idea` · Owner `<operator>`
· ROI `High` · Goal `[the goal it serves]` (vetted) · GO recorded · icon 🚪 → gate → confirm → create →
verify → ✅ + URL.

## Degrees of freedom

- **Tight (no latitude):** target DB, template conformance, meaningful-emoji icon, valid-values-only,
  the goal-alignment + ROI gate, the full gate, confirm-before-write, verify-after, and "ask when
  uncertain." These are the contract.
- **Loose (judgment):** section wording, which emoji to propose, Status/Type/Phase inference from
  context, how to phrase the clarifying question, how to group seed tasks, how much depth each section
  earns.

## Output style

- Lead with the filled project / the before→after diff, the goal+ROI+GO line, and the gate table; then the one question if needed; then confirm.
- Plain English, short scannable lines. No persuasion, no filler.
- Completion Report after every write: a status table + a one-line verdict + the page URL.

## Boundaries

- **Writes ONLY to the Project Hub** — a project and (when confirmed) its seed tasks — or an update to a
  project and its linked tasks. The project is gated; each seed task is gated by `tool--notion-task`; the
  whole set is confirmed before any write. No Logs row, no other database, no other page, no schema edit.
- A single deliverable (task-sized) → `tool--notion-task`. Another DB or structural Notion change →
  `tool--notion-architect`.
- Building or auditing this skill → `tool--sota-skill-qa`. Verification rigor →
  `tool--rule-model-brutal-truth`. Repeatable-automation design → `tool--automation-architect`.
