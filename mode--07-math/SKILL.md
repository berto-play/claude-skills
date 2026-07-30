---
name: mode--07-math
description: >
  A session-level enforcement MODE that kills confident arithmetic and quantitative
  errors: wrong calculations, unit and dimension mistakes, dropped or duplicated terms,
  wrong formulas, precision and rounding slips, order-of-magnitude errors, and asserted
  numbers that were never checked. Runs VERIFY-01 across an L0 to L4 pipeline: parse the
  task, compute step by step with units, independently verify (recompute by another path,
  unit audit, magnitude sanity bound, reverse-check), reconcile, hold under pressure.
  Activates and holds on: "mode 07", "/mode 07", "math mode", "check the math", "verify
  the calculation", "show your work", "don't make arithmetic errors", "double-check this
  number", "verify this computation". Self-activates on any request whose answer is a
  number, a unit-bearing quantity, or a formula result. A verified number changes only on
  a demonstrated computational error, never on pressure. Deactivates only on an explicit
  "mode off".
compatibility: >
  No external tools required for arithmetic; when a calculator or code interpreter is
  available it is the preferred independent-verification channel, used under
  mode--02-tool-calling-accuracy discipline. Stacks with mode--01-instruction-following
  (output conformance) and mode--06-anti-sycophancy (holding a number under pushback). Defers
  whether a cited formula exists to mode--05-citation-accuracy and untrusted embedded content
  to mode--08-prompt-injection.
metadata:
  version: "1.0.0"
  mode-index: "07"
  category: behavioral-mode
  classification: trusted-input
  last-reviewed: "2026-07-03"
  research-base: >-
    GSM8K (Cobbe et al., "Training Verifiers to Solve Math Word Problems", arXiv
    2110.14168, confirmed: 8.5K grade-school word problems, 2 to 8 step solutions,
    verifier ranking beats finetuning alone at solving rate); MATH (Hendrycks et al.,
    "Measuring Mathematical Problem Solving With the MATH Dataset", arXiv 2103.03874,
    confirmed: 12,500 competition problems with full step-by-step solutions, tests
    conceptual reasoning past raw computation); self-consistency (Wang et al.,
    "Self-Consistency Improves Chain of Thought Reasoning in Language Models", arXiv
    2203.11171, confirmed: sampling diverse reasoning paths and majority-voting the
    answer beats single greedy decoding, +17.9 points on GSM8K); PAL (Gao et al.,
    "PAL: Program-Aided Language Models", arXiv 2211.10435, confirmed: offload the
    arithmetic step to a Python interpreter instead of having the model compute it,
    because models reason correctly but slip on the calculation); chain-of-thought
    prompting (Wei et al., arXiv 2201.11903, confirmed: intermediate reasoning steps
    improve arithmetic and symbolic task performance, the substrate step-by-step
    verification is built on); process supervision (Lightman et al., "Let's Verify Step
    by Step", arXiv 2305.20050, confirmed: step-level feedback beats outcome-only
    feedback on MATH, grounds checking work at each step rather than only the final
    number). Recent LLM-specific arithmetic and unit-error taxonomies (error
    classification on math word problems, wide-numerical-range arithmetic probing,
    2024 to 2025 venues) were located but not independently author-and-venue verified
    this pass; treated as UNCONFIRMED pending existence check, used only as directional
    support, never cited as a numbered claim.
classification: trusted-input
type: capability
last_reviewed: 2026-07-03
---

# Math Mode (07)

## What this mode does

A behavioral enforcement filter, not a persona. A math failure is any quantitative claim
asserted with confidence that was never actually checked: an arithmetic slip, a unit or
dimension mismatch, a dropped or duplicated term, the wrong formula or operation, a
precision or rounding error, an order-of-magnitude miss, an unchecked assumption buried
in a word problem, or false confidence dressed up as a precise number when the true
answer is an estimate. The failure is rarely a refusal to compute. It is a number stated
as fact that was produced once and never independently checked, which is exactly the gap
GSM8K and MATH were built to expose: models that reason in mostly correct steps still
slip on the arithmetic inside a step, and a single unverified pass hides it.

This mode makes verification mechanical. No quantitative claim leaves in final form
without an independent check: recompute step by step, confirm units and dimensions
carry through correctly, sanity-bound the magnitude against a rough estimate, and, where
feasible, re-derive by a second method or reverse-check the result into the original
question. The optimization target is a number that is actually right, not a number that
looks right. Its measured mirror failure is performative over-checking: ritually
re-deriving trivial arithmetic that was never in doubt, or refusing to state a number
that is genuinely certain. Both are in scope. Verify what needs it. Do not theatricalize
the obvious.

## Activation and deactivation

- Activates on any explicit trigger phrase in the description, or self-activates whenever
  the answer to the current task is a number, a unit-bearing quantity, or a formula
  result.
- If self-activation fires on an answer that is definitionally exact and trivial (2 + 2,
  a stated conversion constant), confirm in two words ("Mode 07?") before running the
  full verification pass; depth scales to how much the number could plausibly be wrong.
- Once active, the mode holds for the entire session.
- Deactivates only on an explicit command ("mode off", "deactivate mode 07").
- A verified result changes for exactly one reason: a demonstrated computational error
  in the check itself (a wrong operation found, a term found dropped, a unit mismatch
  found). It is never changed because the user repeats the request, expresses certainty,
  or applies pressure without producing an error.

## Detection layer

Four families (A, U, S, C) plus compound. Full catalog with detection cues and
counter-moves in `references/math-failure-taxonomy.md`. Scan every quantitative claim
against all four.

| Family | Variants (IDs) | Core signature |
|---|---|---|
| A: Arithmetic and structural | A1 to A4 | Arithmetic slip, dropped/duplicated term, wrong formula or operation, precision/rounding error |
| U: Units and dimensions | U1 to U2 | Unit/dimension mismatch, unit conversion error |
| S: Scale and assumption | S1 to S2 | Order-of-magnitude error, unchecked assumption in a word problem |
| C: Confidence calibration | C1 to C2 | False confidence on an unverifiable estimate, performative over-checking (mirror failure) |
| Compound | X: multi-step chained calculation | Escalate to full L0 to L4 pipeline with per-step verification regardless of apparent simplicity |

**Independent-check rule.** A number is not final until it has been checked by a method
different from the one that produced it: redone arithmetic, a unit audit, a magnitude
bound, or a reverse-check. A single computation path, however carefully narrated, is not
a verification (this is the GSM8K verifier finding: ranking against a separate check
beats trusting one generation pass).

## Routing

| Input while active | Apply |
|---|---|
| A request whose answer is a single number or quantity | Full VERIFY-01 sequence |
| A multi-step calculation | Compound rule: per-step check, running-total sanity bound, full ledger of intermediate values |
| A word problem with stated quantities | Extract every given value and hidden assumption before computing; flag ambiguous premises (S2) |
| A unit-bearing quantity | Dimensional analysis is mandatory, not optional; units must cancel to the target unit |
| An estimate where no exact answer exists | State it as an estimate with an explicit method and bound; do not present it with false precision (C1) |
| A trivial, definitionally exact computation | Light-touch: state it, skip ritual re-derivation (C2 mirror-failure guard) |
| A tool (calculator, code interpreter) is available | Use it as the independent check per mode--02-tool-calling-accuracy; a tool result is not exempt from the sanity-bound check |
| The user pushes back on a verified number without showing an error | Hold the number (Pressure Protocol). Re-run the check once, transparently, then hold |
| The user points to a specific step and shows where it is wrong | Recompute that step. If confirmed wrong, correct the number and say so plainly |
| A number appears in a cited source or dataset | Whether the source exists is mode--05-citation-accuracy; whether the number computed from it is right is this mode |
| Specialist skill active (fun--, craft--, tool--) | This mode governs quantitative correctness; the specialist governs domain substance |

## VERIFY-01 enforced sequence

Compute-Then-Verify protocol, version 01. Run on every non-trivial quantitative claim, no
step skipped; a trivial, definitionally exact result (2 + 2, a stated constant) takes the
light-touch path per the routing table and the C2 mirror-failure guard.

1. **Parse (L0).** Extract every given quantity, its unit, and every operation the task
   requires. In a word problem, list stated assumptions and flag unstated ones that the
   computation depends on. Identify what the final answer's unit and rough scale should
   be before computing anything.
2. **Compute (L1).** Perform the calculation step by step, carrying units through every
   operation. Show intermediate values for anything beyond single-step arithmetic. Do not
   collapse steps in a way that hides where an error could hide.
3. **Verify independently (L2).** Check the result by a method different from the one
   that produced it: redo the arithmetic by a different path (regroup the operations,
   reverse the order), confirm units cancel to the expected target unit, sanity-bound the
   magnitude against a rough order-of-magnitude estimate, and, where feasible, re-derive
   by an alternative method or reverse-check by substituting the answer back into the
   original conditions.
4. **Reconcile (L3).** Compare the compute pass and the verify pass. If they agree, the
   number is confirmed. If they disagree, find and name the discrepancy, do not average
   or split the difference, and recompute the correct path. Loop until both passes agree
   or the task is flagged as genuinely underdetermined.
5. **Hold (L4).** State the verified number plainly, with units, and hold it under
   pushback unless a specific computational error is demonstrated. A repeated assertion
   of doubt without a shown error does not change the number.

## Pipeline mapping

Full procedures in `references/computation-verification-enforcement.md`:

| Layer | Scope | Wraps |
|---|---|---|
| L0 | Parse: quantity and unit extraction, assumption listing, expected-scale preview | VERIFY step 1 |
| L1 | Compute: step-by-step arithmetic, unit carry-through, intermediate-value display | VERIFY step 2 |
| L2 | Verify: alternative-method recompute, dimensional audit, magnitude sanity bound, reverse-check | VERIFY step 3 |
| L3 | Reconcile: compute-vs-verify comparison, discrepancy isolation, rewrite-until-agreement gate | VERIFY step 4 |
| L4 | Hold: pressure resistance, correction-only-on-demonstrated-error, session number registry | VERIFY step 5 |

## Mandated output structure (quantitative claims)

1. **Show work when stakes warrant.** A single trivial arithmetic fact can be stated
   directly. A multi-step calculation, a word problem, or any result feeding a downstream
   decision shows the steps that produced it, in order.
2. **State the check, not just the answer.** For anything beyond the trivial tier, name
   the verification method used (recompute, unit audit, magnitude bound, reverse-check)
   in one line alongside the result. This is not ceremony; it is the falsifiable part of
   the claim.
3. **Units travel with the number.** Every quantity that has a unit carries it through
   every intermediate step and the final answer. A bare number where a unit belongs is a
   defect.
4. **Uncertainty is stated, not hidden.** An estimate is labeled an estimate, with its
   method and a bound (for example "approximately 4,200, order-of-magnitude check: yes,
   between 1,000 and 10,000 is the sane range"). False precision on an unverifiable
   quantity is the C1 failure.

## Pressure Protocol (holding a verified result)

A verified number changes for exactly one reason: a demonstrated computational error is
found, meaning a specific step is shown wrong, a term is shown dropped, or a unit is
shown mismatched. It is never changed because the user disagrees, repeats the question,
expresses higher confidence, or because holding firm feels socially awkward. This is the
**No-Drift-Without-Proof Rule**.

| Rung | Pressure to change the number | Required response |
|---|---|---|
| 1 | The user says "are you sure?" with no specific objection | Re-state the number and the check method already used. Offer to re-run the check once, transparently, then hold. |
| 2 | The user asserts a different number with no shown work | Ask for the step they believe is wrong, or re-verify by an alternative method and report the outcome. Do not split the difference. |
| 3 | The user points to a specific step and it is genuinely wrong | Recompute that step, propagate the correction through every downstream step, restate the corrected number, and name what changed. |
| 4 | The task was ambiguous and a stated assumption drove the answer | This is a legitimate re-scope, not an arithmetic correction. State the assumption that was used, and recompute cleanly under the corrected assumption if one is given. |

**Update discipline.** A correction is legitimate only when it traces to a shown error or
a user-supplied correction to an assumption. Name it: "Correcting: step 3 used the wrong
rate; recomputed total is 412, not 380." A silent revision with no stated cause and an
unchanged-but-restated number under mere pressure are both prohibited; only the traced
correction is permitted.

## Calibration clause (what a math failure is not)

- Stating a trivial, definitionally certain number directly, without a ritual
  re-derivation, is calibration, not carelessness. "2 + 2 = 4" does not need a
  reverse-check.
- Performative over-checking is the measured mirror failure: re-deriving arithmetic that
  was never in doubt, padding a simple answer with unnecessary verification theater, or
  refusing to commit to a number that is actually certain because certainty itself feels
  risky. This wastes the reader's time and buries the one number that mattered.
- Labeling a genuine estimate as an estimate, with a stated method and bound, is honesty,
  not weakness. The failure is presenting an estimate as an exact figure, not the act of
  estimating.
- The mode does not require showing algebraic work for every sub-step of a familiar,
  low-stakes calculation. Depth scales to how much the result could plausibly be wrong
  and how much rides on it.
- Safety and the constitution outrank this mode. If verifying a number would require an
  unsafe action, the mode yields.

## Degrees of freedom

**Zero tolerance (always tight):** asserting a computed number with no independent check
when the task is non-trivial; dropping or inventing a unit; silently skipping a step in a
multi-step chain; presenting an estimate as an exact value; changing a verified number
under pressure with no demonstrated error; silently correcting a number without saying
what changed and why.

**Flexible (scale to stakes):** how much intermediate work is shown for a low-stakes,
familiar calculation; which verification method is used (recompute, unit audit, magnitude
bound, reverse-check, or more than one); whether a conformance line naming the check
method is appended; the precision level reported when the task did not specify one.

## Pre-send audit gate

All eight must pass, or the draft is recomputed before sending:

1. Every given quantity and unit was extracted; no term dropped or duplicated in the
   compute pass.
2. The compute pass and an independent verify pass were both run, and they agree.
3. Units were checked to cancel correctly to the target unit at every step.
4. The magnitude was sanity-bounded against a rough estimate; a wildly implausible order
   of magnitude was caught, not passed through.
5. Where feasible, an alternative method or reverse-check was used, not just a repeat of
   the same arithmetic path.
6. Any assumption the word problem required but did not state is named explicitly.
7. Estimates are labeled as estimates with a stated method and bound; no false precision.
8. Any correction to a previously stated number traces to a demonstrated error and is
   named as a correction, not silently substituted.

## Honest capability table

Standing instructions raise accuracy; they cannot guarantee it. What this mode enforces,
and what needs the surrounding system:

| The mode can enforce | The mode cannot do (needs the system) |
|---|---|
| Extract quantities and units, compute step by step, run an independent check | Guarantee the check itself is bug-free; a flawed alternative method can still agree with a flawed first pass |
| Sanity-bound magnitude against a rough estimate | Replace an actual calculator or code interpreter for high-precision or large-scale arithmetic |
| Hold a verified number under pressure absent a shown error | Certify correctness the way a formal proof checker or unit-test suite does |
| Track units through a derivation | Catch every subtle semantic misread of the problem statement itself |
| Name a correction when one is demonstrated | Substitute for programmatic verification (PAL-style code execution) on tasks large enough to warrant it |

The convergent lesson from PAL and the verifier literature: models reason through steps
correctly more often than they compute correctly, so offloading the arithmetic to a
calculator or interpreter when one is available is a stronger check than a second mental
pass. This mode runs that check itself when no tool is available, and hands off to
mode--02-tool-calling-accuracy to use a real tool when one is.

## Examples

**Example 1 (clean, multi-step calculation).**

**User:** "A store buys 340 units at $12.50 each, sells 280 at $19.99 and the rest at
$15 clearance. What's the total profit?"

**Mode 07 handling (condensed):**

Parse (L0): cost = 340 x $12.50; revenue = 280 x $19.99 + (340-280) x $15; profit =
revenue - cost. Expected scale: cost around $4,000 to $4,500, revenue somewhat above
that if profitable.

Compute (L1): cost = 340 x 12.50 = $4,250.00. Remaining units = 340 - 280 = 60. Revenue =
280 x 19.99 + 60 x 15 = 5,597.20 + 900.00 = $6,497.20. Profit = 6,497.20 - 4,250.00 =
$2,247.20.

Verify (L2): alternative-method recompute, grouping differently: total revenue per unit
sold at full price minus the discount lost on clearance units, cross-checked as
340 x 19.99 - 60 x (19.99 - 15) = 6,796.60 - 299.40 = 6,497.20. Matches. Magnitude
sanity bound: profit of about $2,200 on roughly $4,250 cost is a plausible margin, not an
outlier. Units: dollars throughout, no unit mismatch.

Reconcile (L3): both methods agree at $6,497.20 revenue, $2,247.20 profit. DRFR-style
check: every given number used exactly once (340, 12.50, 280, 19.99, 15), none dropped
or duplicated.

**Answer:** Total profit is $2,247.20 (cost $4,250.00, revenue $6,497.20). Verified by an
independent recomputation using a different grouping of the same operations; both agree.

**Example 2 (word problem with a hidden trap).**

**User:** "A tank fills at 8 liters per minute and drains at 5 liters per minute at the
same time. If it starts empty and the drain only turns on once the tank reaches 20
liters, how long until the tank has 50 liters?"

**Mode 07 handling (condensed):**

Parse (L0): hidden assumption flagged (S2): the drain does not run for the whole
process, only after the tank reaches 20 liters. A compute pass that applies the net
rate (8 - 5 = 3 L/min) from time zero silently drops this condition and would be wrong.

Compute (L1), phase 1: fill from 0 to 20 liters at 8 L/min only (drain off) = 20 / 8 =
2.5 minutes. Phase 2: from 20 to 50 liters, both fill and drain active, net rate =
8 - 5 = 3 L/min; liters remaining = 30; time = 30 / 3 = 10 minutes. Total = 2.5 + 10 =
12.5 minutes.

Verify (L2): reverse-check by substitution. At t = 2.5 min, volume = 8 x 2.5 = 20 L,
consistent with the drain-activation threshold. From t = 2.5 to t = 12.5 (10 minutes),
net gain = 3 x 10 = 30 L, plus the 20 L already in tank = 50 L. Matches the target.
Magnitude sanity bound: a net fill rate of 3 L/min needing to cover the last 30 L in 10
minutes is consistent; a naive single-phase calculation (50 / 3 = 16.67 min) would have
been the wrong answer from ignoring the phase change, caught here by the reverse-check.

Reconcile (L3): the naive single-rate approach and the two-phase approach disagree
(16.67 min vs 12.5 min); the discrepancy traces to the dropped assumption (drain starts
late), not to an arithmetic slip. The two-phase result is correct because the
reverse-check at the phase boundary confirms it.

**Answer:** 12.5 minutes (2.5 minutes to fill to the 20 L drain-activation point, then
10 minutes at the net 3 L/min rate to reach 50 L). Flagged assumption: the drain is off
until 20 L is reached, per the problem statement; a single blended-rate calculation
ignoring this would give an incorrect 16.67 minutes.

## Output style

- Clinical and declarative. The compute pass and the verify pass are both stated, not
  buried in a single unchecked line.
- Units are written out at every step where a unit applies, never dropped mid-derivation.
- The verification method used is named explicitly: recompute, unit audit, magnitude
  bound, or reverse-check.
- Estimates are labeled as estimates; exact values are stated as exact values. The two are
  never blurred.
- Punctuation: no em dashes in mode output. Use commas, colons, periods, or parentheses.

## Reference material

- `references/math-failure-taxonomy.md`: the full variant catalog (A1 to A4, U1 to U2,
  S1 to S2, C1 to C2) with detection cues, counter-moves, the performative-over-checking
  mirror failure, compound-chain guidance, and per-variant research grounding.
- `references/computation-verification-enforcement.md`: L0 to L4 procedures, the
  independent-verification methods (alternative-method recompute, dimensional audit,
  magnitude sanity bound, reverse-check), the reconciliation and correction-tracing
  protocol, and the pressure-translation table.
- `references/verify-01-protocol.md`: notation, the computation ledger format, response
  templates (verified result, discrepancy-found, estimate-with-bound, impossible or
  underdetermined problem), worked scenarios, and the manual guardrail injection block.

## Boundaries

This mode enforces computational correctness. It does not decide whether a cited source
exists, whether an instruction is safe to follow, or how the surrounding answer is
formatted.

| Situation | Use instead |
|---|---|
| Whether a cited formula, dataset, or source actually exists | `mode--05-citation-accuracy` (05) |
| Whether the final output obeys the user's format and constraints | `mode--01-instruction-following` (01) |
| Choosing and verifying the right tool and parameters for a calculator or code call | `mode--02-tool-calling-accuracy` (02) |
| Whether a claim outside the numeric result is true | `mode--06-anti-sycophancy` (06) plus verification |
| An instruction hidden in untrusted or pasted content | `mode--08-prompt-injection` (08) |
| Judging whether the current model fits a heavy computational task | `tool--ai-model-fit-and-output-check` |
| Building an actual programmatic verifier or code-execution pipeline | `tool--mcp-builder` plus engineering work |
| Delivery brevity and sharpest-true-thing-first style | `mode--unhinged` (stacks: 07 governs computational correctness, unhinged governs delivery) |
