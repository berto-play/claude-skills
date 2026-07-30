# Measurement & Improvement Protocol — tool--ai-model-fit-and-output-check

How this skill is monitored, scored, and improved. Two feedback streams: **qualitative**
(you, using it) and **quantitative** (a fixed ruler, stress-tested across models). Built for a
known fact about these models: *the same question on a different day gives a different answer.*
A noisy thing cannot be measured in one shot — you measure a **rate across many runs**, never a
single output.

---

## Stream 1 — Qualitative (your lived signal)

Every time you invoke the router, log one line. Ten seconds. This catches what numbers cannot:
did it feel right, did it save a mistake, did it annoy you.

| Field | Values |
|---|---|
| Date | auto |
| Task | one line — what you were doing |
| Did you follow its recommendation? | yes / no / overrode |
| Right in hindsight? | clean / off / wrong |
| Note | one line — what it nailed or missed |

---

## Stream 2 — Quantitative (the ruler)

### The ruler — 10 fixed tasks, written once, never changed

A ruler that changes every time measures nothing. Write 10 **real** tasks you actually do, plus
a one-line "what a clean answer looks like" for each. Suggested spread:

- **3 routing calls** (e.g. a Notion update; a long multi-doc synthesis; a bulk rewrite)
- **4 quality checks** (feed it outputs with known flaws — a buried-constraint miss, a fake
  citation, an in-head math error, a source-drift summary — and see if it catches them)
- **3 edge cases** (a trivial task; a sensitive-domain task; a task too long for the window)

### The models to stress it across

**Sonnet 5, Opus 5, and one outside lineage for independence.** Use **Gemini** as the
outside check, **not GPT** (GPT is the most agreeable, which makes it the worst single bias
check). Three lineages, the same 10 tasks. Two models from the same family agreeing is weaker
evidence than it feels — the independent lineage is what makes convergence meaningful.

### The method — built for noise

1. Run all 10 on each model.
2. Grade each into **3 buckets**: **clean / needs-fix / wrong.** No 1–100 scores — that
   precision is fake.
3. **Run borderline ones 2–3 times.** Clean every time = real. Clean once, wrong twice = the
   noise talking, and the skill is not reliable there yet.
4. Record one number per model per round: **clean out of 10.**

### The trend — one table

| Round | Model | Clean / 10 | Notes |
|---|---|---|---|
| 1 | Sonnet 5 | — | baseline |
| 1 | Opus 5 | — | |
| 1 | Gemini | — | outside lineage |

Round 1 sets the baseline. Each improvement round, re-run the same 10.

---

## The stop rule — when is it good enough?

You stop **when the clean/10 number plateaus across 2–3 rounds**, not when it hits perfect — the
noise guarantees it never will. A flat trend means further tweaking is chasing noise, not real
gains. That plateau is the ship signal: *good enough, plateaued* — not *perfect*.

- Number still climbing → keep improving (build v2).
- Number flat across 2–3 rounds → ship as-is; stop tuning.

---

## What this honestly measures (and what it does not)

- **It measures:** whether the skill is **consistent and useful to you**.
- **It does not measure:** whether it is "objectively the best possible." That is not knowable
  with one person and ten tasks — and it is not the bar that matters. "Does it reliably help me"
  is.
- **The one thing it cannot verify:** your own honesty on the grade. "Clean vs needs-fix vs
  wrong" is your judgment, and judgment is mildly noisy and mildly biased toward liking your own
  tool. Partial fix: write the "what clean looks like" line **before** the test, so you grade
  against a fixed standard, not your mood that day.

---

## Decay — every score expires

Re-run the ruler when any of these change: a model version, the routing data in the Bible, a
real-world failure the skill missed, or the skill's own scope. An old score is a stale score.

---

## Status

**v1 — CALIBRATING.** Routing half is provisional (no usage data yet). Quality half is active.
The two-model usage log (Notion, Intelligence Hub) feeds Stream 1. First quantitative round is
unrun. Do not treat any verdict from this skill as validated until at least one full ruler round
plus its follow-up is complete.
