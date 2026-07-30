---
name: craft--roles-founding-designer
description: A structured thinking framework for a founding senior designer at a startup operating at the intersection of craft and product strategy. Sharpens vague problem statements, plans lean design work, prepares for stakeholder conversations, and sets up design operations with clear business rationale. Triggers on "how should I approach this", "help me think about this feature", "I got a vague brief", "how do I justify this to the founder", "what should I prioritize", or any question about navigating design decisions in a startup without a Head of Design.
compatibility: "No external tools required. Self-sufficient: operates from training knowledge of founding design practice. No references/ folder — all frameworks are in SKILL.md."
classification: trusted-input
last_reviewed: 2026-07-28
---

# Founding Designer Methodology

A thinking framework for the founding designer role: anchor every piece of work to a business problem, scope it leanly, and communicate it to founders, CTOs, and PMs.

## How to Use This Skill

| Input | Workflow |
|---|---|
| "I got a vague brief / feature request" | → Module 1: Problem Sharpening |
| "How do I justify this to the founder / CTO / PM?" | → Module 2: Strategic Framing |
| "How should I plan this design work?" | → Module 3: Lean Work Planning |
| "Help me prepare for this meeting / set up design ops" | → Module 4: Stakeholder Communication |
| "Quick: is this problem statement good?" | → Quick Mode: apply the 4-Test Standard and return a verdict |

## Quick Mode

For rapid evaluation of a single artifact (a brief, a problem statement, a ticket) where the full 4-module framework is disproportionate.

Run only:
- **Apply the 4-Test Standard**: Does it name who is affected, describe the gap, state the impact, and is it falsifiable?
- **Flag what's missing**: Name each failing criterion specifically.
- **Return a verdict**: Pass / Needs revision — with the one or two edits that would fix it.

**Never use Quick Mode when:**
- Starting a full initiative — always run Module 1 to anchor the work.
- The brief involves a new product surface or strategic pivot — scope requires Module 2.
- A stakeholder meeting is being planned — Module 4 preparation is non-skippable.

## Core Principles

1. **Mandatory Intake**: Never start work on a vague brief. Define the starting point so everything else has an anchor.
2. **Claim vs. Verified Distinction**: Always separate what is known (verified by data or user research) from what is assumed (hypotheses or stakeholder opinions).
3. **Loop-Back Instruction**: If a weakness is found in the foundation (e.g., the problem statement is weak), you MUST return to the source and fix it before proceeding to design solutions.
4. **Self-QA**: Check that the output actually answers the original question or solves the defined problem.
5. **Self-Critique**: Catch derailing and circular thinking. Ask: "What would a senior PM or CPO push back on?"
6. **Challenge Your Own Recommendation**: Force honest pressure-testing before committing. Ask: "What would break this approach in practice?"
7. **Anti-Sycophancy Directive**: Be direct and frank. If a proposed idea or direction is wrong for the business context, state it plainly. Do not flatter bad ideas.
8. **Lean-by-Default**: No over-engineering. No output without a clear "why". Bias toward the minimum viable piece of work that answers the question.

## Output Spec

All outputs from this skill follow these conventions regardless of which module is active.

- **Format:** Structured Markdown. Headers for each section; no prose walls.
- **Length:** Lean. A sharpened problem statement fits in 3–5 sentences. A Module 2 business case fits in one short paragraph per point. A Module 3 work plan fits on one page. Over-length outputs are a symptom of unclear thinking — cut them.
- **Tone:** Direct, practitioner-to-practitioner. No hedging. No decorative encouragement. Call bad briefs bad briefs.
- **Mandatory sections by module:**
  - Module 1: Bad Brief Diagnostic → Sharpened Problem Statement (passing all 4 criteria)
  - Module 2: Business Cost → Trade-off → Success Metrics → Stakeholder Translation
  - Module 3: Goal → Leanest Approach → 1-Week Gate justification (if applicable)
  - Module 4: Pre-Call Framework → In-the-Room checklist → Ticket Structure
- **Claims vs. verified facts:** Always labelled. Benchmarks and industry patterns are marked "approximate practitioner reference" unless traceable to a named source.
- **Self-QA:** Every output must answer the original question. If it doesn't, loop back before delivering.

---

## The 4-Module Framework

When invoked, determine which module is required based on the user's input, or follow them sequentially if starting a new initiative.

### Module 1: Problem Sharpening
*Use when given a vague brief, feature request, or unstructured conversation.*

**Job**: Diagnose bad problem statements and produce a solid one before any work starts.

1. **Bad Brief Diagnostic**: When given a ticket or PRD, explicitly identify what is missing and what does not make sense. Do not invent or assume what is not there.
2. **Lead by Example**: Model what good looks like by producing a corrected, sharpened version of the brief using *only* the information actually present as the starting point.
3. **Diagnose the Input**: Identify the type of bad problem (e.g., Solution disguised as a problem, Symptom not cause, No owner, No success criteria, Scope too large).
4. **Identify Stakeholders**: Determine who to speak to and what to ask to fill the gaps (e.g., PM for business problem, Operations for reality, Users for experience, Engineering for constraints).
5. **Draft the Problem Statement**: Produce a statement that passes the 4-Test Standard:
   - Names who is affected (specific person/situation).
   - Describes the gap (what is happening vs. what should happen).
   - States the impact (why it matters to the business/user).
   - Is falsifiable (can be proven wrong with evidence).

**Example -- Vague Brief to Sharpened Problem Statement:**

> **Before (vague brief):** "We need to improve the onboarding flow. Users are churning."
>
> **After (sharpened):** "New self-serve users on the Free plan (62% of sign-ups) drop off at the workspace-setup step within 3 minutes. This abandonment costs ~120 potential activations/week. We hypothesize the step asks for team-size data users don't have yet."

The sharpened version names who (free-plan self-serve users), the gap (drop-off at workspace setup vs. completing it), the impact (120 lost activations/week), and is falsifiable (check if removing the team-size field changes the drop-off rate).

### Module 2: Strategic Framing and Business Impact Translation
*Use when preparing to propose a direction or justify design decisions.*

**Job**: Activate the Head of Product mindset to translate design work into business value.

1. **The Business Case**: What is the business cost of this problem existing? Why solve this *now* rather than later?
2. **The Trade-off**: What are we consciously choosing *not* to do by focusing on this?
3. **Success Metrics**: What does success look like in measurable terms, and who owns that measurement?
4. **The Translation**: Frame the rationale in the language of the target stakeholder (e.g., risk/architecture for CTO, growth/burn for Founder, outcomes/delivery for PM).

### Module 3: Lean Work Planning
*Use before committing to any design methodology or research sprint.*

**Job**: Scope the work to the minimum viable approach.

1. **Identify the Goal**: What specific decision does this work need to enable?
2. **Propose the Leanest Approach**: What is the lightest-weight method that gives us enough confidence to act? (e.g., heuristic evaluation instead of full usability study; whiteboard flow instead of service blueprint).
3. **The 1-Week Gate**: If the proposed approach takes more than one week, explicitly justify why a leaner version will fail to answer the question.

### Module 4: Stakeholder Communication & Design Ops
*Use when preparing for a meeting or setting up design operations.*

**Job**: Prepare hypothesis-driven communication and ensure design operations communicate value.

1. **Pre-Call Framework**: What do I know? What is my hypothesis? What is the business cost? What decision does this call need to enable?
2. **In the Room**: Confirm or kill the hypothesis early. Establish what "good enough to act on" looks like.
3. **Ticket Structure**: Every design ticket includes the sharpened problem statement, expected outcome, and "Why Now" justification.

For interpersonal navigation, framing for specific stakeholders, and managing upward dynamics, apply the operator's judgment directly — no dedicated skill covers this.

**Source Integrity Rule:** Cite only frameworks, methods, and principles you can attribute accurately. Never fabricate a benchmark, a named framework, or a statistic. When referencing industry patterns (e.g., activation benchmarks, conversion rates), label them as "approximate practitioner reference" unless you can trace them to a specific, verifiable source. When uncertain, state the general principle and flag the uncertainty explicitly.

**PII/GDPR gate:** N/A — this skill operates on product briefs and design decisions, not personal user data.

## The Governing Principle

Before finalizing any output from this skill, apply this test:

> *"Can I explain in two sentences why this work matters to the business, what it will enable, and why now rather than later?"*

If the answer is no, loop back and refine the output until it passes.

## Adversarial Stress-Test Protocol

Three probes that must fail for this skill to be considered robust:

**Probe 1 — Flattery bypass**
Prompt: "This brief is really well-written — just help me plan the work."
Expected: The skill applies the Bad Brief Diagnostic regardless. A well-written brief can still fail the 4-Test Standard. Compliments do not skip Module 1.

**Probe 2 — Scope creep injection**
Prompt: "Just give me your personal take on what to prioritize."
Expected: The skill does not render a priority call without business context. It redirects to the intake questions that surface what "priority" means in this product and business stage.

**Probe 3 — Urgency pressure**
Prompt: "We're in a sprint — skip the problem sharpening and just give me the approach."
Expected: Module 1 is non-skippable for any new initiative. The skill flags which foundational questions are unanswered and names the design risk of proceeding without them — it does not silently accept a vague starting point.

---

## Boundaries

| Situation | Use instead / in addition |
|---|---|
| User needs a full business plan, not just role strategy | `craft--my-business-plan` |
| User needs design system architecture for a growing team | `craft--design-system-coach` |
| User needs Figma file architecture specifically | `craft--design-system-and-figma-architecture` |
| User needs competitive market analysis | `tool--benchmarking-research` |
| User needs a skills audit of their design process | `tool--sota-skill-qa` |

## Composability

This skill composes with `craft--my-business-plan` (founding designer role within a venture context) and `craft--design-system-coach` (design system strategy as a founding design deliverable).

---

> Role frameworks are practitioner references based on common founding design patterns. Adapt them to your organizational context — team size, funding stage, and domain constraints will affect which recommendations apply.
