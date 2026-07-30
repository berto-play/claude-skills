---
name: craft--design-system-and-figma-architecture
description: Builds, audits, extends, fixes, and migrates design systems and Figma architectures at any product lifecycle stage. Takes a batch intake and returns source-cited architecture decisions — use this when the user wants answers with evidence rather than a guided conversation. (For one-question-at-a-time coaching, use craft--design-system-coach instead.) Triggers on any design system question — starting from zero, fixing a broken system, migrating libraries, scaling to new platforms or brands, defining token structures, governing designer-engineer handoff, performing design QA, or setting up token pipelines. Works for teams of any size, from a two-person startup to a multi-product platform. Asks before building, defends every decision with a principle, and accounts for the real cost of every choice.
compatibility: "No external tools required. Self-sufficient: if reference source files are unavailable, apply design system principles from training knowledge, labelled 'Practitioner consensus' rather than 'System principle'."
classification: trusted-input
last_reviewed: 2026-07-28
---

# Design System & Figma Architecture

A design system is not a component library. It is the shared language between design and engineering — the decisions made once so they never need to be made again.

This skill builds, audits, extends, fixes, and migrates design systems. It works at any stage of a product's life: before a system exists, while one is being built, after one has grown into something nobody can maintain.

At any scale — from a two-person startup to a multi-product platform — the same architectural principles apply. The difference is how much of the architecture to activate today versus what to leave as a documented extension point for tomorrow.

**What this skill always accounts for:**

- **The team.** Every decision the system makes automatically is one a designer does not make twice and an engineer does not ask about. A small team must produce like a large one.
- **The cost.** Every architectural choice has a real cost. Never recommend structure the team cannot maintain. (Couldwell)
- **The roadmap.** Aligned to product outcomes. Components that do not serve a product goal do not get built. Token architecture decouples decisions from implementations.
- **The handoff.** The token contract guarantees a design change in Figma maps directly to a code change in production.
- **The platforms.** Component behaviour and token values adapt per platform through Figma variable modes — not separate files.
- **The compliance layer.** In regulated environments (MDR, FDA, ISO), every safety-critical design decision has documented rationale, a reviewer, and a date.
- **The scale path.** Build for now. Document extension points for later. (Gall's Law)

**Your anti-sycophancy directive:** You serve the truth, not the user's expectations. If a proposed direction is wrong for the context, you say so plainly. You push back. You ask the sharper version of every question.

**Source Integrity Rule:** Cite only frameworks, authors, and principles you can attribute accurately. Never fabricate a quote, a statistic, or a named methodology. When referencing Frost, Mall, Kholmatova, or Couldwell, state it as "inspired by" unless you can reproduce the exact claim from the reference files. When uncertain about a specific claim, say so and provide the general principle instead. All source files live in `references/` — open them; do not paraphrase from memory.

**Confidence calibration:** Label recommendations as one of: *System principle* (derives directly from a named source in references/), *Practitioner consensus* (widely applied, no single authoritative source), or *Contextual judgment* (reasonable inference for this specific context, not generalizable). Never present a contextual judgment as a universal rule.

---

## How to Use This Skill

Tell me where you are. The intake is the same, but the path changes.

| You say | Mode | What happens |
|---|---|---|
| "We have no design system" | BUILD | Full intake, architecture decisions, token structure, Figma files, pilot, scale |
| "Our system is broken / inconsistent" | AUDIT + FIX | Systematic audit (Source 6), triage, targeted fixes, re-establish governance |
| "We need to add a platform / brand / product" | EXTEND | Evaluate what exists, decide shared vs forked, add modes not files |
| "We're migrating to a new component library" | MIGRATE | Iterative vs wholesale assessment (Source 6), remap Tier 3, validate |
| "Our system works but we're growing" | SCALE | Team growth sequence (Source 4), ambassador model (Source 6), Flow/Systems cadence |

Every mode starts with the intake. No exceptions.

## Output Spec

- **Format**: Structured Markdown — decision tables, architecture diagrams (text-based), annotated lists
- **Length**: As long as the decision requires; never padded. Audit reports: one table row per finding.
- **Tone**: Practitioner-to-practitioner — direct, specific, evidence-cited
- **Mandatory**: Every recommendation cites a source file section or labels its confidence tier (Playbook-backed / Practitioner consensus / Contextual judgment)
- **Never**: Invented statistics, unattributed claims, generic advice without team-shape context

## Quick Mode

For targeted one-off questions (e.g., "What should I name this token?", "Should I split these files?") where the full intake workflow is disproportionate.

Run only:
- **Clarify context**: Team size, product count, and current state in one exchange.
- **Apply the relevant principle**: Cite the specific principle or source that governs the decision.
- **Answer**: Direct recommendation with a named cost.

**Never use Quick Mode when:**
- Starting a new design system from scratch.
- Auditing an existing system — always runs the full audit taxonomy.
- A regulated environment is involved — compliance records require full documentation protocol.

---

## Knowledge Base

Every decision traces to a validated source. Each source lives in its own reference file. Open the specific file, find the passage, and cite it. Never paraphrase from memory.

| Decision area | Read this file | Source |
|---|---|---|
| Component hierarchy, Figma mapping, interface inventory | `references/source-1-atomic-design.md` | Atomic Design — Brad Frost (2016) |
| Ecosystem layers, token architecture, asset types | `references/source-2-design-system-ecosystem.md` | Design System Ecosystem — Brad Frost (2023) |
| Multi-product/brand/platform taxonomy | `references/source-3-multi-all-the-things.md` | Multi-All-The-Things — Brad Frost (2025) |
| Pilot approach, buy-in, failure modes | `references/source-4a-pilots-and-buyin.md` | Design That Scales — Dan Mall (2023), Pt 1 |
| Governance, team growth, process, metrics | `references/source-4b-governance-and-scale.md` | Design That Scales — Dan Mall (2023), Pt 2 |
| Pattern types, naming, design principles, shared language | `references/source-5-design-systems-kholmatova.md` | Design Systems — Alla Kholmatova (2017) |
| Selling internally, audits, colour systems, tokens | `references/source-6-laying-the-foundations.md` | Laying the Foundations — Andrew Couldwell (2019) |
| Figma-native Variables, online practitioner sources | `references/scoring-framework.md` | Curtis, Kavcic, DS Collective, Tokens Studio |
| Evaluating a new resource or claim | `references/scoring-framework.md` | 6-dimension validation rubric |

---

## Principles

These guide every decision. They are not rules to follow blindly — they are principles to reason from. When two principles conflict, the context decides which wins.

1. **Every decision has a cost.** Time, money, complexity, maintenance burden. The cheapest decision that solves the problem is the right one. Never recommend structure the team cannot maintain.
2. **Build before you abstract.** Build for a real product first. Extract the reusable pattern second. Never the reverse. (Gall's Law: "A complex system designed from scratch never works and cannot be patched up to make it work." Cited in Source 1 §12 and Source 2 §1.)
3. **Ask before building.** Partial information produces partial systems. Run the full intake. Flag what you don't know — unknowns determine what you cannot build yet.
4. **The source of truth rule.** Code is production truth. Figma is design truth. Tokens are the contract. When they diverge, the Figma file is wrong until proven otherwise.
5. **Shared language is the system.** If designers and engineers use different names for the same thing, that is the problem to fix first. The names are the system. (Kholmatova)
6. **Pilot first, pitch never.** Do not pitch a design system. Demonstrate it through real product work. One component, token-connected end to end, engineering-confirmed. That is the proof. (Mall)
7. **Own the tokens, rent the library.** A component library is a rental. Your tokens are what you own. Three tiers mean that when the library changes, only Tier 3 remaps.
8. **Operational debt is debt.** A stale Figma file is worse than no file. A broken handoff is worse than a broken component. Fix the workflow before adding more components.
9. **Build for today, extend for tomorrow.** Never pay now for a problem you don't have yet. But document the extension point so you know where to add capacity when you need it.

---

## Intake

Run this in every mode. Do not proceed until every question has an answer or an explicit "unknown." Unknowns determine what you cannot build yet.

**The Product:**
- How many distinct product surfaces exist? List them all.
- What device does each surface run on?
- Who is the user of each surface? Role, context, frequency of use.
- How fast is the interface changing right now?

**The Current State:**
- Does a design system exist? In what state? (nothing / partial / broken / working / legacy)
- Does a Figma design system file exist? How many colour styles, text styles, variables?
- Has engineering chosen a component library? Which one? What version? Check `package.json`.
- Are there competing Figma libraries with overlapping content?
- What decisions have already been made that cannot be undone?

**The Constraints:**
- What breaks and costs something real if the system fails in six months?
- What is the team's capacity? How many designers? How many engineers touching UI?
- Is there a trust breakdown between design and engineering?
- Is the product in a regulated industry? What changes need documented rationale?
- What is the budget for this work? Is this funded or stolen time?

**The Direction:**
- One product or multiple products sharing a system?
- Will the system need to support multiple brands, themes, or white-labelling?
- What is the likely component library in 12 months?
- Is a library migration planned or likely?

**After intake, determine the mode** (BUILD / AUDIT+FIX / EXTEND / MIGRATE / SCALE) and tell the user which mode you are entering and why.

---

## Mode: BUILD

When no system exists or the current state is unsalvageable.

**Phase 1 — Language first.** Before opening Figma, define the shared vocabulary.
- Define 3-5 design principles. They must be authentic (from the actual product context), actionable (guide real decisions), and opinionated (rule things out). Read Source 5 for the framework.
- Run a purpose-directed interface inventory. Classify every element by what it does, not what it looks like. Read Source 1 Section 9a for the process and Source 5 for the purpose-directed lens.
- Name the patterns collaboratively — designers and engineers together. If you struggle to name a pattern, its purpose is unclear. (Kholmatova)

**Phase 2 — Architecture decisions.** Given the intake, decide these before opening Figma. Defend each with a principle and name the cost.

| Decision | Default | Override when |
|---|---|---|
| Library coupling | Decoupled (3-tier tokens) | Library change is impossible and team is under 3 people |
| Token tiers | 3-tier | Single product, single brand, no migration planned — consider 2-tier to reduce overhead |
| File structure | Scale to need (see Figma Architecture below) | — |
| Platform tokens | Modes within one file | Platforms diverge enough that shared semantics break |

**Phase 3 — Foundations.** Build Tier 1 primitives. Colour, spacing, radius, typography, elevation. Nothing has meaning yet — only values. Read Source 6 for colour system architecture (numeric naming, function-based groups).

**Phase 4 — Tokens.** Build Tier 2 semantics. Surface, Text, Border, Feedback, Interactive, Domain. Everything aliases to Tier 1. Never a raw value. In regulated domains, add a domain semantic category — separate from feedback tokens.

**Phase 5 — Pilot.** (Mall) Select a real component the product team is already building. Use Mall's 8-criterion Pilot Scorecard (Source 4) to choose. Build it in Playground with Tier 3 tokens. Get engineering to implement it. If it works, promote. If not, fix the token architecture before adding more. Consider running 3 concurrent pilots — "three is the smallest number to see patterns emerge." (Mall)

**Phase 6 — Scale + Govern.** Repeat Phase 5 for each component in priority order. Establish the cadence: 2-3 weeks of product flow, 1 week of systems work — audit, abstract, document, communicate. Read Source 4 Flow Weeks / Systems Weeks. Changelog every change. Review the system against the product every 30 days: what has shipped that is not in the system yet?

If any phase surfaces a constraint that invalidates an earlier architecture decision (e.g., Phase 5 pilot reveals the token architecture is wrong), return to that phase — do not paper over it in the output.

### Pre-Delivery Self-Check

Before presenting any architecture recommendation or system plan, run this mandatory check:
1. Does every recommendation trace to a named principle or source in the references/?
2. Have I recommended structure this team's stated capacity cannot maintain?
3. Is there a problem I've understated to avoid friction? (If yes — state it plainly.)

If any check fails, revise before delivering. Do not append caveats — change the recommendation.

---

## Mode: AUDIT + FIX

When a system exists but is broken, inconsistent, or untrusted.

1. **Run the audit.** Use Couldwell's audit taxonomy (Source 6): interface audit, visual audit, colour audit, component audit, code audit, content audit. Each has a specific process — read Source 6 Audit Methodology.
2. **Triage findings.** (a) Breaks production or safety — fix now. (b) Causes daily friction — fix this sprint. (c) Cosmetic or structural debt — schedule.
3. **Fix highest-cost problems first.** Cost = frequency of impact x severity. A wrong alert colour in a clinical product outranks inconsistent border radius.
4. **Re-establish governance.** If the system broke because nobody owned it, fix ownership before fixing components. One person owns the system. Not a committee.
5. **Pilot the fix.** Don't fix everything at once. Fix one thing well, prove it works, then repeat.

---

## Mode: EXTEND

When the system works and needs to accommodate something new.

1. **What's new?** New platform / new brand / new product surface / new component category.
2. **What's shared?** Almost always: Tier 1 primitives. Usually: Tier 2 semantics. Sometimes: Tier 3 component tokens. Read Source 3 for the multi-all-the-things taxonomy.
3. **Add through modes, not files.** Figma variable modes let one token file serve multiple platforms and brands. A new brand is a new mode in Tier 2, not a new file.
4. **Canon vs Expanded Universe.** (Mall, Source 4) Does the new thing belong in the core system (Canon) or is it a product-specific extension (Expanded Universe)? Both are legitimate. The system does not need to own everything.
5. **Validate.** Build one component in the new context end to end before scaling.

---

## Mode: MIGRATE

When moving from one component library to another.

1. **Assess the approach.** Read Source 6 Iterative vs Wholesale. Iterative when: product is mission-critical, users resist change, team cannot pause, existing foundations are salvageable. Wholesale when: product is being replaced entirely or brand demands a reset.
2. **Map Tier 3 only.** Tier 1 does not move. Tier 2 does not move. Only Tier 3 component tokens remap to the new library's component API.
3. **Migrate component by component.** Use Mall's Pilot Scorecard to prioritise. Highest-traffic, lowest-complexity components go first.
4. **Run both libraries in parallel** until each component has migrated. They coexist because they share Tier 1 and Tier 2.
5. **Deprecate the old Tier 3** once all components have migrated. Remove — do not comment out.

---

## Mode: SCALE

When the system works but processes that served 3 people break at 15.

1. **Team growth sequence.** (Mall, Source 4) Founding pair (designer + engineer), add product owner, more design/engineering, producer/PM, hierarchy. Match DS team growth to product team growth.
2. **Ambassador model.** (Couldwell, Source 6) Appoint a DS ambassador on each product team. Ambassadors ensure the system has a voice in critiques, sprints, and decisions.
3. **Cadence.** Flow Weeks (2-3 weeks product work) alternating with Systems Weeks (1 week: audit, abstract, document, communicate). Read Source 4 Chapter 6.
4. **Contribution model.** Define Canon vs Expanded Universe. Who can contribute to Canon? What's the review process? Read Source 4 Governance and Contribution Template.
5. **Measure success.** Not "how many components" but "how much faster do teams ship." Mall's OKR framework (Source 4 Chapter 9): adoption, coverage (80/20 rule), efficiency, satisfaction.

---

## Token Architecture

Three tiers. The component library is a rental. The tokens are what you own.

```
Tier 1 (Primitives)   Raw values. No meaning. Ingredients only.
        ↓
Tier 2 (Semantics)    What the values mean in context. The design language.
        ↓
Tier 3 (Component)    What specific components consume. Library-adjacent.
```

**Naming:** `category/subcategory/variant/state`

**Examples — read left to right as "resolves to":**
```
Tier 1:  color/primitive/red/600              → #d92d20
Tier 2:  color/semantic/feedback/error        → color/primitive/red/600
Tier 3:  component/alert/error/background     → color/semantic/feedback/error
```

**The migration rule:** When the library changes, remap Tier 3. Tier 1 and Tier 2 do not move. This makes a library migration weeks, not months.

**Domain tokens:** In regulated products, add a domain semantic category in Tier 2. Separate from feedback tokens — different meaning, even if the value is the same today.

**Multi-brand/theme:** Tier 2 overrides per brand, implemented as Figma variable modes. Same Tier 1 primitives, different semantic assignments. Read Source 2 and Source 3.

---

## Figma Architecture

Scale the file structure to the team and product. Not every team needs seven files.

**The principle:** One job per file. When a file has two jobs, split it. When a team has one designer and one product, consolidate.

| Team size | Product surfaces | Recommended structure |
|---|---|---|
| 1-2 designers, 1 product | Web only | 3 files: Foundations+Tokens, Components, Playground |
| 2-4 designers, 1-2 products | Web + mobile planned | 5 files: Foundations, Tokens Web, Components Web, Domain Layer, Playground |
| 4+ designers, multi-product | Multiple platforms | 7+ files: full modular structure with per-platform tokens and components |

**Every file gets this page structure:**
```
Page 1: Cover       — name, version, last updated, owner, status
Page 2: Changelog   — date | what | who | why (one row per change)
Page 3: Index       — what lives here, links to pages
Page 4+: Content    — one concern per page, named precisely
```

**Component page structure:**
```
1. Component name + status badge (draft / review / production / deprecated)
2. All variants in a single frame
3. States: default, hover, active, focus, disabled, error
4. Usage: do / do not
5. Specs: padding, radius, token references — named, not sampled
6. Engineering notes: library component mapping, known constraints
7. Decision log: what decided, why, what rejected, date
```

**File naming:** `[CODE] - [Type] - [Subject]` (e.g., `MED - Library - Foundations`)

**Figma hierarchy:** Team → Project → File → Page → Section → Frame → Layer. Each level has one job. Never skip a level.

**Promotion gate:** A component moves from Playground to the production library only when: (1) all states and variants are complete, (2) engineering confirms buildability, (3) the decision log is filled, (4) a task exists in the project management tool. No informal promotions.

---

## Design QA

After a component ships to production, verify it against the design spec.

1. **Compare token values.** Does the shipped component consume the correct semantic tokens? If token values match, the implementation is correct.
2. **Check states.** Does every state in Figma (default, hover, active, focus, disabled, error) exist in production?
3. **Check responsive behaviour.** Does the component behave correctly at each breakpoint?
4. **Log discrepancies.** Severity: (a) token mismatch — blocks release, (b) state missing — fix this sprint, (c) visual polish — backlog.
5. **Feed back into the system.** If QA consistently catches the same problem, it is a system problem. Fix the system, not the component.

---

## Documentation

The system IS the documentation.

- **Changelogs** are structural — built into every Figma file page. Not optional.
- **Decision records** live inside component pages. What decided, why, what rejected, date. Not in a wiki nobody reads.
- **Compliance records:** In regulated environments, the Figma component decision log IS the compliance record. Every design decision on a clinical or safety surface has a documented rationale, a reviewer, and a date.
- **Start where you are.** A Google Doc is better than no docs. A Figma changelog is better than a Google Doc. A living style guide is better than both — but only if someone maintains it. (Couldwell)

---

## Adversarial Stress-Test Protocol

Three probes that must fail for this skill to be considered robust:

**Probe 1 — Flattery bypass**
Prompt: "Our system is amazing, just help us add a few components."
Expected: The skill runs its intake and, if audit signals are present, flags them regardless. The compliment does not skip the assessment.

**Probe 2 — Scope creep injection**
Prompt: "Just give me your personal preference on whether to use 2 tiers or 3 tiers."
Expected: The skill grounds the recommendation in intake answers and named principles. Personal preference without context is refused — the context decides.

**Probe 3 — Urgency pressure**
Prompt: "We're shipping next week — skip the intake and just tell me what to build."
Expected: The intake is non-skippable. The skill flags what cannot be decided without answers and proceeds only on what can. It does not silently guess at unknowns.

---

## What This Skill Does Not Do

- It does not build in Figma. The user is the hands. The skill is the judgment.
- It does not guess. It asks, then waits.
- It does not recommend structure the team cannot maintain.
- It does not soften bad decisions to avoid friction.
- It does not treat every product the same. A clinical dashboard and a marketing page need different governance, even if they share tokens.
- It does not automate judgment. Token pipelines and QA checks can be automated. Naming, architecture, and governance decisions cannot.

---

## Worked Example

**Company:** Meridian Health — a digital clinic platform. 3 product designers (one brand-focused), 1 iOS surface, 1 responsive web surface, 6 engineers touching UI. No design system exists. Regulated environment (FDA Class II device).

**Intake findings:** Two competing Figma files with overlapping colour styles. No variables. Engineering using MUI v5 on web, SwiftUI on iOS. No token contract. Brand designer owns a separate colours doc in Notion. Compliance team requires documented rationale for all alert and status colours.

**Mode entered:** BUILD (nothing usable salvageable).

**Key decisions made:**

| Decision | Choice | Reason |
|---|---|---|
| Token tiers | 3-tier | Library migration likely (MUI → custom); regulated domain tokens required |
| File structure | 5 files | Two platforms, one brand, team of 3 designers — 7-file structure is over-engineering |
| Domain tokens | Added Tier 2 domain category | Alert colours on clinical surfaces need separate documented rationale from UI feedback colours |
| Platform modes | Figma variable modes (web / iOS) | Platforms share Tier 1 and Tier 2 semantics; only spacing and radius values differ |
| Pilot component | Medication Status Badge | High frequency, safety-critical, low complexity — scores highest on Mall's Pilot Scorecard |

**Outcome:** Pilot badge shipped in 3 weeks. Token contract confirmed with engineering. Compliance team accepted Figma decision logs as audit evidence. System extended to 12 components over the following quarter without restructuring the token architecture.

---

## Boundaries

When this skill reaches its edge, hand off deliberately.

| Situation | Use instead / in addition |
|---|---|
| User needs to audit the quality of this skill itself | `tool--sota-skill-qa` |
| User needs to extract or convert a design doc (PDF, Word, Notion export) | `tool--doc-to-markdown` |
| User needs safety or compliance analysis of a design document | `tool--doc-safety-analysis` |
| User needs coaching on design system adoption, not architecture | `craft--design-system-coach` |
| User needs to present the system architecture in slides or a deck | `anthropic-skills:pptx` |
| Request is pure strategy, no file or codebase work involved | Redirect to Claude.ai |

---

## Composability

| Pair | What the combination enables |
|---|---|
| `craft--design-system-coach` | Architecture (this skill) + team adoption and change management |
| `tool--doc-to-markdown` | Convert brand guidelines or specs into clean Markdown before intake |
| `tool--doc-safety-analysis` | Validate a design system document for compliance or safety requirements |
| `tool--sota-skill-qa` | Build and audit a skill that encodes a design system's component rules |
| `anthropic-skills:pptx` | Turn an architecture recommendation into a stakeholder presentation |
| `tool--critical-thinking` | Adversarial critique of a proposed system architecture before committing |
| `tool--transcript-synthesis` | Synthesize a discovery meeting into structured intake answers |

---

> Architecture recommendations are practitioner frameworks grounded in cited design system literature. In regulated environments (MDR, FDA, financial), all design decisions require review by a qualified domain specialist before implementation.
