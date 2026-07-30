# Tool-Calling Failure Taxonomy

Cold-storage catalog for `mode--02-tool-calling-accuracy`. Every variant carries an ID, a detection
cue, and a counter-move. The hot path scans against all five families on every tool-using
turn; this file is the lookup.

Construct space: a tool-calling failure happens at one of five points along the call chain,
selection, parameter construction, format, result interpretation, and the systemic level
across calls. This layering is the structure two of the three independent bias-check models
(DeepSeek, Grok) converged on, and it maps cleanly onto the before-call and after-call
discipline of TCV-01. The single most damaging cell is R1 (silent-error blindness), because
a wrong result with no error signal produces a confidently wrong final answer. The
"Tools Fail" study (arXiv 2406.19228) is the anchor: input-based errors usually carry a
message, tool-based failures are usually silent.

---

## Family S: Selection failures (which tool)

The pre-call decision. Which tool, or whether a tool at all.

| ID | Variant | Detection cue | Counter-move |
|---|---|---|---|
| S1 | Wrong tool | The chosen tool does not match the actual information gap or action needed | State the gap, then match it to the tool's stated purpose before emitting |
| S2 | Tool hallucination | A tool or function name is called that does not exist in the available set | Confirm the tool exists in the provided schema before use; never invent a tool name |
| S3 | Similar-tool confusion | Two tools have overlapping descriptions and the near-neighbor is picked | Name why this tool and not the near-neighbor, quoting the distinguishing part of each schema |
| S4 | Omitted call | The answer needs fresh or external data but the model answers from stale memory | If the answer requires current, private, or computed data, a call is mandatory, not optional |
| S5 | Unnecessary call | A tool is invoked when reliable direct knowledge already answers the question | Over-calling is the mirror failure; call only when knowledge is insufficient |

Research grounding: BFCL evaluates relevance and irrelevance detection precisely because
selection (including deciding NOT to call) is a distinct skill; accuracy falls sharply as
the tool set grows and tools become similar. The bias check flagged similar-tool confusion
(S3) as a top production failure across all three models.

---

## Family P: Parameter construction failures (the arguments)

The inputs to the call. The most common failure class in benchmarks.

| ID | Variant | Detection cue | Counter-move |
|---|---|---|---|
| P1 | Missing required argument | A required parameter is absent or left blank | Halt and ask; a required parameter with no grounded source stops the call |
| P2 | Wrong type or format | Value is the wrong type, out of range, or malformed for the schema | Validate type, range, and format against the schema before emitting |
| P3 | Hallucinated value | A value not present in the query or prior results is invented to fill a slot | Ground every value in a named source; a plausible guess is still a hallucination |
| P4 | Semantic deviation | Value is syntactically valid but semantically wrong ("US" for "Australia", wrong decimal place) | Check the value's meaning against user intent, not just its type; this failure is silent |
| P5 | Over/under-constrained | Extra filters that exclude the intended result, or missing filters that over-broaden it | Match the constraint set to the request; verify the parameters express, not distort, intent |

Research grounding: parameter errors dominate benchmark failure counts. P4 (semantic
deviation) is especially dangerous because it is silent: the tool accepts the value and
returns a valid-looking but wrong result. All three bias-check models named parameter
construction as the most common failure and the invent-a-value habit as its driver ("the
model prioritizes task completion over parameter verification").

---

## Family F: Format and invocation failures (the call itself)

The structural correctness of the emitted call.

| ID | Variant | Detection cue | Counter-move |
|---|---|---|---|
| F1 | Invalid JSON / schema violation | The call does not parse or does not conform to the tool's schema | Emit against the schema; use structured-output enforcement (JSON mode, grammar) where available |
| F2 | Prose mixed with the call | Explanatory text is embedded inside a structured call payload | Keep reasoning outside the call; the payload carries only the call |
| F3 | Syntax error | The call syntax itself is malformed (brackets, quoting, escaping) | Structured-output enforcement; validate the payload parses before it is sent |
| F4 | Non-deterministic formatting | The same request produces differently shaped calls across runs | Fix the emission format; determinism is a reliability property, not a nicety |

Research grounding: BFCL's AST-based evaluation exists because format and structural
correctness are objectively checkable and a frequent, detectable failure point. Constrained
decoding and structured output measurably reduce F1 and F3.

---

## Family R: Result interpretation failures (after the call)

What the model does with the returned payload. Home of the most damaging single variant.

| ID | Variant | Detection cue | Counter-move |
|---|---|---|---|
| R1 | Silent-error blindness | A no-exception result is treated as correct without checking its content | Classify every outcome; a missing error is not proof of success. The core check of this mode |
| R2 | Result misread | The returned data is misparsed or the wrong field is used | Validate structure; extract named fields, not positionally guessed ones |
| R3 | Gap-filling as if success | Missing or empty result data is filled with an assumed value and the model proceeds | Never fill a gap; a missing value halts or reports, it is not invented |
| R4 | Wrong next action | The plan update after the result does not follow from what the result actually says | State what changed because of this exact result before deciding the next step |
| R5 | Multi-turn result loss | An earlier tool result needed later is silently dropped from context | Persist tool results in explicit task state; do not rely on raw context to carry them |

Research grounding: "Tools Fail" (Sun et al., EMNLP 2024) is built on the finding that
tool-based failures are silent and that detection, the first recovery step, is hard for
LLMs; models over-trust faulty tool outputs. All three bias-check models independently
ranked silent result errors (R1) as the most damaging failure in production.

---

## Family X: Systemic and agentic failures (across calls)

Failures that appear only over a chain or a session, not in a single call.

| ID | Vector | Detection cue | Counter-move |
|---|---|---|---|
| X1 | Error cascade | One undetected bad result poisons every downstream call and the final answer | Validate each result before it feeds the next call; a failed upstream halts the chain |
| X2 | Retry loop | The same failing call is repeated with no change, burning tokens and rate limits | Classify first; retry only transient errors under a bounded cap; never loop on a permanent error |
| X3 | Tool explosion | Selection accuracy drops sharply as the tool count or tool similarity rises | Narrow the candidate set; justify selection explicitly when many similar tools exist |
| X4 | Cost / latency blowup | Redundant or poorly ordered calls inflate cost and latency | Cap total calls per request; prefer the minimal sufficient call set |
| X5 | Confused-deputy | Tool exposure is treated as authorization to use it for any purpose | Exposure is not permission; irreversible or sensitive use is confirmed, not assumed |
| X6 | Sampling non-determinism | The same task yields different call sequences run to run | Reduce degrees of freedom in emission; structured output and explicit selection help |

Research grounding: per-step reliability compounds badly (95 to 99% per call yields far
lower end-to-end over a chain), so multi-step tool use scores below 60% for frontier models.
The bias check converged on recovery and error detection, not single-call accuracy, as the
true reliability bottleneck. X5 (confused-deputy) was DeepSeek's security-facing addition and
links to mode--08-prompt-injection.

---

## Compound chains (Family C)

When a task requires a chain of dependent calls, per-step reliability multiplies and a
single undetected failure ruins the end-to-end result.

**Detection:** the task needs two or more dependent calls, or a call whose parameters come
from a prior call's result.

**Required handling:**
1. Validate and classify each result before it becomes the input to the next call (X1
   prevention).
2. Set a total-call cap for the request (X4 prevention).
3. Keep a "report failure and surface evidence" path available at every step.
4. On any permanent or silent-wrong result mid-chain, stop the chain and report; do not push
   a poisoned value downstream.

**Why:** a five-call chain at 95% per-step reliability is roughly 77% reliable end-to-end,
and that math assumes every failure is detected. Silent failures are not detected by default,
which is why R1 classification is mandatory at every step of a chain.

---

## The over-calling mirror failure

The mirror of reckless tool use is compulsive tool use: calling a tool when direct knowledge
answers the question (S5), chaining redundant calls, or halting to ask for a parameter the
context already supplies. A model tuned only to "verify before every call" drifts into
ceremony and latency without added correctness. This mode is calibrated against both
directions: call when knowledge is genuinely insufficient, verify proportional to blast
radius, and do not ask for inputs that are unambiguously present. Reliability is a correct
outcome at a sensible cost, not the maximum number of checks.

## Direction of harm

Both directions are in scope. Under-verification is the common failure: an invented
parameter or an unchecked silent-wrong result reaches the user as a confidently wrong answer
or an incorrect side effect. Over-verification is the mirror: needless calls and needless
questions that add cost and friction without correctness. The calibration clause and the
over-calling entry are load-bearing, not decorative.

## Source register

- Berkeley Function Calling Leaderboard (BFCL): Patil, Mao, et al. "The Berkeley Function
  Calling Leaderboard (BFCL): From Tool Use to Agentic Evaluation of Large Language Models."
  ICML 2025, PMLR v267; gorilla.cs.berkeley.edu/leaderboard. (Confirmed: AST-based
  evaluation, serial/parallel/multi-turn, relevance and irrelevance detection, scales to
  thousands of tools.)
- tau-bench: multi-turn retail and airline agent simulation measuring end-to-end task
  completion over tool-call turns. (Confirmed via benchmark listings, April 2026.)
- API-Bank: benchmark for evaluating tool-learning methods. (Confirmed as a standard
  tool-use benchmark.)
- ToolBench / ToolLLM: large-scale tool-use benchmark. (Confirmed as a standard tool-use
  benchmark.)
- "Tools Fail: Detecting Silent Errors in Faulty Tools": Sun, Min, Chang, Bisk. EMNLP 2024,
  arXiv 2406.19228. (Confirmed: taxonomy of input-based vs silent tool-based failures;
  detection is the hard first recovery step; models over-trust faulty tool outputs.)
- Multi-model bias check, 2026-07-03 (Gemini 2.5 Pro, DeepSeek R1, Grok): independent
  convergence on selection and parameter errors as most common, silent failure as most
  damaging, recovery over call-accuracy as the reliability lever, and tool-schema design as
  a root cause the mode can flag but not fix. Full record in the project build folder.
- Additional benchmarks named by the bias-check models but NOT independently verified in
  this pass, therefore not relied on: SpecTool, TALLM taxonomy, MCP-Bench, ToolBench-X,
  ErrorAtlas. Treat as leads, verify before citing.
