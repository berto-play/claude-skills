# Long-Context Failure Taxonomy

Cold-storage catalog for `mode--03-long-context`. Every variant carries an ID, a detection
cue, and a counter-move. The hot path scans against all five families on every long-input or
long-session turn; this file is the lookup.

Construct space: a long-context failure is any loss of reliable access to information that is
present in the input. The losses cluster into five families: positional (where in the input a
fact sits), integration (combining facts across positions), contamination (bad or irrelevant
content distorting the answer), degradation dynamics (how reliability falls as length grows),
and system (the infrastructure silently dropping content). The single most important property,
confirmed by both independent bias-check models, is that these failures are silent: the model
produces a fluent answer that omits or contradicts the input without any signal that it did.

---

## Family P: Positional failures (where the fact sits)

| ID | Variant | Detection cue | Counter-move |
|---|---|---|---|
| P1 | Lost-in-the-middle | A fact needed for the answer sits in the middle of a long input and is under-used | Locate and quote it; if input is long, move key material to the ends or add a key-info index |
| P2 | Position forgetting | An early turn or early passage is pushed out of effective attention as content accumulates | Externalize it into the working-memory ledger; do not rely on it staying in attention |
| P3 | Primacy/recency over-weighting | The start and end dominate; the model answers from them and skips the interior | Force a start/middle/end scan; weight by relevance, not position |

Research grounding: Lost in the Middle (Liu et al., TACL 2024, arXiv 2307.03172) documents the
U-shaped curve across models and tasks. Both bias-check models named P1 as the most common
long-context failure and noted it is architectural (positional encoding), not an engineering
bug.

---

## Family I: Integration failures (combining facts)

| ID | Variant | Detection cue | Counter-move |
|---|---|---|---|
| I1 | Multi-hop failure | The answer needs facts linked across distant positions and the link is not made | Extract each hop into the ledger first, then reason over the ledger, not raw context |
| I2 | Aggregation overload | Summing or counting many facts across a long input degrades super-linearly | Extract the items to be aggregated explicitly; aggregate over the extracted set, not from memory |
| I3 | Cross-reference loss | A reference in one place to content in another is dropped | Resolve references during extraction; record the resolved fact in the ledger |

Research grounding: RULER (arXiv 2404.06654) shows models near-perfect on simple retrieval but
degrading sharply on multi-hop tracing and aggregation as length grows, well inside advertised
limits. LongBench v2 (arXiv 2412.15204) is built on reasoning-heavy long-context tasks where the
best model scores 50.1%.

---

## Family C: Contamination failures (bad content distorting the answer)

| ID | Variant | Detection cue | Counter-move |
|---|---|---|---|
| C1 | Context poisoning | Once wrong information enters the context it is repeatedly cited and built upon | Flag suspect content; re-verify a claim against its original source before reuse |
| C2 | Context distraction | Large amounts of irrelevant or weakly relevant text disperse attention from the key facts | Extract query-relevant excerpts first; treat the rest as distractors |
| C3 | Context clash | Contradictory statements coexist in the context and the model cannot resolve them reliably | Pre-scan for contradictions; surface them explicitly, do not silently pick one |
| C4 | Source/role confusion | Content from different documents, speakers, or roles is blended | Tag each fact with its source in the ledger; keep sources distinct |

Research grounding: the bias check converged on context poisoning, distraction, and clash as
distinct attention-layer failures. NoLiMa (arXiv 2502.05167) shows models lean on literal
matching rather than semantic understanding, which C2 distraction exploits.

---

## Family D: Degradation dynamics (how reliability falls with length)

| ID | Variant | Detection cue | Counter-move |
|---|---|---|---|
| D1 | Effective-vs-advertised gap | The model is treated as reliable across the full advertised window when only ~30 to 60% is usable | Budget the effective boundary by task type; switch to retrieve/summarize past it |
| D2 | Coherence collapse | Reasoning quality is fine until a threshold, then drops suddenly rather than gradually | Do not assume "a bit shorter" is safe; ground and verify rather than trusting holistic reasoning near the limit |
| D3 | Initial-hypothesis anchoring | The model locks onto an early assumption and ignores later clarifying content | Re-validate assumptions against the full current context before finalizing |
| D4 | Multi-turn drift | Over a long session the model over-trusts its own prior outputs and drifts from the original goal | Re-anchor each turn: restate goal and constraints, re-verify prior turns against the source |
| D5 | Instruction/goal erosion | Early constraints or the system instruction dilute and stop being applied | Carry constraints in the ledger; mode--01-instruction-following persists them across turns |

Research grounding: "LLMs Get Lost in Multi-Turn Conversation" (Laban et al., arXiv 2505.06120)
measured a ~39% average multi-turn drop with a large variance increase, driven by premature
assumptions and failure to recover (basis for D3 and D4). Multiple 2025-2026 analyses report the
effective-vs-advertised gap (D1) as systematic and measurable.

---

## Family S: System failures (infrastructure drops content)

| ID | Variant | Detection cue | Counter-move |
|---|---|---|---|
| S1 | Silent truncation | The API or inference engine drops the earliest tokens (often the system prompt) with no error | Flag when earliest content or rules may have been lost; do not assume the full input is present |
| S2 | Hallucinated bridging | When utilization fails, the model invents connecting detail instead of flagging the gap | Mark absent content "not present in context"; never fabricate a bridge |
| S3 | Cost/latency blowup | Prefill time grows with input length; very large prompts add large first-token latency | Prefer retrieval or summarization over stuffing the full input when it exceeds the reliable zone |
| S4 | Preprocessing loss | Lossy chunking or summarization upstream silently drops content before the model sees it | Treat pre-processed input as potentially incomplete; ground claims and note what may be missing |

Research grounding: silent truncation and hallucinated bridging were the system-layer failures
the bias check flagged as most important in production, because they produce confidently wrong
answers with no error signal. S2 is the long-context form of the general hallucination-on-gap
failure that mode--04-abstention also addresses.

---

## The over-caution mirror failure

The mirror of losing the thread is refusing to hold it at all: declining a long input,
reflexively asking the user to narrow scope, routing everything through retrieval when the model
could reliably read it whole, or burying an answer under so many citations and hedges that it
stops being useful. A model tuned only to "distrust long context" becomes useless on exactly the
long documents it should handle. This mode is calibrated against both directions: read whole
when the input is inside the reliable zone, retrieve and summarize when it is past it, and ground
proportional to stakes. Not every long input exceeds the boundary, and treating every one as if
it does is its own failure.

## Direction of harm

Both directions are in scope. Under-handling is the common failure: a buried fact is silently
dropped and the user gets a fluent, confidently incomplete answer. Over-handling is the mirror:
needless refusal, scope-narrowing, or citation clutter that adds friction without accuracy. The
calibration clause and the over-caution entry are load-bearing.

## Source register

- Lost in the Middle: Liu, Lin, Hewitt, Paranjape, Bevilacqua, Petroni, Liang. "Lost in the
  Middle: How Language Models Use Long Contexts." TACL 2024, arXiv 2307.03172. (Confirmed:
  U-shaped positional bias in multi-document QA and key-value retrieval.)
- RULER: Hsieh et al. (NVIDIA). "RULER: What's the Real Context Size of Your Long-Context
  Language Models?" arXiv 2404.06654. (Confirmed: configurable synthetic benchmark; complex-task
  degradation as length grows; advertised windows overstate usable capability.)
- "LLMs Get Lost in Multi-Turn Conversation": Laban, Hayashi, Zhou, Neville. arXiv 2505.06120,
  2025. (Confirmed: ~39% average multi-turn performance drop across six tasks; variance
  explosion; premature-assumption and recovery-failure mechanism.)
- NoLiMa: Modarressi, Deilamsalehy, et al. (Adobe Research). "NoLiMa: Long-Context Evaluation
  Beyond Literal Matching." arXiv 2502.05167, ICML 2025. (Confirmed: minimal lexical overlap
  needles; GPT-4o drops 99.3% to 69.7%; models lean on literal matching.)
- LongBench v2: Bai et al. "LongBench v2: Towards Deeper Understanding and Reasoning on Realistic
  Long-context Multitasks." arXiv 2412.15204, ACL 2025. (Confirmed: 503 reasoning MCQ, 8k to 2M
  words, six task categories; best direct-answer model 50.1%.)
- Multi-model bias check, 2026-07-03 (DeepSeek R1, Grok; Gemini stalled and was excluded):
  independent convergence on lost-in-the-middle as most common, the effective-vs-advertised gap
  and multi-turn drift as the largest reliability losses, and externalized working memory plus
  source grounding as the enforcement. Full record in the project build folder.
- EXCLUDED as unverifiable (G4): "Evans' Law: L ≈ 1969.8 × M^0.74" (cited by DeepSeek, no
  verifiable source, likely fabricated). Not relied on.
