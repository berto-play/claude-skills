# Math Failure Taxonomy

Cold-storage catalog for `mode--07-math`. Every variant carries an ID, a detection cue,
and a counter-move. The hot path scans against all four families on every quantitative
claim; this file is the lookup.

Construct space: a math failure is a quantitative claim asserted with confidence that was
never independently checked. Failures fall into two static kinds, arithmetic and
structural errors inside the computation itself (family A) and units-and-dimensions
errors in how quantities carry meaning (family U), one scale kind, errors in the size and
plausibility of the result (family S), and one calibration kind, mismatches between
stated confidence and actual certainty (family C). Family X covers compound multi-step
chains where per-step error risk accumulates.

---

## Family A: Arithmetic and structural errors

Errors inside the computation path itself. The most common class, and the one GSM8K and
MATH were built to expose: models reason through the right steps but slip inside a step.

| ID | Variant | Detection cue | Counter-move |
|---|---|---|---|
| A1 | Arithmetic slip | A basic operation (add, multiply, divide) computed wrong; a transcription error carrying a wrong digit forward | Recompute by a different grouping or order of operations; a slip rarely survives a second independent path |
| A2 | Dropped or duplicated term | A given quantity used zero times or more than once; a step in a multi-step chain silently skipped | List every given quantity at L0 parse and confirm each is used exactly the number of times the problem requires |
| A3 | Wrong formula or operation | The wrong relationship applied (area formula where perimeter was needed, addition where the problem needs multiplication) | State the formula or operation chosen and why, before computing; check it against the units it should produce |
| A4 | Precision and rounding error | Intermediate rounding compounds into a materially wrong final answer; a rounded intermediate value used as if exact | Carry full precision through intermediate steps; round only the final reported value, and state the rounding rule used |

Research grounding: GSM8K (Cobbe et al., arXiv 2110.14168) shows models frequently reach
the right reasoning chain but fail inside the arithmetic of a step, which is the reason
the paper trains a separate verifier rather than trusting a single generation. PAL (Gao
et al., arXiv 2211.10435) addresses A1 to A4 directly by offloading the computation step
to a Python interpreter once the reasoning chain is set, on the finding that models
reason correctly far more often than they compute correctly.

---

## Family U: Units and dimensions

Errors in how a quantity's unit or dimension carries through a calculation. Distinct from
family A because the arithmetic on the numbers can be entirely correct while the unit
attached to the result is wrong or missing.

| ID | Variant | Detection cue | Counter-move |
|---|---|---|---|
| U1 | Unit or dimension mismatch | A result stated with no unit, the wrong unit, or units that do not match across an equation (adding meters to seconds) | Carry the unit through every step of the derivation; confirm the final unit is what the question asked for, not merely a number |
| U2 | Unit conversion error | A conversion factor applied upside down, mixed (miles with kilometers), or omitted entirely | Write the conversion factor as an explicit fraction that cancels the source unit and leaves the target unit; check the cancellation, not just the multiplication |

Research grounding: dimensional analysis is the standard cross-check in physical-science
and engineering computation; a result whose units do not reduce to the expected target
unit is invalid regardless of whether the numeric arithmetic was correct. This family sits
outside the pure word-problem benchmarks (GSM8K, MATH) because those datasets are
largely unit-light; it is included here because unit-bearing quantitative claims are
common in applied requests this mode must also cover.

---

## Family S: Scale and assumption errors

Errors in whether the result is the right size, and whether the problem's own premises
were fully surfaced before computing.

| ID | Variant | Detection cue | Counter-move |
|---|---|---|---|
| S1 | Order-of-magnitude error | The result is off by a factor of 10, 100, or more from what a rough estimate would predict, usually from a misplaced decimal or a unit-scale slip (thousands vs millions) | Before computing, state the expected rough range; after computing, check the result falls inside that range, and explain any large deviation |
| S2 | Unchecked assumption in a word problem | The problem has a condition that changes partway through (a rate that starts late, a threshold, a discount that only applies above a quantity) and the computation applies a single blended rule for the whole problem | Enumerate every stated condition and any phase change at L0 parse; treat a missed phase or threshold as a dropped term, not a rounding issue |

Research grounding: MATH (Hendrycks et al., arXiv 2103.03874) is built from competition
problems specifically because they test conceptual and multi-condition reasoning past
raw computation, which is where S1 and S2 concentrate; a numerically fluent model can
still misread which regime of the problem applies. Order-of-magnitude estimation (S1) is
the standard first check in applied quantitative work precisely because it catches scale
errors that a step-by-step recompute using the same flawed setup would not.

---

## Family C: Confidence calibration

Not an error in the arithmetic itself but a mismatch between the stated confidence and
the actual certainty of the number. Includes the mirror failure this mode is calibrated
against.

| ID | Variant | Detection cue | Counter-move |
|---|---|---|---|
| C1 | False confidence on an unverifiable estimate | A quantity that is genuinely an estimate (a population figure, a rough cost, a real-world measurement without a precise source) is stated with false precision, as if it were computed exactly | Label the number as an estimate, state the method used to derive it, and give an explicit bound or range rather than a single falsely precise figure |
| C2 | Performative over-checking (mirror failure) | A trivial, definitionally exact computation is padded with ritual re-derivation, multiple restatements, or unnecessary hedging that adds no verification value | Recognize when a result is definitionally certain (a stated constant, single-digit arithmetic); state it directly, and reserve visible verification for results that are actually at risk of error |

Research grounding: the self-consistency method (Wang et al., arXiv 2203.11171) formalizes
C1's counter for genuinely hard problems: sampling multiple independent reasoning paths
and taking the majority answer is a calibrated way to report confidence on a
non-deterministic reasoning process, rather than asserting one path's output as certain.
Process supervision (Lightman et al., arXiv 2305.20050) grounds the general principle that
checking should concentrate where a step is actually at risk of being wrong, which is the
same asymmetry that makes C2 a failure: uniform, maximal verification on every claim
regardless of risk wastes the check budget on the trivial and dilutes attention on the
claims that need it.

---

## Compound chains (Family X)

When a task requires a chain of dependent calculations, per-step error risk compounds:
even a high per-step accuracy produces a much lower end-to-end accuracy once enough
steps are chained.

**Detection:** count the distinct computational steps required to reach the final answer.
If the chain has three or more dependent steps, or any step feeds a later step's input,
treat it as compound.

**Required handling:**
1. Parse and list every step before computing any of them (L0), including the expected
   unit and rough scale of each intermediate value.
2. Compute each step in order, carrying forward exact intermediate values (A4 guard).
3. Verify at least the final result by an alternative method or reverse-check (L2), and
   verify any intermediate value that a downstream step is unusually sensitive to.
4. Run the full reconciliation (L3) comparing the compute pass and the verify pass before
   reporting; a discrepancy at any step invalidates every step after it and forces a
   recompute from that point forward, not a patch to the final number alone.

**Common compound patterns and where they break:**
- Multi-phase word problems (a rate that changes partway through, as in Example 2 of
  SKILL.md): the failure is applying one blended formula across a boundary the problem
  actually splits (S2), not an arithmetic slip inside either phase.
- Chained unit conversions (convert A to B to C to D): the failure concentrates in a
  single flipped conversion factor partway through the chain (U2), which then silently
  propagates a wrong unit through every later step.
- Running totals with several additions and subtractions (profit and loss, inventory
  counts): the failure is a dropped or duplicated term (A2) partway through the list,
  caught by confirming every given number was used exactly once.

---

## The performative-over-checking mirror failure

The mirror of asserting an unchecked number is checking so ritually that trivial claims
get the same ceremony as genuinely risky ones. Two forms:

- **Ritual re-derivation:** re-deriving a definitionally certain fact multiple times, or
  narrating an elaborate verification process for arithmetic that was never in doubt
  ("2 + 2 = 4, let me double-check: 2 + 2 = 4, confirmed"). This adds no information and
  buries the one number in a response that actually needed scrutiny.
- **Certainty refusal:** declining to state a number plainly because stating any number
  with confidence feels risky, hedging a genuinely certain result into a vague range for
  no reason grounded in actual uncertainty.

This mode is calibrated against both. Verification depth scales to how much a result
could plausibly be wrong and how much rides on it. A trivial, definitionally exact
computation is stated directly. A multi-step calculation, a word problem with a hidden
condition, or any number feeding a downstream decision gets the full compute-then-verify
treatment. Uniform maximal ceremony on every number is not rigor; it is noise that hides
the signal.

## Direction of harm

Both directions are in scope. The common failure is under-checking: a number is asserted
with the confidence of a checked fact when it was only computed once, and the reader has
no way to tell without redoing the work themselves, which defeats the purpose of trusting
the answer. The mirror failure is over-checking: ceremony that consumes the reader's
attention on claims that were never at risk, which trains the reader to skim past
verification language rather than trust it. A mode that only prevented under-checking
would push toward the second failure, so the calibration clause and the C2 entry are
load-bearing, not decorative.

## Source register

- GSM8K: Cobbe, Kosaraju, Bavarian, Chen, Jun, Kaiser, Plappert, Tworek, Hilton, Nakano,
  Hesse, Schulman. "Training Verifiers to Solve Math Word Problems." arXiv 2110.14168,
  Oct 2021. (Confirmed: 8.5K grade-school math word problems, 7.5K train / 1K test,
  solutions take 2 to 8 steps, verifier-ranking beats finetuning-only at solve rate.)
- MATH: Hendrycks, Burns, Kadavath, Arora, Basart, Tang, Song, Steinhardt. "Measuring
  Mathematical Problem Solving With the MATH Dataset." arXiv 2103.03874, Mar 2021.
  (Confirmed: 12,500 competition mathematics problems, AMC/AIME-level, full step-by-step
  solutions, tests conceptual reasoning past raw computation.)
- Self-consistency: Wang, Wei, Schuurmans, Le, Chi, Zhou. "Self-Consistency Improves
  Chain of Thought Reasoning in Language Models." arXiv 2203.11171, Mar 2022. (Confirmed:
  sampling diverse reasoning paths and majority-voting the final answer beats single
  greedy decoding; +17.9 points on GSM8K.)
- PAL: Gao, Madaan, Zhou, Alon, Liu, Yang, Callan, Neubig. "PAL: Program-Aided Language
  Models." arXiv 2211.10435, Nov 2022. (Confirmed: offloads the arithmetic execution step
  to a Python interpreter while the model handles decomposition, on the finding that
  models reason correctly more often than they compute correctly.)
- Chain-of-thought: Wei, Wang, Schuurmans, Bosma, Ichter, Xia, Chi, Le, Zhou.
  "Chain-of-Thought Prompting Elicits Reasoning in Large Language Models." arXiv
  2201.11903, Jan 2022. (Confirmed: intermediate reasoning steps improve arithmetic,
  commonsense, and symbolic task performance; the substrate step-by-step verification is
  built on.)
- Process supervision: Lightman, Kosaraju, Burda, Edwards, Baker, Lee, Leike, Schulman,
  Sutskever, Cobbe. "Let's Verify Step by Step." arXiv 2305.20050, May 2023. (Confirmed:
  step-level process feedback outperforms outcome-only feedback on the MATH test set;
  grounds checking each step rather than only the final number.)
- Recent LLM-specific arithmetic and unit-error classification work (error taxonomies on
  math word problems, wide-numerical-range arithmetic probing, 2024 to 2025 venues) was
  located during this research pass but not independently confirmed on author, venue, and
  exact claims. Treated as UNCONFIRMED pending existence check: used only as directional
  context for families A and S, never cited as a numbered, sourced claim.
