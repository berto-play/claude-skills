# Source 6: Laying the Foundations -- Andrew Couldwell (2019)

Confidence: HIGH -- Full text extracted and verified.

---

## Table of Contents

- [What This Source Adds](#what-this-source-adds)
- [Selling a Design System Internally](#selling-a-design-system-internally)
  - [Stakeholder-Specific Pitch Strategies](#stakeholder-specific-pitch-strategies)
  - [The Selling Sequence](#the-selling-sequence)
  - [Office Politics as a Design Constraint](#office-politics-as-a-design-constraint)
- [Digital Foundations: The Brand Layer Above the System](#digital-foundations-the-brand-layer-above-the-system)
  - [What Digital Foundations Cover](#what-digital-foundations-cover)
  - [Digital Foundations vs Design System Documentation](#digital-foundations-vs-design-system-documentation)
  - [Marketing vs Product: Two Systems, One Brand](#marketing-vs-product-two-systems-one-brand)
- [The Foundations Model: Product Design Variant](#the-foundations-model-product-design-variant)
- [Iterative vs Wholesale Approaches](#iterative-vs-wholesale-approaches)
  - [Decision Criteria for Choosing an Approach](#decision-criteria-for-choosing-an-approach)
  - [The Four Paths](#the-four-paths)
  - [The Disruption Factor](#the-disruption-factor)
  - [The Frankenstein Monster Effect](#the-frankenstein-monster-effect)
  - [Ordering Iterative Work](#ordering-iterative-work)
- [Audit Methodology](#audit-methodology)
  - [The Audit Taxonomy](#the-audit-taxonomy)
  - [Colour Audit Process](#colour-audit-process)
  - [Visual Audit Process](#visual-audit-process)
  - [Post-Audit Consolidation](#post-audit-consolidation)
- [Colour System Architecture](#colour-system-architecture)
  - [Colour Groups by Function](#colour-groups-by-function)
  - [Numeric Naming Scale for Colour](#numeric-naming-scale-for-colour)
  - [Documenting Colour Intent](#documenting-colour-intent)
- [Design Tokens: Practical Implementation](#design-tokens-practical-implementation)
  - [Sass Variable vs Design Token](#sass-variable-vs-design-token)
  - [Combining Variables and Tokens](#combining-variables-and-tokens)
  - [Token Scope Beyond Colour](#token-scope-beyond-colour)
- [Documentation Strategy](#documentation-strategy)
  - [The Distinction: UI Kit vs Style Guide vs Design System](#the-distinction-ui-kit-vs-style-guide-vs-design-system)
  - [Documentation Tooling Progression](#documentation-tooling-progression)
  - [What to Document Per Category](#what-to-document-per-category)
- [Governance: Guardians, Ambassadors, and Leads](#governance-guardians-ambassadors-and-leads)
  - [Design System Lead Responsibilities](#design-system-lead-responsibilities)
  - [Ambassador Model for Multi-Team Orgs](#ambassador-model-for-multi-team-orgs)
  - [First Responder Rotation](#first-responder-rotation)
  - [Keeping the Team in the Loop](#keeping-the-team-in-the-loop)

---

## What This Source Adds

Sources 1-5 already cover: atomic design hierarchy (Frost), ecosystem architecture and token tiers (Frost), multi-product scaling (Frost), pilot-based buy-in and hot potato process (Mall), pattern types/naming/shared language (Kholmatova).

Couldwell's unique contributions are operational and tactical. Where other sources describe what a design system is or how to structure it architecturally, this book provides the practitioner playbook for getting one built inside an actual organisation: how to pitch it to different audiences, how to choose between iterative and wholesale approaches, how to run colour and visual audits step by step, how to implement design tokens in code, and how to govern the system after launch.

---

## Selling a Design System Internally

Mall (Source 4) covers the pilot approach -- prove value by shipping real product work. Couldwell addresses the earlier, more political problem: how to get permission and allies before you have anything to show.

### Stakeholder-Specific Pitch Strategies

**Product managers / leaders** -- Sell on shipping speed, faster iteration cycles, staff retention, and recruiting. A well-run system unifies teams and creates healthier work culture.

**Engineers** -- Sell on unified code, version control, consistency, performance, naming conventions. If not fluent in their language, co-present with a developer.

**Designers** -- Sell on output quality, not constraints. Emphasise that the system curbs rogue elements (buttons, inputs) and frees them for layout, flows, and problem-solving. Do not frame it as a threat to creativity.

**All parties** -- Pitch as collaborative. A system designed by a few people and closed to changes will fail.

### The Selling Sequence

1. Start small -- never lead with "let's build a design system"
2. Arm yourself with data, problem statements, and industry examples
3. Identify specific problems the system will solve (see Audit Methodology)
4. Do impressive design/build work to get people talking
5. Build collaborators one person at a time
6. Create a compelling name and presentation
7. Tailor the pitch per audience
8. Earn trust through demonstrated results
9. Never stop advocating -- selling is permanent

Key insight: "Selling a design system isn't a stage of the process. It's ongoing." One powerful opposing stakeholder can kill the system. Broad support is insurance against organisational change.

### Office Politics as a Design Constraint

Understand what is really driving each stakeholder (business metrics, customer happiness, or self-interest) and frame the system's value in their terms.

---

## Digital Foundations: The Brand Layer Above the System

No other source addresses the relationship between brand guidelines and design system documentation as a two-layer architecture. Couldwell names the brand layer "Digital Foundations" and positions it as a prerequisite that sits above and feeds into any number of product-specific design systems.

### What Digital Foundations Cover

- Mission, values, and design/engineering principles
- Brand identity (logo, typography, colour, photography, illustration, creative direction, motion)
- Brand tone of voice and copywriting (including shared vocabulary -- e.g. "team" vs "employee", "members" vs "customers")
- Formatting standards (sentence case vs title case, date/time/currency by region, Oxford comma, "and" vs "&")
- Accessibility and inclusivity requirements
- Responsive design grids and breakpoints
- Process documentation (design-to-code workflow, file naming, responsibilities)

### Digital Foundations vs Design System Documentation

| Digital Foundations | Design System Documentation |
|---|---|
| Brand-level, spanning all digital properties | Product-specific |
| Covers identity, formatting, shared vocabulary | Covers components and patterns built from the foundations |
| General enough for all products at the company | Specific to one website or product |
| Example: IBM Design Language | Example: IBM Carbon |
| A company has one set | A company may have multiple |

Operational value: This two-layer separation means you can have multiple design systems (marketing site, product app, internal tools) that all inherit from the same Digital Foundations. The brand stays consistent even when the component libraries diverge.

### Marketing vs Product: Two Systems, One Brand

Product design and marketing design are distinct disciplines with different objectives, use cases, and audiences. They should not share a single design system, but they should share Digital Foundations.

Example -- Dropbox: the bold colours, extreme spacing, and dramatic typeface of their marketing site would not work in their product. Yet both share brand blue, the logo, sentence case, and certain guiding principles. Digital Foundations capture what is inherent across all properties; design systems capture what is specific to each.

---

## The Foundations Model: Product Design Variant

The basic Foundations Model (foundations -> components -> patterns -> templates -> pages) overlaps substantially with Frost's atomic model (Source 1). However, Couldwell adds a product-design variant that maps differently:

| Web Design Term | Product Design Term |
|---|---|
| Templates | Features ("super patterns" -- an assortment of foundations, components, and patterns performing specific tasks, potentially several layers deep) |
| Pages | Screens (snapshots in time of the product in use) |
| -- | User Journeys (mapping the complex interactions within features) |

Operational implication: for product design, the system must include not just the elements but also how features are used -- showing all screens in a user journey, mapping potential actions, and documenting decision trees and flow charts.

Designer responsibility: mock up all states (hover, error, disabled, empty, loading, submitted), all user journeys, and all edge cases. "Don't assume the developers will figure it out. It's the designer's job."

---

## Iterative vs Wholesale Approaches

No other source provides explicit decision criteria for when to iterate on an existing product vs start from scratch.

### Decision Criteria for Choosing an Approach

**Use iterative when:** product is mission-critical and used daily, users would resist radical change (e-commerce, productivity tools), team cannot pause product work, existing product has salvageable foundations.

**Use wholesale when:** existing product is being replaced entirely, brand demands a complete reset, new product from scratch, team has runway for months of exploration before shipping.

**Use a hybrid (recommended):** wholesale design for coherence, iterative build and rollout for safety.

### The Four Paths

1. Wholesale design, wholesale build, wholesale launch
2. Iterative design, iterative build, iterative launch
3. Wholesale design, then iterative build and launch
4. Wholesale design and build, then iterative launch (phased release)

Path 3 was used for Adobe Portfolio: months of wholesale design exploration, developers fed components bottom-up (foundations -> components -> patterns -> features), MVP released in phased beta releases, each opening to more users.

### The Disruption Factor

Assess disruption risk before choosing. **High risk** (daily-use productivity tools, established e-commerce): go slow, integrate in sprints. **Low risk** (new products, pre-scale): more freedom for wholesale change. Cautionary example: Snapchat 2018 -- 1.25M petition signers, 3M daily users lost, stock drop. Counter-example: Airbnb 2016 -- wholesale redesign became a new standard for iOS.

### The Frankenstein Monster Effect

Primary risk of the iterative approach: redesigning components in isolation without considering the whole product produces a mismatched patchwork. Prevention: "A jigsaw puzzle starts as a whole picture before it's broken down into pieces."

### Ordering Iterative Work

1. **By importance:** Tackle highest-impact problems first. Builds credibility faster.
2. **By disruption level:** Start with foundations (colours, text styles), move up to components (buttons, inputs), then patterns (navigation, cards, footer). Product gradually morphs with minimal user disruption.

---

## Audit Methodology

Frost (Source 1) covers the interface inventory concept. Kholmatova (Source 5) covers the purpose-directed variant. Couldwell provides the most granular operational methodology, splitting audits into distinct types with step-by-step processes.

### The Audit Taxonomy

| Audit Type | Scope | What You Examine |
|---|---|---|
| Interface audit | Broad term covering all types below | Everything |
| Visual audit | Design-based: what you can see | Foundations, components, patterns |
| Pattern audit | Subset of visual audit | Patterns only |
| Component audit | Subset of visual audit | Components only |
| Code audit | What you cannot see | CSS, HTML, JavaScript |
| Colour audit | Cross-property | Every unique colour across all digital properties |
| Content audit | Communication | What you communicate and where |

Visual and code audits often dovetail -- teams can divide and conquer, running them simultaneously.

### Colour Audit Process

1. Audit all websites and products across the company
2. Note every unique colour for text, links, backgrounds, inputs, buttons, error states, borders
3. For each colour, answer:
   - Why are we using this colour?
   - What function does it provide?
   - Do we need multiple variants of the same hue?
   - Which colours can we safely remove, and what replaces them?
4. Work with a front-end developer to introduce Sass variables for all correct brand colours
5. Pair a designer with a developer and replace all hard-coded hex values with the correct Sass variable

Real finding: Hipmunk's colour audit revealed an "alarming amount" of rogue colours, leading to a stripped-down semantic colour system.

### Visual Audit Process

1. Assemble a small task force of front-end-minded people
2. List every feature, page, screen, and user journey in the product; divide among the group
3. Agree on a shared storage location and capture template
4. Each auditor screenshots every unique variant of every element (buttons, headers, text styles, links, inputs, toggles, pagination, tooltips, cards, tables, navigation, footer)
5. Organise by category; capture unique variants, not every instance -- note size, spacing, colour, text styling, border-radius, drop shadows, icons, hover/error states

### Post-Audit Consolidation

For each component type: Are variants random or intentional? Where/when/how are they used? What are their functions?

Example: 26 button styles found in a single product. Analysis revealed two functional clusters (prominent actions vs secondary actions). Consolidated to Primary, Secondary, Tertiary. After consolidation: build new components with a developer, then sweep the product replacing rogue variants.

---

## Colour System Architecture

Not covered by any other source at this level of operational detail.

### Colour Groups by Function

Divide the palette into groups with explicit use-case rules:

**Primary brand colours** -- The core colours of the brand. Used for prominent elements: calls to action, backgrounds, possibly large headers. Rarely used for body copy (accessibility constraints).

**Utility colours** -- Black and shades of grey. Used for copy and headers. Must contrast well with primary colours. Ensure greyscale values used for body text pass accessibility checks on light backgrounds.

**Secondary / tertiary / accent colours** -- Used less frequently. Can be functional (red for warnings, green for success) or complementary (flexible background colours). Define explicit restrictions -- e.g. "secondary palette is for backgrounds only, never for calls to action or copy."

### Numeric Naming Scale for Colour

Use a base colour (e.g. `#62a5d7` = `$blue50`) and generate a scale by darkening and lightening in increments:

- `$blue20` -- lightest
- `$blue30`
- `$blue40`
- `$blue50` -- base
- `$blue60`
- `$blue70`
- `$blue80` -- darkest

Why this works:
- Numeric values avoid ambiguity ("darker" and "lighter" are relative and confusing)
- Two-digit values leave room to insert intermediates later (`$blue55` between `$blue50` and `$blue60`)
- The naming convention is identical in the design library, the code, and the documentation

### Documenting Colour Intent

Do not just output a colour palette. For each colour, document:
- Why it was chosen
- When and where to use it (and when not to)
- The hex value and its system name
- How to reference it in code
- Example use cases showing correct application

---

## Design Tokens: Practical Implementation

Frost (Source 2) positions tokens as "sub-atomic particles" and describes the three-tier architecture conceptually. Couldwell provides the code-level implementation showing how tokens actually work.

### Sass Variable vs Design Token

| Type | Nature | Example |
|---|---|---|
| Sass variable | Generic, reusable value applicable in many scenarios | `$white: #ffffff;` |
| Design token | Specific value for a specific purpose | `$color-background-light: #ffffff;` |

The critical distinction: if you change `$white` from `#ffffff` to an off-white, it breaks everywhere `$white` is used (text, SVGs, borders). If you change `$color-background-light`, only background colours are affected.

### Combining Variables and Tokens

The most scalable architecture uses both layers:

```
/* Layer 1: Sass variable (raw value) */
$blue50: #62A5D7;

/* Layer 2: Design token (semantic assignment) */
$brand-color-primary: $blue50;

/* Layer 3: Application in CSS */
.primary-header { color: $brand-color-primary; }
```

To change the brand primary colour from blue to red: update `$brand-color-primary: $red50;` and every component using the token updates automatically. The raw variable `$blue50` continues to exist for any non-brand uses of that blue.

This two-layer pattern (raw values -> semantic tokens) maps directly to Frost's three-tier token architecture (Source 2): global tokens = raw values, alias tokens = semantic assignments, component tokens = application.

### Token Scope Beyond Colour

Tokens and variables apply to all visual properties:
- Font families and font sizes
- Line heights
- Spacing (margins and padding) -- e.g. GitHub Primer's 8-point grid: 4px, 8px, 16px, 24px, 32px, 40px, 48px, 56px
- Border radius
- Shadows
- Opacity values
- Global states (disabled, active)

---

## Documentation Strategy

### The Distinction: UI Kit vs Style Guide vs Design System

| Level | What It Is | What It Lacks |
|---|---|---|
| UI kit | Random assortment of design elements, not based in real content or use cases | Everything: no guidelines, no context, no real-world grounding |
| Style guide | Singles out foundations and components as deliberate design decisions | Depth: showcases elements but does not explain why, where, when, and how |
| Design system | Comprehensive: design, code, guidelines, documentation | Nothing, when done right |

Analogy: "A style guide is like a diagram of IKEA furniture parts without the assembly instructions. You might build something that resembles the picture on the box, or you might create something entirely different."

### Documentation Tooling Progression

**Stage 1: Google Docs** -- Fast, accessible, team commenting, privacy controls. Good enough for early stages. Do not let design ego block progress.

**Stage 2: GitBook or equivalent** -- Markdown in GitHub, generated as a navigable website. Better search, code embedding.

**Stage 3: Living style guide** -- Live elements pulled from production code. Auto-updates examples; write-ups still need manual maintenance.

Key rule: "A Google Doc that exists is better than a beautiful documentation site that never gets written."

### What to Document Per Category

| Category | What to Document |
|---|---|
| Colour | Why chosen, intent/use-case rules per colour, hex + code name, code reference syntax, example applications, do's and don'ts |
| Brand identity | Logo sizing, overlay rules, colour variants (white-on-dark, black-on-light), asset downloads, minimum sizes, spacing |
| Typography | Limited typefaces with intent, font-weight restrictions, text styles with size/line-height/letter-spacing, margin/padding, performance impact |
| Components | All states (normal, hover, disabled, error, placeholder, value), when to use each variant, code + design docs, anatomy breakdowns |
| Patterns | Modularity rules, purpose, example use cases, responsive behaviour, conditional logic, edge cases (long words, missing JS, empty states) |
| Grid/layout/spacing | Consistent spacing rules, modularity, responsive grid, breakpoint behaviour |

---

## Governance: Guardians, Ambassadors, and Leads

Mall (Source 4) covers the three-legged stool (designer + engineer + PM) and canon vs expanded universe. Couldwell adds the ambassador model for multi-team organisations and practical maintenance rituals.

### Design System Lead Responsibilities

- Advocate permanently for the system
- Establish and enforce processes for updates and maintenance
- Educate designers on system design practices
- Research new ways to manage and maintain the system
- Promote designer-developer collaboration
- Keep everyone engaged and informed
- Handle resistance to the system
- Recruit design system specialists when applicable

Core stance: "The goal is to educate, not enforce. People don't respond well to orders. Approach from a teaching and collaborating perspective, not as the ruling authority."

### Ambassador Model for Multi-Team Orgs

For organisations with multiple product teams and/or locations:

- Appoint a design system ambassador on each product team
- Ambassadors ensure the system has a voice in design critiques, sprints, and product discussions
- Ambassadors define and document their team's patterns and components
- Ambassadors take ownership of keeping their team synced on system initiatives
- Ambassadors report back to the design system team, creating a feedback loop

When looking for ambassadors: seek systems thinkers who are passionate about design systems and willing to advocate for best practices (Sprout Social criteria).

### First Responder Rotation

From GitHub's practice: a rotating on-call duty where whoever is assigned triages issues, assigns urgency levels, and responds to requests for help or code review. The rest of the team stays focused on deep work.

Complement the rotation with office hours (e.g. three days per week) for in-person questions.

### Keeping the Team in the Loop

Six practices: (1) Discuss proposed changes as a team before individuals act unilaterally. (2) Maintain a changelog (what, why, when). (3) Post release notes via email/Slack/internal blog. (4) Dedicated Slack channel for system questions. (5) Office hours for open discussion. (6) Task tracker for requests, bugs, and questions.

For every change, document: why it was updated, what problem it solves, what improved.

---

## Key Quotes Worth Preserving

"You can design a system in a matter of days, but you won't get much further without the support and collaboration of developers, product managers, and other stakeholders."

"Selling a design system isn't a stage of the process. It's ongoing."

"A design system is a marathon, not a sprint. They're never finished."

"Systems design is not only scientific and meticulous, it's the mastery of interacting with people in a sensitive and effective way." -- Linzi Berry, Lyft
"Perhaps the most powerful advantage design systems have is that they allow us to create better work together."
