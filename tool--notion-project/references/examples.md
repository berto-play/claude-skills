# Worked examples

Covers the three cases — **Create (+seed tasks)**, **Update**, **Fallback** — plus the
goal-alignment ask-path and drift behavior that apply to all of them.

---

## Example 1 — Create (clean, single project)

**Operator:** "Start a project to build a macOS menu-bar Notion capture app. Product, ~1 week, serves my 'ship useful Mac tools' goal. High ROI."

- **Goal vet (G11):** fetch the Goals DS, confirm the "ship useful Mac tools" goal exists, link it. ROI `High`. Strategy Gate → **✅ GO** — "a 3-second capture door removes the biggest friction in the system."
- **Build:** Project Name `FrontDoor` · Status `Ready` · Type `🚀 Product / App` · Phase `Idea` · Owner `<operator>` · ROI `High` · Link to Goals `[ship useful Mac tools]` (verified) · Effort `High` · Quarter `FY26 Q3` · others N/A. Icon 🚪 (proposed + confirmed). Body 01–09 filled + Strategy Gate; reference callouts dropped; Trackers block intact.
- **Gate** 11/11 → **confirm** → **create** → **verify** by re-fetch.

> ✅ Created — [FrontDoor](url) · Ready · 🚀 Product / App · Idea · ROI High · Goal linked · 🚪 · gate 11/11, verified live.

---

## Example 2 — Create + seed tasks (has obvious first work)

**Operator:** "Create a project to launch the studio website replacement — a few first tasks: pick the platform, migrate content, ship the landing page. Client work, medium ROI, serves the 'studio online' goal."

- **Goal vet (G11):** link the "studio online" goal, ROI `Medium`, Strategy Gate ✅ GO recorded.
- **Project:** `Studio Website Replacement` · Ready · 🤝 Client Work · Phase `Discovery` · Owner set · ROI `Medium` · Goal `[studio online]` · icon 🌐. Brief filled.
- **Obvious first work → propose 3 seed tasks**, each created via `tool--notion-task` (🎫), `Link to Project Hub` → the new project, Status `Ready`, Owner set:
  1. Pick the platform (≤ $5/mo)
  2. Migrate content
  3. Ship the landing page
- **Confirm** the project + goal/ROI/GO + all 3 seed tasks + gate table (project gated here; each task gated by `tool--notion-task`). On yes → create the project, then the 3 tasks (project's `Link to Tasks Tracker` + `Progress` auto-fill from each task's back-link). **Verify** all 4 by re-fetch.

> ✅ Created project + 3 seed tasks linked. Project gate 11/11; each task gate 10/10; verified.

---

## Example 3 — Update (project & linked tasks)

**Operator** (pastes a project link): "Move FrontDoor to the Launch phase, mark it Completed, and close the 'ship the landing page' task."

- **Identify** the target from the URL; fetch it + its linked tasks (don't guess).
- **Build change:** project `Phase` → `Launch`, `Status` → `Completed`; task "ship the landing page" `Status` → `Completed`. Nothing else touched. Goal link + ROI untouched.
- **Gate (update):** valid Phase + Status options ✓, no MUST-set blanked ✓, goal/ROI intact (G11) ✓, template intact ✓.
- **Confirm — before → after diff:**

> Project · Phase: Build → **Launch** · Status: In Progress → **Completed**
> Task "ship the landing page" · Status: In Progress → **Completed**

- On yes → `update_properties` on both rows → **verify** by re-fetch.

**Why:** updates change only what was asked, show the diff first, and never blank a required field or drop the goal link.

---

## Example 4 — Goal-alignment ask (G11 in action)

**Operator:** "Spin up a project to reorganize my icon library."

- **Goal vet:** fetch the Goals DS — no goal clearly covers this, and it reads as low-ROI busywork.
- **Do NOT create silently.** Ask ONE question:

> This doesn't map to a current Goal and looks low-ROI. Before I create it: which Goal does it serve, or is it an explicit exception? (Or it may be task-sized — I can file it as a task instead.)

- On answer → link the goal (or record the exception + rationale in the Strategy Gate), set ROI, then proceed. If it's really one deliverable → route to `tool--notion-task`.

**Why:** no goalless projects. The bar for a project is higher than for a task.

---

## Example 5 — Fallback (the safety net)

- **Ambiguous:** "do the website project" → create a new one or update an existing one? → **ask which**; don't pick silently.
- **Not found:** "update the branding project" → no match in Project Hub → say so, show the closest matches, offer to create it (→ Case A).
- **Task-sized:** "make a project to fix the footer link" → that's one deliverable → route to `tool--notion-task`.
- **Out of scope:** "add a column to the Project Hub" → structural change → route to `tool--notion-architect`; don't do it here.

**Core:** ask / route / never guess-write.

---

## Carried behavior (all cases)

- **Ungroundable select** (Type/Phase/ROI not in the request) → ONE consolidated question; never guess. A wrong Type is worse than one question.
- **Meaningful icon** — propose an emoji from the project's subject and confirm; never ship the template's `paste_blue` placeholder.
- **Drift** — if the live template/schema/options differ from the spec snapshot, surface the diff, conform to **LIVE**, and flag the snapshot for update. Building on a stale snapshot is exactly the failure this skill prevents.
