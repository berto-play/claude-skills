# Design System Playbook — Part 2 of 4

*Modules covered: 3.1 – 3.6 (Figma Architecture)*

---

# Part 3 — Architecture

## 3.1 Figma Hierarchy

*Nine levels, top to bottom. Each one has a single job. Each one keeps the system findable.*

Figma organises everything in a clear hierarchy, from the company account down to a single layer. The structure exists so anyone, designer or not, can find what they need without asking. Each level answers a different question: where does this live, who owns it, what is it for, what does it contain. When the levels are respected, the system is navigable. The same nine levels exist in some form in every design tool. Figma is where they run today.

### The 9 levels

```
1. ORGANIZATION       company account (Enterprise plan)
2. WORKSPACE          groups of teams under a business unit
3. TEAM               group of people sharing projects
4. PROJECT            folder, one purpose per project
5. FILE               one job per file
6. PAGE               one concern per page
7. SECTION            groups related frames inside a page
8. FRAME              an artboard: screen, component, or spec
9. LAYER / COMPONENT  the smallest unit
```

### What lives where

| Level | Job | Example |
|---|---|---|
| Organization | Billing, SSO, plugin policy | [Client] |
| Workspace | Group teams by business unit | [Client] Health |
| Team | Group people who share projects | [Client] Product |
| Project | Group files by purpose | MED - Design System |
| File | One job: library, product, exploration | MED - Library - Foundations |
| Page | One concern: cover, changelog, content | Colour Tokens |
| Section | Group related frames | Semantic — Feedback |
| Frame | A single artboard | Alert / Error / Default |
| Layer | The atomic piece | icon-alert-circle |

### How to shape your hierarchy

1. **Do we need a Workspace?** Only if the company has multiple business units that need separate billing or governance.
2. **One Team or multiple?** One Team per group of people who share permissions, cadence, and rituals.
3. **How should Projects be sliced?** By product surface, by concern, or by team. Most common pattern: one Project per product surface, plus a separate Project for the design system.
4. **When does a new File justify a new Project?** When grouping it with existing files would make either harder to find. Most new files belong inside an existing Project.

### Best practices

- Every File lives inside a Project. Every Project lives inside a Team.
- One Project per product or per concern. Never per person.
- The design system lives in its own Project, shared across products.
- Naming is the navigation.
- Prefix everything with the company code (e.g. MED).

### Common mistakes

- Using Projects as dumping grounds (`General`, `Misc`)
- Putting the design system inside a product project
- One Project per designer instead of per work area
- Mixing exploration files into a Library project

### Rule of thumb

A good Project, File, or Page is easy to explain in one sentence. If the explanation feels forced, the structure needs rethinking.

**Sources:** Figma's product structure, Couldwell S6.

---

## 3.2 File Architecture

*One job per file. The number of files scales with the team, the products, and the brands.*

The file architecture is the layout of the design system inside Figma. Each file does one job and only one job. A library file holds reusable components. A product file holds screens and flows. A playground file holds drafts. Mixing jobs in a single file is the most common reason a system becomes hard to maintain.

### Questions to ask before deciding the file count

1. **How many designers are touching the library directly?** More than two means a separate Playground file.
2. **How many distinct user types and platforms?** Web for doctors and a React Native app for patients are two different consumption contexts.
3. **How many brands or white-labels?** Brands live as token modes inside Foundations, not as separate files.
4. **What is the regulatory layer?** Healthcare, finance, and safety-critical products often need a Domain file.

### Recommended shape by team size

| Context | Files needed |
|---|---|
| 1 designer, 1 product | 3: Foundations, Components, Playground |
| 2-4 designers, 1-2 products | 5: Foundations, Components Web, Components Mobile, Domain, Playground |
| 4+ designers, multi-product | 7+: Foundations, Components per platform, Domain, Brand layer, Playground |

### Recommended shape for [Client]

```
Project: MED - Design System
├── MED - Library - Foundations
├── MED - Library - Components Web
├── MED - Library - Components Mobile
├── MED - Library - Domain (Clinical)
└── MED - Playground

Project: MED - RPM Pro                  → MED - Product - RPM Pro
Project: MED - RPM Patient              → MED - Product - RPM Patient
Project: MED - Medical Device Admin     → MED - Product - Medical Device Admin
Project: MED - HomMed                   → MED - Product - HomMed
```

Five library files. One product file per surface. The Webflow marketing site sits outside and consumes brand tokens only.

### File naming

Format: `[CODE] - [Type] - [Subject]`

- **Library** — `MED - Library - Foundations`
- **Product** — `MED - Product - RPM Pro`
- **Exploration** — `MED - Exploration - Q2 2026`

### Best practices

- One job per file. If a file has two jobs, split it.
- Library files are subscribed to. Never edit them from a product file.
- Product files consume libraries. They never publish components.
- Exploration files are time-boxed. Archive at end of quarter.
- Foundations is the only file every other file subscribes to.

### Common mistakes

- A single mega-library that holds tokens, components, and screens
- Editing library components from inside a product file
- Letting exploration creep into Library files
- Skipping a Playground at two or more designers

### Rule of thumb

Add a file when the cost of mixing two jobs exceeds the cost of maintaining a separate one. Until then, fewer files is always cheaper.

**Sources:** Skill body, Couldwell S6. The [Client] shape is our synthesis.

---

## 3.3 Page Template

*Every file gets the same four required pages. They are the navigation contract.*

Every Figma file in the system, regardless of its job, starts with the same four pages: Cover, Changelog, Index, and Content. The template means that anyone opening any file knows what they are looking at within five seconds. In a regulated company, this template is also part of the audit trail.

### The four required pages

```
Page 1: Cover       file name, version, owner, status, last reviewer
Page 2: Changelog   date | what | who | why | reviewer
Page 3: Index       links to every content page
Page 4+: Content    one concern per page
```

### Cover page contents

- File name (matching `[CODE] - [Type] - [Subject]`)
- Version number
- Owner (one person, not a team)
- Status: Draft / Active / Frozen / Deprecated
- Last updated date
- Last reviewer name and date
- For 3rd-party libraries: library name and version pin

### Changelog page contents

A simple table, newest at top:

| Date | What changed | Who | Why | Reviewer |
|---|---|---|---|---|
| 2026-04-22 | Added critical alert variant | A. Solis | Clinical protocol update | Dr. Bianchi |

### Index page contents

A list of every content page in the file with one-line descriptions. Update whenever a page is added or renamed.

### Content pages

One concern per page. Named precisely. No orphan frames outside a section.

### Best practices

- Required pages always come first, in this order
- Cover updates whenever any other page changes substantively
- Changelog is structural, not optional, even in non-regulated work
- In regulated contexts, the changelog is a compliance artifact

### Rule of thumb

If a new person opening the file cannot tell what it is, who owns it, and what changed last, the template is broken.

**Sources:** Couldwell S6, Skill body.

---

## 3.4 Component Page Structure

*Every component page tells the same story in the same order. The story is what makes it usable.*

A component page is the entry point for everyone who uses that component. Designers reference it before reusing. Engineers reference it before building. Reviewers reference it for sign-off. The structure below is consistent across every component, so the lookup is always predictable.

### What every component page contains

```
1. Component name + status badge (Draft / Review / Production / Deprecated)
2. All variants in a single frame
3. All states: default, hover, active, focus, disabled, error
4. Usage guidance: do / don't with annotated examples
5. Specs: padding, radius, token references (named, not sampled)
6. Engineering notes: library mapping, RN vs Web differences, known constraints
7. Decision log: what was decided, why, what was rejected, date, reviewer
8. Compliance tag (if the component touches a clinical or safety surface)
```

### Status badges

- **Draft** — under construction, do not consume
- **Review** — pending engineering and design sign-off
- **Production** — safe to consume in product files
- **Deprecated** — do not consume, scheduled for removal

### What goes in the decision log

Every component page carries a decision log. In a regulated component, this is the compliance record.

| Field | Example |
|---|---|
| Decided | Use red 600 for critical alerts |
| Why | Highest contrast that passes WCAG AA on the chosen background |
| Rejected | Red 700 (too dark on dark mode), orange (used for warning, would conflict) |
| Date | 2026-04-22 |
| Reviewer | Dr. Bianchi (clinical), A. Solis (design) |
| Linked docs | MDR usability file §3.4 |

### Best practices

- States are visible on the page, not nested inside variants
- Token references are written as text, not just applied as fills
- Do/don't examples use real screens, not abstract shapes
- Decision log entries are short but specific

### Common mistakes

- Documenting only the default state
- Hiding states inside component variants without showing them visually
- Using sampled colour values instead of token references
- Skipping the decision log "until later"

### Rule of thumb

A component page is complete when a new engineer can build the component, and a reviewer can sign it off, without asking a question.

**Sources:** Frost S1, Couldwell S6, Skill body.

---

## 3.5 File Organization

*Organize by what changes at the same rate. Match the principle to the file type, not the team's preference.*

Inside each file, content can be organised by domain, by feature, by quarter, or by section. None of these is universally right. Each solves a different problem. The mistake is picking one principle and forcing it across every file. Match the organising principle to the half-life of the content.

### The four approaches

| Approach | Best for | Fails when |
|---|---|---|
| **By domain** *(Clinical, Auth, Billing)* | Library files, design systems, regulated products | Domains are not clearly defined |
| **By feature** *(Onboarding, Dashboard)* | Product files | Features overlap or get renamed often |
| **By quarter** *(Q2 2026, Q3 2026)* | Explorations, scratchpads | Used for anything durable |
| **By site section** *(Home, Pricing)* | Marketing sites and content-heavy IA | Applied to product apps |

### Mix by file type

| File type | Organize by |
|---|---|
| Libraries (tokens, components) | Domain |
| Product work (screens, flows) | Feature or flow |
| Explorations | Quarter |
| Marketing site | Section |

### The half-life rule

- Tokens change rarely → group by domain
- Components change occasionally → group by domain
- Features ship on cycles → group by feature
- Explorations die fast → group by time
- Marketing pages map to URLs → group by section

### Common mistakes

- Organising libraries by quarter (rots fast)
- Organising explorations by domain (clutters)
- Mixing two principles in one file

### Rule of thumb

Stable things organised by time will rot. Disposable things organised by domain will clutter. Pick the principle that matches the half-life of the content.

**Sources:** Skill body, our synthesis of the half-life rule.

---

## 3.6 Naming Conventions

*Names are the navigation. A good name means someone finds a component in 3 seconds without asking.*

Naming is the first system inside the system. Before a component exists, it has a name, and that name has to work in Figma, in code, in tickets, and in Slack. If two people name the same thing differently, you do not have a system. You have two.

### Naming hierarchy

```
PROJECT     [CODE] - [Purpose]                  → MED - Design System
FILE        [CODE] - [Type] - [Subject]         → MED - Library - Foundations
PAGE        [Concern, singular]                 → Colour Tokens
SECTION     [Group descriptor]                  → Semantic — Feedback
FRAME       [Component] / [Variant] / [State]   → Alert / Error / Default
COMPONENT   [Category] / [Name] / [Variant]     → Feedback / Alert / Error
TOKEN       [category]/[subcategory]/[variant]  → color/action/primary
```

### Component naming — the four rules

**1. Describe function, not form**
- ✅ `Alert` ❌ `YellowBox`
- ✅ `Button / Primary` ❌ `BlueButton`

**2. Match engineering 1:1**
- Code says `<Alert>` → Figma says `Alert`
- Code says `Toast` → never call it `Snackbar` in Figma

**3. Search-first ordering**
- Distinctive word first — typing 3 characters should find it
- ✅ `Button / Primary / Hover` ❌ `Primary / Button / Hover`

**4. Predictable state suffixes**
- Always: `/Default`, `/Hover`, `/Active`, `/Focus`, `/Disabled`, `/Error`
- Never improvise

### Memorability

**Do:**
- Use industry-standard names: Modal, Toast, Popover, Tooltip, Badge, Chip
- Add synonyms in the component description (so "snackbar" finds Toast)
- Stay consistent across Figma / code / docs / Slack

**Don't:**
- Invent cute internal names (`Squirrel`, `MedNotif`)
- Include brand or product in component names (`ClientButton`, `RPMAlert`)
- Use abbreviations unless industry-standard

### Token naming — the contract

```
TIER 1   color/blue/500              → raw value, no meaning
TIER 2   color/action/primary        → what it means in context
TIER 3   button/primary/fill         → what a specific component uses
```

Every level reads left-to-right as "narrows to." If you cannot read it that way, the name is wrong.

### Rule of thumb

If you cannot find a component by typing its first three letters, rename it.

**Sources:** Kholmatova S5, Couldwell S6. Search-first ordering and memorability rules are our synthesis.

---

Continued in playbook-part-3.md. (Part 4 — Foundations: modules 4.1–4.4)
