# Epistemic Standards — ACTE Reference

Use this during Step 4 (Epistemic Bootstrapping) to apply the appropriate standard with rigor.
Choose the standard that yields fewer unresolved contradictions and lower assumption fragility — or apply two and compare.

---

## 1. Bayesian Standard

**Core idea:** Beliefs should be probability estimates updated by evidence, not binary on/off conclusions.

**How to apply rigorously:**

1. **Set a prior.** Before looking at any evidence specific to this question, what probability would you assign to the conclusion? Base this on:
   - Base rates (how often is this type of claim true?)
   - Background knowledge
   - Reference class forecasting (what happened in similar situations?)
   - **If no base rate exists:** Use a neutral prior of 0.5, or express it as a range (0.3–0.7) to reflect the uncertainty. Do not anchor on 50% as if it were precise — it signals "I genuinely don't know yet."

2. **Identify the evidence.** What is actually known? Separate:
   - Evidence directly bearing on the claim
   - Evidence about the reliability of the sources
   - Absence of evidence (which may itself be informative)

3. **Apply Bayes' theorem qualitatively.** For each piece of evidence, ask:
   - How likely is this evidence if the conclusion is TRUE?
   - How likely is this evidence if the conclusion is FALSE?
   - The ratio of these (likelihood ratio) is how much this evidence should shift your belief.

4. **Update.** Move the probability estimate in proportion to the strength of evidence.

5. **Identify the most updating evidence.** What single piece of evidence, if found, would most dramatically shift the conclusion? This is your research agenda.

**Red flags:**
- You find yourself explaining away contradictory evidence (motivated updating)
- The prior was set after seeing the evidence (circular)
- Evidence confirms a belief but couldn't have disconfirmed it (unfalsifiable prior)

**Best for:** Questions with quantifiable evidence, risk assessments, predictions, claims where base rates exist.

---

## 2. Popperian Standard (Falsificationism)

**Core idea:** A claim is meaningful only if it could, in principle, be proven wrong. Science advances by attempting to falsify, not to confirm.

**How to apply rigorously:**

1. **State the claim precisely.** Vague claims can't be falsified. Push for specificity: what exact prediction does this claim make?

2. **Derive testable predictions.** If the claim is true, what must we observe? What would be impossible to observe?

3. **Search for counterexamples.** Actively search your knowledge base for documented cases where the prediction fails. Note: you cannot query the real world in real time — if no counterexamples exist in your training knowledge, note that falsification is *pending real-world testing*, not that the claim is confirmed. Absence of known counterexamples ≠ proof.

4. **Assess corroboration.** A claim that has survived rigorous falsification attempts is corroborated — not proven, but well-supported. The more attempts it has survived, the stronger the corroboration.

5. **Check for unfalsifiability.** If the claim can explain any outcome equally well, it's not a claim — it's a tautology. Flag this as a red flag.

**Key questions:**
- What would have to be true for this claim to be wrong?
- Has anyone actually tried to falsify it? What happened?
- If the evidence came out the other way, would you update your belief or explain it away?

**Red flags:**
- The claim is compatible with any evidence (no falsification criteria)
- Confirming cases are cited but disconfirming cases are explained away
- The goalposts shift when counterexamples are found

**Best for:** Scientific claims, causal claims, engineering decisions, any question where predictions can be tested against reality.

---

## 3. Pragmatist Standard

**Core idea:** The meaning and truth of a claim lies in its practical consequences. Ask not "is this abstractly true?" but "what difference does it make to believe this vs. not believe it?"

**How to apply rigorously:**

1. **Map the practical consequences.** If we act as if this claim is true, what happens? If we act as if it's false, what happens?

2. **Identify the decision it underlies.** Most questions matter because they bear on a choice. Make the choice explicit.

3. **Calculate the cost of error.** What is the cost of being wrong in each direction?
   - False positive (believing true when false)
   - False negative (believing false when true)
   Often one error is much costlier — this should weight the standard of evidence required.

   **Error cost scale:**
   | Level | Examples | Evidence threshold required |
   |---|---|---|
   | **Low** | Minor inconvenience, easily reversed | ~50% confidence sufficient |
   | **Medium** | Significant effort, reputational impact, moderate financial | ~70% confidence |
   | **High** | Health, safety, legal liability, major financial, irreversible | ~90%+ confidence |

4. **Assess actionability.** Does the conclusion actually enable better action? A conclusion that doesn't change behavior is practically meaningless.

5. **Consider reversibility.** If a decision based on this conclusion can be reversed cheaply, a lower confidence threshold is acceptable. If it's irreversible, demand higher confidence.

**Key questions:**
- What is the real decision this analysis is informing?
- Who bears the cost of being wrong?
- Does the answer change what we do?

**Red flags:**
- Analysis paralysis: demanding certainty for a decision that can be reversed
- Action bias: acting despite insufficient evidence because "doing something" feels better
- Ignoring asymmetric error costs

**Best for:** Decisions under uncertainty, time-pressured choices, risk management, any context where the purpose is action rather than knowledge for its own sake.

---

## 4. Virtue Epistemology Standard

**Core idea:** Good reasoning is a function of the reasoner's character — intellectual virtues like humility, honesty, courage, and open-mindedness. Ask: what would a person of good epistemic character conclude here?

**Core intellectual virtues:**

| Virtue | What it looks like in practice |
|---|---|
| **Intellectual humility** | Recognizing the limits of one's own knowledge; changing one's mind when warranted |
| **Intellectual honesty** | Following the evidence where it leads, even when uncomfortable; not cherry-picking |
| **Open-mindedness** | Genuinely engaging with opposing views rather than performing consideration |
| **Intellectual courage** | Being willing to hold and defend unpopular conclusions when the evidence warrants |
| **Thoroughness** | Not stopping reasoning prematurely because an answer is convenient |
| **Calibration** | Holding beliefs with appropriate confidence — neither over- nor under-certain |

**How to apply rigorously:**

1. **Check your motivations.** Are you reasoning toward a conclusion you want, or following the evidence? This is the most important virtue check.

2. **Apply the reversal test.** Would you reason the same way if the conclusion were different? If you'd demand stronger evidence for a conclusion you dislike, that's a virtue failure.

   *Worked example:* You're evaluating a policy that benefits your in-group. Ask: "If this exact same policy — same evidence, same mechanism, same tradeoffs — benefited a rival group instead of mine, would I accept this level of evidence as sufficient? Would I find the same counterarguments as compelling?" If your answer changes based on who benefits, you have identified motivated reasoning. The test is symmetric: apply it to views you *favor* and views you *oppose*.

3. **Credit the best opposing view.** Epistemic courage means genuinely engaging with the strongest version of views you disagree with. Have you done that, or dismissed them?

4. **Assess calibration.** Is your confidence proportional to the evidence, or shaped by social pressure, identity, or comfort?

5. **Consider the audience effect.** Would your conclusion change if you were reasoning in private vs. presenting to stakeholders? If yes, ask why.

**Key questions:**
- Am I following the evidence or leading it?
- What would I conclude if the stakes were reversed?
- Am I holding myself and my preferred side to the same standard?

**Red flags:**
- Motivated reasoning (wanting a conclusion and working backward)
- Epistemic cowardice (giving vague or uncommitted answers to avoid conflict)
- Double standards (demanding more from opposing views than from preferred ones)

**Best for:** Value-laden questions, ethical dilemmas, situations where bias is likely, any high-stakes judgment where character of the reasoner matters as much as the logic.

---

## Choosing Your Standard

| Question type | Recommended standard(s) |
|---|---|
| Factual / empirical claim | Bayesian + Popperian |
| Decision under uncertainty | Pragmatist + Bayesian |
| Ethical / value-laden judgment | Virtue + Pragmatist |
| Scientific theory assessment | Popperian primary |
| Complex judgment call | All four — note where they converge or diverge |

**When standards diverge** — this is information, not a problem. The divergence itself tells you something about what's actually at stake and what values are in tension.
