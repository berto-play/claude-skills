# Hallucination Taxonomy & Validation Gates

Distilled from the Manus AI Guardrails Directive v2.0.0 and Microsoft Azure AI best practices. Use this when building or auditing safety-critical skills that handle document extraction, regulatory content, or factual claims.

---

## Violation Types

| Code | Type | What It Is | Example |
|---|---|---|---|
| V-01 | Factual hallucination | Generating a fact that is incorrect or invented | Stating a person holds a position they do not hold |
| V-02 | Temporal hallucination | Presenting outdated info as current, or fabricating dates | Citing a repealed law as still in force |
| V-03 | Contextual hallucination | Adding concepts or implications not present in the source | Summarizing a neutral document as having a negative conclusion |
| V-04 | Extrinsic hallucination | Generating information unsupported by provided source documents | Citing a statistic that appears nowhere in the data |
| V-05 | Intrinsic contradiction | Output contradicts the source or itself | Stating a value as both 10% and 15% in the same output |
| V-06 | Unsupported inference | Drawing a conclusion from premises not established in the source | Concluding profitability from revenue growth when profitability isn't addressed |
| V-07 | Intent fabrication | Assuming or stating intent not explicitly stated | "The author clearly intended to argue that..." when no intent is stated |
| V-08 | Gap filling | Generating plausible data to complete an incomplete dataset | Estimating a missing quarterly figure from surrounding data |
| V-09 | Scope creep | Introducing analysis or recommendations beyond the request | Providing investment advice when asked only to summarize |
| V-10 | Confidence inflation | Presenting uncertain information with false certainty | Stating "X is the case" when the source says "X may be the case" |
| V-11 | Source fabrication | Inventing citations, URLs, paper titles, or author names | Generating a fake academic citation to support a claim |
| V-12 | Semantic drift | Gradually shifting a term's meaning across a long output | Using "revenue" and "profit" interchangeably when the source distinguishes them |
| V-13 | Omission-induced misalignment | Selectively omitting key qualifiers or caveats | Dropping "under certain conditions" from a conditional statement |

---

## Severity Classification

| Severity | Violation Codes | Risk | Required Action |
|---|---|---|---|
| **Critical** | V-01, V-02, V-04, V-07, V-11 | Direct fabrication. High real-world harm. | Full retraction. Explicit user notification. |
| **High** | V-03, V-05, V-06, V-10 | Contextual distortion or unsupported inference. | Retract affected section. Flag for review. |
| **Medium** | V-08, V-09, V-12, V-13 | Gap filling, scope creep, or omission. | Remove offending content. Annotate gaps. |

---

## Pre-Delivery Validation Gates

Run before delivering any output from a safety-critical skill. All gates must pass.

| Gate | Question | Required |
|---|---|---|
| G-01 | Is every factual claim traceable to the provided source? | YES |
| G-02 | Does the output contain pre-trained knowledge presented as fact? | NO |
| G-03 | Does the output contain unsupported inference or logical leaps? | NO |
| G-04 | Does the output fill data gaps with estimates? | NO |
| G-05 | Does the output present conditional statements as definitive? | NO |
| G-06 | Does the output exceed the explicit scope of the request? | NO |
| G-07 | Does the output contain fabricated citations or statistics? | NO |
| G-08 | Has any qualifier or caveat from the source been omitted? | NO |
| G-09 | Is the output internally consistent (no contradictions)? | YES |
| G-10 | Has the request been answered using only the provided source? | YES |

If any gate fails: remove the offending content. If removal leaves the question unanswerable, issue an explicit inability statement instead of guessing.

---

## Edge-Case Response Patterns

Use these when a safety-critical skill encounters situations where normal output would risk hallucination.

**Insufficient source data:**
State what specific data is missing and what source would be needed. Do not attempt to answer from general knowledge.

**Ambiguous request:**
State the ambiguity. Ask one targeted clarification question. Do not pick an interpretation and proceed.

**Conflicting sources:**
State both claims with their sources. Do not silently resolve by picking the "most likely" one. Ask which source should be treated as authoritative.

**Partial answer:**
Provide what the source supports. Explicitly state what parts of the question cannot be answered and why.

---

## When to Apply This Taxonomy

Not every skill needs these gates. Apply them to skills where outputs affect:
- Legal or regulatory compliance
- Medical or clinical decisions
- Financial data or reporting
- Document extraction presented as factual
- Any output the operator will act on without independent verification

For strategy, brainstorming, or communication-framing skills, the validation gates are unnecessary overhead. Use the skill's own degrees-of-freedom calibration instead.
