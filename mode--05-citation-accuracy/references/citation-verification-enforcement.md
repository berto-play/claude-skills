# Citation-Verification Enforcement (L0 to L4)

The procedural layer for Mode 05, behind the CITE-01 sequence in `SKILL.md`.
`references/citation-failure-taxonomy.md` holds the detection catalog;
`references/cite-01-protocol.md` holds notation and templates.

The organizing idea: every citation gets two independent checks, existence and faithfulness,
run as distinct passes. Existence is necessary but not sufficient. A real source cited for a
claim it does not support (family S) passes every surface check and is the harder failure.

---

## L0: Extract claim-citation pairs

**Goal:** every point where a claim is attached to a source is decomposed into an atomic
pair.

Procedure:

1. Scan the draft for every citation, named study, quote, URL, and identifier.
2. Decompose each into a pair: {the specific claim, the specific source asserted to support
   it}. A source backing three claims is three pairs, not one.
3. A quote is a pair whose faithfulness check is verbatim-match, not just topical support.

---

## L1: Verify existence

**Goal:** confirm each source is a real, findable work.

Procedure:

1. For each source, check that its title, author, venue, year, and identifier resolve to a
   real work. Use available tools (search, fetch, a user-supplied document) rather than
   recall.
2. A source recalled from training but not checked this turn is `unverified`, not
   `confirmed`. Confidence of recall is not verification.
3. Check identifiers specifically: a DOI or arXiv ID must resolve to the claimed paper, not
   merely be well-formed. A well-formed ID that resolves to a different paper is an I-family
   failure.

---

## L2: Verify faithfulness

**Goal:** confirm each source actually supports the specific claim attached to it.

Procedure:

1. Read enough of the source to check the claim against it, not just the title or abstract.
2. Classify the result:
   - **Supports:** the source states or directly backs the claim.
   - **Contradicts:** the source states the opposite.
   - **Partial:** the source supports a narrower version; the claim overstates it.
   - **Unrelated:** the source is real but does not address this claim.
3. Only `supports` permits assertion as a confident citation. The other three force a label
   or a correction.

---

## L3: Generate under the cite-or-label rule

**Goal:** every pair is either asserted (existence AND faithfulness both pass) or explicitly
labeled.

Procedure:

1. Assert a citation as fact only when both checks pass; write it as a normal confident
   sentence.
2. Label every unverified pair inline or immediately adjacent: "unverified", "could not
   confirm this supports the claim", "the source exists but does not state this". Never bury
   the label in a footnote.
3. Do not manufacture a citation for a claim that did not request one. Adding a source to
   look rigorous is itself a fabrication risk.

---

## L4: Reconcile and persist

**Goal:** no mismatch ships silently, and verification state carries across turns.

Procedure:

1. Before sending, walk every pair against its L1/L2 result. Any mismatch forces a
   correction or a label, never silent omission.
2. Persist verification state: a source checked earlier is not re-asserted from memory on a
   later turn if the claim has changed. A source verified for one claim is not automatically
   verified for a different claim from the same document (re-run L2 for the new claim).

---

## The existence-check method

In order of strength:

1. **User-supplied text:** the user pasted or uploaded the source. Strongest; check directly.
2. **Tool fetch:** search or retrieval resolves the exact work and its metadata.
3. **Identifier resolution:** the DOI/arXiv ID loads the claimed paper.
4. **No tool available:** existence cannot be confirmed here. Label `unverified`; do not
   assert.

When a check runs and actively shows the source does not exist (an arXiv/DOI that resolves
to nothing, a search that returns no such title or author), classify it `nonexistent`: this
is a fabrication (F-family). Drop the citation and, if a claim depended on it, report that
the source could not be found rather than substituting another.

Recall alone never reaches "confirmed". It reaches, at most, "candidate to verify".

---

## The faithfulness-check method (the four-way classification)

For each existing source, after reading the relevant passage:

| Result | Meaning | Output treatment |
|---|---|---|
| Supports | source backs the claim as stated | assert the citation plainly |
| Contradicts | source states the opposite | correct the claim; report the contradiction |
| Partial | source supports a narrower version | scope the claim down to what the source shows |
| Unrelated | source real but off-point | drop the citation; find a real one or label the gap |

The **Faithfulness-first rule**: existence passing does not end the check. The S-family
failure (real source, wrong claim) is caught only here.

---

## Self-review script (run before send)

1. Is every claim-citation pair extracted, none skipped?
2. Is every source's existence checked this turn, not asserted from recall?
3. Is every source's faithfulness to its specific claim checked, not just its existence?
4. Is every quote checked verbatim against source text?
5. Does every identifier resolve to the claimed work?
6. Is every unverifiable source labeled explicitly?
7. Is any claim buried under blanket hedging where verification actually succeeded
   (over-hedging check)?
8. Did any pushback citation pass both checks before it changed the answer?

Any failure forces a correction or a label.

---

## Pressure-translation table (citation-pressure rule)

| Pressure | What it is | Enforcement response |
|---|---|---|
| A confidently recalled but unchecked source | recall, not verification | Verify or label unverified before asserting |
| "I found a study, Bloom et al., you're wrong" | named-source pushback | Counts only after both checks; ask for link/text or verify independently |
| "A citation would make this look credible" | rhetorical pull | Do not add one; an uncited true claim beats a fabricated cited one |
| No verification tool available | environment limit | Label the limitation; mark every source claim unverified |

The **Cite-or-Label Rule**: nothing is asserted as a real, supporting citation until it has
passed existence and faithfulness. Naming a source is not evidence; a verified source is.

---

## Session persistence

Maintain a lightweight registry:

- Sources verified this session, with which specific claim each was verified for.
- Sources labeled unverified and why.
- Any faithfulness mismatch found (source exists, claim overstated), so a later turn does not
  silently re-assert the overstated version.

The convergent limit (per the SKILL.md research base and bias check): prompt-level discipline
reduces fabrication and raises faithfulness catch-rate, but real verification requires
retrieval or a supplied source. Without a tool, the honest ceiling is explicit labeling, not
confirmation. This mode is doubly bound: its own citations about citation research must be
impeccable, or it is self-refuting.
