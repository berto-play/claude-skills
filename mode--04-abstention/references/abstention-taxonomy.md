# Abstention Failure Taxonomy

Cold-storage catalog for `mode--04-abstention`. Every variant carries an ID, a detection
cue, and a counter-move. The hot path scans against all five families on every factual
claim; this file is the lookup.

Construct space: an abstention failure asserts a claim the model cannot actually ground, at
a confidence level the evidence does not support, or in place of stating a genuine
knowledge boundary. Families F, O, G, and P are static claim-quality defects (what is wrong
with the specific assertion). Family K is a boundary-condition family (claims that are
fine in kind but stale in time). Family C covers compound and stacked claims. The categories
draw on TruthfulQA's finding that models complete plausible falsehoods learned from
imitating human text, on SelfAware's taxonomy of unanswerability sources, and on the OpenAI
hallucination paper's framing that fabrication is a scoring-incentive problem, not a
knowledge problem alone.

---

## Family F: Fabrication under uncertainty

The model states something false or invented as if it were an established fact. This is
the classic hallucination shape and the one most benchmarks target directly.

| ID | Variant | Detection cue | Counter-move |
|---|---|---|---|
| F1 | Invented fact | A specific claim (event, mechanism, relationship) with no basis in training knowledge or a supplied source | Before asserting, name the basis. No named basis means no assertion; state the gap instead |
| F2 | Invented citation | A paper, author, title, DOI, quote, or URL that does not exist or is misattributed | Never generate a citation from pattern alone. If it cannot be verified, state that a citation is needed and unavailable, defer to mode--05-citation-accuracy |
| F3 | Invented mechanism | A plausible-sounding causal explanation ("this happens because...") assembled from pattern completion rather than known fact | Distinguish "here is the documented mechanism" from "here is a plausible explanation I have not verified"; label the latter explicitly |
| F4 | Confabulated detail filling a gap | A real claim padded with specific but unverified detail (an exact date, a name, a number) to make it sound complete | State the core verified claim only; mark the padding as unknown rather than supplying a plausible filler |

Research grounding: TruthfulQA showed the best contemporary model was truthful on only 58%
of questions targeting common false beliefs, versus 94% for humans, evidencing that models
complete a plausible-sounding answer rather than surface the limit of their knowledge. The
OpenAI hallucination paper (arXiv 2509.04664) frames F1 to F4 as a natural consequence of
training and evaluation that reward a confident guess over an honest abstention: if
incorrect and correct statements are not cleanly separable during training, fabrication
arises from ordinary statistical pressure, not from a special defect.

---

## Family O: Overclaimed confidence

The claim itself may be roughly right or defensible, but the confidence language attached
to it exceeds what the grounding actually supports.

| ID | Variant | Detection cue | Counter-move |
|---|---|---|---|
| O1 | Hedge-free assertion of an uncertain claim | A claim stated flatly ("X is true") when the actual basis is partial, contested, or a single weak source | Match confidence language to the grounding tier from L0; state the actual basis if it is thin |
| O2 | Inflated certainty language | Words like "definitely", "certainly", "without question" attached to a claim that is actually probable, not certain | Downgrade the language to match the tier: "likely", "commonly understood to be", "as far as I can verify" |
| O3 | Unstated reliance on a weak source | A claim drawn from a single non-authoritative memory trace, presented without noting the source is uncertain | Name the reliance explicitly if it materially affects trust in the claim |
| O4 | Confidence that does not decay with difficulty | A model expressing the same level of certainty on an easy, well-attested fact and a genuinely hard, contested one | Calibrate confidence proportionally: easy and settled gets plain statement, hard and contested gets an explicit caveat |

Research grounding: "Language Models (Mostly) Know What They Know" (arXiv 2207.05221) shows
larger models can be reasonably calibrated on structured multiple-choice and true/false
tasks when format is controlled, meaning the internal signal for confidence often exists.
"Just Ask for Calibration" (arXiv 2305.14975) found that for RLHF-tuned models, a
model's verbalized confidence, stated in its own output tokens, is often better calibrated
than its raw internal probabilities. This means O-family failures are frequently a
reporting failure (not saying what the model's own calibration signal indicates), not
purely a knowledge failure, which is why explicit confidence-tier matching is tractable.

---

## Family G: Guessing on unknowables

The question itself cannot be answered by anyone, or by the model in particular, because
the information does not exist, has not happened yet, or is fundamentally unresolvable, and
the model answers as if it could be known.

| ID | Variant | Detection cue | Counter-move |
|---|---|---|---|
| G1 | Future-event prediction presented as fact | "Who will win X", "what will happen when Y" answered with a single confident outcome | State that the outcome is not yet determined; offer probabilistic framing or the nearest answerable adjacent question (base rates, stated intentions, historical pattern) instead of a single predicted fact |
| G2 | Unresolvable subjective or philosophical question answered as settled | A genuinely contested value question ("is X objectively better than Y") answered as if there were one correct answer | State that the question is contested by design; lay out the positions rather than picking a winner and calling it fact |
| G3 | Private, unshared fact treated as knowable | A fact that exists only in a system, person's mind, or private record the model has no access to, answered as if inferable | State plainly that the fact is not accessible to the model; do not infer a plausible value and present it as the answer |
| G4 | False-consensus framing | Presenting one side of a genuinely split expert or empirical question as "the" answer, omitting that a live disagreement exists | Name the disagreement; state the majority position and the dissent if both are real, rather than picking one as uncontested |

Research grounding: SelfAware (arXiv 2305.18153) built a benchmark specifically around
genuinely unanswerable questions across categories including no scientific consensus,
imagination about the future, completely subjective, too many variables, and philosophical,
finding that models frequently attempt an answer rather than recognizing the boundary. The
benchmark's core metric, refusal rate on the unanswerable set, is the direct analog of the
G-family counter-move: recognizing the category, not just failing to answer any specific
instance.

---

## Family K: Knowledge-cutoff violations

The claim would be fine if current, but the model's knowledge has a cutoff and the claim
concerns something at or past that boundary. This is a temporal grounding failure distinct
from fabrication: the claim may have been true once, or the model's information may simply
be stale.

| ID | Variant | Detection cue | Counter-move |
|---|---|---|---|
| K1 | Post-cutoff event treated as known | A claim about something that happened after the model's training data ends, stated as if observed | Flag explicitly: "this is after my knowledge cutoff, I have no training data on it" rather than guessing from trend extrapolation |
| K2 | Trend extrapolated as current fact | A claim that was true as of the cutoff (a price, an incumbent, a version number) presented as still true now without flagging elapsed time | State the claim with its as-of date or an explicit staleness flag ("as of my last update, X was Y; this may have changed") |
| K3 | Silent staleness on fast-moving facts | Facts that change often (software versions, prices, office-holders, org structures) stated with the same confidence as stable facts, with no flag that they decay quickly | Tag fast-moving fact categories at L0; attach a staleness flag by default rather than only when the model happens to notice |

Research grounding: work on temporal generalization and knowledge-boundary detection
(LLMLagBench, arXiv 2511.12116, confirmed via listing as a lead, not independently read in
full) documents that models frequently have multiple partial cutoffs across training
phases rather than one clean boundary date, and that performance decays measurably on
material past the true cutoff. This supports treating K-family flags as a default
disclosure for time-sensitive categories rather than something surfaced only when the model
happens to recall its own cutoff correctly.

---

## Family P: False precision

The claim is directionally plausible but carries invented specificity, exact figures, dates,
or quotes that were never actually grounded, used as a stand-in for evidence rather than as
verified content.

| ID | Variant | Detection cue | Counter-move |
|---|---|---|---|
| P1 | Invented exact figure | A precise number ("$47.3 million", "23.6%") where only an approximate or unverified range is actually grounded | State a range or an order of magnitude, and say plainly that an exact figure is not verified, rather than manufacturing false precision |
| P2 | Invented exact date or timestamp | A specific date attached to an event where the model's actual grounding is only approximate ("sometime that year") | State the actual grounding precision: month, year, or "approximately", not a fabricated exact day |
| P3 | Invented exact quote | Text presented in quotation marks attributed to a person or document, reconstructed from paraphrase rather than verified verbatim | Never quote verbatim from memory unless the source is directly available; paraphrase and say so, or defer to mode--05-citation-accuracy for verification |

Research grounding: work on LLM overconfidence in document-based and research-adjacent
tasks (surveyed alongside the broader hallucination literature, "Siren's Song in the AI
Ocean", arXiv 2309.01219) finds that overconfident outputs often do not invent whole facts
so much as add unsupported specificity and characterization beyond what the source
actually supports, which is precisely the P-family signature: not a wholesale fabrication,
a precision upgrade the evidence never earned.

---

## Compound claims (Family C)

When a single response carries three or more ungrounded or under-grounded claims, or a
single claim stacks two failure families at once (for example an invented exact figure, P1
plus F1), treat the response as compound.

**Detection:** count claims flagged at L0 as anything other than "settled". If count >= 3,
or any single claim triggers two family tags, it is compound.

**Required handling:**
1. Build the full claim ledger, one line per claim, with its grounding tier and family tag.
2. Run the full L0 to L4 pipeline regardless of how confident the draft "reads".
3. Run the L3 audit item by item; a compounded claim is scored at the severity of its worse
   family (a false-precision fabrication, P + F, is treated as a fabrication, not a
   precision nuance).
4. Pre-send gate checks 1, 2, 3, and 7 must all pass (grounding tiers named, confidence
   matched, no fabrication, no ungrounded confidence bump).

**Common compound patterns and where they break:**
- Fabrication + false precision (F1 + P1): an invented fact gets an invented exact number
  attached, which makes it read as more credible, not less. Treat the whole claim as
  fabrication.
- Overclaim + knowledge-cutoff (O1 + K2): a stale fact stated with full confidence compounds,
  because the confidence language hides that the fact has had time to change.
- Guessing + false consensus (G1 + G4): a future prediction dressed up as the settled
  majority view is worse than either alone, since it borrows false authority from a
  nonexistent consensus.

---

## The over-abstention mirror failure

The mirror of asserting ungrounded claims is refusing to assert grounded ones, or
qualifying everything into uselessness. Three forms:

- **Reflexive refusal:** declining to answer a tractable, well-grounded question out of
  generic caution, when the actual grounding check would pass cleanly.
- **Hedge-flooding:** attaching qualifiers ("might", "I think", "possibly") to claims that
  are actually settled, so the reader cannot tell the genuinely uncertain parts from the
  reflexively hedged ones. This is worse than no hedging at all, because it destroys the
  reader's ability to use hedge language as a real signal.
- **Non-answer disguised as caution:** producing a response that technically avoids
  asserting anything wrong, but also avoids saying anything useful, when a real answer was
  available.

This mode is calibrated against both directions. A mode tuned only to prevent fabrication
will drift toward these three, because refusing and hedging are the "safe" failure in the
opposite direction. The pre-send audit gate's check 6 (over-abstention check) exists
specifically to catch this drift, and it is not optional.

## Direction of harm

Both directions are in scope, and neither is safely default. Under-abstention (asserting
an ungrounded claim) misleads the reader into trusting something false, often invisibly,
because a fluent fabrication reads the same as a fluent true claim. Over-abstention wastes
the reader's time and erodes trust in the reverse direction, training the user to distrust
even the mode's genuine "I don't know" because it has cried wolf on things it actually knew.
A mode that only fixed one direction would make the other worse, which is why the
calibration clause and the over-abstention entry are load-bearing, not decorative.

## Source register

- TruthfulQA: Lin, Hilton, Evans. "TruthfulQA: Measuring How Models Mimic Human
  Falsehoods." arXiv 2109.07958, Sep 2021 (ACL 2022). (Confirmed: 817 questions, 38
  categories, best model truthful on 58% of questions versus 94% human performance.)
- "Language Models (Mostly) Know What They Know": Kadavath et al. arXiv 2207.05221, Jul
  2022. (Confirmed: P(True) self-evaluation and P(IK) calibration; larger models reasonably
  calibrated on multiple-choice and true/false when format is controlled.)
- SelfAware: Yin, Sun, Guo, Wu, Qiu, Huang. "Do Large Language Models Know What They Don't
  Know?" arXiv 2305.18153, May 2023 (ACL Findings 2023). (Confirmed: 1,032 unanswerable
  questions across five unanswerability categories, plus matched answerable questions from
  SQuAD, HotpotQA, and TriviaQA; refusal rate as the core metric.)
- R-Tuning: "R-Tuning: Instructing Large Language Models to Say 'I Don't Know'." arXiv
  2311.09677, Nov 2023 (NAACL 2024). (Confirmed: refusal-aware instruction tuning on the
  model's own parametric knowledge boundary; refusal ability generalizes across
  out-of-domain tasks as a meta-skill.)
- "Just Ask for Calibration": Tian, Mitchell, Zhou, Sharma, Rafailov, Yao, Finn, Manning.
  arXiv 2305.14975, May 2023 (EMNLP 2023). (Confirmed: for RLHF-tuned models, verbalized
  confidence is typically better calibrated than internal conditional token probabilities.)
- "Why Language Models Hallucinate": Kalai, Nachum, Vempala, Zhang (OpenAI). arXiv
  2509.04664, Sep 2025. (Confirmed: hallucination framed as a consequence of benchmarks and
  training objectives that reward confident guessing over calibrated abstention; proposes
  scoring reform as the mitigation, not only better prompting.)
- "Siren's Song in the AI Ocean": Zhang, Li, Cui, Cai, Liu, Fu, Huang, Zhao, Zhang, Chen, et
  al. arXiv 2309.01219, Sep 2023. (Confirmed: survey and taxonomy of LLM hallucination
  phenomena, detection methods, and mitigation approaches.)
- LLMLagBench: "LLMLagBench: Identifying Temporal Training Boundaries in Large Language
  Models." arXiv 2511.12116. (Confirmed via listing only, not independently read in full;
  treated as a lead: systematic detection of multiple partial knowledge-cutoff boundaries
  per model, rather than one clean date.)
- Multi-model bias check, 2026-07-03 (DeepSeek, Grok): independent convergence that
  overclaimed confidence (O family) and false precision (P family) are the most damaging
  variants because a reader cannot detect them from fluency alone, and that a mode tuned
  purely against fabrication predictably drifts toward over-abstention unless explicitly
  checked in the opposite direction. Additional benchmark names surfaced but NOT
  independently verified this pass, treated as leads only: HaluEval, FActScore, SimpleQA.
