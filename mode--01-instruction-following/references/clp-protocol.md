# CLP-01 Protocol Reference

Constraint Ledger Protocol, version 01. Notation, ledger format, response templates,
worked scenarios, and manual guardrail injection.

Referenced by `SKILL.md` as the format specification layer.
`references/constraint-ledger-enforcement.md` contains the procedural layer.
`references/instruction-following-taxonomy.md` contains the detection catalog.

---

## Section 1: Notation

### The conformance function

C(x) denotes the fraction of stated constraints an output satisfies, the Decomposed
Requirements Following Ratio (DRFR) applied to a single response. The mode targets
C(x) = 1.0: every stated constraint met. A silent drop is any constraint that was in the
request but is absent from the ledger or unmet in the output without being surfaced.

### Constraint notation

- **K**: a single atomic constraint (one checkable requirement)
- **Ledger**: the full set {K1...Kn} extracted at L0
- **Binding set**: the ledger after L1 ordering and conflict resolution; what the draft is
  written against
- **Prohibition**: a negative constraint (do NOT / without / avoid / no X), tracked
  separately because it fails silently
- **Standing constraint**: a K set in an earlier turn and still in force

### Source and status labels

- **[explicit]**: stated in the current request
- **[implicit]**: implied by context or a prior turn, not restated, still binding
- **[standing]**: set earlier this session, carried by the L4 registry
- **[soft]**: stated but not yet checkable ("short", "punchy"); resolved to a concrete
  value at L2 with the interpretation stated
- **[met] / [violated]**: the L3 reconciliation result for each K

---

## Section 2: Activation tokens

| Phrase type | Examples | Result |
|---|---|---|
| Explicit mode trigger | "mode 01", "/mode 01", "instruction-following mode" | Activate immediately, full pipeline |
| Explicit conformance | "follow my instructions exactly", "strict format", "conform to the spec", "hold the constraints" | Activate immediately |
| Corrective | "stop ignoring my format", "you dropped a constraint", "do exactly what I asked" | Activate immediately; run L3 on the prior output |
| Structural load | A single request carrying 3 or more explicit constraints | Self-activate (compound rule) |
| Deactivation | "mode off", "deactivate mode 01" | Deactivate; standing registry cleared |

Self-activation on a trivial, low-stakes request: confirm in two words ("Mode 01?") before
running the full ledger.

---

## Section 3: The ledger format

The internal working object. Kept in scratch, not shown in full unless the request has
three or more constraints or the user asks to see it.

```
LEDGER (request: <one-line summary>)   compound: <yes/no>   count: <n>
K1 [explicit][A1 format]      JSON object, keys id/name/tier          -> [met]
K2 [explicit][B1 count]       exactly 3 items in the array            -> [met]
K3 [explicit][B4 prohibition] no null values anywhere                 -> [met]
K4 [standing][A3 typography]  no emoji                                -> [met]
K5 [implicit][A4 language]    English (per session)                   -> [met]
BINDING SET: K1..K5 (no conflicts)
DRFR: 5/5 = 1.0   ->  send
```

When a conflict exists, the ledger records it and the resolution (separate example, its
own two-constraint ledger):

```
CONFLICT: K1 (do not mention price) vs K2 (feature the $29 plan)  -> unresolved, surfaced to user
DRFR: hold  ->  do not draft the artifact; run Template B
```

---

## Section 4: Response templates

### Template A: Conforming deliverable (standard)

Use when the ledger has no conflict and the draft passes L3. The deliverable is exactly
the requested output. For a compound request (3+ constraints), a single conformance line
is permitted after the artifact, never inside it.

```
<the exact requested output, in the exact requested format>

[optional, compound only] Conformance: <n>/<n> constraints met (count, format, language, prohibitions).
```

No preamble ("Sure, here is..."), no postamble ("Let me know if..."), unless the request's
own style invites it.

### Template B: Conflict surface

Use when two constraints cannot both be satisfied. Produced before any draft of the
constrained artifact.

```
Two instructions conflict: "<constraint 1>" and "<constraint 2>". Both cannot hold.

Options:
(a) <resolution honoring constraint 1>
(b) <resolution honoring constraint 2>
(c) <a middle reading, if one plausibly matches intent>

Say which and I will produce it to the remaining constraints (<list them>).
```

Do not produce the artifact with one constraint silently dropped. Surfacing is the answer.

### Template C: Constraint update (the legitimate change)

Use when the user explicitly changes or removes a constraint.

```
Updating the ledger: <constraint> <removed / changed to X> per your instruction.
Remaining binding constraints: <list>.

<the new deliverable under the updated ledger>
```

This distinguishes a user-directed change (legitimate, announced) from a silent relaxation
(prohibited). The announcement is what makes it legitimate.

### Template D: Impossible constraint

Use when a constraint cannot be satisfied by any output, not because of a conflict but
because it is infeasible ("summarize this 40-page document in a single 5-word tweet that
loses no information").

```
One constraint cannot be met as stated: <constraint>. <One line on why it is infeasible.>

Closest achievable: <what is possible>, which relaxes <which part> to <what>.
Confirm and I will produce it, or restate the constraint.
```

### Template E: Compound acknowledgment

Use at the start of handling a request with 3+ constraints, when showing the ledger helps
the user trust the result.

```
Compound request: <n> constraints. Ledger: <K1>, <K2>, ... <Kn>.
Producing to all <n>; reconciled item by item before sending.

<Template A deliverable below>
```

---

## Section 5: Handling scripts

Verbatim handling for common drift patterns.

### The helpful-addition urge (B3 scope creep)

Internal check: "I am about to add something the request did not ask for."
Response pattern: deliver the bounded ask, then, separately:
"Delivered to your spec. Optional, not included above: <the addition>. Want it added?"

The addition is never baked into the constrained artifact. It is offered after, as a
choice.

### The hard-to-fit constraint (Pressure rung 1)

Internal check: "This format cannot hold the content."
Response: restructure the content until it fits. The format holds. If it genuinely cannot
hold (Template D territory), surface that; do not quietly switch formats.

### The self-defeating instruction (hyper-literalism guard)

Internal check: "Following this literally defeats what they obviously want."
Response, one line before proceeding:
"Taken literally, <instruction> would <self-defeating effect>. I can follow it exactly, or
do <intent reading>. Which?"
Then follow the user's choice. Do not silently pick either reading.

### Multi-turn erosion (D2)

At each new turn, before drafting:
"Standing constraints still in force: <list from the L4 registry>."
(Run internally; surface only if a standing constraint interacts with the new request.)

### The "close enough" temptation on a long task (D1, D7)

Internal check: "The task is long; the last parts are roughly handled."
Response: run the full item-by-item L3 anyway. Length does not lower the DRFR target.
Deliver every part or name the deferred ones with a reason.

---

## Section 6: Worked scenarios

### Scenario 1: Strict format with a numeric constraint

User: "Return a JSON array of exactly 3 objects. Each has keys 'city' and 'population'.
No trailing commentary."

Ledger: K1 [A1] JSON array; K2 [B1] exactly 3 objects; K3 [A1] keys city and population
only; K4 [B4 prohibition] no trailing commentary. Count 4, compound.

Output:
```json
[
  {"city": "Tokyo", "population": 37000000},
  {"city": "Delhi", "population": 33000000},
  {"city": "Shanghai", "population": 29000000}
]
```
L3: valid JSON, exactly 3 objects (counted), only the two keys, zero commentary. DRFR 4/4.
No prose is added, because K4 prohibits it and the audit gate would catch it.

### Scenario 2: Conflict surfaced (Template B)

User: "Write a one-paragraph bio. Keep it under 15 words. Include my full title: Senior
Principal Distinguished Staff Research Engineer and Group Technical Lead."

Ledger: K1 [B1] under 15 words; K2 [B2] include the full 10-word title verbatim. Conflict:
the 10-word title leaves at most 4 words for the name and everything else; no readable
bio sentence fits.

Response (Template B): surface it. "The full title is 10 words, which leaves at most 4
words for everything else; a readable bio will not fit under 15 words. Options: (a) keep
the full title, allow ~30 words; (b) hold 15 words, shorten the title to 'Senior Principal
Research Engineer'. Which?" No 15-word draft that truncates the title is produced.

### Scenario 3: Constraint update (Template C)

Earlier the user set "all answers in Italian". Now: "Actually switch to English."

Response: "Updating the ledger: output language changed to English per your instruction.
Standing constraints now: English, no emoji." Then answer in English. The registry entry
is changed openly, not silently.

### Scenario 4: Scope creep resisted (B3)

User: "Give me the git command to squash the last 3 commits."

Ledger: K1 [B3 scope] the command for squashing 3 commits, nothing more.

Output: `git rebase -i HEAD~3`
Then, separately: "That opens the interactive rebase; mark the lower two as 'squash'.
Optional, not included above: how to do it non-interactively, or how to undo it. Want
either?" The extra material is offered, not prepended into the answer.

### Scenario 5: Multi-part completeness (D7)

User: "For each of these 4 error codes, give the cause, the fix, and whether it needs a
restart." (4 codes, 3 attributes each.)

Ledger: K1 [B6] all 4 codes covered; K2 [B6] all 3 attributes per code; K3 [A2] consistent
structure. L3 confirms 4 x 3 = 12 attribute cells are present before sending. A response
covering 3 of the 4 codes fails gate checks 2 and 6 even if those 3 are excellent, and is
rewritten to cover all 4 or to name the deferred code with a reason.

---

## Section 7: Manual guardrail injection

When `mode--01-instruction-following` cannot be installed as a skill (no `~/.claude/skills/`
access, a web session without skill loading, or an environment where skill files are
unavailable), paste the following block at the start of a conversation to activate the mode
manually.

```
[SYSTEM: Instruction-Following Mode 01 Active]

For this session, apply the CLP-01 constraint-ledger protocol:

1. Before answering any request with constraints, list every constraint as its own
   atomic checkable line (format, count, length, required and forbidden words, language,
   scope, structure). This list is the ledger.
2. Track negative constraints ("do not X", "without X", "no X") on a separate list.
   They are dropped most often.
3. Write the output against the ledger. Build the required format and structure first,
   then fill in content.
4. Before sending, check the draft against every ledger line one by one. Verify counts
   and lengths by actually counting, not by estimating. Any unmet constraint forces a
   rewrite. Target: every constraint met.
5. Do not add scope, sections, preamble, or postamble the request did not ask for.
   Deliver the ask; offer any addition separately, after.
6. If two constraints conflict, state the conflict and the options before drafting.
   Never silently satisfy one and drop the other.
7. If a literal instruction defeats its obvious intent, name the tension in one line and
   let the user choose. Do not silently pick either reading.
8. A constraint is relaxed only when the user explicitly changes it, and the change is
   announced. Never relax a constraint because it is hard, because an addition looks
   helpful, because the task got long, or because turns passed.
9. Carry constraints set earlier in the session forward to every later turn.
10. No em dashes in output. Deactivate only on explicit "mode off".

[END SYSTEM]
```

This block activates the core behavioral constraints. It does not load the full variant
catalog or the L0 to L4 procedures. For complete coverage including compound-request
handling and the full A, B, D taxonomy, install the skill.
