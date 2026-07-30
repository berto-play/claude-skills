# Authority Mismatch Protocol

## What triggers the 😵‍💫

The 😵‍💫 fires when a claim in the draft output meets any of these conditions:

1. It reads as a compliance finding but its only supporting atoms come from
   interview or meeting transcripts (experiential sources).

2. It states or implies that a regulatory requirement is met based on what
   a participant said, without a corresponding regulatory atom confirming
   the requirement exists and is met.

3. It presents a pattern from interview atoms as evidence of regulatory
   alignment without a regulatory atom being cited.

4. An atom trace is attempted and fails: no regulatory atom can be found
   to support the compliance claim.

This is a mechanical check. It does not rely on judgment alone.
If the atom trace fails, the flag fires.

---

## What happens when the 😵‍💫 fires

### 1. The claim is removed from its current output section immediately.

It does not remain as a finding with a caveat. It is removed entirely.

### 2. A visible 😵‍💫 block replaces it.

```
😵‍💫 AUTHORITY MISMATCH — Claim removed from findings.

What the draft said:
"[exact claim as drafted]"

Atom trace attempted:
→ [Interview atom ID, source, speaker]: supports the claim
→ No regulatory atom found that confirms this requirement
  exists or that the claim constitutes compliance.

Why this cannot stay as a finding:
Interview content is experiential data. It describes what
people do or believe. It cannot confirm regulatory compliance
without a corresponding regulatory source.

Where this claim goes:
→ Moved to: NEEDS HUMAN DECISION section.
→ Question for human reviewer: Does [regulatory document]
  confirm this requirement? If yes, please provide the
  document and I will rerun the analysis.
```

### 3. The claim is logged in the NEEDS HUMAN DECISION section.

With the exact question a human reviewer needs to answer
in order to resolve it.

### 4. The pre-delivery audit counts it.

Every 😵‍💫 caught is counted in the dashboard. The total
is visible to the user before they read the output.

---

## What the 😵‍💫 does NOT do

- It does not delete the underlying interview finding.
  The experiential content is preserved in the WHAT THE INTERVIEWS TELL US section.
  Only the compliance claim is removed.

- It does not guess at a resolution.
  It states the problem and routes it to human review.

- It does not fire on research synthesis findings in Mode 2.
  In Mode 2 there are no compliance claims. The 😵‍💫 is a
  compliance-specific trigger.

---

## Tone of the 😵‍💫 block

Calm. Directive. Not alarming. The flag is serious but the language
does not panic. It clearly explains what happened, why, and what to do next.

---

## Edge case: user asks to proceed anyway

If the user explicitly asks to include the flagged claim despite the mismatch:

```
I can include this in a clearly labelled UNVERIFIED CLAIMS section,
separate from all findings, with the 😵‍💫 flag and full atom trace
visible inline. It will not appear in any findings section.

Would you like me to do that?
```

Wait for confirmation. Never silently include a flagged claim in findings.
