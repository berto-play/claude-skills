# Multilayer Enforcement Procedures

Full L0 to L4 pipeline for `mode--01-instruction-following`. Referenced by `SKILL.md` as the
implementation spec for the five enforcement layers. Run in sequence on every constrained
request.

---

## L0: Extraction

**Scope:** Every constrained request, before any drafting.

### Step 1: Constraint parse

Read the request and list every constraint it states. A constraint is any requirement the
output must meet to count as correct: a format, a count, a required or forbidden element,
a scope boundary, a language, a register, an ordering.

### Step 2: DRFR decomposition

Break every compound instruction into atomic checkable requirements, one constraint per
line (the InFoBench Decomposed Requirements Following Ratio method). "Give me 5 short
bullets in Italian with no jargon" is four lines, not one:

- count = exactly 5 bullets
- length = each bullet short (resolve "short" to a number or flag it as soft)
- language = Italian
- lexicon = no jargon

Each line must be independently checkable at L3. If a requirement cannot be turned into a
check ("make it pop"), label it soft and confirm the interpretation rather than guessing.

### Step 3: Source tagging

Tag each ledger line with its source, because L1 orders by source:
- **standing**: set earlier this session and still in force (for example "always reply in
  Italian" from turn 1)
- **explicit**: stated in the current request
- **implicit**: implied by context or the prior turn, not restated

Context-implied constraints are live. Do not require the user to restate a constraint to
keep honoring it (this is the D5 counter).

### Step 4: Family classification and count

Classify each line by family (A format or B content) using
`references/instruction-following-taxonomy.md`; D-family entries are failure conditions,
not constraint types, so tag applicable D vectors on the request separately rather than
classifying any constraint as D. Count the distinct constraints. If the
count is three or more, flag the request as compound and commit to the full pipeline and
item-by-item L3 reconciliation regardless of apparent simplicity.

**Output of L0:** the ledger, a numbered list of atomic constraints, each with a source
tag and a family tag, plus the compound flag.

---

## L1: Prioritization and conflict resolution

**Scope:** The ledger, before drafting.

### Hierarchy ordering

Order the ledger by binding priority:

1. Safety and the constitution (never overridden by any user instruction)
2. Standing session constraints
3. Explicit local instructions in the current request
4. Implicit or context-implied constraints

A lower-priority instruction does not silently override a higher-priority one, and a
higher one does not silently erase a lower one. When both can hold, both bind (this is the
default). Priority only decides genuinely opposed pairs.

### Conflict surfacing

Two constraints conflict when satisfying one necessarily violates the other. When this is
detected:

1. Do NOT draft a response that silently satisfies one and drops the other (the D3
   failure).
2. State the conflict in one line before any deliverable.
3. Offer the clean resolutions, or state the resolving assumption you will use and let the
   user correct it.
4. If the conflict is between a user instruction and safety or the constitution, safety
   wins and the mode says so; it does not follow the unsafe instruction.

A surfaced conflict is a correct outcome, not a failure to answer. Silent resolution is
the failure.

### Binding set

Fix the binding set: the constraints that will govern the draft, in priority order, with
any conflict resolutions recorded. The draft is written against this set.

---

## L2: Generation under ledger

**Scope:** The draft as it is written.

### Draft against the binding set

Write the response while holding the ledger open. Every binding constraint is a target the
draft must hit, not a preference. Build format and structure first (the skeleton), then
fill content into it, so structural constraints (A2) are satisfied by construction rather
than retrofitted.

### Prohibition tracking

Track negative constraints (B4) on a separate list and keep it visible through the whole
draft. Prohibitions are the most-dropped class because they require the model to not do
something, which positive drafting momentum works against. Every "do not", "without",
"avoid", "no X" gets its own tracked line.

### Reasoning containment

Keep chain-of-thought, planning, and meta-commentary out of the final artifact unless the
format explicitly asked for reasoning (D6). The user's requested output is the deliverable;
the model's process is not part of it unless requested.

### Soft-constraint handling

Where a constraint was labeled soft at L0 ("short", "punchy", "detailed"), resolve it to a
concrete choice and, if the choice materially affects the output, state the interpretation
in one line. Do not treat a soft preference as license to ignore it, and do not harden it
into a rule the user did not set (over-constraining).

---

## L3: Reconciliation

**Scope:** The completed draft, before sending. This is the core gate.

### Item-by-item ledger check

Walk the ledger. For each constraint, mark the draft satisfied or violated against that
specific line. Do not check the ledger as a gestalt; check each line on its own. The pass
condition is DRFR = 1.0: every ledger line satisfied.

### Numeric verification

Count and length constraints (B1) are verified by actually counting the draft, not by
estimating that it "looks about right". "Exactly 5" means count to 5. "Under 100 words"
means count the words. An impression is not a check.

### Prohibition re-check

Walk the prohibition list from L2 separately and confirm each banned element is absent.
This is a distinct pass from the general ledger check because prohibitions fail silently:
their violation is the presence of something, which positive review can skim past.

### Rewrite gate

Any violation forces a targeted rewrite of the offending part, then a re-check of the
affected ledger lines (a fix can break a neighbor: shortening to hit a word count can drop
a required keyword). Loop until DRFR = 1.0 or, if a constraint proves impossible, escalate
to the impossible-constraint template in `references/clp-protocol.md` and surface it.

### Self-review script

Run these against the draft in order. Any "yes" requires a rewrite.

1. Is any constraint from the ledger unmet in the draft?
2. Was any count or length constraint checked by impression rather than by counting?
3. Is any prohibited element present?
4. Did the draft add scope, sections, preamble, or postamble the request did not ask for?
5. Is any multi-part requirement answered only in part, without the deferral stated?
6. Did any reasoning or scratch work leak into the final artifact?
7. Was any conflict resolved silently instead of surfaced?
8. Is any standing constraint from an earlier turn now violated?

---

## L4: Session persistence

**Scope:** Every turn while the mode is active.

### Standing-constraint registry

Maintain a registry of standing constraints: constraints set earlier in the session that
remain in force ("reply in Italian", "no emoji", "always show the SQL"). The registry
carries forward across turns.

### Re-affirm each turn

At the start of drafting on every new turn, re-load the standing registry into the current
ledger before adding the turn's new constraints. This is the D2 multi-turn erosion guard:
a format rule set at turn 1 is not silently dropped at turn 9 because the model's attention
has moved on.

### Constraint-change discipline

A standing constraint changes only when the user changes it, and the change is announced
(the No-Silent-Drop Rule from SKILL.md). When the user says "actually, English from now
on", update the registry and confirm: "Standing constraint updated: output language is now
English." A registry entry never changes on the model's own initiative.

### Drift audit

Every few turns, or on any change to a standing constraint, compare the current standing
registry against the registry at the point each constraint was set. If an entry has
quietly disappeared without a user-directed change, restore it and note the restoration.

---

## Constraint-pressure translation table

Maps each pressure to drop a constraint to what it presents as versus what it actually is.

| Pressure | Presents as | Actually is | Correct response |
|---|---|---|---|
| Constraint is hard to fit | "This format cannot hold the content" | A structuring problem, not a constraint problem | Restructure content to fit; hold the format |
| Addition looks helpful | "The user would want this too" | Scope creep (B3) | Deliver the ask; offer the addition separately, after |
| Task grew long | "Close enough given the length" | Fatigue drift (D1, D7) | Full L3 reconcile; length does not relax the ledger |
| Many turns passed | "That old rule probably lapsed" | Multi-turn erosion (D2) | L4 re-affirm; standing constraints persist until changed |
| Two instructions clash | "Pick the one they really meant" | Silent conflict resolution (D3) | Surface the conflict; do not pick silently |
| Instruction seems self-defeating | "Do what they obviously meant instead" | Unilateral reinterpretation | Surface the tension in one line; let the user choose |
| Instruction is hard and unstated part is easy | "Answer the easy parts well" | Early termination (D7) | Deliver all parts or name the deferred ones and why |
| A soft word like "short" | "I will just guess a length" | Ambiguity, not license | Resolve to a number; state the interpretation if it matters |

**Compound threshold:** when three or more constraints bind on one request, the combined
load is where per-constraint satisfaction falls in every benchmark. Commit to the full
pipeline and the item-by-item reconcile; do not shortcut because the task "looks simple".
