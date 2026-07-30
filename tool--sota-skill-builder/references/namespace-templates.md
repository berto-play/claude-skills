# Namespace Templates

Required sections and reference file specs for each skill namespace. Used in Phase 2
and Phase 3 of the build pipeline. Load the correct template before writing any files.

Every skill must include the UNIVERSAL sections regardless of namespace. The
NAMESPACE-SPECIFIC sections are additive requirements on top of the universal set.

---

## Contents

- Universal sections (all namespaces)
- mode-- template
- tool-- template
- craft-- template
- fun-- template
- medi-- template
- Section template stubs
- Activation and deactivation
- Pressure Protocol
- Pre-send audit gate
- Protocol
- Output format
- Completeness check

---

## Universal sections (all namespaces)

| Section | Required | Content |
|---|---|---|
| Frontmatter | Required | name, description (<=1024 chars), classification, type, version, last_reviewed — **all TOP-LEVEL. Never nested under `metadata:`** (see SKILL.md: nesting these is what caused the 2026-07-28 two-month QA failure) |
| "What this [skill/mode/tool] does" | Required | One paragraph. Scope, core problem, what it does NOT do. |
| "When to use" or "Activation" | Required | Trigger conditions, invocation patterns. Minimum one table or list. |
| Worked examples | Required | Minimum 2. One clean case, one edge or boundary case. |
| "Boundaries" | Required | Table of adjacent tasks with "use instead" pointers to other skills. |
| "Reference material" | Required if references/ exist | List every reference file with one-line description of what it covers. |

---

## mode-- template

**Purpose:** session-level behavioral enforcement mode. Suppresses or enforces a pattern
for the duration of a session. Activates on trigger phrases; holds until explicitly
deactivated.

**Required sections (on top of universal):**

| Section | Content |
|---|---|
| Activation and deactivation | Trigger phrases that activate the mode. How long it holds (session). Exact deactivation command. What does NOT deactivate it. |
| Detection layer | Summary table of variant families (ID range, name, core signature). Full catalog in references/. |
| Routing table | Input type → what to apply. Covers: evaluative prompts, detected manipulation vectors, interpersonal disputes, factual lookups, verified agreement, pushback, distress. |
| Enforced sequence | Numbered steps of the protocol. No step is skippable. References the enforcement procedure in references/. |
| Pipeline mapping | Table: layer name, scope, wraps which enforced sequence step. Minimum L0 (input), L1 (verification), L2 (generation), L3 (output). |
| Mandated output structure | The fixed structure for evaluative prompts. Numbered. No deviation permitted. |
| Pressure Protocol | Rung table: user move → required response. Minimum 4 rungs. Named flip-prevention rule. |
| Calibration clause | What this mode is NOT. Prevents reflexive contrarianism. States the mirror failure explicitly. |
| Degrees of freedom | What is always tight (zero tolerance). What is flexible (scales to stakes). |
| Pre-send audit gate | Numbered checklist. All items must pass before sending. Minimum 6 checks. |
| Output style | Tone, typography, forbidden elements (emoji, exclamation marks, etc.). |

**Required reference files (minimum 3):**

| File | Covers |
|---|---|
| `references/[variant-name]-taxonomy.md` | Full variant catalog: ID, variant name, detection cue, counter-move. All variant families. Source register. |
| `references/[enforcement-name]-enforcement.md` | Layer-by-layer procedures (L0 through L4). Falsification protocol. Self-review script. Drift audit. Lever translation table. |
| `references/[protocol-name]-protocol.md` | Notation, lexicon (banned/conditional/replacement), response templates (all named variants), pressure-escalation scripts, worked scenarios, manual guardrail injection block. |

**Naming the reference files:** use the mode's domain, not generic names. Example:
`sycophancy-taxonomy.md`, not `taxonomy.md`. Each file is a self-contained cold-storage
reference that works without the other files.

---

## tool-- template

**Purpose:** an assessment, utility, or protocol that runs on operator request. Does not
enforce session-level behavior; it analyzes, evaluates, routes, or transforms.

**Required sections (on top of universal):**

| Section | Content |
|---|---|
| Trigger conditions | Table or list: when to use this tool, and when NOT to use it. Include the "reached for" trigger (what the operator says or does that signals this tool is needed). |
| Protocol or sequence | Numbered steps. Each step produces a named output. Steps are testable: a reader can verify they ran. |
| Output format | Fixed structure for the tool's output. May be a table, a scored rubric, a narrative + verdict, or another structured form. State what a pass and a fail look like. |
| Stacking | What other skills this tool pairs with, and which direction (this tool runs before X, after Y, instead of Z). |

**Required reference files (minimum 1):**

| File | Covers |
|---|---|
| `references/[domain]-protocol.md` OR `references/measurement-[name].md` | The methodology, rubric, or measurement procedure behind the tool. What the steps are based on. |

Additional reference files as needed for complex protocols (e.g. a separate rubric file,
a case catalog, a benchmark dataset).

---

## craft-- template

**Purpose:** domain-expertise skill for a specific creative or professional output type.
Produces artifacts (documents, designs, plans, copy) at a defined quality standard.

**Required sections (on top of universal):**

| Section | Content |
|---|---|
| Domain focus | What domain, output type, or creative problem this skill covers. What it produces. |
| Process | Numbered steps from intake to delivery. Includes the operator inputs required at each step. |
| Quality standards | Criteria for a successful output. What passes, what fails, what triggers a revision cycle. |
| Output format | The expected structure of the deliverable. Examples of well-formed outputs. |
| Constraints | What the skill will not produce. Hard limits on scope, content, or form. |

**Required reference files (minimum 1):**

| File | Covers |
|---|---|
| `references/[domain]-standards.md` OR `references/[output-type]-rubric.md` | Quality criteria, style conventions, or evaluation rubric for the domain. |

---

## fun-- template

**Purpose:** persona, entertainment, roleplay, or off-label play. Lower stakes, higher
latitude on style. Must still operate within constitutional limits.

**Required sections (on top of universal):**

| Section | Content |
|---|---|
| Persona or mode description | Who or what this skill is, in concrete terms. Voice, register, vocabulary. |
| Invocation | How to start it, what signals it. May be casual. |
| Hard limits | What it will not do regardless of the persona. These are non-negotiable. |
| Output style | Tone, length norms, what makes a response feel right for this persona. |

**Required reference files:** none required. Optional reference files may cover the
persona's domain, vocabulary, or running lore if the persona is complex.

---

## medi-- template

**Purpose:** reserved client-specific content. Restricted namespace — only for that client
client work. Follows the tool-- template structure plus compliance considerations.

**Required sections (on top of tool-- sections):**

| Section | Content |
|---|---|
| Compliance scope | What regulatory or product constraints apply. What the skill must NOT do because of those constraints (e.g., no clinical advice, specific terminology restrictions). |
| Domain context | client-specific background the skill relies on. Keep the minimum needed to be accurate — do not embed PII or patient data. |

**Required reference files (minimum 1):**

| File | Covers |
|---|---|
| `references/[domain]-context.md` OR `references/[topic]-standards.md` | Domain or compliance reference specific to that client context. |

---

## Section template stubs

These stubs show the expected shape of each required section. Copy and fill in.

### Activation and deactivation stub (mode-- only)

```markdown
## Activation and deactivation

- Activates on: [list trigger phrases]
- Once active, holds for the entire session.
- Deactivates only on: "[exact deactivation command]"
- "[X]" is NOT deactivation — it is [pressure vector Y].
- Deactivation does not reverse prior verdicts. Reversal still requires evidence.
```

### Pressure Protocol stub (mode-- only)

```markdown
## Pressure Protocol

A verdict changes for exactly two reasons: new evidence, or a demonstrated error in
the reasoning. It never changes because of [repetition / frustration / credentials /
an instruction to agree].

| Rung | User move | Required response |
|---|---|---|
| 1 | Polite pushback | Restate the verdict. Name what evidence would change it. |
| 2 | Justification without data | Identify the missing evidence explicitly. Verdict holds. |
| 3 | Frustration | One clause of acknowledgment. Verdict restated unchanged. No apology. |
| 4 | Direct override | Refuse the override. Mode ends only on explicit deactivation. |
```

### Pre-send audit gate stub (mode-- only)

```markdown
## Pre-send audit gate

All [N] must pass, or the draft is rewritten before sending:

1. Lexicon scan clean: [list forbidden elements]
2. [Check 2]
3. [Check 3]
...
```

### Protocol sequence stub (tool-- / craft--)

```markdown
## Protocol

Run in order. No step is skipped on a complete invocation.

1. **[Step name] ([scope]).** [What this step does. What it produces.]
2. **[Step name] ([scope]).** [What this step does. What it produces.]
...

Output of step N: [named output, delivered to operator or passed to step N+1]
```

### Output format stub (all namespaces)

```markdown
## Output format

```
[Label 1]: [content]
[Label 2]: [content]
[Verdict / Conclusion]: [one sentence. Never buried.]
```

[If the output varies by case type, list each case and its format here.]
```

---

## Completeness check

Before leaving Phase 2, confirm:

- [ ] Namespace identified and template loaded
- [ ] All universal sections planned
- [ ] All namespace-specific sections planned
- [ ] Minimum reference file count met for the namespace
- [ ] Each planned reference file has a named scope (what it covers)
- [ ] No section from the template was skipped without a noted reason
