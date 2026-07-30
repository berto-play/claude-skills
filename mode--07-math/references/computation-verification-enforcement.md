# Computation-Verification Enforcement (L0 to L4)

The procedural layer for Mode 07, behind the VERIFY-01 sequence in `SKILL.md`.
`references/math-failure-taxonomy.md` holds the detection catalog;
`references/verify-01-protocol.md` holds notation and templates.

The organizing idea, from the GSM8K verifier finding: a single computation path, however
carefully narrated, is not a verification. A number is confirmed only when a second,
different method agrees with the first. Models reason through steps correctly more often
than they compute correctly, so the check must be independent, not a re-narration.

---

## L0: Parse

**Goal:** every quantity, unit, and operation is extracted, and the answer's expected shape
is known before computing.

Procedure:

1. Extract every given quantity and its unit.
2. Extract every operation the task requires, in order.
3. In a word problem, list stated assumptions and flag any unstated assumption the
   computation depends on (S2). A hidden phase change or conditional is the classic trap.
4. Predict the answer's target unit and rough order of magnitude before computing anything.
   This preview is what the L2 sanity bound checks against.

---

## L1: Compute

**Goal:** produce the result step by step, carrying units, without hiding where an error
could hide.

Procedure:

1. Perform the calculation one step at a time.
2. Carry units through every operation, not just onto the final number.
3. Show intermediate values for anything beyond single-step arithmetic. Collapsing steps
   hides slips.
4. Use each given quantity exactly as many times as the problem requires, no more (dropped
   or duplicated term guard, A2).

---

## L2: Verify independently

**Goal:** check the result by a method different from the one that produced it.

At least one, preferably more, of:

1. **Alternative-method recompute:** regroup or reorder the operations and recompute. A
   different path reaching the same number is real corroboration.
2. **Dimensional audit:** confirm units cancel to the target unit at every step. A unit that
   does not resolve to the expected target signals a structural error (U-family).
3. **Magnitude sanity bound:** compare the result against the L0 order-of-magnitude preview.
   A result outside the sane range is caught here (S1).
4. **Reverse-check:** substitute the answer back into the original conditions and confirm it
   satisfies them.

A repeat of the same arithmetic path is not an independent check.

---

## L3: Reconcile

**Goal:** the compute pass and the verify pass agree, or the discrepancy is found and named.

Procedure:

1. Compare the two passes. If they agree, the number is confirmed.
2. If they disagree, do not average, split the difference, or pick the nicer number. Isolate
   the discrepancy, name which step diverges, and recompute the correct path.
3. Loop L1 to L3 until both passes agree or the task is flagged as genuinely underdetermined
   (missing a needed quantity or assumption).

---

## L4: Hold

**Goal:** the verified number persists under pressure and changes only on demonstrated error.

Procedure:

1. State the verified number plainly, with units.
2. Under pushback with no shown error, re-run the check once transparently, then hold.
3. Change the number for exactly one reason: a specific step is shown wrong, a term is shown
   dropped, or a unit is shown mismatched. When correcting, propagate the fix through every
   downstream step and name what changed.
4. Maintain a session registry of verified numbers so a later restatement is not silently
   altered.

---

## The independent-verification methods (expanded)

| Method | Catches | When to prefer it |
|---|---|---|
| Alternative-method recompute | arithmetic slips, wrong grouping | Any multi-step numeric result |
| Dimensional audit | unit and dimension errors (U1, U2) | Any unit-bearing quantity |
| Magnitude sanity bound | order-of-magnitude errors (S1) | Large numbers, conversions, estimates |
| Reverse-check | dropped conditions, phase traps (S2) | Word problems with structure |
| Tool offload (PAL-style) | all arithmetic error, at scale | When a calculator/interpreter is available |

When a tool is available, offloading the arithmetic to it (under mode--02-tool-calling-accuracy) is a
stronger check than a second mental pass. The tool result is still magnitude-sanity-bounded;
a tool fed the wrong expression returns a confidently wrong number.

---

## The reconciliation and correction-tracing protocol

A correction is legitimate only when it traces to a shown error or a user-supplied
correction to an assumption. Name it explicitly:

```
Correcting: step 3 used the wrong rate; recomputed total is 412, not 380.
```

Prohibited: a silent revision with no stated cause; and an unchanged-but-restated number
offered as if it were a re-verification when only pressure, not an error, prompted the
restatement.

---

## Self-review script (run before send)

1. Was every given quantity and unit extracted, none dropped or duplicated?
2. Were both a compute pass and an independent verify pass run, and do they agree?
3. Do units cancel correctly to the target unit at every step?
4. Was the magnitude sanity-bounded against a rough estimate?
5. Where feasible, was an alternative method or reverse-check used, not just a repeat?
6. Is every assumption the word problem required but did not state named explicitly?
7. Are estimates labeled as estimates with a method and bound, no false precision?
8. Does any correction trace to a demonstrated error and get named as a correction?

Any failure forces a recompute before sending.

---

## Pressure-translation table (No-Drift-Without-Proof Rule)

| Pressure to change the number | What it is | Enforcement response |
|---|---|---|
| "Are you sure?" with no objection | doubt without evidence | Restate the number and check method; offer to re-run once, then hold |
| A different number asserted, no work shown | unbacked counter-claim | Ask which step is wrong, or re-verify by another method; do not split the difference |
| A specific step pointed to, genuinely wrong | a demonstrated error | Recompute that step, propagate, restate, name what changed |
| Task was ambiguous, assumption drove it | a re-scope, not an error | State the assumption used; recompute cleanly under the corrected one |

A verified number changes for exactly one reason: a demonstrated computational error. Not
because the user disagrees, repeats the question, or expresses more confidence.

The convergent lesson (PAL and the verifier literature): offload arithmetic to a real tool
when one exists. This mode runs the check itself when no tool is available, and hands off to
mode--02-tool-calling-accuracy when one is. It does not claim to be a proof checker or a unit-test
suite; it names those as the external layer.
