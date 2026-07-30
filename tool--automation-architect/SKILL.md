---
name: tool--automation-architect
description: >
  Interactive builder for high-accuracy, repeatable automations. Trigger it to CREATE a new
  automation and it guides you — one question at a time — to a complete spec built as a
  reconciliation loop (desired state → observe → diff → act → verify → report → rollback). It
  hard-wires the proven rigor (verify-after-act, idempotency, completion-report verdict gate,
  logging) while staying customizable to each domain's rules (local repo, Notion workspace, or
  anything else). Activates on "create an automation", "new automation", "automate this task",
  "build a reconciler / maintenance routine", "use the automation blueprint", "automation architect".
classification: trusted-input
last_reviewed: 2026-07-28
---

# Automation Architect

Builds high-accuracy, repeatable automations by treating every one as a **reconciliation loop** —
the model Kubernetes and Terraform use: declare what "correct" is, detect drift from it, fix the
drift, verify, report. The loop and its rigor are **fixed**. The domain rules are **yours to fill**.

## When to use

Trigger to design a NEW repeatable task that must run accurately and often: maintenance routines,
sync jobs, ship pipelines, audits, drift fixers — across the local repo, Notion, or anywhere else.

Do **not** use for a one-off action (just do it) or to *run* an existing automation (that is the
automation's own job — this skill only builds them).

## Where automations live — one control plane, many targets

Every automation **lives and runs on the local machine** (the repo) — that is the brain. None live
"in Notion." The local filesystem, Notion, GitHub, and claude.ai are **targets** the automation
reconciles, each through its own adapter.

- **Brain:** the repo — desired-state spec + logic + trigger.
- **Targets:** local filesystem (usually the source of truth) · Notion (mirror / dashboard / log) ·
  GitHub (backup) · claude.ai (skill cloud).
- **The local↔Notion link:** today Notion is read/written through the **MCP connector inside a Claude
  session** — which is why the weekly maintenance runs as a session, not a pure script. Fully-headless
  Notion automation would instead use a Notion API token: same architecture, different transport.
- **Source of truth:** for each thing managed, name which side wins. Default: filesystem → Notion.
  Conflicts must resolve deterministically — never guess.

So a "Notion automation" IS a local automation that talks to Notion. Build accordingly.

## The fixed core — never customized (this is the "true nature")

Every automation built here has these six steps plus two properties. None may be dropped.

1. **Desired state** — a concrete, checkable definition of "correct" (the contract).
2. **Observe** — read the actual current state.
3. **Diff** — desired vs actual → the drift list.
4. **Act** — apply the minimal fixes.
5. **Verify** — re-read reality; confirm it now matches desired (closed loop).
6. **Report** — a Completion Report: status table + ✅/⚠️ verdict.
- **Idempotent** — safe to re-run; if already correct, it changes nothing.
- **Recoverable** — every change is logged and reversible via that log.

**Verdict gate** (inherited from `tool--rule-model-brutal-truth`): ✅ only after every desired-state
check is verified with proof. Any miss → ⚠️ + the exact next action. Never ✅ on a hope.

## The build flow — ask ONE question at a time, in order

Coach the operator through the card below. Ask one focused question, wait, then the next. Pre-fill
anything you can infer (especially the domain guardrails). Never ask what you can read.

1. **Name & goal** — one line. What does "correct" look like when it's done?
2. **Domain / adapter** — local repo · Notion · other. Load that domain's rules (Adapter library below).
3. **Desired state (contract)** — the checklist of correct. Push until each item is concrete and
   checkable ("two-way relations exist", not "Notion is tidy").
4. **Observe** — exactly how to read the actual state in this domain.
5. **Diff** — how correct vs actual is compared; what counts as drift; **which side is the source of
   truth** (which way it syncs) so conflicts resolve deterministically.
6. **Act** — the fix steps, within the domain's rules.
7. **Verify** — how each fix is confirmed at the user-visible layer.
8. **Guardrails** — the non-negotiables (auto-fill from the domain; ask only for task-specific ones).
9. **Decision gates** — the few runtime questions worth asking, and when. Default to none.
10. **Logs & rollback** — where it records; how to undo.
11. **Trigger & cadence** — the manual command phrase and/or the schedule.

## Adapter library — pre-fill the operator's domains

**Local repo (Claudio AI)**
- Observe: filesystem scan · `git status` · `ls -la ~/.claude/skills` · read `xEngine/CHANGELOG.md`.
- Act: bash · git · the `skills` CLI.
- Guardrails: **Rule Zero** — never destroy (move to `xGraveyard/` first); log every structural
  change to `xEngine/CHANGELOG.md` (repo + master); sync is verified, not assumed.
- Logs / rollback: `xEngine/CHANGELOG.md` (read backwards to reverse a change).

**Notion workspace**
- Observe: `notion-fetch` (schema only — **no row reads**) · manifest SHA hashes · ancestor-path for
  wiki children.
- Act: `notion-update-page` / `notion-update-data-source` (MCP connector).
- Guardrails: **no writes without per-action permission** (sole exception: the Skills Inventory
  page); **frozen DBs** (Application Tracker, Company Intel) never altered; the constitution wins.
- Logs / rollback: the Notion Logs DB (never the filesystem changelog).
- Known API limits → route to UI: can't read rows or wiki-home bodies; can't set relative-date
  filters, DB icons, or inline-block titles.

**Other** — ask the operator to define observe / act / guardrails / logs. The six steps stay identical.

## Worked example — "Skills stay shipped" (repo + Notion)

> **Automation:** Skills stay shipped · **Domain:** repo + Notion · **Trigger:** every skill change, or "ship skills"
> **Desired state:** every live skill has a zip, a symlink, a row in the Notion Skills Inventory, and is pushed to GitHub.
> **Observe:** scan `live/capability/`; `ls` symlinks; `ls packaged/`; fetch the Inventory page; `git status`.
> **Diff:** any skill missing a zip / symlink / inventory row / unpushed commit.
> **Act:** pack → symlink → update Inventory → commit + push.
> **Verify:** `unzip -l`; `readlink`; re-fetch the Inventory page; `git log`.
> **Guardrails:** Rule Zero; the Inventory page is the one standing-authorized Notion write.
> **Logs / rollback:** changelog (repo + master) + Notion Logs DB.

## Output

Produce a filled **Automation Spec card** (the fields from the worked example). Then, matched to the domain:
- **Repo automation** → a runnable skeleton (bash / `skills` CLI) implementing the six steps, or a precise step-by-step procedure.
- **Notion / in-session automation** → a procedure an agent runs in-session (Notion writes need the MCP connector, so they can't be pure bash).

Finish with a Completion Report (status table + verdict), state the **trigger phrase**, and say where the spec is saved.

## Output style
- Interactive: one question at a time, plain English, no walls of text.
- Pre-fill aggressively from what's known; confirm rather than interrogate.
- Concrete over vague — reject fuzzy desired-state items and ask for a checkable version.

## Boundaries
- **Designs and specs** automations; it does **not** execute them (the automation does that itself).
- If the automation IS a skill → hand the build/QA to `tool--sota-skill-qa`.
- For Notion structure design → `tool--notion-architect`. For the verdict / verification rigor → `tool--rule-model-brutal-truth`.
