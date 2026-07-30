# Output Format Guide

The output must be readable by a human who is not a compliance expert.
Rigour lives in the engine. The surface is calm, clear, and scannable.

---

## General rules

- Plain sentence findings, not bullet soup
- One idea per bullet when bullets are used
- Source tags are small and inline, not footnotes
- Headers describe what the section contains in plain language
- No bureaucratic section codes visible to the user
- Flags are calm and directive, not alarming
- Every finding has a source tag the user can trace

---

## Source tag format

```
↳ [Source file, Speaker/Section] [Transcript 2, Speaker A]
↳ Said by 3 participants [T1, Speaker A] [T3, Speaker B] [T5, Speaker C]
↳ Regulatory source: [Document name, Article/Section]
```

Small. Inline. Always present. Never hidden.

---

## Mode 1 output: Compliance Validation

The verification block and self-critique pass appear first, before any
content sections. They are part of the visible output, not hidden processing.

─────────────────────────────────────────────
CLAIM VERIFICATION
─────────────────────────────────────────────

C-01: [claim text] — SUPPORTED — [atom ID, source]
C-02: [claim text] — PARTIAL — [what is supported / what was removed]
C-03: [claim text] — UNSUPPORTED — removed, see NEEDS HUMAN DECISION

─────────────────────────────────────────────
HONESTY CHECK
─────────────────────────────────────────────

1. Strongest argument this analysis is incomplete or misleading: [answer]
2. Did the question shape what I found? (Confirmation bias): [Yes / No / Explain]
3. Did I use outside knowledge to fill a gap? (Contamination): [Yes / No / Explain]
4. Did I connect two facts the source never connected? (Inference): [Yes / No / Explain]
5. Did I treat "not mentioned" as "does not exist"? (Absence check): [Yes / No / Explain]
6. Did this check change any finding?: [Yes — what changed and why / No]

─────────────────────────────────────────────
WHAT THE DOCUMENTS REQUIRE
─────────────────────────────────────────────

Each requirement extracted verbatim from regulatory source.
Original language preserved. Translation offered below if needed.

"[Exact operative text]"
↳ [Document name, Article/Section]
Translation: [if applicable]

─────────────────────────────────────────────
WHAT THE INTERVIEWS TELL US
─────────────────────────────────────────────

What practitioners and users describe in practice.
These are experiential findings, not compliance evidence.

[Finding in plain language, hedges preserved]
↳ [Source, Speaker] [Source, Speaker]
Note: [Any hedge or inference preserved here]

─────────────────────────────────────────────
WHERE THINGS LINE UP
─────────────────────────────────────────────

Points where interview content is consistent with
what the regulatory documents require.

[Plain language description of alignment]
↳ Interview source: [file, speaker]
↳ Regulatory source: [document, article]

─────────────────────────────────────────────
WHERE THINGS DO NOT LINE UP
─────────────────────────────────────────────

Requirements in the documents that interview content
does not address, contradicts, or leaves unclear.

[Plain language description of gap]
↳ Required by: [document, article]
↳ Not addressed in: [which transcripts]
→ FLAGGED FOR HUMAN REVIEW

─────────────────────────────────────────────
NEEDS HUMAN DECISION
─────────────────────────────────────────────

These items cannot be resolved by analysis alone.
A qualified reviewer needs to answer the specific
question listed under each item.

ITEM 1:
[Plain description of the unresolved point]
Question for reviewer: [Specific question]
Relevant sources: [document or transcript references]

[😵‍💫 items appear here with full atom trace visible]

─────────────────────────────────────────────

---

## Mode 2 output: Research Synthesis

Same as Mode 1: verification block and self-critique appear first,
before any content sections.

─────────────────────────────────────────────
CLAIM VERIFICATION
─────────────────────────────────────────────

C-01: [claim text] — SUPPORTED — [atom ID, source transcript]
C-02: [claim text] — PARTIAL — [what is supported / what was removed]
C-03: [claim text] — UNSUPPORTED — removed, see REMOVED CLAIMS below

Note: for Mode 2, pattern threshold is checked here.
A claim labelled CONFIRMED PATTERN must have 3 or more atom citations.
A claim labelled EMERGING SIGNAL must have exactly 2.
Any claim below threshold is downgraded or removed before output.

─────────────────────────────────────────────
REMOVED CLAIMS
─────────────────────────────────────────────

Claims that could not be traced to a source atom, or that did not meet
the pattern threshold required for their label. Listed here so the user
can see what was excluded and why. Not the same as open follow-ups.

[Removed claim]: [reason — no atom found / threshold not met]

─────────────────────────────────────────────
HONESTY CHECK
─────────────────────────────────────────────

1. Strongest argument this analysis is incomplete or misleading: [answer]
2. Did the question shape what I found? (Confirmation bias): [Yes / No / Explain]
3. Did I use outside knowledge to fill a gap? (Contamination): [Yes / No / Explain]
4. Did I connect two facts the source never connected? (Inference): [Yes / No / Explain]
5. Did I treat "not mentioned" as "does not exist"? (Absence check): [Yes / No / Explain]
6. Did this check change any finding?: [Yes — what changed and why / No]

─────────────────────────────────────────────
WHO WAS INTERVIEWED
─────────────────────────────────────────────

[N] transcripts processed.
[Date range if determinable]
[Speaker roles as identified in transcripts]
[Any attribution uncertainty flagged here]

─────────────────────────────────────────────
CONFIRMED PATTERNS (found in 3 or more sources)
─────────────────────────────────────────────

[Pattern in plain language]
↳ Said by [N] participants across [N] transcripts
  [T1, Speaker A] [T3, Speaker B] [T5, Speaker C]

─────────────────────────────────────────────
EMERGING SIGNALS (found in 2 sources)
─────────────────────────────────────────────

Not yet a pattern. Worth watching.

[Signal in plain language]
↳ [T2, Speaker A] [T4, Speaker B]

─────────────────────────────────────────────
ISOLATED FINDINGS (found in 1 source only)
─────────────────────────────────────────────

These come from a single participant. They are not
patterns and are not treated as representative.

[Finding in plain language]
↳ [T1, Speaker A]

─────────────────────────────────────────────
CONTRADICTIONS AND UNRESOLVED DEBATES
─────────────────────────────────────────────

Points where participants said different things,
or where a meeting debate was not resolved.
Presented without resolution or judgment.

[Topic]
Position A: [what was said] ↳ [T1, Speaker A]
Position B: [what was said] ↳ [T3, Speaker B]
Status: Unresolved. No assumption made.

─────────────────────────────────────────────
OPEN FOLLOW-UPS
─────────────────────────────────────────────

Questions raised in interviews or meetings that
were not answered. Unclear statements that need
clarification. Nothing assumed.

[Follow-up item]
↳ Raised by: [source]
↳ Unresolved as of: [transcript date if known]

─────────────────────────────────────────────

---

## Mode 3 output

The order depends on what the user chose at mode selection.

**Order A (Research Synthesis first):**

First mode output: full Mode 2 section structure above.

Then:

══════════════════════════════════════════════
BLIND SPOT AUDIT — between modes
══════════════════════════════════════════════
[Blind spot audit results: findings from Mode 2 checked
for confidence drift, threshold violations, and any
interview content that drifted toward compliance language]
══════════════════════════════════════════════

Second mode output: full Mode 1 section structure above.

**Order B (Compliance Validation first):**

First mode output: full Mode 1 section structure above.

Then:

══════════════════════════════════════════════
BLIND SPOT AUDIT — between modes
══════════════════════════════════════════════
[Blind spot audit results: findings from Mode 1 checked
for confidence drift, unsupported claims, and any
regulatory framing that might contaminate Mode 2]
══════════════════════════════════════════════

Second mode output: full Mode 2 section structure above.

In both orders:
- A clear visual break separates the two mode outputs
- Findings from the first mode are never automatically
  carried into the second mode as evidence
- The user connects findings across modes if needed
- Each mode section includes its own verification block
  and self-critique before its findings are shown

---

## Tone guide

Think of a careful, experienced researcher presenting findings to a
clinical team. The science is solid. The language is clear. Nothing
is overstated. Uncertainty is named without apology.

Not:
"It was evidenced across multiple data points that the
teleconsultation workflow exhibits compliance-adjacent characteristics."

But:
"Three participants described the teleconsultation taking around
20 minutes. No document in this session sets a time requirement,
so this cannot be treated as a compliance finding."

Plain. Honest. Traceable.
