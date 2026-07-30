# Design System Playbook — Part 4 of 5

*Modules covered: 5.1 – 5.5 (Workflow)*

---

# Part 5 — Workflow

## 5.1 End-to-End Flow

*From request to shipped to documented. Eight steps, every feature.*

The end-to-end flow is the path every new feature follows. Designers know what to do at each step. Engineers know when they are pulled in. Product managers know when to expect updates. The flow is the same whether the feature is a new screen, a new component, or an extension of an existing one.

### The eight steps

```
1. REQUEST          PM / stakeholder describes the need
2. TRIAGE           Designer: what already exists? 3rd-party / library / custom?
3. DESIGN           Designer drafts in product file, using library + Playground
4. HANDOFF          Designer + lead eng: walk-through, feasibility, estimate
5. BUILD            Eng implements, references tokens not hex values
6. DESIGN QA        Designer vs shipped build — tokens match, states present
7. PROMOTE          New components graduate Playground → Library
8. DOCUMENT         Decision log + changelog + (regulated) reviewer sign-off
```

### Component decision tree (step 2)

```
Does the library have it?              YES → use it
                            NO ↓
Can we compose library primitives?     YES → build in Overrides
                            NO ↓
Does 3rd-party library have it?        YES → import + alias tokens
                            NO ↓
Pure custom?                                → Playground → promote later
```

### Who is involved when

| Stage | Designer | FE Eng | PM | Compliance |
|---|---|---|---|---|
| 1. Request | – | – | ✅ | – |
| 2. Triage | ✅ | – | – | – |
| 3. Design | ✅ | – | – | – |
| 4. Handoff | ✅ | ✅ | optional | – |
| 5. Build | – | ✅ | – | – |
| 6. Design QA | ✅ | ✅ | – | – |
| 7. Promote | ✅ | ✅ | – | – |
| 8. Document | ✅ | – | – | ✅ if clinical |

### Best practices

- Triage happens before design, not during
- Handoff is a real meeting, not a Slack message
- QA happens against the shipped build, not staging
- Promotion only happens when all gate criteria are met

### Common mistakes

- Skipping triage and rebuilding existing components
- Designing in a library file instead of a product file
- Promoting a component before engineering review

### Rule of thumb

A feature is not done when it ships. It is done when it is documented and the component is in the library.

**Sources:** Mall S4, Couldwell S6, Skill body.

---

## 5.2 Promotion Gate

*Playground is for drafts. Library is for production. Promotion is the bridge with a checklist.*

The promotion gate is the moment a component moves from a draft in Playground to a production component in the Library. It is intentional, reviewed, and logged. Without a gate, drafts leak into production and the library loses meaning.

### The promotion checklist

A component is ready when all of the following are true:

- [ ] All variants are complete
- [ ] All states are documented (default, hover, active, focus, disabled, error)
- [ ] Every fill, stroke, radius, shadow references a Tier 3 token
- [ ] Do/don't annotations are added
- [ ] Decision log is filled (what decided, what rejected, why)
- [ ] Engineering has reviewed and confirmed buildability
- [ ] A task exists in the project tool (e.g. ClickUp)
- [ ] Design sign-off is recorded
- [ ] If clinical: clinical reviewer sign-off is recorded

### How the gate runs

1. Designer marks the component as `Review` status
2. Engineering reviews buildability
3. Reviewer (and clinical reviewer if relevant) signs off in the decision log
4. Designer changes status to `Production` and moves the frame to the Library file
5. Changelog entry is added in both the Playground and Library files

### Best practices

- Status badges are visible at the top of every component frame
- Promotion is an event, not a passive state change
- The Playground version is deprecated, not deleted, after promotion

### Common mistakes

- Promoting components without engineering review
- Promoting "pretty close" components and patching them later
- Skipping the decision log because it feels like overhead

### Rule of thumb

If the component cannot pass the checklist, it is not ready. There is no informal promotion.

**Sources:** Mall S4, Couldwell S6, Skill body.

---

## 5.3 Handoff Specs

*The handoff is the artifact that engineering builds from. Everything they need lives in one place.*

A handoff spec is what a designer gives to engineering when a feature is ready to build. It is not a separate document. It is a set of agreed-upon contents inside the Figma product file. The goal is that an engineer can build the feature without asking a question.

### What every handoff includes

- All screens at all relevant breakpoints
- All states for each interactive element
- All token references (named, not sampled)
- Edge cases: empty, error, loading, disabled
- Interaction notes: what triggers what, what animates, what persists
- Accessibility notes: tab order, ARIA labels, screen reader behaviour
- Localization notes: which strings are translated, max lengths
- Linked decision log entries for any non-obvious choice

### Handoff page structure (inside the product file)

```
1. Cover           feature name, owner, target sprint
2. Flow diagram    screen-to-screen transitions
3. Screens         all states, all breakpoints
4. Specs           token references, interaction notes
5. Edge cases      empty / error / loading / disabled
6. Decisions       linked decision log entries
```

### Best practices

- Token names are written next to the elements, not just applied as styles
- Interaction notes use plain language, not Figma annotations only
- Edge cases are designed, not assumed
- Handoff is reviewed in a real meeting, not handed off async

### Common mistakes

- Handing off only the happy path
- Skipping edge cases because "the dev will figure it out"
- Using sampled hex values instead of token names
- Treating handoff as the end of design's job

### Rule of thumb

A complete handoff is one where the engineer asks zero questions before starting and finishes a build that matches the spec.

**Sources:** Couldwell S6, Skill body, our synthesis.

---

## 5.4 Design QA

*After it ships, check the spec against the build. Token mismatches are bugs, not opinions.*

Design QA is the final loop. After a feature ships to production (or staging close to it), the designer compares the live build against the Figma spec. The goal is not subjective polish. It is verifying the contract: token values match, states exist, edge cases work.

### What to check

- **Token values** — does the build use the correct semantic tokens? If yes, the build is correct.
- **States** — does every state in Figma exist in the build?
- **Responsive** — does the component behave correctly at each breakpoint?
- **Accessibility** — tab order, focus rings, ARIA labels, screen reader behaviour
- **Localization** — does Italian render correctly? Do strings overflow at max length?

### Severity levels

| Severity | Definition | Action |
|---|---|---|
| **A. Blocker** | Token mismatch on a clinical component, missing required state, broken accessibility | Block release |
| **B. This sprint** | Visual mismatch, missing edge case, wrong copy | Fix before next major release |
| **C. Backlog** | Polish, micro-spacing, animation tuning | Track, fix later |

### Where findings live

- Logged as tickets in the project tool with severity tag
- Linked to the relevant component decision log
- If the same problem keeps appearing, it is a system problem, not a component problem

### Best practices

- QA happens against staging or production, never against the dev environment
- One round, captured in writing
- Patterns of failure feed back into the system: if a state is always missing, the template is wrong

### Common mistakes

- Treating QA as optional
- Reporting issues verbally without ticketing them
- QA-ing the happy path only
- Not closing the loop with a re-test after fixes

### Rule of thumb

If QA keeps catching the same kind of mistake, the system is failing. Fix the system, not the component.

**Sources:** Skill body, Couldwell S6.

---

## 5.5 Rituals and Cadence

*The system stays alive because the team meets it on a schedule. Rituals are how.*

Rituals are recurring, scheduled meetings or work blocks that keep the design system healthy. Without them, the system rots quietly while everyone is busy shipping product. With them, the system grows alongside the product.

### The four rituals

| Ritual | Cadence | Length | Purpose |
|---|---|---|---|
| **Weekly DS sync** | Weekly | 30 min | What's new in Playground, what's promoted, what's stuck |
| **Per-feature handoff** | Per feature | 30-60 min | Designer + lead eng walk-through, feasibility, estimate |
| **Monthly Systems Day** | Monthly | 1 day | Audit, promote, clean up legacy components |
| **Quarterly health check** | Quarterly | 1 hour | Component usage, deprecation candidates, library version review |

### What happens in Systems Day

- Audit which components are used vs unused
- Promote pending Playground items
- Deprecate components no one consumes
- Update changelog gaps
- Migrate one piece of legacy library content into the clean structure

### Flow Weeks vs Systems Weeks (for larger teams)

At 5+ designers, alternate:

- 2-3 weeks Flow (product feature work)
- 1 week Systems (audit, abstract, document, communicate)

### Best practices

- Rituals are non-negotiable. If they get cancelled, the system rots.
- Every ritual has an owner, not a committee.
- Decisions made in rituals are logged the same day.

### Common mistakes

- Skipping Systems Day "because we are too busy" (the busiest teams need it most)
- Treating handoff as a Slack message instead of a meeting
- Letting the weekly sync turn into status reporting

### Rule of thumb

If the team has not touched the design system in two weeks, the rituals are not working.

**Sources:** Mall S4, Couldwell S6.

---

Continued in playbook-part-5.md. (Part 6 — Governance: modules 6.1–6.4)
