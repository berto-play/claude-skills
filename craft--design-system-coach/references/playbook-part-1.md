# Design System Playbook — Part 1 of 3

*Modules covered: 1.1 – 2.2 (Context and Scope)*

---

# [Client] Design System Reference

*A working reference covering the architecture, workflow, and governance of the [Client] design system. Written for designers, engineers, product managers, and anyone shipping UI work. Use it linearly the first time. Jump to specific sections when you need an answer.*

---

## Table of Contents

**Part 1 — Context**
1.1 What a design system is (and isn't)
1.2 Scaling by team size
1.3 Regulated products
1.4 Company shapes: multi-product vs multi-brand

**Part 2 — Scope**
2.1 Surfaces
2.2 Product inventory

*(Parts 3–6 are in playbook-part-2.md and playbook-part-3.md)*

---

# Part 1 — Context

## 1.1 What a Design System Is (and Isn't)

*The shared language between design and engineering. Not a Figma file. Not a component library.*

A design system is a shared toolkit that helps a team build products faster and keep them consistent. It contains reusable components, tokens, and rules, all guided by clear principles. Decisions made once become decisions nobody has to make again. For the business, it means less duplicated work, fewer bugs, and easier expansion into new products or brands. It lives in Figma, in code, and in how the team works together. Tools change. The system stays.

### The three layers

```
DECISIONS + GOVERNANCE     why it exists, who owns it, how it changes
COMPONENTS + TOKENS        what design and code both consume
TOOLS + RITUALS            where it lives, how it stays alive
```

The bottom layer is where the system is stored. The middle is what most people picture when they hear "design system." The top is what most teams forget, and it's the layer that decides whether the system survives past its first year.

### Is

- Tokens, components, decisions, governance — the full stack
- A living contract that changes with the product
- Shared ownership between design and engineering
- The institutional memory of every *"why did we do it this way?"*

### Isn't

- A single Figma file — that's an artifact, not a system
- A component library alone — that's a rental, and only one layer
- A style guide PDF — that's a snapshot, and snapshots rot
- A one-time project — systems that stop evolving get abandoned

### Business incentives and impact

- **Ship faster** — teams stop rebuilding the same component three times
- **Scale cheaper** — a new product, partner, or white-label is weeks, not quarters
- **Lower risk** — consistent patterns mean fewer bugs and easier audits
- **Onboard faster** — a new hire learns the system once and works across the whole company

### Scalability and flexibility

- New product arrives → reuses existing components and tokens
- New brand or white-label → add a mode, not a new file
- New platform → extend token modes, not rebuild the library
- New regulation → decision log absorbs the requirement, no separate docs

### Rule of thumb

If designers and engineers use different names for the same thing, you don't have a system yet. You have two.

**Sources:** Frost S1, Kholmatova S5, Couldwell S6, Mall S4 (scalability framing).

---

## 1.2 Scaling by Team Size

*Match governance weight to team size. Too little causes drift. Too much slows the team down.*

Governance is how the team keeps the design system healthy. Too little, and work gets duplicated, products look inconsistent, and compliance becomes a risk. Too much, and every decision slows down. The right amount depends on the size of the team and how complex the product is. Check in whenever either changes. The thresholds below are signals, not rules.

### Engineers (anyone writing frontend code)

- **1-2 engineers**
  - Lightweight handoff. A shared Slack channel is enough.
  - Decision logs kept inside Figma component pages.
- **3-8 engineers**
  - Promotion gate required: Playground moves to Library only on review.
  - Changelog maintained and actually read.
  - Token naming consistent enough that no one asks twice.
- **8+ engineers**
  - Ambassador model: one engineer per product team owns DS alignment.
  - Formal contribution process with PR template and review criteria.
  - Automated token sync from Figma to code.

### Designers (anyone shipping UI work)

- **1 designer**
  - You are the system. No governance layer needed.
  - Three files: Foundations, Components, Playground.
  - Your memory is the decision log.
- **2-4 designers**
  - Shared conventions are non-negotiable.
  - One person owns the library. Others consume.
  - Promotion gate required. Two people diverge within a week without it.
- **5-10 designers**
  - Dedicated DS owner, even part-time.
  - Changelog people actually read.
  - Formal contribution flow.
- **10+ designers**
  - Ambassador model across product teams.
  - Canon vs Expanded Universe split: core system vs product-specific extensions.
  - Flow Weeks and Systems Weeks cadence: 2-3 weeks of product work, 1 week on the system.

### Rule of thumb

Re-evaluate governance whenever team size crosses a threshold. If no one can tell you what changed last month, governance is too light. If everyone is complaining about process, it's too heavy.

**Sources:** Mall S4, Couldwell S6. Numeric thresholds are our heuristic.

---

## 1.3 Regulated Products

*When a product touches healthcare, finance, or safety, design decisions become compliance records.*

A regulated product is one where the law requires proof of how decisions were made. This applies to healthcare (MDR, FDA), finance (SOX, PSD2), and any product that affects safety. If the product does not meet the rules, it cannot ship, or it ships and creates legal risk. In a regulated company, the design system is not only a productivity tool. It is also part of the audit trail.

### What regulated changes

- Every decision on a clinical or safety-critical surface is logged with a reason, a reviewer, and a date
- The Figma decision log is a compliance record, not a convenience
- Old components are deprecated, not deleted. Version history is preserved.
- Domain-specific tokens are separated from general feedback tokens, so regulators can see intent clearly

### Tokens in regulated products

In a normal product, an error colour is an error colour. In a regulated product, a clinical alert and a form error are two different things, even if they use the same red today. That difference has to be visible in the token architecture.

```
FEEDBACK    color/semantic/feedback/error     → form validation, system messages
CLINICAL    color/semantic/clinical/critical  → patient safety, vitals out of range
```

Today they may share the same primitive value. Tomorrow a regulator may ask you to differentiate them. If the tokens are already separate, that is a one-line change instead of a refactor.

### In practice: an Italian healthcare example

**Component:** `Vitals Alert / Critical` used in the RPM Patient app (React Native)
**Trigger:** a vital sign reading crosses a clinically defined threshold
**Regulation it touches:** MDR 2017/745 (Class IIa SaMD), Agenas telemedicine guidelines, UNI CEI EN 62366 (usability for medical devices), GDPR

What the design system must carry for this one component:

- Colour choice with WCAG AA contrast recorded, tested for colour vision deficiencies
- Threshold copy matching the clinical protocol signed off by the physician reviewer
- Icon matching an approved internal standard, consistent with ISO 7010 principles
- Screen reader announces severity in Italian before anything else
- Decision log fields: designer, clinical reviewer + professional order number, ISO date, MDR file section, Agenas reference, component version

For a non-clinical surface (e.g. a Webflow marketing CTA), none of this applies. The difference is the surface, not the component type.

### What stays the same

- Component structure
- Component names and naming rules
- Governance principles
- Token tiers and aliasing logic

### Rule of thumb

If you cannot produce a dated, signed rationale for every clinical or safety-related component, you are not audit-ready. Make the documentation structural, so nobody has to produce it under pressure later.

**Sources:** Skill body, Couldwell S6. Italian healthcare specifics are our synthesis.

---

## 1.4 Company Shapes: Multi-Product vs Multi-Brand

*The shape of the company determines the shape of the design system.*

Before deciding how the design system is structured, identify what kind of company it serves. Two companies with the same number of products may need very different systems if one is single-brand and the other is multi-brand. Confusing these shapes is one of the most common reasons a design system over- or under-engineers.

### The three shapes

| Shape | Description | Recognisable example |
|---|---|---|
| **Single product, single brand** | One product, one identity | Spotify, Slack |
| **Multi-product, single brand** | Many products, all under one identity | Uber, IBM, Salesforce, Adobe |
| **Multi-brand enterprise** | Multiple brands with distinct identities sharing infrastructure | Meta, Marriott, Volkswagen Group |

### How to tell which one you are

Open all your products side by side.

- If they share visual identity (logo, colour, type) → multi-product single brand
- If they look like different companies → multi-brand
- If you only have one → single product

### What changes by shape

- **Single product** — one library file, no brand modes needed
- **Multi-product, single brand** — one library, many product files, no brand modes
- **Multi-brand enterprise** — one library, brand modes in tokens, possibly per-brand component overrides

### Where [Client] sits today

[Client] is **multi-product, single brand** today (RPM Pro, RPM Patient, Medical Device Admin, HomMed, all under the [Client] identity).

The day a white-label partner ships under their own brand on top of the platform, [Client] becomes **multi-brand**. The system must be ready to add a brand mode without restructuring.

### Rule of thumb

Multi-brand is about visual identity, not product count. Five products that look the same are multi-product single brand. Two products that look different are multi-brand.

**Sources:** Frost S3 (*Multi-All-The-Things*). Examples are our synthesis from public brand identity.

---

# Part 2 — Scope

## 2.1 Surfaces

*A surface is a distinct UI your users interact with. Count them before you design the system.*

A surface is any interface where users perform a meaningful task. Different surfaces often need different components, layouts, or token values. Knowing how many surfaces you have tells you how many files, modes, and governance layers the system needs. Miscounting is the most common cause of a design system that looks right on paper but breaks in practice.

### The 4-question test

For each candidate UI, ask:

1. **Different user role?** Is the person using it fundamentally different? (patient vs clinician vs admin)
2. **Different primary job?** Is the core task fundamentally different? (booking vs reviewing vs managing)
3. **Different context of use?** (clinical vs office vs home, mobile one-handed vs desktop focused)
4. **Different component needs?** Would the same Button or Card need to look or behave differently here?

**Two or more "yes" answers means it is a separate surface.**

### What IS a surface

- A customer-facing app
- An admin or operations tool
- A partner or clinician portal
- A mobile app with a distinct user from its web counterpart
- A marketing site (usually outside the design system)

### What is NOT a surface

- The same app on desktop, tablet, mobile (responsive, not separate)
- iOS and Android versions of the same React Native app
- A modal, form, or single screen (those are components inside a surface)
- A role view inside the same app (that is a sub-view, not a surface)

### Common miscounts

- **Over-counting** — treating every breakpoint as a surface
- **Under-counting** — assuming the internal dashboard is just an extension when ops users have different workflows
- **Missing internal tools** — ops dashboards, support consoles, CMS systems often forgotten

### Rule of thumb

If two UIs could be used by the same person, for the same task, in the same context, they are one surface. If any of those change, count them separately.

**Sources:** Frost S3. The 4-question test is our synthesis to make Frost's taxonomy actionable.

---

## 2.2 Product Inventory

*Before designing the system, map what exists. You cannot scale what you have not counted.*

A product inventory is a simple list of every product, application, and tool the company ships or uses internally. It includes customer-facing products, internal tools, partner portals, and marketing sites. The inventory is the source of truth for how many surfaces the system has to serve, who uses each one, and which are regulated.

### Where to start

The inventory usually already lives in people's heads. The goal is to get it on paper in one place. Ask the company AI, the engineering lead, and the product team the same question, then reconcile the answers. Disagreements are useful. They show where the team is already misaligned.

### Questions to ask

1. List every product, application, and tool the company owns, including internal and partner-facing. Name, URL or app, one-line description.
2. For each, who are the primary user roles? Are they using the same screen or different ones?
3. What platforms does each one run on? (responsive web, iOS, Android, desktop)
4. Which are in active development, maintenance, or being deprecated? Any new ones planned in the next 6-12 months?
5. Which operate in a regulated environment?

### Mapping sheet columns

| Column | Example |
|---|---|
| Name | RPM Web App |
| URL / bundle ID | app.example.com |
| Platform | Web, desktop |
| Primary users | Doctors, admins, CS, ops |
| Brand | [Client] |
| Regulated? | Yes (MDR Class IIa) |
| Status | Active |
| Surface count | 1 |
| Notes | Centro Servizi is role-gated inside this |

### Tagging taxonomy

Tag every product with these three labels:

- **Audience:** customer, partner, internal, public
- **Platform type:** web, mobile native, mobile RN, desktop, embedded
- **Risk layer:** regulated, sensitive, standard

Tags become filters. They drive which files require signed decision logs, which need accessibility review, which need translation.

### Rule of thumb

If the inventory does not match what the company AI, engineering lead, and product team all say, stop building until it does. Wrong inventory means wrong system.

**Sources:** Couldwell S6. Mapping sheet and tagging are our synthesis.

---

Continued in playbook-part-2.md. (Part 3 — Architecture: modules 3.1–3.6)
