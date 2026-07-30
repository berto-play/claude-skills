# Project Hub — canonical spec (the drift tripwire)

Snapshot captured **2026-07-06**. This is NOT the source of truth — the **live** data source and
default template are. Fetch them every run (SKILL.md step 2) and compare against this snapshot
(step 3). If they differ, conform to LIVE and flag that this file needs updating.

## IDs

| Thing | ID / URL |
|---|---|
| Database | `24015c23-aec3-80f7-9aad-c83ceca8cdc6` |
| Data source (parent for create) | `collection://24015c23-aec3-807f-bfd0-000be29a5fd5` |
| Default template (`default_page_template`) | `24015c23-aec3-803a-b7f3-cab866b318a7` ("Project Brief Template Main") |
| Template's own icon | `icons/paste_blue` (a **placeholder** — do NOT copy it onto real projects). Real projects use a meaningful emoji chosen per project. |
| Related: Goals Tracker DS | `collection://24015c23-aec3-8064-a984-000b9ac3d427` |
| Related: Tasks Tracker DS | `collection://24015c23-aec3-800a-972d-000b2157c22a` |
| Related: Sprints DS | `collection://37215c23-aec3-8055-8977-000bfd73537d` |

> The Project Hub is nested under Home → Deliver Work in the workspace tree. Projects **contain**
> tasks: a Task's `Link to Project Hub` back-links here, and the project's `Link to Tasks Tracker`
> + `Progress` rollup reflect them.

## Icon policy (differs from tool--notion-task)

Unlike tasks (fixed `🎫`), **each project gets its own meaningful emoji** (FrontDoor = 🚪, a product
= 🚀, a design project = 🎨). Pass the emoji directly in `icon`. Propose one from the project's
subject and confirm with the operator. Never ship a project with the template's `paste_blue`
placeholder. If a built-in `icons/<name>` icon is ever wanted instead, pass the full
`https://www.notion.so/icons/<name>.svg` URL — the bare `icons/<name>` form stores broken.

## Full property schema + fill policy

| Property | Type | Tier | Value rule |
|---|---|---|---|
| **Project Name** | title | MUST | Concise project name. No emoji, no icon glyph, no prefix in the title. |
| **Status** | status | MUST | One of the 8 options below. New project: `Unscoped` if not yet scoped · `Ready` if scoped + actionable · `Next Up` if imminent. |
| **Project Type** | select | MUST | One of the options below. If not groundable from the request → ASK. Never guess. |
| **Phase** | multi_select | MUST | One or more of the 9 lifecycle options. A new project is usually `Idea` or `Discovery`. Usually a single value. |
| **Owner** | person | MUST | Default = the operator. Resolve the user ID at runtime via `notion-get-users`; do not hardcode it. JSON array of user IDs. |
| **ROI Estimate** | select | MUST | `High` / `Medium` / `Low`. Required by the goal-alignment rule — a project must state its return. If ungroundable → ASK. |
| **Link to Goals Tracker** | relation | VET (G11) | Link the real Goal this project serves. Fetch the Goals DS to find it. JSON array of page URLs. Blank ONLY as an explicit, confirmed exception recorded in the Strategy Gate. |
| **Urgency** | select | WHEN-GROUNDED | `High` / `Medium` / `Low`. Feeds Priority Score. Set when known. |
| **Effort** | select | WHEN-GROUNDED | `High` / `Medium` / `Low`. Feeds Priority Score. Set when known. |
| **Quarter** | select | WHEN-GROUNDED | One of the options below. Set if the timeframe is known. |
| **Timeframe** | date (range) | WHEN-GROUNDED | Start (+ optional end). Format: `date:Timeframe:start` (ISO), `date:Timeframe:end` (ISO or omit), `date:Timeframe:is_datetime` (0 date-only / 1 datetime). A made-up date is worse than blank. |
| **Client** | person | WHEN-GROUNDED | Only for client work. Resolve via `notion-get-users`. JSON array of user IDs. |
| **Local Path** | text | WHEN-GROUNDED | Filesystem path where the work lives, if any. |
| **Notes / Links** | text | WHEN-GROUNDED | One-liner summary + any URLs/refs from the request. |
| **Link to Tasks Tracker** | relation | WHEN-GROUNDED | Usually auto-populated by each task's `Link to Project Hub` back-link — do NOT set by hand for seed tasks. |
| **Blocked by** | relation (self) | WHEN-GROUNDED | Other Project Hub rows this depends on. |
| **Blocking** | relation (self) | WHEN-GROUNDED | Other Project Hub rows this blocks. |
| **Date Created** | created_time | NEVER SET | Auto-stamps. Satisfies the "every project has a creation date" rule inherently. |
| **Last Edited** | last_edited_time | NEVER SET | Auto. readOnly. |
| **Priority Score** | formula | NEVER SET | Auto (derived from Urgency/Effort/ROI). readOnly. |
| **Progress** | rollup | NEVER SET | Auto (from linked tasks' status). readOnly. |
| **Time Est. (hrs)** | rollup | NEVER SET | Auto (sum of linked tasks' estimates). readOnly. |
| **Time Spent (hrs)** | rollup | NEVER SET | Auto (sum of linked tasks' time spent). readOnly. |
| **Est. (d/w/mo)** | formula | NEVER SET | Auto. readOnly. |
| **Spent (d/w/mo)** | formula | NEVER SET | Auto. readOnly. |

"Blank only if confirmed N/A" means: in the confirmation step, show the property as `— N/A
(confirmed)` so the operator can see it was a deliberate decision, not a skip.

## Select / status / phase options (EXACT — copy verbatim, never invent)

- **Status** (status): `Unscoped` · `Ready` · `Next Up` · `On Hold` · `In Progress` · `Blocked` · `Completed` · `Killed`
- **Phase** (multi_select): `Idea` · `Discovery` · `Define` · `Refine` · `Build` · `Test` · `Launch` · `Maintain` · `Archived`
- **Project Type** (select): `🤝 Client Work` · `🤖 Agent / System` · `💰 Grant / Funding` · `📚 Content / GPT` · `🚀 Product / App` · `⚡ Mini / Internal` · `🎨 Creative / Personal` · `Digital Product` · `Physical Product` · `Service` · `Installation`
- **Urgency** (select): `High` · `Medium` · `Low`
- **Effort** (select): `High` · `Medium` · `Low`
- **ROI Estimate** (select): `High` · `Medium` · `Low`
- **Quarter** (select): `FY26 Q1` · `FY26 Q2` · `FY26 Q3` · `FY26 Q4` · `Vision / Someday`

> The last four Project Type options (`Digital Product`, `Physical Product`, `Service`,
> `Installation`) are legacy/no-emoji; prefer the emoji options for new projects. If the live schema
> shows an option not listed here → that is drift (step 3). Use the live option.

## Default template structure (snapshot)

The Project Brief template injects a **Strategy → Execution** brief. Fill each section, drop the
helper text and the "How to use" callout, keep the structure and order. Tick only the checkboxes you
can answer honestly (the template's own instruction: *Lean but deep. Strategy first.*).

```
> Lean but deep. Tick a section only when the answer is honest. Strategy first.
>   Why → Scope → Value → Success → Review → Execution → Context

# What is a Project?        (reference callout — drop on real projects)
<callout 🧭 How to use>     (drop on real projects)

# Strategy — The Thinking
## Why
### 01 · Purpose & Outcome        (toggle) — outcome, why now, what "done" looks like
### 02 · Audience & Insight        (toggle) — who for, the insight, current behaviour, desired change
### 03 · Core Proposition          (toggle) — the one guiding idea (fails if it needs "and")
## Scope
### 04 · Scope & Constraints        (toggle) — IN / OUT / constraints
## Value
### 05 · Leverage & Capability      (toggle) — reusable asset, scale beyond v1, what it unlocks
## Success
### 06 · Success & ROI              (toggle) — short-term, long-term, 💰 value, 💸 cost, cost vs impact
## Review
<callout ⚖️ Strategy Gate>          — ✅ GO / ❌ NO-GO, one-line decision, why it deserves resources

# Execution — The Doing
### 07 · Phases                     (table: Phase | Purpose/Status | Outcome)
### 08 · Risks & Next Action        (ranked risks + the single highest-leverage next action)
## Context
### 09 · Links & Parking Lot        (🔗 Links & Local Paths · 🪦 Parking Lot)

# Trackers — Tasks & Sprints        (embedded Tasks view + add-task button — leave the block intact)
```

**Mapping:** 01 ← purpose/outcome + why-now + done-definition. 02 ← audience + insight. 03 ← the one
idea. 04 ← in/out/constraints. 05 ← reusable asset + scale + unlocks. 06 ← short/long outcomes + 💰
value + 💸 cost + verdict. **Strategy Gate ← the GO/NO-GO decision + the goal it serves (G11).** 07 ←
phase table. 08 ← ranked risks + one next action. 09 ← links + local path (mirror Local Path / Notes)
+ parking lot. Keep the embedded Trackers block at the bottom untouched.

## Create mechanism (exact)

1. `notion-create-pages`:
   - `parent`: `{ "type": "data_source_id", "data_source_id": "24015c23-aec3-807f-bfd0-000be29a5fd5" }`
   - `template_id`: `"24015c23-aec3-803a-b7f3-cab866b318a7"`
   - `icon`: the chosen meaningful emoji (e.g. `"🚪"`) — pass directly. Do NOT copy the template's `paste_blue`.
   - `properties`: the map (correct names from the live schema, expanded formats above)
   - **omit `content`** — the template provides the body.
2. `notion-fetch` the new page to read the template's placeholder body.
3. `notion-update-page` (`command: "update_content"`) — drop the "What is a Project?" reference block
   and the 🧭 "How to use" callout, replace each section's helper line (`old_str`) with the real
   content (`new_str`), and fill the Strategy Gate. Never add/remove/reorder the numbered sections;
   leave the embedded Trackers block intact.
4. `notion-fetch` again → verify (SKILL.md step 7).

> Trap (from hard-won experience): the create tool applies the DB default template when `template_id`
> is given, but ALWAYS verify the created row against the **echoed schema** — locked templates can
> spawn locked rows, property names must match the live schema exactly, and the icon must actually
> render (not a broken `/icons/...` path).

## Seed-task mechanism (Case A, step 6) — delegate, don't reinvent

Seed tasks are real Tasks Tracker rows, not project sub-blocks. Create each via the
**`tool--notion-task`** flow (its Case A), with one addition: set `Link to Project Hub` = JSON of the
new project's page URL (e.g. `"[\"https://www.notion.so/<project-id>\"]"`). The project's `Link to
Tasks Tracker` and `Progress` rollup **auto-fill** from that back-link — do NOT set them by hand.
Keep the seed set small; the full plan lives in the project brief. Each task is gated by
`tool--notion-task`.

## Update mechanism (Case B)

1. **Find the target row.** Operator pastes a page URL (use directly) OR names the project →
   `notion-search` (query_type `internal`, `data_source_url` = the Project Hub DS) and present the
   match(es) to confirm. Never guess the row.
2. **Read** it with `notion-fetch` — current properties + body + linked tasks.
3. **Change properties:** `notion-update-page` `command: "update_properties"`, passing ONLY the
   changed properties (omitted ones are left untouched). Same expanded formats + valid-values rules
   as create. To clear a value pass `null` — but NEVER null a MUST-set property, and never silently
   drop the goal link or ROI.
4. **Change body:** `notion-update-page` `command: "update_content"` with `content_updates`
   (old_str → new_str) matching the exact existing text. Keep template sections intact.
5. **Linked tasks:** add (via `tool--notion-task`), or update each the same way (Status →
   `Completed`, reschedule, etc.).
6. **Verify** every change by re-fetch (SKILL.md Case B step 6). Never claim done on API success.
