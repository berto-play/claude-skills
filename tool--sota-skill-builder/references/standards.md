# Current Skill Standards — sourced, dated, verifiable

## Contents

- Sources of record
- Hard requirements (Anthropic)
- Authoring requirements (Anthropic)
- Anti-patterns Anthropic names explicitly
- Workspace conventions
- Known platform behaviours that are not spec
- Re-verification

---

## Sources of record

Re-verify before relying on this file. It ages; the docs move.

| Source | URL | Revision date | Accessed |
|---|---|---|---|
| Agent Skills overview (Anthropic) | platform.claude.com/docs/en/agents-and-tools/agent-skills/overview | none stated | 2026-07-29 |
| Skill authoring best practices (Anthropic) | platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices | none stated | 2026-07-29 |
| anthropics/skills repository | github.com/anthropics/skills | none stated | 2026-07-29 |
| agentskills.io specification (**third party**) | github.com/agentskills/agentskills/docs/specification.mdx | none stated | 2026-07-29 |

**Where the two disagree, Anthropic wins.** The third-party spec forbids consecutive
hyphens in `name`; Anthropic's documentation does not. Every skill in this workspace
uses `namespace--name`, and Anthropic's stated rule permits it.

---

## Hard requirements (Anthropic)

Failing any of these is a BLOCKER. `scripts/validate_skill.py` checks all of them.

**`name`**
- maximum 64 characters
- lowercase letters, digits and hyphens only
- no XML tags
- **no reserved words: "anthropic", "claude"**
- must match the containing folder name

**`description`**
- non-empty
- maximum 1024 characters
- no XML tags
- states what the skill does **and** when to use it
- written in **third person** — "Processes Excel files", never "I can help you…"

**Optional fields:** `license`, `compatibility` (max 500 chars), `metadata`.

**Discovery (Claude Code):** `~/.claude/skills/` (personal) or `.claude/skills/` (project).
Filesystem-based; no upload step. Skills do **not** sync between Claude Code, claude.ai
and the API — each surface is separate.

---

## Authoring requirements (Anthropic)

| Requirement | Limit |
|---|---|
| SKILL.md body | under 500 lines |
| Reference depth | exactly one level from SKILL.md |
| Reference files over 100 lines | must open with a table of contents |
| Level 1 metadata | ~100 tokens, always loaded |
| Level 2 instructions | under 5k tokens, loaded on trigger |
| Level 3 resources | loaded only when read |
| Evaluations | at least three, written **before** documentation |
| Model coverage | tested on Haiku, Sonnet and Opus |
| Paths | forward slashes only |

**Why depth matters:** when a reference is reached through another reference, Claude may
preview it with `head` rather than reading it whole, and act on partial information.

**Naming:** Anthropic recommends gerunds (`processing-pdfs`) or noun phrases
(`pdf-processing`). This workspace uses `<namespace>--<name>`, which is compatible.
Anthropic explicitly warns against inconsistent patterns *within a collection* — so
match the workspace, do not import a different scheme.

---

## Anti-patterns Anthropic names explicitly

- Windows-style backslash paths
- offering many options where one default plus an escape hatch will do
- time-sensitive statements ("before August, use the old API")
- inconsistent terminology across a skill
- vague names: `helper`, `utils`, `tools`, `documents`, `data`
- assuming packages are installed
- explaining things Claude already knows — every token competes with the conversation
- scripts that defer errors to Claude instead of handling them
- unexplained constants
- deeply nested references

---

## Workspace conventions

Measured across 51 live skills on 2026-07-29, not assumed:

| Convention | Coverage |
|---|---|
| `<namespace>--<name>` | 51/51 |
| top-level `classification` | 49/49 capability |
| top-level `last_reviewed` | 49/49 |
| `compatibility` | 33/49 |
| `metadata` | 25/49 |
| `type` / `version` | 1/49 — **not** conventional here |
| `references/` | 40/49 |
| `tests/` | 15/51 |

Valid namespaces: `tool` · `craft` · `mode` · `fun` · `medi`.
`classification` must be exactly `trusted-input` or `untrusted-input`.

**Frontmatter must be top-level.** Nesting `classification` under `metadata:` is what
caused the 2026-07-28 failure: the QA grep matched nothing, returned 1, `set -e` aborted
the run, zero bytes were emitted, and the pipeline recorded success for two months.

**Untrusted-input skills** additionally require a real security section and injection
tests covering all five categories in `00 Shared Global Assets/Standards/SECURITY-STANDARD.md`.

---

## Known platform behaviours that are not spec

Observed, reproducible, and not written down by Anthropic:

- **claude.ai slugs names.** `tool--x` is stored as `tool-x`, lowercased. Verification
  must compare slugged names or it will report a false miss.
- **claude.ai has an ingestion ceiling.** Skills between roughly 62KB and 416KB can be
  accepted with HTTP 200 and then silently dropped. Only re-reading `updated_at` catches it.
- **There is no skills API for claude.ai.** Upload, download, delete and enable are driven
  through a logged-in browser tab.

None of these affect Claude Code, which reads from the filesystem directly.

---

## Re-verification

State-of-the-art status expires when the platform, model, tools, dependencies or
specification materially change. This file records what was true on **2026-07-29**.
Re-read the two Anthropic sources before trusting it, and update the dates when you do.
