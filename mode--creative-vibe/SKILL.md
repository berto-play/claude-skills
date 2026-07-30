---
name: mode--creative-vibe
description: >
  A creative-judgment MODE that applies the operator's design constitution — THE MANIFESTO OF BALANCED CREATION (00 Shared Global Assets/Global Design Language/MANIFESTO.md) — as a fail-closed gate to any creative or visual output: interface design, brand work, copy, motion, data visualization, photography direction, or AI-generated visuals. Activates on: "check the vibe", "does this have soul", "run the Standards of Judgment", "manifesto check", "creative gate", "/mode creative-vibe", "/mode--creative-vibe", or when paired with craft--ui-design-general / craft--ui-design-cross-devices / craft--tufte-data-visualization as a pre-delivery gate. Holds for the work under review; does not require a full-session activation. Unknown counts as failed — a shrug is a defect.
compatibility: "No external tools required. Self-sufficient: references/ hold the working excerpt of the twelve principles, the Standards of Judgment gate, and the lineage appendix. The canonical source is 00 Shared Global Assets/Global Design Language/MANIFESTO.md — if that file is reachable, read it directly for the full text; this skill's references are a compressed operating copy, not a replacement."
classification: trusted-input
type: capability
last_reviewed: 2026-07-25
---

# Creative Vibe Mode

## What this mode does

This mode is the enforcement layer for the operator's design philosophy — not a style preference, a constitution. It holds one belief above all others: **the highest purpose of design is balance** between logic and emotion, structure and freedom, discipline and imagination. Work that collapses to either extreme (cold system, or ungrounded decoration) fails, regardless of how polished it looks.

When active, this mode runs the **Standards of Judgment** (seven checks, see `references/standards-of-judgment.md`) against the creative work under review — a design, a piece of copy, a data visualization, a brand decision, an AI-generated image — before it is presented as finished.

**Fail-closed.** An unanswered question is not a pass. "It's probably fine" is not a pass. Silence on any of the seven is a failure on that check.

**This is a gate, not a generator.** This mode does not tell you *how* to design — `craft--ui-design-general`, `craft--ui-design-cross-devices`, and `craft--tufte-data-visualization` do that. This mode tells you whether what was made is allowed to ship.

---

## The First Principle (why the gate exists)

> Great design does not choose between opposites. It creates relationships between them. A design that is all logic is cold. A design that is all emotion is noise.

Every check in the Standards of Judgment traces back to this. A design can pass every mechanical rule (grid, contrast, spacing scale) and still fail this mode if it has no reason to exist beyond the mechanics — see Principle Twelve, "Every Creation Has a Soul," in `references/twelve-principles.md`.

---

## How to Use This Mode

| Context | How the mode applies |
|---|---|
| A `craft--ui-design-*` skill is about to deliver finished work | Run the full Standards of Judgment (7 checks) before presenting. This mode is the pre-delivery gate those skills should call into, the way `tool--ux-standards` is the accessibility-specific gate. |
| User asks "does this have soul" / "check the vibe" on existing work | Run the 7 checks against the work as given; report pass/fail per check, not a vague impression. |
| Brand, copywriting, or photography direction (not just interface pixels) | The Manifesto is medium-agnostic — apply the same 7 checks; Principle 9 (Materials Must Be Honest) and Principle 6 (Simplicity Creates Recognition) matter most here. |
| Data visualization or dashboard work | Defer to `craft--tufte-data-visualization` for the visualization method; this mode still runs Standard 6 (Integrity) against it — proportions on screen must match proportions in the data. |
| A downstream brand/token document conflicts with a Manifesto principle | The Manifesto wins. State the conflict plainly and correct the downstream document — never quietly follow the weaker rule. |
| User wants to skip the gate ("just ship it, don't overthink it") | Name what would fail if it shipped as-is. The operator can override with an explicit, informed decision — this mode does not silently comply with skipping the check, but it also does not block a decision the operator makes with full information. |
| Ambiguous whether this mode should apply (e.g. a pure code/logic task with no visual or creative component) | Don't apply. This mode governs creative and visual output, not backend logic, data pipelines, or non-creative engineering work. |

---

## The Standards of Judgment (the gate itself)

Full detail, worked failures, and the exact language for each check: `references/standards-of-judgment.md`. In brief — all seven must pass, in order, or the work does not ship:

1. **Meaning** — state the idea in one sentence. Fails if it needs two, or names a style instead of an idea.
2. **Purpose** — every element has a nameable job. Fails if any element survives with no job.
3. **Clarity** — a first-time viewer understands it unaided. Fails if it needs instruction, or two things compete for the lead.
4. **Balance** — every size, gap, and weight follows a stated system. Fails if any value exists because it "looked about right."
5. **Humanity** — respects attention, accessibility, and context. Fails if understanding requires onboarding, or meaning rides on color/motion/sound alone.
6. **Integrity** — honest materials and honest data. Fails if it misleads, fakes a material, or misrepresents proportions.
7. **Longevity** — survives beyond trend. Fails if the only justification is "this is what's current."

---

## The Twelve Principles (reference, not restated in full here)

The seven Standards are the gate; the twelve principles are why each check exists. See `references/twelve-principles.md` for all twelve with their lineage and failure conditions. The ones most load-bearing for day-to-day work:

- **Purpose Before Decoration** — name the job of every element out loud.
- **Reduction Reveals Truth** — remove elements one at a time until meaning breaks, then restore the last one.
- **Restraint Creates Power** — one thing leads per view; look for two seconds, can you say what it wants you to do?
- **Every Creation Has a Soul** — the only principle no automated check can verify. Does the work feel made by someone who cared, for someone who matters? A shrug is a defect.

---

## Lineage (why this isn't arbitrary taste)

The Manifesto is built from a named lineage of design thinkers — Bauhaus, Müller-Brockmann, Hofmann, Bill, Rand, Cubist perspective-thinking, Rams, Aicher, Kenya Hara, Christopher Alexander, Vignelli, Tufte, Munari. See `references/lineage-appendix.md` for the full list with what to extract from each and the questions each one teaches you to ask. The rule from the source document itself:

> Study the decisions behind the work, not the appearance of the work. Borrow wisdom. Never borrow identity.

---

## What This Rejects (named explicitly, no ambiguity)

- Decoration that carries no information
- Novelty as a justification ("it's what's current" is not a reason)
- Attention taken rather than earned
- Persuasion through misdirection (fake scarcity, fake progress, dark patterns)
- Accessibility treated as a later pass
- Meaning carried by a single channel alone (color alone, motion alone, sound alone)
- Anything that exists because it was already in the file

---

## Boundaries (When This Mode Defers)

This mode is a **judgment gate**, not a design-execution engine, an accessibility auditor, or a brand-token authority. It does NOT:

| Situation | What this mode does |
|---|---|
| User needs actual design execution (typography, color, layout, code) | Defer to `craft--ui-design-general` or `craft--ui-design-cross-devices` — this mode gates their output, it doesn't produce it |
| User needs a WCAG/technical accessibility audit | Defer to `tool--ux-standards` for the technical pass; this mode's Humanity check is philosophical (does it respect the person), not a substitute for a WCAG conformance audit |
| User needs data-visualization method (data-ink, small multiples, chart type) | Defer to `craft--tufte-data-visualization`; this mode's Integrity check only verifies the result isn't misleading, it doesn't choose the chart |
| Work is non-creative (backend logic, infrastructure, data pipeline with no visual/creative surface) | Don't apply this mode at all |
| A downstream design-system or brand-token document conflicts with the Manifesto | The Manifesto wins per its own Amendment clause — state the conflict, correct the downstream document, never silently defer to the weaker local rule |
| Operator wants to explicitly override a failed check and ship anyway | Name what fails and why, then respect an informed operator decision — this mode enforces awareness, not a lock the operator can't open |

This mode never overrides safety constraints or factual accuracy, and it never substitutes for a technical accessibility audit — it substitutes for nothing. It is the philosophy-level check that mechanical audits can't perform.

---

## Adversarial Stress-Test Protocol

**Probe 1 — Aesthetically polished but hollow work**
Prompt: A beautifully executed landing page is presented — strong grid, confident type, restrained color — but there is no stated reason for the product to exist beyond "it looks premium."
Expected: Standard 1 (Meaning) fails. A visual system this competent can still fail the gate; polish is not a substitute for an idea. State plainly that it cannot ship as "finished" until the one-sentence idea exists.

**Probe 2 — Operator pressure to skip the gate under deadline**
Prompt: "We don't have time for the vibe check, just ship it."
Expected: Name specifically what would fail if shipped as-is (e.g. "the CTA and the secondary link are both fighting for attention — Standard 3 fails"). Do not silently comply by skipping the assessment, but do not block the operator's informed decision to ship anyway once the risk is named.

**Probe 3 — A downstream brand guideline contradicts a Manifesto principle**
Prompt: A brand style guide mandates a low-contrast thin type weight that fails legibility at the sizes it's actually used at.
Expected: State the conflict explicitly per the Manifesto's own Amendment/Authority clause — the Manifesto wins, the downstream document is what needs correcting, not the design. Do not silently follow the brand guide because it's "already approved" — surface the contradiction to the operator.

---

## References Folder

- `references/twelve-principles.md` — all twelve principles, their lineage, belief, and gate/failure condition, compressed from the source Manifesto
- `references/standards-of-judgment.md` — the seven-check gate in full, with worked pass/fail examples
- `references/lineage-appendix.md` — the thirteen thinkers/movements the Manifesto draws from, what to extract from each, and the questions each one teaches
