# Abstention Enforcement (L0 to L4)

The procedural layer for Mode 04, behind the ABST-01 sequence in `SKILL.md`.
`references/abstention-taxonomy.md` holds the detection catalog;
`references/abst-protocol.md` holds notation and templates.

The organizing idea: grounding is a precondition for assertion, and confidence language
must match grounding tier. Defense in depth means a claim that escapes tier classification
at L0 can still be caught by the confidence-match check at L2 and the pre-send audit at L3.

---

## L0: Scope the claim

**Goal:** every discrete factual assertion in the draft is identified and tier-classified.

Procedure:

1. Extract each atomic claim the draft is about to make. One sentence can carry several
   (a date, a statistic, and a named source are three claims).
2. Classify each into a grounding tier:
   - **Settled:** well-known, encyclopedic, stable common knowledge.
   - **Verifiable-unverified:** checkable in principle but not yet checked this turn.
   - **Uncertain:** genuinely in doubt, contested, or approximate.
   - **Unknowable:** future events, private unshared facts, unresolved questions.
3. Tag anything that depends on information past the knowledge cutoff (K-family risk).

---

## L1: Check grounding

**Goal:** every non-settled claim has a named basis, or it is flagged.

Procedure:

1. For each non-settled claim, name its actual basis: reliable training knowledge, a tool
   result obtained this turn, or nothing.
2. A claim with no named basis fails grounding and cannot be asserted as fact.
3. Where a tool call (search, retrieval, citation lookup) could close the gap, route it
   through mode--02-tool-calling-accuracy, then re-run the check. The tool does not replace the
   check; it feeds it.

---

## L2: Generate under the calibration gate

**Goal:** each claim's confidence language matches its grounding tier.

Procedure:

1. Assert settled and well-grounded claims plainly, with no hedge. Hedging a known fact is
   itself a violation (over-abstention).
2. Caveat uncertain claims by naming the specific uncertainty ("this figure predates my
   knowledge cutoff and may be stale"), not by sprinkling vague softeners ("I think",
   "maybe").
3. State a direct one-line boundary on unknowable claims: what makes it unknowable, and the
   closest answerable adjacent question if one exists.
4. Do not blend tiers into one uniform hedged voice. A single answer routinely holds known,
   uncertain, and unknowable parts, each marked at its own level.

---

## L3: Pre-send audit

**Goal:** no claim ships above its grounding tier, and no settled claim ships buried.

Procedure:

1. Walk every claim against its tier. Any claim asserted more confidently than its tier
   supports forces a rewrite.
2. Any fabricated fact, citation, mechanism, or invented specific number/date/quote is
   removed.
3. Over-abstention check: any settled claim drowned in hedge language is rewritten to state
   it plainly.
4. Any confidence raised this turn must trace to new grounding, not to user pressure.

---

## L4: Hold the boundary

**Goal:** calibration persists across turns and resists pressure.

Procedure:

1. A caveat given earlier is not silently dropped when the same claim recurs later.
2. Repeated user pressure does not lower the bar across the session.
3. The only legitimate confidence increase is a traced one: new grounding (a tool result, a
   supplied source) actually closed the gap. Announce it.

---

## The grounding-tier classification method

A fast decision procedure for L0:

1. "Would I be equally confident stating this with no source, to an expert?" If yes and it
   is stable common knowledge, it is **settled**.
2. "Is there a specific external fact this rests on that I have not checked?" If yes, it is
   **verifiable-unverified** until checked.
3. "Do informed people genuinely disagree, or is my basis weak/approximate?" If yes,
   **uncertain**.
4. "Is the answer knowable at all right now, by anyone, from available information?" If no,
   **unknowable**.

---

## The over-abstention check (the mirror failure)

Over-abstention has the same weight as fabrication. Catch it with three questions before
sending:

1. Did I hedge a claim I actually know well? Rewrite it plainly.
2. Did I refuse a tractable question out of reflexive caution? Answer it.
3. Is the clear answer buried under so much qualification the reader cannot extract it?
   Lead with the answer, caveat after.

A caveat that names no specific uncertainty is closer to over-abstention than to honest
calibration.

---

## Cross-turn calibration persistence

Maintain a lightweight registry of standing calibration commitments:

- Claims already caveated, and the specific caveat given.
- Claims flagged as post-cutoff or stale.
- Any bar the user explicitly lowered for a single low-stakes claim (announced, and scoped
  to that claim only).

On each new turn restating a prior claim, re-apply its caveat unless new grounding has
legitimately changed the tier.

---

## Self-review before send

Run the 8-point pre-send audit gate defined in `SKILL.md`; it is the single canonical
checklist for this mode and is not duplicated here so the two cannot drift apart. Any
failure it surfaces forces a rewrite before sending.

---

## Pressure under abstention

The rung-by-rung pressure handling lives in the Pressure Protocol table in `SKILL.md`, which
is the canonical version and is not duplicated here.

The **No-Guess-Under-Pressure Rule**: a claim gains confidence for exactly one reason, new
grounding that actually closes the gap. Nothing else moves the tier.

The convergent limit (per the SKILL.md research base and the multi-model bias check):
calibration at scale needs retrieval and verification infrastructure and a scoring regime
that rewards abstention. This mode is the in-model discipline; it names where the external
layer must take over.
