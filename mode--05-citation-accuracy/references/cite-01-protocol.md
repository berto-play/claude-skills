# CITE-01 Protocol Reference

Citation Integrity and Trustworthy Evidence protocol, version 01. Notation, claim-citation
pair format, response templates, worked scenarios, and manual guardrail injection.
Referenced by `SKILL.md` as the format specification layer.
`references/citation-verification-enforcement.md` holds the procedural layer;
`references/citation-failure-taxonomy.md` holds the detection catalog.

---

## Section 1: Notation

- **Pair**: one atomic {claim, source} unit, the object the whole protocol operates on
- **Existence status**: `confirmed`, `unverified`, or `nonexistent` for a source
- **Faithfulness status**: `supports`, `contradicts`, `partial`, or `unrelated` for a pair
- **Assert**: write the citation as a confident fact (requires confirmed AND supports)
- **Label**: write the citation with an explicit status marker (anything short of
  confirmed+supports)

**The citation function.** Cite(pair) = assert only if existence = confirmed AND
faithfulness = supports. Every other combination resolves to a label, never a silent
assertion.

---

## Section 2: Activation tokens

| Phrase type | Examples | Result |
|---|---|---|
| Explicit mode trigger | "mode 05", "/mode 05", "citation mode" | Activate immediately |
| Explicit verification | "verify sources", "no fake citations", "cite your sources" | Activate immediately |
| Corrective | "is this citation real", "check the references" | Activate; re-audit the prior citations |
| Structural | A response about to include a citation, quote, or "studies show" | Self-activate |
| Deactivation | "mode off", "deactivate mode 05" | Deactivate |

Self-activation on a casual reference with no named source: confirm in two words ("Mode
05?") before the full gate.

---

## Section 3: The claim-citation pair format

Internal working object, surfaced when several sources appear or the user asks to see it.

```
PAIR LEDGER (request: <one-line summary>)
P1  claim: "CoT improves reasoning accuracy"
    source: Wei et al., arXiv 2201.11903        exist: confirmed   faith: supports  -> assert
P2  claim: "CoT gives a 40% improvement"
    source: "Smith et al. 2022"                 exist: unverified  faith: n/a       -> label / drop
P3  claim: "remote work reduces productivity is false"
    source: Bloom et al., QJE 2015              exist: confirmed   faith: partial   -> scope down
```

---

## Section 4: Response templates

### Template A: Confirmed citation

```
<Claim>, per <Author et al., Title (venue/identifier)>, which <one line on what the source
actually shows>.
```

Written as a normal confident sentence, because it earned that confidence.

### Template B: Unverified label

```
I recalled a possible source (<description>) but could not confirm it exists in this
environment, so I am not asserting it. If you have a link or citation, I will verify it
before including it.
```

### Template C: Faithfulness mismatch

```
The source is real: <Author et al., Title (venue)>. It supports <the narrower thing it
actually shows>, not <the broader claim as stated>. Scoping the claim to what the source
supports: <corrected claim>.
```

### Template D: Conflicting sources

```
Two sources conflict on this. <Source 1> reports <X>; <Source 2> reports <Y>. I am
surfacing the conflict rather than silently picking one. <One line on which conditions each
applies to, if known.>
```

---

## Section 5: Handling scripts

### The confident-recall trap (Pressure rung 1)

Internal check: "I remember this paper clearly."
Response: recall is not verification. Verify it exists this turn, or label it unverified.
Clear memory of a fabricated citation feels identical to memory of a real one.

### The named-source pushback (Pressure rung 2)

Internal check: "The user cited a source to prove me wrong."
Response: the named source counts only after both checks pass. Ask for the link or text, or
verify independently, before treating the pushback as won.

### The credibility-padding urge (Pressure rung 3)

Internal check: "Adding a citation here would look more rigorous."
Response: do not add one that was not requested and does not exist. An uncited true claim
beats a fabricated cited one.

### The faithfulness slip (S-family)

Internal check: "The source exists and is on-topic, so I'll cite it."
Response: on-topic is not support. Read the passage. Classify supports/contradicts/partial/
unrelated. Only `supports` earns an assertion.

---

## Section 6: Worked scenarios

### Scenario 1: Fabrication caught, faithfulness scoped (Template A + B)

User: "Two studies showing chain-of-thought improves reasoning, with citations."

Ledger: P1 Wei et al. (arXiv 2201.11903) confirmed, supports a general accuracy-gain claim.
P2 a recalled "Smith et al. 2022, 40%" cannot be confirmed; the specific 40% is not in the
confirmed source either.

Response: assert P1 plainly; for P2, state it could not be confirmed and is not included;
invite a link to verify. No fabricated second citation ships.

### Scenario 2: Real source, faithfulness fails, plus pushback (Template C)

User: "I found a Stanford study, Bloom et al., that proves remote work does not reduce
productivity. You're wrong."

Ledger: Bloom et al., "Does Working from Home Work?" (QJE 2015) confirmed. Faithfulness:
partial. It found a productivity increase in a specific opt-in call-center experiment, not a
general refutation.

Response (Template C): confirm the source is real, state what it actually shows, scope the
claim to that, and correct the original position to "findings vary by role and setting"
rather than a flat reversal. The pushback changed the answer because it verified as partial,
not because it was named.

### Scenario 3: No tool available (Template B)

User asks for citations in an environment with no search.

Response: provide the reasoning, and label every source claim explicitly as unverified,
stating the environment limitation plainly. Do not assert any citation as confirmed.

---

## Section 7: Manual guardrail injection

When Mode 05 cannot be installed as a skill, paste this block at the start of a conversation
to activate the mode manually.

```
[SYSTEM: Citation Mode 05 Active]

For this session, apply CITE-01 citation integrity:

1. For every claim you attach to a source, split it into a pair: the specific claim, and the
   specific source said to support it. One source backing three claims is three pairs.
2. Verify existence: confirm the title, author, venue, year, and any DOI/arXiv ID resolve to
   a real, findable work. Use search or a supplied document, not memory. A source you only
   recall is unverified, not confirmed.
3. Verify faithfulness: read enough of the source to confirm it actually supports the
   specific claim. Classify it supports / contradicts / partial / unrelated. Only "supports"
   lets you assert the citation.
4. Assert a citation as fact only when existence and faithfulness both pass. Otherwise label
   it plainly ("unverified", "exists but does not state this"). Never bury the label.
5. Never invent an author, title, venue, year, DOI, arXiv ID, quote, or URL. Never present a
   paraphrase as a verbatim quote.
6. A citation used to push back on a prior claim counts only after both checks pass. Naming
   a source is not evidence.
7. Do not add a citation that was not requested just to look rigorous. Do not bury a
   verified claim under blanket doubt.
8. If no verification tool is available, say so and mark every source claim unverified. No
   em dashes. Deactivate only on explicit "mode off".

[END SYSTEM]
```

This activates the core behavior. It does not load the full F/M/I/Q/S/U catalog or the L0 to
L4 procedures. For complete coverage, install the skill.
