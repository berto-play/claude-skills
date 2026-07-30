# VERIFY-01 Protocol Reference

Compute-Then-Verify protocol, version 01. Notation, computation-ledger format, response
templates, worked scenarios, and manual guardrail injection. Referenced by `SKILL.md` as
the format specification layer. `references/computation-verification-enforcement.md` holds
the procedural layer; `references/math-failure-taxonomy.md` holds the detection catalog.

---

## Section 1: Notation

- **Given**: a quantity stated in the problem, with its unit
- **Op**: one operation the task requires
- **Compute pass**: the first calculation, step by step (L1)
- **Verify pass**: an independent second check by a different method (L2)
- **Bound**: the order-of-magnitude range the answer must fall within (set at L0)
- **Confirmed**: compute pass and verify pass agree (L3)

**The verification function.** V(number) = confirmed only if an independent method agrees
with the compute pass. A single path, however carefully narrated, gives V = unconfirmed.

---

## Section 2: Activation tokens

| Phrase type | Examples | Result |
|---|---|---|
| Explicit mode trigger | "mode 07", "/mode 07", "math mode" | Activate immediately |
| Explicit verification | "check the math", "verify the calculation", "show your work" | Activate immediately |
| Corrective | "that number is wrong", "double-check this" | Activate; recompute the flagged result |
| Structural | The task's answer is a number, unit-bearing quantity, or formula result | Self-activate |
| Deactivation | "mode off", "deactivate mode 07" | Deactivate |

Self-activation on a definitionally trivial computation (2 + 2, a stated constant): confirm
in two words ("Mode 07?") before the full pass; depth scales to how much the number could be
wrong.

---

## Section 3: The computation ledger format

Internal working object, surfaced for multi-step work or when the user asks to see it.

```
COMPUTE LEDGER (task: store profit)
GIVENS: 340 units, $12.50/unit cost, 280 @ $19.99, 60 @ $15
BOUND (L0): cost ~$4,000-4,500; profit plausibly ~$2,000s
COMPUTE (L1): cost = 340 x 12.50 = 4,250.00
              rev  = 280 x 19.99 + 60 x 15 = 5,597.20 + 900 = 6,497.20
              profit = 6,497.20 - 4,250.00 = 2,247.20
VERIFY (L2): regroup: 340 x 19.99 - 60 x (19.99-15) = 6,796.60 - 299.40 = 6,497.20  [match]
             magnitude: ~$2,200 profit on ~$4,250 cost, plausible  [pass]
             units: dollars throughout  [pass]
RECONCILE (L3): both passes agree; every given used once  ->  confirmed
```

---

## Section 4: Response templates

### Template A: Verified result

```
<Answer with units>. Verified by <method: independent recomputation / reverse-check / unit
audit>; both passes agree.
```

For a trivial result, state it directly with no ceremony.

### Template B: Discrepancy found

```
The two methods disagreed: <method 1> gave <X>, <method 2> gave <Y>. The discrepancy traces
to <the specific step / dropped assumption>. The correct result is <Z>, because <reason the
correct path is right>.
```

### Template C: Estimate with bound

```
Approximately <value> (this is an estimate, not an exact figure). Method: <how it was
derived>. Order-of-magnitude check: <the sane range it falls in>.
```

### Template D: Impossible or underdetermined problem

```
This cannot be solved as stated: <what is missing or contradictory, e.g. a needed quantity
is absent, or two conditions conflict>. If you supply <the missing piece / the intended
assumption>, I will compute it cleanly.
```

---

## Section 5: Handling scripts

### The single-pass confidence trap (independent-check rule)

Internal check: "I narrated the steps carefully, so the number is right."
Response: careful narration is not verification. Run a genuinely different method. Only
agreement between two paths confirms the number.

### The performative-over-checking urge (C2 mirror failure)

Internal check: "I'm about to reverse-check 2 + 2."
Response: do not. Trivial, definitionally certain arithmetic is stated directly. Verification
theater on the obvious buries the number that mattered.

### The hidden-assumption trap (S2)

Internal check: "The word problem has a phase change / conditional I might be flattening."
Response: list stated and unstated assumptions at L0. A blended single-rate calculation over
a two-phase process is the classic wrong answer; the reverse-check at the phase boundary
catches it.

### The false-precision reflex (C1)

Internal check: "I'm giving an exact figure for something that is really an estimate."
Response: label it an estimate, state the method and bound. False precision on an
unverifiable quantity is the C1 failure.

### The pressure-to-change (No-Drift-Without-Proof)

Internal check: "The user insists the number is wrong but showed no error."
Response: re-run the check once, transparently, then hold. The number changes only when a
specific step is shown wrong.

---

## Section 6: Worked scenarios

### Scenario 1: Multi-step calculation (Template A)

User: "340 units at $12.50 cost, sell 280 at $19.99 and 60 at $15 clearance. Total profit?"

See the ledger in Section 3. Compute gives $2,247.20 profit; an independent regrouping
reaches the same $6,497.20 revenue; magnitude and units pass.

Answer: "Total profit is $2,247.20 (cost $4,250.00, revenue $6,497.20). Verified by an
independent recomputation using a different grouping; both agree."

### Scenario 2: Word problem with a phase-change trap (Template A + flagged assumption)

User: "A tank fills at 8 L/min and drains at 5 L/min, but the drain only starts once the
tank hits 20 L. Starting empty, how long to reach 50 L?"

Parse: flag the hidden assumption (drain off until 20 L). Phase 1: 0 to 20 L at 8 L/min = 2.5
min. Phase 2: 20 to 50 L at net 3 L/min = 30/3 = 10 min. Total 12.5 min. Reverse-check at the
phase boundary confirms it; a naive single-rate 50/3 = 16.67 min would be wrong.

Answer: "12.5 minutes. Flagged assumption: the drain is off until 20 L, per the problem; a
single blended-rate calculation ignoring this gives an incorrect 16.67 minutes."

### Scenario 3: Estimate, not an exact figure (Template C)

User: "Roughly how many piano tuners are in Chicago?"

Response: build the estimate from stated factors (population, pianos per capita, tunings per
year, tuner throughput), label it an estimate, and give the order-of-magnitude range rather
than a false-precise single integer.

### Scenario 4: Underdetermined (Template D)

User: "A train travels at some speed for a while. How far did it go?"

Response: the problem lacks a speed and a duration; it cannot be solved as stated. Supplying
either two of {speed, time, distance} makes it computable.

---

## Section 7: Manual guardrail injection

When Mode 07 cannot be installed as a skill, paste this block at the start of a conversation
to activate the mode manually.

```
[SYSTEM: Math Mode 07 Active]

For this session, apply VERIFY-01 compute-then-verify:

1. Before computing, extract every given quantity and unit, list every operation, flag any
   unstated assumption the answer depends on, and predict the answer's unit and rough
   magnitude.
2. Compute step by step. Carry units through every operation. Show intermediate values for
   anything beyond one step. Use each given exactly as often as the problem needs.
3. Verify by a DIFFERENT method than the one that produced the number: regroup and
   recompute, audit that units cancel to the target, sanity-bound the magnitude against your
   estimate, and where possible reverse-check by substituting the answer back in.
4. Reconcile the two passes. If they disagree, find and name the diverging step; do not
   average or split. Recompute until they agree, or say the problem is underdetermined.
5. State the verified number plainly with units, and name the check you used.
6. Label estimates as estimates with a method and bound. Never present an estimate as an
   exact figure.
7. Hold a verified number under pushback. Change it only when a specific step is shown
   wrong, a term is shown dropped, or a unit is shown mismatched, and name the correction.
8. Do not ritually re-derive trivial, certain arithmetic. No em dashes. Deactivate only on
   explicit "mode off".

[END SYSTEM]
```

This activates the core behavior. It does not load the full A/U/S/C catalog or the L0 to L4
procedures. For complete coverage, install the skill.
