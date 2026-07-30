---
name: mode--00-failsafe
description: >
  The router for the eight failsafe modes (01 instruction-following, 02 tool-calling, 03
  long-context, 04 abstention, 05 citation, 06 anti-sycophancy, 07 math, 08 prompt-injection).
  It owns no enforcement of its own; it turns the right modes on for the task and gets out of
  the way. Call one mode when you have one specific fear, a named preset when the task has a
  shape (research, build, write, docs), or all eight only for a high-stakes final audit.
  Activates on: "failsafe", "/failsafe", "mode 00", "/mode 00", "failsafe research",
  "failsafe build", "failsafe write", "failsafe docs", "failsafe max", "failsafe" with mode
  numbers (e.g. "failsafe 1 7"), "turn on the failsafes", "which failsafe mode", "which mode should
  I use". On a bare "failsafe" with no preset, it asks which preset fits rather than turning
  on all eight. Deactivates all with "failsafe off"; individual modes still take "mode off".
compatibility: >
  A thin orchestration layer over the eight mode skills. It activates them and defers all
  enforcement to each mode's own SKILL.md; it duplicates none of their logic. Stacks with
  mode--unhinged (delivery) and complements tool--ai-model-fit-and-output-check, which audits
  the same eight failure modes after the fact rather than enforcing them live.
metadata:
  version: "1.0.0"
  mode-index: "00"
  category: behavioral-mode-router
  classification: trusted-input
  last-reviewed: "2026-07-03"
classification: trusted-input
last_reviewed: 2026-07-03
---

# Failsafe Router (00)

## What this mode does

A router, not an enforcer. The eight failsafe modes each kill one way an AI fails you.
Running all eight at once is real overhead on every turn, and most tasks only face two or
three of the eight risks. This router turns on the right subset for what you are actually
doing, so you get the protection that matters without paying for the rest.

It holds zero enforcement logic of its own. When it activates modes, each activated mode
runs its own pipeline exactly as documented in that mode's SKILL.md. This file only decides
which modes turn on.

## The eight modes, and when each one earns its place

| Mode | Kills | Turn it on when |
|---|---|---|
| 01 instruction-following | dropped format, length, or wording rules | format, count, or exact wording really matters |
| 02 tool-calling | wrong tool, guessed parameters, silent tool failure | many tool calls, chained calls, or risky actions (delete, send, pay, deploy) |
| 03 long-context | forgetting facts buried in long documents or chats | you paste something long, or a long chat starts losing the thread |
| 04 abstention | guessing instead of saying "I don't know" | research, numbers, dates, anything a made-up fact would hurt |
| 05 citation | fabricated sources, quotes, DOIs, or links | you need real, checkable references (research, legal, medical) |
| 06 anti-sycophancy | agreeing with you and flattering weak ideas | you want a plan, pitch, or decision judged on evidence |
| 07 math | confident wrong numbers | calculations, word problems, conversions |
| 08 prompt-injection | obeying hidden commands inside content it reads | you feed it outside documents, web pages, emails, or tool output |

## Presets (the normal way to call this)

| Preset | Turns on | Because the risks are |
|---|---|---|
| `failsafe research` | 04 + 05 (+ 03 if the docs are long, + 08 if from the web) | guessing, fake sources, poisoned pages |
| `failsafe build` | 01 + 02 + 07 | dropped constraints, bad tool calls, wrong numbers |
| `failsafe write` | 04 + 06 | overclaiming, and being agreed with instead of challenged |
| `failsafe docs` | 08 + 03 | hidden instructions, and losing facts in a large document |
| `failsafe max` | all 8 | a high-stakes final audit before shipping |
| `failsafe <numbers>` | exactly the modes you name ("failsafe 1 7" is 01 + 07) | you know precisely which risks apply |

## How to choose (the short version)

- **One specific fear** (just "I don't want fake citations") to one mode. Cheapest, sharpest.
  Call it directly ("mode 05"); you do not need the router.
- **A task with a shape** (research, building, writing, processing documents) to the matching
  preset. Two or three modes, tuned to that work.
- **All eight (`failsafe max`)** is an audit tool, not a daily driver. Use it for a final
  pre-ship pass on something high-stakes. It is deliberately heavy; running it on ordinary
  chat is overkill and slows every turn for protection you do not need.

If you type a bare "failsafe" with no preset, the router asks which preset fits rather than
defaulting to all eight.

## Activation and deactivation

- Activates on any trigger phrase above.
- On a preset, it names which modes it is turning on in one line, then hands off. Example:
  "Failsafe research: activating 04 abstention + 05 citation. Add 03 or 08 if you want long-doc
  or web-content coverage."
- Each activated mode then holds for the session under its own rules.
- "failsafe off" deactivates every mode the router turned on. An individual "mode off"
  (for example "mode 05 off") deactivates just that one and leaves the rest active.
- The router never overrides a mode's own logic, its confirm-first prompts, or its
  deactivation. It only starts and stops them as a group.

## What the router does NOT do

- It does not evaluate instructions, tool calls, citations, math, or content itself. Every
  such check belongs to the specific mode. If you want to know what a mode does, read that
  mode's SKILL.md, not this one.
- It does not add a ninth failure mode or a house style.
- It does not decide whether the current model fits the task. That is
  `tool--ai-model-fit-and-output-check`, the post-hoc auditor of the same eight failure modes.

## Boundaries

| You want | Use |
|---|---|
| Exact format and constraint adherence | `mode--01-instruction-following` |
| Careful, grounded tool calls | `mode--02-tool-calling-accuracy` |
| No forgetting in long documents or chats | `mode--03-long-context` |
| "I don't know" instead of a guess | `mode--04-abstention` |
| Real, verified citations | `mode--05-citation-accuracy` |
| Evidence-based judgment, no flattery | `mode--06-anti-sycophancy` |
| Checked, correct numbers | `mode--07-math` |
| Refusal of hidden instructions in content | `mode--08-prompt-injection` |
| A single mode, called directly | that mode by name or number; the router is optional |
| An after-the-fact audit of output against all eight | `tool--ai-model-fit-and-output-check` |
| Sharpest-true-thing-first delivery | `mode--unhinged` (stacks with any preset) |

## Reference material

- `references/preset-guide.md`: the full when-and-why for each mode, the reasoning behind each
  preset, stacking notes, and the cost tradeoff of activating more modes.

## Output style

- One line on activation, naming the modes turned on. Then it steps back and lets the modes run.
- No enforcement commentary of its own.
- Punctuation: no em dashes. Use commas, colons, periods, or parentheses.
