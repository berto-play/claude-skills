# Source 4a: Design That Scales -- Dan Mall (2023)
# Part 1: Pilots and Buy-In (Chapters 4-5)

Confidence: HIGH -- Full text extracted and verified.

---

## Table of Contents

1. [Central Thesis](#central-thesis)
2. [Chapter 4: The Broken Business of Buy-In](#chapter-4-the-broken-business-of-buy-in)
3. [Chapter 5: Pilots -- The Best Way to Start and Sustain a Design System](#chapter-5-pilots----the-best-way-to-start-and-sustain-a-design-system)

---

## Central Thesis

The traditional approach to design system buy-in is broken. You cannot pitch a design system in the abstract. You cannot ask people to adopt a new tool cold. You cannot expect product teams to contribute to something they do not use. The solution is the pilot -- linking design system work directly to real product work so the system grows organically from actual use.

Mall's core definition: "A design system is a connected, package-managed, version-controlled software product that contains the smallest set of components and guidelines a particular organization needs to make digital products consistently, efficiently, and happily."

The most mature form is a practice -- not a project, not a product, but an ongoing organizational habit. "Design systems aren't a process tool; they're a delivery product."

---

## Chapter 4: The Broken Business of Buy-In

The standard four-step narrative -- pitch the project, create building blocks, get teams to use them, get teams to contribute back -- "almost never works, and teams are left scratching their heads as to why. It's because this plan is entirely wishful thinking."

### Failure Mode 1: Pitching Vaporware

Proposing a design system to leadership is tough because the system being pitched does not exist yet. "It's an attractive concept that rarely works." Mall compares it to trying to get VC funding with no product-market fit: "executive leadership will be much more likely to bless a design system initiative after people are steadily using a design system. Until then, you're just crossing your fingers for a leadership team that has a high tolerance for risk, a rarity these days."

**Operational takeaway:** Do not pitch the system. Demonstrate it through pilot results. Don't even say the words "design system" to leadership initially.

### Failure Mode 2: Abstract Systems Are Difficult to Make

Building foundational components in a vacuum produces bloated, unusable output. "You end up with a system that's more 'book smart' than 'street smart.'" Color palettes end up "including every hue of the rainbow and multiple shades of grayscale with little guidance on how to use any of them."

**Operational takeaway:** Never start with abstract components. Start with a real product use case that gives components context.

### Failure Mode 3: Cold Adoption -- Convincing Someone to Use a New Tool

Even with good components, getting teams to adopt is hard because of Zipf's Law (the principle of least effort): "given the choice, animals, people, and even well-designed machines will choose the path of least resistance."

**Operational takeaway:** Frame adoption as making the team's existing deadline easier to hit, not as a new obligation.

### Failure Mode 4: Product Teams Do Not Contribute

Two reasons product teams don't contribute:
1. **Too many new things.** "More than 40 percent of a person's daily actions are driven by habits. Introducing too many new tools and processes into a team's standard way of creating products simply won't take."
2. **It's not their job.** "A product team's job is to make product, not contribute to a design system."

**Operational takeaway:** Contribution is the design system team's job, not the product team's job. The DS team extracts and abstracts from product work.

---

## Chapter 5: Pilots -- The Best Way to Start and Sustain a Design System

### Linking Design System to Product Work

"Linking design system work directly to ongoing digital product work is one of the most successful ways to get traction." This uses Newton's law of inertia: "objects in motion tend to stay in motion."

Five inflection points that are ideal boarding platforms:
- Rebranding
- Replatforming
- Re-org
- Content migration
- Merging with or acquiring another company

Talk about outcomes, not the tool: "Doing work in this way will put us six weeks ahead of schedule." / "See how many fewer bugs we had by reusing code that's already been through QA?"

### The Pilot Scorecard (Full 8 Criteria)

Score every candidate product on a 0-10 scale across these eight factors:

| # | Criterion | What it measures |
|---|-----------|-----------------|
| 1 | **Potential for common components** | Does this pilot have many components reusable in other products? |
| 2 | **Potential for common patterns** | Does this pilot have many interaction patterns reusable elsewhere? |
| 3 | **High-value elements** | Is there a component with high business value at the heart of this project? |
| 4 | **Technical feasibility** | How simple is the technical implementation? |
| 5 | **Available champion** | Will someone on this product see it through and evangelize the DS? |
| 6 | **Scope** | Is this accomplishable in a pilot timeframe of 3-4 weeks? |
| 7 | **Technical independence** | Is the work decoupled enough from legacy code? |
| 8 | **Marketing potential** | Will this work excite others to use the design system? |

Average the scores and order highest to lowest. That is the pilot sequence.

### Types of Design System Pilots

- **The Indiana Jones:** Refactor a codebase from custom components to DS-connected ones without changing UX.
- **The Facelift:** Migrate products to a new visual language (post-rebrand).
- **The Speed Run:** Test how quickly an interface can be built with DS components.
- **The Surrogate:** DS team role-plays as a product team, building front-end prototypes.
- **The Perimeter:** Build the products with the least red tape, not the flagship.

**Key semantic distinction:** "What do we need to be able to make?" not "What do we want to be able to make?"

### Multiple Concurrent Pilots

Run three pilots concurrently. "Three is the smallest number to see patterns emerge."

The "three times is a pattern" rule: "When do you contribute a component to the design system? When three or more teams need it or are using it."

### Improving the System as You Use It

The Half-Blood Prince textbook metaphor: a design system is not the textbook written in advance -- it is the notes in the margin written by someone who has actually done the work.

Key mindset: the DS team's primary job is to collect the organization's best practices, not to create best practices. Gravitate toward "collect, curate, and establish."

### The Piloting Process -- The Measuring Spoon Cycle

1. Make a feature or product.
2. Extract and abstract components from that product to start the system.
3. Make another product using the components you previously extracted.
4. Return to Step 2.

"Then repeat this process. Forever. For as long as your company exists."

### Extracting and Abstracting Components

When reconciling multiple variations of the same component:

| Category | Question | Directive |
|----------|----------|-----------|
| **All** | What does every version have in common? | Every characteristic is mandatory. |
| **Most** | What do most versions have in common? | Every characteristic is highly recommended. |
| **Some** | What do some versions have in common? | Facilitate a workshop to form consensus. |
| **Few** | What do few versions have in common? | Delay including these characteristics. |

The IKEA effect: "People place disproportionately higher value on items they had a hand in creating."

### Minimum Viable Design System

"Just three elements can be a design system." Start with three components that other teams could use right now.

Design systems are emergent: "Creating a design system in a vacuum and then trying to plug it into an organization's rapidly moving product design process rarely works."
