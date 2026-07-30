# Verification Block and Self-Critique Pass

These are Layers 5 and 6 of the hallucination minimisation stack.
They run after atomisation and mode execution, before the pre-delivery audit.
Both are visible. Neither can be skipped or run silently.

---

## Layer 5: Verification Block

After draft findings are produced, run a claim-by-claim verification pass
before showing anything to the user.

### How it works

Build a flat list of every atomic claim in the draft output.
Assign each an ID: C-01, C-02, C-03 and so on.

For each claim, locate the exact supporting atom and check:
- Does the subject or actor match exactly?
- Does the statement, obligation, or value match exactly?
- Are all conditions, hedges, and scope qualifiers preserved?
- Is this claim from a numeric value, threshold, or date?
  If yes: verify it a second time before it appears in output.
- Does this claim cross the pattern threshold required for its label?
  If it is called a confirmed pattern: are there 3 or more source atoms?
  If it is called an emerging signal: are there exactly 2?

Assign one of three support labels:

**SUPPORTED:** Fully traceable to a named atom with a named source.
Keep. Attach the atom ID and source citation.

**PARTIAL:** Some parts supported, others not.
Rewrite to match only what is supported.
Move unsupported parts to the NEEDS HUMAN DECISION section.

**UNSUPPORTED:** Cannot be traced to any atom.
Remove from findings entirely.
Add to NEEDS HUMAN DECISION with a note explaining what was removed and why.

### Verification block format

Output this block before presenting final findings.
A claim with no entry in this block may not appear in output.

Use the header CLAIM VERIFICATION and plain rows, not XML tags.
The output should be readable, not technical.

```
─────────────────────────────────────────────
CLAIM VERIFICATION
─────────────────────────────────────────────
C-01: [claim text] — SUPPORTED — [atom ID, source citation]
C-02: [claim text] — PARTIAL — [what is supported / what was removed]
C-03: [claim text] — UNSUPPORTED — removed, see NEEDS HUMAN DECISION
```

This block is shown to the user. It is not hidden processing.
It is part of the output, placed before the findings sections.

---

## Layer 6: Self-Critique Pass

After the verification block, run a mandatory self-critique before delivering
final output. This step is visible. It cannot be performed silently.

### The six questions

Use the header HONESTY CHECK. Plain numbered format, no XML tags.

```
─────────────────────────────────────────────
HONESTY CHECK
─────────────────────────────────────────────

1. Strongest argument this analysis is incomplete or misleading:
   [State it honestly. Do not minimise it.]

2. Did the question shape what I found?
   (Confirmation bias check.)
   [Yes / No / Explain]

3. Did I use outside knowledge to fill a gap in the provided material?
   (Training knowledge contamination check.)
   [Yes / No / Explain]

4. Did I connect two facts the source never connected?
   (Logical inference check.)
   [Yes / No / Explain]

5. Did I treat "not mentioned here" as "does not exist anywhere"?
   (The material not mentioning X does not mean X is not required elsewhere.)
   [Yes / No / Explain]

6. Did this check change any finding or confidence level?
   [Yes — state what changed and why / No]
```

### What happens when self-critique changes a finding

The changed version appears in the final output.
The original version is not shown.
The change is declared explicitly:

"Self-critique revised this finding: [original] → [revised]. Reason: [reason]."

The skill does not silently adjust. Every change is visible and explained.

### What the self-critique does NOT do

It does not re-run the full analysis.
It does not add new findings.
It checks the findings already produced for the six failure modes above.
If none apply: state "No issues identified" for each question and proceed.

---

## When these layers run

In Mode 1 and Mode 2: verification block and self-critique run once,
after the mode completes and before the pre-delivery audit.

In Mode 3: they run after each mode separately. After the first mode,
they run before the blind spot audit. After the second mode, they run
before the pre-delivery audit. The numbered order below is the authority.

Order in Mode 3:
1. First mode completes
2. Verification block for first mode
3. Self-critique for first mode
4. Blind spot audit
5. Second mode runs on raw atoms
6. Verification block for second mode
7. Self-critique for second mode
8. Pre-delivery audit
9. Output delivered
