# Tasks Tracker — canonical spec (the drift tripwire)

Snapshot captured **2026-06-17**. This is NOT the source of truth — the **live** data source and
default template are. Fetch them every run (SKILL.md step 2) and compare against this snapshot
(step 3). If they differ, conform to LIVE and flag that this file needs updating.

## IDs

| Thing | ID / URL |
|---|---|
| Database | `24015c23-aec3-807b-b2f3-c7a7b9024258` |
| Data source (parent for create) | `collection://24015c23-aec3-800a-972d-000b2157c22a` |
| Default template (`default_page_template`) | `24015c23-aec3-80e7-b792-dff1a28f5ca4` ("Task Tracker Template Main") |
| Default template icon | `🎫` (emoji — set directly. Standard set 2026-06-17. If a built-in `icons/<name>` is ever used, pass the full `https://www.notion.so/icons/<name>.svg` URL; the bare form stores broken) |
| Sub-task template | `37d15c23-aec3-80e4-914d-f00fbc64466e` ("Sub-tasker Template", icon 🎫) — lean child template: 🎯 Objective · ✅ Done when. Use for subtasks (Case A step 5). |
| Related: Goals Tracker DS | `collection://24015c23-aec3-8064-a984-000b9ac3d427` |
| Related: Project Hub DS | `collection://24015c23-aec3-807f-bfd0-000be29a5fd5` |
| Related: Sprints DS | `collection://37215c23-aec3-8055-8977-000bfd73537d` |

> Parent tasks use the default template above; **subtasks use the Sub-task template**. The two
> "New page" templates on this DS are unused — ignore them.

## Full property schema + fill policy

| Property | Type | Tier | Value rule |
|---|---|---|---|
| **Task Name** | title | MUST | Concise deliverable. No emoji, no icon glyph, no prefix in the title. |
| **Status** | status | MUST | One of the 8 options below. Default rule: `Unscoped` if scope unclear · `Ready` if scoped+actionable · `Next Up` if imminent. |
| **Area** | select | MUST | One of the 6 options below. If not groundable from the request → ASK. Never guess. |
| **Priority** | select | MUST | One of the 4 options below. If not groundable → ASK. Never guess. |
| **Owner** | person | MUST | Default = the operator. Resolve the user ID at runtime via `notion-get-users` / user search; do not hardcode it. JSON array of user IDs. |
| **Due Date** | date | WHEN-GROUNDED | Only if a date is given/derivable. A made-up due date is worse than blank. Format: `date:Due Date:start` (ISO), `date:Due Date:is_datetime` (0 for date-only). |
| **Link to Goals Tracker** | relation | WHEN-GROUNDED | Link only a real Goal that the task serves. Fetch the Goals DS to find it. JSON array of page URLs. |
| **Link to Project Hub** | relation | WHEN-GROUNDED | Link only a real Project. Fetch the Project Hub DS to find it. JSON array of page URLs. |
| **Parent task** | relation (limit 1) | WHEN-GROUNDED | Only if the request says this is a subtask of an existing task. JSON of a single page URL. |
| **Sprint** | relation (limit 1) | WHEN-GROUNDED | Link the relevant sprint if applicable. JSON of a single page URL. |
| **Notes / Links** | text | WHEN-GROUNDED | Any URLs/refs from the request. |
| **Task Time Est. (hrs)** | number | WHEN-GROUNDED | If estimable. JS number. Mirror the ⏱️ Estimate body section. |
| **Sub-task** | relation | LEAVE BLANK | Children are created in their own runs. |
| **Time Spent (hrs)** | number | LEAVE BLANK | New ticket — none spent yet. |
| **Date Created** | created_time | NEVER SET | Auto-stamps. Satisfies the "every ticket has a creation date" rule inherently. |
| **Last Edited** | last_edited_time | NEVER SET | Auto. readOnly. |
| **Task Progress** | rollup | NEVER SET | Auto. readOnly. |

"Blank only if confirmed N/A" means: in the confirmation step, show the property as `— N/A
(confirmed)` so the operator can see it was a deliberate decision, not a skip.

## Select / status options (EXACT — copy verbatim, never invent)

- **Status** (status): `Unscoped` · `Ready` · `Next Up` · `On Hold` · `In Progress` · `Blocked` · `Completed` · `Killed`
- **Area** (select): `📸 Photography` · `🎨 Design` · `🧩 Fractional` · `💰 Investing` · `🔭 Exploration` · `🏢 Studio`
- **Priority** (select): `🔴 Urgent` · `High 🔥` · `Medium` · `Low`

If the live schema shows an option not listed here → that is drift (step 3). Use the live option.

## Default template structure (snapshot)

Body the template injects (fill each section, drop the helper text, keep the structure):

```
> 📋 A Task is a specific deliverable with a measurable outcome that contributes to a Goal,
>   Project, or Initiative. If the work requires its own plan, milestones, or multiple
>   deliverables, it should be broken down into Subtasks or promoted to a Project.

#### 🎯 Objective
One sentence describing the outcome.

#### ✅ Acceptance Criteria
What must be true for this Task to be considered complete?

---
#### ⏱️ Estimate
Hours, points, t-shirt size, or effort level.

---
#### 🚧 Blocked By (optional)
Dependencies, decisions, approvals, or prerequisite work.

---
#### 📊 Scope Status
- Unscoped
- Ready
- In Progress

---
#### 📝 Subtasks
Create the child tasks required to complete this Task.

---
#### 📎 Context (optional)
Links, files, references, decisions, or handoff information.
```

**Mapping:** 🎯 Objective ← the deliverable (one sentence). ✅ Acceptance Criteria ← bulleted
done-conditions. ⏱️ Estimate ← effort (mirror Task Time Est. property). 🚧 Blocked By ←
dependencies or "None." 📊 Scope Status ← mirror the Status property. 📝 Subtasks ← child tasks
needed (or "None — single deliverable"). 📎 Context ← links/refs (mirror Notes / Links).

## Create mechanism (exact)

1. `notion-create-pages`:
   - `parent`: `{ "type": "data_source_id", "data_source_id": "24015c23-aec3-800a-972d-000b2157c22a" }`
   - `template_id`: `"24015c23-aec3-80e7-b792-dff1a28f5ca4"`
   - `icon`: the live template icon — currently the `🎫` emoji; pass `"🎫"` directly. (If the template ever uses a built-in `icons/<name>` icon, pass the full `https://www.notion.so/icons/<name>.svg` URL — the bare form stores broken — or omit `icon` to inherit. Always verify the rendered icon.)
   - `properties`: the map (correct names from the live schema, expanded formats above)
   - **omit `content`** — the template provides the body.
2. `notion-fetch` the new page to read the template's placeholder body.
3. `notion-update-page` (`command: "update_content"`) — replace each placeholder helper line
   (`old_str`) with the real section content (`new_str`).
4. `notion-fetch` again → verify (SKILL.md step 8).

> Trap (from hard-won experience): the create tool applies the DB default template when
> `template_id` is given, but ALWAYS verify the created row against the **echoed schema** — locked
> templates can spawn locked rows, and property names must match the live schema exactly.

## Subtask mechanism (Case A, step 5)

Each subtask is its own row in the same data source, linked to the parent:

1. `notion-create-pages` — `parent`: the Tasks Tracker data source; `template_id`:
   `"37d15c23-aec3-80e4-914d-f00fbc64466e"` (Sub-task template); `icon`: `"🎫"`; `properties`:
   MUST set `Task Name`, `Status`, `Owner`, and **`Parent task`** = JSON of the parent's page URL
   (e.g. `"[\"https://www.notion.so/<parent-id>\"]"`). Area/Priority inherit from the parent unless
   told otherwise.
2. Fill the two body sections (🎯 Objective · ✅ Done when) via `update_content`.
3. The parent's `Sub-task` relation **auto-fills** from the `Parent task` back-link — do NOT set it
   by hand (baked/duplicate relations are a known trap).
4. Verify each child by re-fetch. Keep subtasks tiny — the full plan lives on the parent.

## Update mechanism (Case B)

1. **Find the target row.** Operator pastes a page URL (use directly) OR names the task →
   `notion-search` (query_type `internal`, `data_source_url` = the Tasks Tracker DS) and present the
   match(es) to confirm. Never guess the row.
2. **Read** it with `notion-fetch` — current properties + body + linked sub-tasks.
3. **Change properties:** `notion-update-page` `command: "update_properties"`, passing ONLY the
   changed properties (omitted ones are left untouched). Same expanded formats + valid-values rules
   as create. To clear a value pass `null` — but NEVER null a MUST-set property.
4. **Change body:** `notion-update-page` `command: "update_content"` with `content_updates`
   (old_str → new_str) matching the exact existing text. Keep template sections intact.
5. **Sub-tasks:** add (subtask mechanism above), or update each child the same way (e.g. Status →
   `Completed`, reschedule Due Date).
6. **Verify** every change by re-fetch (SKILL.md Case B step 6). Never claim done on API success.
