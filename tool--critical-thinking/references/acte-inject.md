# ACTE Inject — Compressed Critical Thinking Module

This is the injectable version of the Advanced Critical Thinking Engine (ACTE v1.1).
Use it when you need a critical thinking checkpoint without running the full 9-step engine.
It runs in 4 phases and annotates whatever artifact is in scope.

---

## How to Use This Reference

When your skill reaches a review checkpoint:

1. Identify the **current artifact** (plan, draft, argument, code, decision, etc.)
2. Run the 4 phases below against it
3. Annotate the artifact with findings using `[ACTE-FLAG: ...]` markers
4. Evaluate the gate — PASS means continue; HOLD means surface the issue before proceeding

---

## Phase 1 — Assumption Scan

List every assumption embedded in the current artifact. For each:
- **Fragility**: 1 (rock solid) → 5 (purely speculative or contradicted by evidence)
- **CRITICAL**: flag if the artifact collapses if this assumption is false

Focus on CRITICAL assumptions. What happens if each is wrong?

---

## Phase 2 — Adversarial Strike (2 rounds max)

**Round 1**: State the strongest argument *against* the artifact's core claim or approach. Where is the reasoning weakest?

**Round 2**: Take the most vulnerable point from Round 1 and press harder. Can the artifact survive this attack?

For each attack: either **repair** (if it exposed a real flaw) or **confirm resilient** (if it doesn't land).

> **If a Round 1 or Round 2 attack is fatal and cannot be repaired:** Stop immediately. Output HOLD without proceeding to Phase 3. The artifact is not ready to continue.

Common attack vectors: false assumptions, missing alternatives, logical fallacies (false dichotomy, hasty generalization, post hoc, circular reasoning, motivated reasoning, appeal to emotion), unstated biases, unconsidered edge cases.

---

## Phase 3 — Mini Confidence Tensor

Score three dimensions (0–100):

| Dimension | What to score | Formula |
|---|---|---|
| **Logical soundness** | Surviving fallacies or contradictions? 100 = none. | 0 fallacies → 100. 1–2 → 70. 3+ → 40. |
| **Assumption stability** | How fragile are the CRITICAL assumptions? | `100 − (avg_fragility × 20)`. Avg fragility 1 → 80. Avg 2 → 60. Avg 3 → 40. |
| **Adversarial resilience** | Fraction of attacks in Phase 2 successfully repelled. | All repelled → 100. One fatal unrepaired → below 60. |

**Mini-overall** = average of the three. Label: *Weak* (<60) / *Acceptable* (60–79) / *Strong* (≥80)

---

## Phase 4 — Gate

**HOLD** if any of:
- Any mini-tensor dimension < 60
- Any CRITICAL assumption has fragility ≥ 4 and is **unchecked** — meaning it has not been directly challenged or evaluated against available evidence in the reasoning above
- A Phase 2 attack is fatal and unrepaired (this should already have triggered HOLD in Phase 2)

**PASS** otherwise — continue the parent workflow.

**Escalate to full 9-step ACTE if:**
- Any HOLD condition persists after one repair attempt
- Stakeholder impact is high (health, safety, legal, financial, or ethical consequences)
- Contradictions multiply across multiple assumptions

---

## Output Format

Annotate the artifact inline:
```
[ACTE-FLAG: <issue> — <suggested fix or question to resolve>]
```

Then append a mini-summary block:

```
ACTE Checkpoint:
  Gate: PASS | HOLD
  Mini-Tensor: soundness=X, assumption_stability=X, adversarial_resilience=X, overall=X (Weak/Acceptable/Strong)
  Key findings: [list, or "none — artifact is sound"]
  Action: [what to fix before continuing, or "proceed"]
```

---

## When to Inject

| Inject at... | Run phases... | Focus on... |
|---|---|---|
| Beginning (pre-task) | 1 only | Assumption scan of the problem definition |
| Middle (checkpoint) | 1 + 2 | Internal consistency of work-in-progress |
| End (pre-delivery) | 1 + 2 + 3 + 4 | Full quality gate before output ships |

At the end, if stakes are high or HOLD conditions appear, escalate to the full 9-step ACTE engine.
