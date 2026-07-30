# Scoring Rubric — 20 Metrics

Every metric is binary: PASS or FAIL. No partial credit.

---

## A: Metadata (4 metrics)

| ID | Metric | Pass Criteria | Fail Criteria | Quick Fix |
|---|---|---|---|---|
| A1 | Name-folder match | `name` field in YAML frontmatter exactly matches the folder name | Any mismatch between `name` and folder | Rename folder or update `name` field |
| A2 | Description length | Description ≤ 1000 characters | Description > 1000 characters | Cut secondary detail. Move to SKILL.md body. |
| A3 | Trigger coverage | Description names specific activation triggers — input types, keywords, or scenarios that cause the skill to fire | Description only states what the skill does, not when it activates | Add "Activates on:", "Triggers when:", or list trigger phrases |
| A4 | Third person voice | Description uses third person present tense: "Analyses...", "Builds...", "Translates..." | Description uses second person: "Use this skill to...", "Helps you..." | Rewrite opening verb to third person |

---

## B: Workflow & Content (5 metrics)

| ID | Metric | Pass Criteria | Fail Criteria | Quick Fix |
|---|---|---|---|---|
| B1 | Routing mechanism | Has a routing table, decision tree, or mode selector that maps inputs to workflows. Single-workflow skills exempt — mark as PASS. | ≥2 distinct workflows exist but no routing mechanism tells the AI which to use | Add a routing table: "Input → Workflow" |
| B2 | Concrete examples | ≥1 example showing realistic input AND expected output. The example must be specific to the skill's domain, not generic. | No examples, or examples are generic placeholders ("Example: user asks a question, skill responds") | Add one domain-specific input→output example |
| B3 | Guardrail calibration | Skill has degrees of freedom that vary by task sensitivity — tight for high-risk outputs, flexible for low-risk. Single-risk skills exempt (all tight or all flexible) — mark as PASS. | Mixed-risk skill with uniform guardrails (all tight or all flexible) or no guardrails at all | Add a "Degrees of Freedom" section with tight/flexible split |
| B4 | Output specification | Skill defines the format, tone, or structure of its outputs (bullets, tables, prose, structured sections, etc.) | No guidance on what the output looks like | Add an "Output Style" section with 3-5 bullets |
| B5 | Boundary declaration | Skill explicitly states what it does NOT do and where those tasks go instead | No boundaries section, or boundaries are vague ("This skill focuses on X" without naming exclusions) | Add a "Boundaries" section listing excluded tasks and their home skills |

---

## C: Architecture (4 metrics)

| ID | Metric | Pass Criteria | Fail Criteria | Quick Fix |
|---|---|---|---|---|
| C1 | Progressive disclosure | SKILL.md contains routing logic and workflows. Reference material (taxonomies, templates, extended examples, regulatory text) lives in `references/`. Skills under 150 lines with no reference material are exempt — mark as PASS. | All content in SKILL.md with no reference files, AND SKILL.md > 200 lines | Extract reference blocks >50 lines to `references/`. Add route from SKILL.md. |
| C2 | SKILL.md line budget | SKILL.md ≤ 500 lines | SKILL.md > 500 lines | Extract content to references until under budget |
| C3 | File size discipline | No single file in the skill exceeds 400 lines | Any file (SKILL.md or reference) > 400 lines | Split large files or extract subsections |
| C4 | Reference routing | Every file in `references/` is cited or routed from SKILL.md. No orphan files. Skills with no references are exempt — mark as PASS. | Reference files exist but SKILL.md never mentions them | Add "See `references/filename.md`" at the relevant point in SKILL.md |

---

## D: Anti-Patterns (4 metrics)

| ID | Metric | Pass Criteria | Fail Criteria | Quick Fix |
|---|---|---|---|---|
| D1 | No identity preamble | Skill does not start with "You are a..." or assign an identity/persona to the AI | Contains "You are a...", "Act as...", "Your role is..." | Delete the identity line. Start with what the skill does. |
| D2 | No filler | No motivational paragraphs, "Why this matters" sections, or sentences that don't constrain behavior. Test: "If I delete this sentence, does the AI respond differently?" If no → filler. | Contains filler paragraphs that add no behavioral constraint | Delete every sentence that fails the deletion test |
| D3 | No duplication | Each concept, rule, or instruction appears once in the skill (across SKILL.md + references) | Same information stated in multiple places (e.g., "always cite sources" appears in 3 sections) | Find the best version, keep it, delete the duplicates |
| D4 | No dead weight | Every section in the skill adds at least one behavioral constraint. No sections exist purely for organizational aesthetics. | Sections with headers but no actionable content, or sections that restate the description | Delete sections that don't add constraints. Merge thin sections into adjacent ones. |

---

## E: Sophistication (3 metrics)

| ID | Metric | Pass Criteria | Fail Criteria | Quick Fix |
|---|---|---|---|---|
| E1 | Example realism | Examples use domain-specific scenarios with realistic detail (names, quantities, regulatory references, stakeholders, technical terms from the skill's domain) | Examples are generic or placeholder ("The user uploads a document. The skill analyzes it.") | Rewrite example with concrete domain detail — real regulatory articles, realistic data, named stakeholders |
| E2 | Composability | Skill has clear boundaries with adjacent skills. No functional overlap. Handoff points are named ("See skill-name for X"). Skills with no adjacent skills are exempt — mark as PASS. | Skill duplicates functionality that exists in another skill in the fleet, or handles tasks better suited to a different skill without acknowledging the boundary | Define handoff points. Add to Boundaries section. Remove duplicated functionality. |
| E3 | Proportional structure | Skill complexity matches task complexity. A simple skill (1-2 tasks) is short and direct. A complex skill (5+ workflows, multiple risk levels) has routing, guardrails, and references. | Simple skill is over-engineered (300+ lines for a 2-step task) OR complex skill is under-built (50 lines for a 6-workflow multi-risk task) | Scale up or down. Add structure for complex skills, remove structure for simple ones. |

---

## Verdict Thresholds

| Score | Verdict | Meaning |
|---|---|---|
| 19–20 / 20 | **SOTA** | State of the art. Ship without changes. |
| 17–18 / 20 | **Strong** | Minor issues. Quick fixes, then re-audit. |
| 14–16 / 20 | **Adequate** | Structural problems. Targeted rework needed. |
| < 14 / 20 | **Needs Rebuild** | Fundamental issues. Re-run BUILD from Step 1. |
