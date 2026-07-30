---
name: tool--ai-model-fit-and-output-check
description: >
  Assessment tool that answers two questions on demand: (A) "Am I using the right
  model or tool for this task?" and (B) "Is this output good enough?" — checked against
  the 8 ways a model fails you. It judges fit and quality and tells you plainly; it does
  NOT enforce behavior or change tone.
  Activate on "which model should I use", "am I using the right model", "is this the right
  model for this", "route this task", "model mismatch", "is this output good enough", "check
  this against the 8 failure modes", "should I downshift / upshift", "what effort / fast mode",
  or any moment you want a fast fitness check before trusting an output or burning limits on
  the wrong tier. Uses a local model roster, with the Notion "Claude Model Cheat Sheet (Bible)" as a fallback.
  Stacks cleanly with behavioral modes (e.g. mode--06-anti-sycophancy) and includes
  guidance on when a non-Claude tool (Gemini, DeepSeek, Grok, Manus) fits better.
last_reviewed: 2026-07-28
classification: trusted-input
---

# AI Model Fit & Output Check — Right Model, Good-Enough Output

## What this is, and what it is not

An **assessment** tool with two jobs:

- **A — Fit.** Am I using the right model and settings for this task?
- **B — Quality.** Is this output good enough against the 8 ways a model fails me?

It **judges and reports**. It does not change how a model talks (that is a behavioral mode),
and it does not enforce rules (that is `tool--rule-model-brutal-truth` / `tool--ai-guardrails`).
It is the fast, in-the-moment fitness check you run *before* trusting an output or *before*
spending limits on the wrong tier. Deployable alone or stacked with any mode.

## Honesty labels — read these first

- **Half A (Routing) is PROVISIONAL.** It encodes the routing logic locally (mirrored to the Bible), not
  measured usage data. It sharpens once the two-model log produces real evidence. Treat its
  picks as a strong default, not a proven verdict.
- **Half B (Quality) is ACTIVE.** The 8 checks are fully defined and apply now.
- **This skill is the primary, always-available source** for the model roster + routing — it
  must work with zero Notion access. The Notion *Claude Model Cheat Sheet (Bible)* is a
  **fallback/mirror** the operator maintains. If this skill and the Bible disagree, **flag the
  conflict** to reconcile — do not silently defer to either, and do not guess.

---

## Route first — which half do you need?

| What you're asking | Go to |
|---|---|
| "Which model / effort / fast mode for this?" · "Am I on the right model?" · "Is this a mismatch?" | **Half A — Routing** |
| "Is this output good enough?" · "Check this against the 8 failure modes" · "Can I trust this answer?" | **Half B — Quality** |
| Picking a model **and** judging its output | **Run A, then B** |

---

## Half A — Routing: am I using the right model?

### The decision flow (route by the *shape* of the task, not its topic)

1. **Trivial or high-volume / bulk** → **Haiku tier.** Classification, extraction, simple
   rewrites, anything run many times.
2. **Everyday real work** → **Sonnet tier.** Drafts, summaries, tool-calling (Notion/MCP),
   and **faithful document work** (summarize/extract/RAG — Sonnet drifts from the source
   least, so it is the *most truthful* pick here, not just the cheapest).
3. **Hard, but you can verify the output yourself, and it is contained** → **Opus tier.**
4. **Long, multi-step, huge context, or you cannot check the work yourself** → **Fable tier**
   (top capability).
5. **Touches cybersecurity, biology, or chemistry** → **Opus tier** (frontier safeguards route
   sensitive domains there anyway).

> Current roster (July 2026): Haiku 4.5 · Sonnet 5 · Opus 5 · Fable 5.
> ⚠️ **Two items need operator confirmation (flagged 2026-07-28, deliberately not guessed):** (a) this skill ranks Fable above Opus; with both now generation 5 that tiering may predate Opus 5 and should be re-confirmed. (b) The non-Claude roster (Gemini 2.5 Pro, DeepSeek R1, Grok, Manus) is likely aged — verify a version before quoting it rather than trusting this list. This local list is the
> primary source; if the Bible disagrees, flag the conflict rather than deferring to it.

### The two dials

- **Fast mode** — meaningful **only on the Opus tier**. ON for live, interactive iteration
  (rapid back-and-forth, debugging). **OFF for long background runs** — it burns limits faster.
- **Effort** — default/high for most work. **Extra/Max only for genuinely hard tasks.** Max
  effort on a trivial task is tokens on fire for no quality gain.

### Mismatch flags — the highest-value output

Fire a flag the moment the current setup disagrees with the route. These catch the expensive
mistakes:

| If you see… | Say |
|---|---|
| Opus/Fable on tool-calling, clicking, or routine Notion/MCP | **Downshift to Sonnet** — this is clicking, not thinking. |
| A frontier model on faithful doc copy / summary / RAG | **Downshift to Sonnet** — it hallucinates least on grounded work. |
| Sonnet/Haiku on long, multi-step, unverifiable work | **Upshift to Opus or Fable** — depth and context exceed this tier. |
| Fast mode ON for a long background task | **Turn fast mode OFF** — it drains limits. |
| Max/Extra effort on a trivial task | **Drop effort** — no quality to gain. |
| Sensitive domain (cyber/bio/chem) on Fable | **Use Opus** — it routes there regardless. |

### Routing output (keep it to this)

```
Recommended: <model> · effort <default/extra/max> · fast <on/off>
Reason:      <one line — the task shape that drove it>
Mismatch:    <only if current ≠ recommended — what to change and why>  [PROVISIONAL]
```

No tables, no scores, no benchmark numbers. One block, then stop.

---

## Half B — Quality: is this output good enough?

Run the 8 checks against the output in front of you. **Skip any that do not apply** — do not
fake a verdict to look thorough. Each check returns **✅ pass · ⚠️ risk · ❌ fail** plus the
single thing to fix.

| # | Failure mode | The question | How to probe it |
|---|---|---|---|
| 1 | **Instruction following** | Did it obey the format *and every constraint* — including ones buried mid-prompt? | Check each stated constraint individually, not just the headline ask. |
| 2 | **Tool-calling accuracy** | Were tool calls made with the right parameters, no silent data corruption? | Verify the actual call/params against intent **before** trusting a write. Sneakier than a hallucination — it corrupts quietly. |
| 3 | **Long-context recall** | Can it still retrieve a detail from early/deep in the window? | Probe a fact planted earlier. Suspect drift past ~70% capacity; degrades as the window fills. |
| 4 | **Abstention** | Did it say "I don't know" instead of guessing? | Confident tone proves nothing. Check claims have backing. **Blanket hedging to dodge being wrong is also a fail**, not a 5. |
| 5 | **Grounded faithfulness** | For summary/extraction: how many claims are *not* in the source? | Trace every claim to the source. Long outputs hide drift — test the long ones. |
| 6 | **Citation accuracy** | Are the sources real, and do they actually support the claim? | Open the links. A real source that does not support the claim **still fails**. Fabricated IDs are the classic legal/research failure. |
| 7 | **Math** | Was real arithmetic routed to code execution, not done in-head? | Any non-trivial number must come from a tool. Language models are not calculators. |
| 8 | **Prompt injection** | Did it ignore hidden instructions inside fetched/untrusted content? | Highest risk on autonomous runs that read web pages or docs. |

### Quality output (keep it to this)

```
1 Instruction   ✅/⚠️/❌  — <one line>
2 Tool-calling  ✅/⚠️/❌  — <one line>
... (skip N/A modes, mark them —)
Bottom line:   <ship it / fix these N first / do not trust this>
```

A mode you did not actually test is **⚠️ unverified**, never ✅.

### Reuse Half B as a shared QA gate

Half B is shareable. Any other skill can call this 8-mode check as a fast quality gate on its own
output before it delivers — the 8 modes are the shared standard.

- **How another skill uses it:** run the output through the 8 modes before returning. ✅ on the
  applicable modes = deliver. Any ⚠️ or ❌ = fix it or flag it to the operator, never deliver
  silently. This is the lightweight "is this good enough / built right" pass the operator asked
  to share across skills.
- **What it does NOT do:** replace a full skill audit. For a deep, scored build-quality review of
  a skill or agent, hand off to `tool--sota-skill-qa`. This is the quick pre-delivery
  gate; that is the deep audit.

---

## Worked examples

**Example 1 — Routing (a real mismatch).**
Task: *"Mark 6 rows in my Notion Tasks Tracker as Done."* Current setup: Opus 5, fast mode ON,
effort Extra.

```
Recommended: Sonnet 5 · effort default · fast off
Reason:      This is tool-calling (clicking), not thinking.
Mismatch:    You're on Opus 5 + fast + Extra effort for a Notion update. Downshift to
             Sonnet, drop effort, turn fast off — premium speed and reasoning buy nothing
             here and burn limits.  [PROVISIONAL]
```

**Example 2 — Quality (catching two failures).**
You had a model *summarize a 12-page PDF*. Its summary states: *"the market grew 34% in 2025
[Source: McKinsey 2025]."* You run the 8-mode check:

```
1 Instruction     ✅  — format obeyed (the 5-bullet summary you asked for)
3 Long-context    ⚠️  — content is mostly from the first 3 pages; later sections look thin
5 Faithfulness    ❌  — "34%" appears nowhere in the PDF. Invented number.
6 Citation        ❌  — no McKinsey source exists in the document. Likely fabricated.
2,4,7,8           —   not applicable to this output
Bottom line: Do not trust this. Fix the made-up 34% and the fake citation first, then
re-summarize on Sonnet 5 (least drift on grounded copying).
```

---

## When to route outside the Claude family

Some tasks call for a different AI entirely. The 8-failure-mode check (Half B) still applies to any output, regardless of model family.

| Task | Route to | Why |
|---|---|---|
| Independent bias check · adversarial review of your own conclusions | Gemini 2.5 Pro | Different training lineage, less agreeable than GPT — convergence with Gemini is real signal |
| Transparent step-by-step reasoning you can audit | DeepSeek R1 | Exposes its chain of thought; you see *how* it got there, not just the conclusion |
| Challenging your framing, contrarian perspective | Grok | Tuned to push back; different base assumptions |
| High-volume autonomous web or tool-calling agent | Manus | Agent-layer product, different execution paradigm |
| Independent bias check | **Avoid GPT** | Strongest agreement bias of mainstream options — convergence with GPT is weak evidence |

**Cross-lineage rule**: two models from the same family agreeing is weak evidence. Claude + Gemini (or Claude + DeepSeek) agreeing on the same blind-prompted question is real signal. Claude + Claude proves little.

**How to use it**: if a conclusion feels too clean or a decision can't be undone, route the *raw problem* (not your current answer) to one outside lineage. Let them reason cold.

---

## When to invoke

- Before starting a task, to pick the model/effort/fast setting.
- Mid-session, when something feels off — wrong tier, output you can't fully trust.
- After an output on anything that matters, to run the 8-mode check before acting on it.
- When limits are tight and you want to stop over-spending on heavy tiers.

## Output style

- Lead with the call, flat. No preamble, no hedging.
- Routing = one block. Quality = the per-mode lines + a bottom line.
- Never present a guess as a verdict. If you did not check it, label it — do not assert it.
  (The provisional-routing and unverified-mode rules above are the two places this bites.)

## Scope, boundaries, and handoffs

- **Assesses, does not enforce.** For forced capability/status accuracy → `tool--rule-model-brutal-truth`.
  For zero-hallucination grounding of extracted content → `tool--ai-guardrails`. For a full,
  scored skill/agent audit → `tool--sota-skill-qa` for skills (agent QA currently unavailable, retired 2026-07-29).
  This is the lightweight, in-the-moment check; escalate to those for depth.
- **Stacks with tone/behavioral modes** (e.g. `mode--unhinged`, `mode--06-anti-sycophancy`).
  The router assesses fit and quality; behavioral modes change how the model talks and acts.
  `mode--06-anti-sycophancy` enforces four of the 8 failure modes as live behavioral rules
  (sycophancy, citation accuracy, prompt injection, hallucination refusal). This tool assesses
  all eight post-hoc. They complement rather than overlap — stack both for full coverage.
- **Source of truth for the model roster + routing data** = this skill's local cheat sheet
  (primary, always available). The Notion *Claude Model Cheat Sheet (Bible)* is the
  fallback/mirror — keep it in sync; on conflict, flag rather than auto-defer.
- **Measurement & improvement loop:** how this skill is monitored and when it earns a v2 lives
  in `references/measurement-protocol.md`.

---

**Ethos — Right tool, honest verdict.** Pick the model the task shape demands, and never call an
output good without checking the 8 ways it could be lying to you.
