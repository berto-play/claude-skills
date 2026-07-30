# Atomisation Protocol

Every source is broken into atomic claims before any synthesis begins.
An atomic claim is the smallest independently verifiable unit of meaning.

---

## What an atom is

One subject. One statement. One source tag.

If a sentence contains two facts, it produces two atoms.
If a sentence contains a hedge, the hedge produces its own atom.
If a speaker infers something they did not directly observe, the inference
is a separate atom labelled as such.

---

## Atom format

```
[A-001]
Source: [filename, page/section if available]
Speaker: [name or role as identified in transcript]
Type: STATEMENT / HEDGE / INFERENCE / OPERATIVE-TEXT
Original language: [exact words, in source language]
```

---

## Atom types

**STATEMENT:** Something directly said or written as fact by the speaker or document.

**HEDGE:** A qualifier attached to a statement. Extracted separately so confidence
level is never inflated. Examples: "I think", "usually", "in most cases", "we try to",
"it seems like", "patients appear to".

**INFERENCE:** The speaker draws a conclusion about something they did not directly
observe or measure. Example: "patients seem satisfied" produces an inference atom
because satisfaction is being inferred, not reported as measured.

**OPERATIVE-TEXT:** A term, clause, threshold, deadline, eligibility criterion, or
classification that carries legal or regulatory weight. These atoms are never
paraphrased. They are quoted verbatim in the original language and flagged for
verbatim reproduction in output.

---

## Example atomisation

Raw passage from an interview:
"We usually complete the teleconsultation within 20 minutes and patients seem satisfied."

Atoms produced:

```
[A-001]
Source: [Transcript file, Speaker role]
Type: HEDGE
Original: "usually"
Note: Duration claim is qualified. Not always true.

[A-002]
Source: [Transcript file, Speaker role]
Type: STATEMENT
Original: "complete the teleconsultation within 20 minutes"
Note: Approximate duration, subject to A-001 hedge.

[A-003]
Source: [Transcript file, Speaker role]
Type: INFERENCE
Original: "patients seem satisfied"
Note: Satisfaction is inferred by the speaker, not measured or reported by patients.
```

Atoms NOT produced from this passage:
- No atom claiming compliance with any time standard
- No atom claiming measured patient satisfaction rates
- No generalisation to other speakers or sessions

---

## Regulatory document atomisation

Regulatory documents follow the same protocol with one addition:
every atom includes the article, section, or clause reference exactly
as it appears in the document.

```
[R-001]
Source: [Document name, Article/Section reference]
Type: OPERATIVE-TEXT
Original: [Exact text in original language]
Translation: [Translation if needed, clearly labelled as translation]
Authority level: [e.g. Agenas Linea Guida]
```

---

## Separation rule

Interview atoms and regulatory atoms are kept in separate pools
throughout the atomisation pass. They are never merged at this stage.

Cross-referencing between interview atoms and regulatory atoms happens
only in Step 4 (Mode 1) and only under the authority mismatch detection rules.

Cross-referencing between interview atoms (to identify patterns) happens
in Mode 2. These are two different operations. Mode 2 never cross-references
interview atoms against regulatory atoms.

---

## What the atomisation pass catches

- Confidence inflation: a hedged claim that might otherwise be presented as certain
- Inference creep: a speaker's assumption presented as an observed fact
- Generalisation from a single source before the pattern threshold is checked
- Operative text that would otherwise be paraphrased and lose legal precision
