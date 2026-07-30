# Citation Failure Taxonomy

Cold-storage catalog for `mode--05-citation-accuracy`. Every variant carries an ID, a detection cue,
and a counter-move. The hot path scans against all six families on every claim-citation
pair; this file is the lookup.

Construct space: a citation failure breaks one of two links, existence (does the source
exist and is it correctly described) or faithfulness (does the source actually say what
the claim asserts). Families F, M, I, and Q are existence-and-metadata failures. Family S
is the faithfulness failure, the harder one, because it survives a surface check. Family U
covers the access layer (URLs). Family C covers compound defects on a single source.

---

## Family F: Fabricated source

The source does not exist. The most visible failure and, per real-world incident data, the
one that has produced sanctioned court filings.

| ID | Variant | Detection cue | Counter-move |
|---|---|---|---|
| F1 | Wholesale fabrication | A title, author set, and venue that do not resolve to any real work when checked | Verify existence via search or a bibliographic tool before asserting; absence of a result is absence, not a retrieval failure to paper over |
| F2 | Invented venue or invented author | A real-sounding journal, conference, or author name attached to a work that does not exist under that description | Check the venue and author independently, not just the title; a plausible venue name is not evidence it published this |
| F3 | Plausible but untraceable title | A title styled exactly like real work in the field, invented to fit the claim | Treat "sounds like a real paper" as zero evidence; the check is resolution, not plausibility |

Research grounding: HALoGEN (arXiv 2501.08292, confirmed) classifies fabrication as Type C
error, distinct from Type A (misremembered training data) and Type B (wrong training data),
and evaluates roughly 150,000 generations across nine domains including scientific
attribution. The Mata v. Avianca sanction (S.D.N.Y. 2023, independently confirmed via
multiple press and legal sources) is a real-world instance of family F1: six fabricated
case citations submitted in a federal filing, $5,000 sanction.

---

## Family M: Misattribution

The source exists, but a metadata element is wrong. Easier to miss than F because the
citation "resolves" to something real, just not the right something.

| ID | Variant | Detection cue | Counter-move |
|---|---|---|---|
| M1 | Wrong author | A real paper's findings attributed to a different researcher or team | Check the author list against the actual source, not against who is well known to work in the area |
| M2 | Wrong title | The correct work, described with a title that belongs to a different paper (often a similar one by the same authors) | Verify title-to-content match, not just author-to-content match; authors publish multiple similar-sounding papers |
| M3 | Wrong venue or year | Correct paper, wrong journal, conference, or publication year stated | Check publication metadata independently; a preprint year and a conference-acceptance year often differ and both get misstated |
| M4 | Real work, wrong person entirely | A finding correctly described but attributed to an entirely different, unrelated author or team | Full-chain check: does this specific person/team's actual output contain this claim, not just does a similar claim exist somewhere |

Research grounding: AIS (arXiv 2112.12870, confirmed) frames attribution as requiring the
output be verifiable against an identified source, which fails immediately if the source
identity itself (author, venue) is wrong even when the underlying claim is correct.

---

## Family I: Identifier failure

The numeric or alphanumeric handle is wrong. Identifiers create a false sense of precision:
a DOI or arXiv ID reads as verifiable, so it gets less scrutiny than prose, which is
backwards.

| ID | Variant | Detection cue | Counter-move |
|---|---|---|---|
| I1 | Nonexistent DOI or arXiv ID | An identifier formatted correctly but resolving to nothing | Resolve the identifier, do not just check its format; a well-formed ID is not a real one |
| I2 | ID resolves to a different paper | The identifier is real but points to a different work than the one described around it | Cross-check that the resolved title/author matches the claimed title/author, not just that the ID resolves at all |
| I3 | Malformed or truncated ID presented as exact | A partial or garbled identifier presented with full confidence | Verify the complete identifier string; do not round a partial match into a confirmed one |

Research grounding: the confirmed anchors in this mode (Gao et al. ALCE, Rashkin et al.
AIS, Min et al. FActScore, RARR, Ravichander et al. HALoGEN) were each verified in this
build by resolving the stated arXiv ID and cross-checking title and author against
independent listings (arXiv abstract pages, ACL Anthology, GitHub repos, Hugging Face
paper pages), which is the I1/I2 counter-move applied to this document's own citations.

---

## Family Q: Quote fabrication

Text presented inside quotation marks that the source does not contain verbatim.

| ID | Variant | Detection cue | Counter-move |
|---|---|---|---|
| Q1 | Fabricated quote | Quotation marks around text the source never states, in whole or in part | Reproduce a quote only after checking it against the actual source text; if the source text is unavailable, do not quote, paraphrase instead and say so |
| Q2 | Paraphrase presented as verbatim | Close paraphrase given quotation marks, implying word-for-word accuracy it does not have | Quotation marks are reserved for confirmed verbatim text; everything else is reported as paraphrase, explicitly |

Research grounding: the 2026 Oregon federal sanction (independently confirmed via multiple
legal-industry sources) involved 8 invented quotations alongside 23 fabricated citations,
making Q1 a documented real-world failure mode with direct legal consequence, not a
theoretical concern.

---

## Family S: Source-does-not-support-claim (unfaithful attribution)

The source is real, correctly identified, and correctly described. The specific claim
attached to it is not what the source says. This is the faithfulness failure, and it is the
hardest to catch because everything about the citation "checks out" except the one thing
that matters.

| ID | Variant | Detection cue | Counter-move |
|---|---|---|---|
| S1 | Overstatement | The source shows a narrower, weaker, or conditional finding; the claim states it as general or unconditional | Read the source's actual scope and conditions, not just its conclusion sentence; state the claim at the source's actual scope |
| S2 | Contradiction | The source states the opposite of, or a materially different result than, the attached claim | Read for disconfirming detail, not just confirming detail; if the source disagrees with the claim, say so plainly, do not cite it anyway |
| S3 | Unrelated | The source is real and on-topic in a general sense, but does not address the specific claim at all | Check claim-to-source correspondence at the level of the specific assertion, not the general subject area |

Research grounding: FActScore (arXiv 2305.14251, confirmed) exists precisely because
long-form generations mix supported and unsupported atomic facts under a single citation,
and its human evaluation found ChatGPT biographies scored only 58% factual precision even
when sources were nominally present. RARR (arXiv 2210.08726, confirmed) was built to find
and fix exactly this gap: it retrofits attribution by checking whether existing text is
actually supported by retrieved evidence, then edits unsupported claims rather than
accepting the citation at face value. AIS's core test, "according to the source, the
response," is a faithfulness test, not merely an existence test.

---

## Family U: URL and access failure

The web-specific case of existence and faithfulness failure.

| ID | Variant | Detection cue | Counter-move |
|---|---|---|---|
| U1 | Stale or dead URL | A link presented as current that returns nothing, or points to content that has moved or been removed | Fetch the URL before asserting it is live and current; do not assume a URL format implies a working page |
| U2 | URL resolves to different content | The link works but does not contain the claimed material | Confirm the fetched content actually matches the claim, not just that the server returned 200 |

Research grounding: this family is the web analogue of I2 (identifier resolves to the
wrong target) and is covered by the same ALCE citation-quality framing (arXiv 2305.14627,
confirmed), which scores citation quality on whether cited passages actually support the
generated statement, independent of whether the link or ID format is well-formed.

---

## Compound failures (Family C)

When two or more citation defects stack on a single source (for example, a real source
cited under the wrong year, quoting text it does not contain, for a claim it does not
support), the defects compound rather than average out. A single verified element (the
source exists) does not offset an unverified one (the quote is fake).

**Detection:** any source with more than one flagged element across F, M, I, Q, S, U.

**Required handling:**
1. Check every element independently: existence, author, title, venue, year, identifier,
   quote text, faithfulness. A pass on one does not imply a pass on another.
2. Report each failing element by name rather than a single blended "this citation is bad."
3. A compound-failure source is never partially rehabilitated by asserting only its
   correct elements; the whole citation is corrected or labeled, not selectively repaired.

---

## The over-hedging mirror failure

The mirror of asserting fabricated or unfaithful citations is refusing to attribute
anything with confidence, or labeling every source "unverifiable" regardless of whether
verification actually succeeded. Two forms:

- **Blanket hedging:** attaching "unverified" or "cannot confirm" to a source that was, in
  fact, checked and confirmed, because hedging feels safer than a plain assertion.
- **Citation refusal:** declining to cite any source at all, even when a real, checkable,
  faithful one is available and would strengthen the answer, out of general caution about
  fabrication risk.

This mode is calibrated against both. A citation that passed both the existence check and
the faithfulness check is stated plainly, as fact, not hedged for appearances. Hedging that
does not track actual verification status is itself a miscalibration, not a safety margin.
The goal is accurate labeling, not maximum caution.

## Faithfulness-first note (scope boundary with plain fabrication)

Family S deserves separate weight because it is undercaught relative to F, M, I, and Q.
Existence checks (does this paper exist, is this the right author) are mechanically
verifiable and increasingly tool-assisted. Faithfulness checks (does this specific sentence
in the source support this specific claim in the output) require actually reading the
relevant passage, and are the step most often skipped once a source is confirmed real. A
citation practice that only checks existence and stops there will still produce
FActScore-style failures: technically-cited, still-unsupported claims.

## Direction of harm

Both directions are in scope. The common failure is under-verification: a citation is
asserted without checking either link, and the reader has no way to tell without
re-verifying it themselves, which defeats the purpose of citing at all. The mirror failure
is over-hedging: real, checkable, faithful sources buried under blanket caution until the
response carries no usable attribution. A mode that only prevented fabrication would push
output toward the second failure, so the calibration clause and the over-hedging entry are
load-bearing, not decorative.

## Source register

- ALCE: Gao, Yen, Yu, Chen. "Enabling Large Language Models to Generate Text with
  Citations." arXiv 2305.14627, EMNLP 2023. (Confirmed: first LLM citation-generation
  benchmark; datasets ASQA, QAMPARI, ELI5; automatic metrics for fluency, correctness, and
  citation quality, correlated with human judgment.)
- AIS: Rashkin, Nikolaev, Lamm, Aroyo, Collins, Das, Petrov, Tomar, Turc, Reitter.
  "Measuring Attribution in Natural Language Generation Models." arXiv 2112.12870.
  (Confirmed: Attributable to Identified Sources framework; "according to the source, the
  response" verifiability test; two-stage human annotation pipeline across four datasets.)
- FActScore: Min et al. "FActScore: Fine-grained Atomic Evaluation of Factual Precision in
  Long Form Text Generation." arXiv 2305.14251, EMNLP 2023. (Confirmed: atomic-fact
  decomposition; percentage of atomic facts supported by a reliable source; ChatGPT
  biography generations scored 58%; automated estimator under 2% error against humans.)
- RARR: "RARR: Researching and Revising What Language Models Say, Using Language Models."
  arXiv 2210.08726. (Confirmed: post-hoc attribution-finding and edit system; retrofits
  unsupported claims with retrieved evidence while preserving the original text.)
- HALoGEN: Ravichander et al. "HALoGEN: Fantastic LLM Hallucinations and Where to Find
  Them." arXiv 2501.08292, ACL 2025. (Confirmed: 10,923 prompts across nine domains
  including scientific attribution; atomic-unit verification against external knowledge
  sources; Type A/B/C error classification for incorrect recollection, incorrect training
  data, and fabrication.)
- Real-world legal grounding (confirmed via multiple independent press and legal-industry
  reports, not an academic paper): Mata v. Avianca, S.D.N.Y. 2023, six fabricated ChatGPT
  case citations, $5,000 sanction under Judge P. Kevin Castel; a May 2026 Oregon federal
  case, 23 fabricated citations plus 8 invented quotations, $110,000 combined sanction.
- UNCONFIRMED pending existence check this pass, named in search results but not
  independently verified against a primary source, treated as leads only: GhostCite,
  CiteCheck, AttributionBench.
