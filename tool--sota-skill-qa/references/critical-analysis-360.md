# 360 Critical Analysis Protocol

Run after all mechanical evaluations (Tier 1, Tier 2, Adversarial) but before the Launch Gate. This is not a checklist — it is a structured thinking exercise. No scores. No pass/fail. The output is a written analysis that surfaces what metrics cannot catch.

The mechanical evaluations answer: "Does this skill meet the criteria?"
This protocol answers: "Should this skill exist in this form?"

---

## Contents

- Think-Prompt-Think Workflow
- Cognitive Toolkit
- Intellectual Dispositions
- Universal Standards
- The Six Perspectives
- The Seven Dimensions
- The Three Self-Challenge Loops
- First Principles Test
- Socratic Drill
- Output Format
- 360 CRITICAL ANALYSIS — [skill-name]
- When to Run

---

## Think-Prompt-Think Workflow

Every step of this protocol follows a three-phase cycle. Do not skip phases.

**Phase 1 — THINK FIRST.** Before evaluating anything, state your own position. What do you already believe about this skill's quality? What is your goal for this evaluation? Write it down. This prevents the evaluation process from manufacturing a conclusion you didn't start with.

**Phase 2 — PROMPT DELIBERATELY.** Now run the evaluation. At each perspective and dimension, actively argue the counter-position. Play devil's advocate against your Phase 1 belief. If you entered thinking the skill is strong, look for why it's weak. If you entered thinking it needs work, look for what's actually solid.

**Phase 3 — THINK AGAIN.** Vet every finding against your own experience and external knowledge. Ask:
- "What are the possible biases in my evaluation?"
- "What data or perspectives did I leave out?"
- "Does this finding actually address my original concern or just sound rigorous?"
- "Can I verify the most critical claim I'm making about this skill?"

If Phase 3 changes your Phase 1 position, state the change explicitly and why.

---

## Cognitive Toolkit

Apply these six cognitive skills throughout the analysis. They are not steps — they are lenses that operate continuously.

| Skill | How it applies to skill evaluation |
|---|---|
| **Analysis** | Break the skill into its constituent parts. Evaluate each independently before evaluating the whole. A skill can have excellent workflows and terrible metadata. |
| **Interpretation** | What does this skill actually mean in practice? Not what the author intended — what will the AI do when it reads these instructions? The gap between intent and interpretation is where failures hide. |
| **Inference** | What logical conclusions follow from this skill's design? If the routing table sends ambiguous inputs to Workflow 1, what downstream effects does that create? Trace the chain. |
| **Evaluation** | Judge the credibility and strength of each element. Is this example genuinely teaching the AI something, or is it decorative? Is this guardrail enforceable or aspirational? |
| **Explanation** | For every finding, state the reasoning. "This section is weak" is not a finding. "This section is weak because the instruction conflicts with the guardrail in Section 3, creating ambiguity about which takes precedence" is a finding. |
| **Self-Regulation** | Monitor your own evaluation process. Are you being thorough or rushing? Are you anchored to the Tier 1 score? Are you evaluating the skill or defending your earlier work on it? |

---

## Intellectual Dispositions

These are not optional attitudes — they are evaluation requirements.

**Open-mindedness.** The skill might be wrong in a way you haven't considered. The operator might use it differently than you imagine. The domain might have evolved since the references were written.

**Intellectual humility.** You may not have enough domain expertise to evaluate the content. State this explicitly when it applies. "I cannot verify whether this regulatory reference is current" is more valuable than a false positive.

**Healthy skepticism.** A skill that reads well may still perform poorly. A confident structure is not evidence of correct behavior. Question what looks right.

**Inquisitiveness.** If something in the skill seems unusual — a guardrail that's tighter than expected, a workflow step that seems redundant — investigate before dismissing. The unusual element might be the most important one, or it might be a mistake. Both are worth knowing.

**Objectivity.** If you built this skill, you are biased toward it. If someone else built it, you may be biased against it. Name the bias. Evaluate the skill, not its author.

---

## Universal Standards

Every finding in the 360 analysis must meet these standards. If a finding fails any standard, revise it before including it.

| Standard | Test |
|---|---|
| **Clarity** | Could someone who hasn't read this conversation understand this finding? |
| **Accuracy** | Is this finding factually correct? Have I verified it against the actual SKILL.md text? |
| **Precision** | Does this finding point to a specific section, line, or interaction — or is it vague? |
| **Relevance** | Does this finding matter for the skill's actual use, or is it theoretical? |
| **Logic** | Does the reasoning chain hold? Does the evidence support the conclusion? |

---

## The Six Perspectives

Evaluate the skill from each perspective. For each, run the Think-Prompt-Think cycle.

### 1. Operator Lens
*"Would I actually use this?"*

- Does this skill save time or create overhead?
- Is the intake proportional to the task?
- Does the output format match how I'd actually use it?
- What's the minimum viable invocation?
- After using this skill 50 times, what would I want changed?

### 2. AI Execution Lens
*"Can the AI follow these instructions without ambiguity?"*

- Are there conflicting directives? What wins when they collide?
- Is the routing table exhaustive? What happens with unmatched input?
- Are guardrail boundaries precise enough that the AI knows where tight ends and flexible begins?
- Are there implicit instructions the AI must infer but that are never stated?
- Could two sessions interpret the same SKILL.md and produce meaningfully different outputs?

### 3. Domain Expert Lens
*"Is this domain knowledge correct and complete?"*

- What would a domain expert disagree with?
- Are examples realistic to practitioners, or do they read like textbook exercises?
- What domain knowledge does the skill assume vs. what it provides?
- Is any content outdated, oversimplified, or misattributed?
- What's the most important domain element this skill does NOT cover — intentional or oversight?

### 4. Adversary Lens
*"Where are the seams?"*

- What's the easiest path to harmful, wrong, or misleading output?
- What's the most dangerous failure mode — confident, wrong output the operator would act on?
- Can guardrails be contradicted through carefully crafted input?
- What's the skill's single point of failure?

### 5. Maintainer Lens
*"Can someone who didn't build this update it in 6 months?"*

- Is the architecture legible on first encounter?
- Are reference file names self-explanatory?
- If domain knowledge changes, where exactly does the update go?
- Are there implicit dependencies between sections that would break under independent edits?
- How many files change to add a new workflow? If >2, architecture may be over-coupled.

### 6. System Lens
*"How does this affect the fleet?"*

- Does this skill create dependencies other skills rely on?
- Is there functional overlap with another skill that neither acknowledges?
- Does this skill's activation compete with another skill's triggers?
- Does this skill's domain knowledge contradict another skill's references?
- If 5 more skills were added, would this skill's boundaries still be clear?

---

## The Seven Dimensions

For each dimension, write one sentence stating the current state and one sentence stating the risk.

| Dimension | Question | Risk if weak |
|---|---|---|
| **Correctness** | Is each claim, workflow step, and example factually right? | Wrong output presented as correct. |
| **Completeness** | What's missing that should be here? | User hits undocumented edge case. |
| **Coherence** | Do all sections work toward the same goal without contradiction? | Inconsistent behavior across sessions. |
| **Clarity** | Is every instruction unambiguous? | AI interpretation varies unpredictably. |
| **Cost** | Is the token budget proportional to the value delivered? | Wasteful loading on every invocation. |
| **Currency** | Is the domain knowledge current? What expires first? | Outdated practices recommended. |
| **Composability** | Does this skill know its neighbors? Are handoffs explicit? | User stuck between skills or gets conflicting output. |

---

## The Three Self-Challenge Loops

### Loop 1: The Kill Argument
*"What is the strongest argument that this skill should NOT ship?"*

State it as if you believe it. Steel-man it — the strongest version, not a strawman. Then respond: does it hold, or can it be addressed? If it holds, the skill does not ship.

### Loop 2: The Hidden Assumption
*"What assumption am I making that I have not verified?"*

Name the assumption you're least confident about. State what would change if it were wrong. Then ask: "What if the opposite of this assumption is true?" — this is counterfactual reasoning. If the opposite is plausible, the assumption needs verification before launch.

### Loop 3: The Production Failure
*"If this skill fails in production in 30 days, what is the most likely cause?"*

Not the worst case. The most LIKELY cause. Name it. Check: does the Launch Gate catch it? If not, add a gate.

---

## First Principles Test

Before finalizing, ask: "If I started from scratch knowing what I know now, would I build this skill at all? Would it look like this?"

If the answer is "no, I'd build something different" — that's not a reason to scrap it (sunk cost is real), but it IS a finding that belongs in the Launch Card under Known Limitations.

---

## Socratic Drill

Pick the one element of the skill you're most confident about. Ask "why?" five times:

1. "This skill has 6 workflows." Why 6?
2. "Because there are 6 distinct input types." Why are those the 6?
3. "Because the operator uses them weekly." How do we know?
4. "Because the intake established it." Did the intake verify or assume?
5. "..." — either there's bedrock or the foundation is arbitrary.

If the drill reaches bedrock (a verified fact or a stated design decision), the element is sound. If it reaches "because it seemed right," that's a finding.

---

## Output Format

```
## 360 CRITICAL ANALYSIS — [skill-name]

### Phase 1: Initial Position
[What I believe about this skill before evaluating]

### Perspectives
1. Operator: [findings]
2. AI Execution: [findings]
3. Domain Expert: [findings]
4. Adversary: [findings]
5. Maintainer: [findings]
6. System: [findings]

### Dimensions
| Dimension | State | Risk |
|---|---|---|
| Correctness | [one sentence] | [one sentence] |
| ... | ... | ... |

### Self-Challenge
1. Kill Argument: [steel-manned argument + response]
2. Hidden Assumption: [assumption + counterfactual + consequence]
3. Production Failure: [most likely cause + gate check]

### First Principles: [Would I rebuild this? Why/why not?]
### Socratic Drill: [Element chosen + 5 whys + bedrock or gap]

### Phase 3: Final Position
[What I believe now. What changed. What didn't.]

### Verdict Modifier
Does this analysis change the mechanical verdict?
[YES — downgrade from X to Y because... / NO — mechanical verdict stands]

### Bias Disclosure
[What biases might have affected this evaluation? Builder bias? Anchoring to Tier 1 score? Domain unfamiliarity?]
```

---

## When to Run

- **Always** before Launch Gate for new or rebuilt skills
- **Recommended** for Post-Launch Review at 30+ days
- **Optional** for Fleet Benchmark (3 lowest-scoring skills only)
- **Never** for quick structural fixes or reference reorganization
- **Never** when mechanical evaluations already failed badly (<14 structural) — fix basics first
