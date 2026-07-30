# Source 4b: Design That Scales -- Dan Mall (2023)
# Part 2: Governance, Process, Metrics, and Scale (Chapters 6-10)

Confidence: HIGH -- Full text extracted and verified.

---

## Table of Contents

1. [Chapter 6: Governance and Contribution](#chapter-6-governance-and-contribution)
2. [Chapter 7: Roles and Responsibilities](#chapter-7-roles-and-responsibilities)
3. [Chapter 8: Process and Workflow -- The Hot Potato Process](#chapter-8-process-and-workflow)
4. [Chapter 9: Success Metrics for a Design System](#chapter-9-success-metrics)
5. [Chapter 10: Evangelism Never Stops](#chapter-10-evangelism-never-stops)
6. [Conclusion and Evolution](#conclusion)
---

## Chapter 6: Governance and Contribution

### The WordPress Analogy

WordPress powers 43% of the top 10 million websites. The governance breakdown:
- 0.05% of users get paid to work on it
- 0.01% of users contribute back to it
- 0.0002% of users are directly responsible for it

**Translation:** If 100 people use your DS: ~2 should be expected to contribute back, ~6 should be paid to work on it, 1 should be directly responsible.

### Governance Framework

**WHAT** -- Define when to use the system vs. make something custom. How does something new make its way back?
**WHY** -- Self-evident to anyone doing the work.
**WHO** -- "It's the job of the design system team to contribute to the design system; it's not the product team's job."
**WHEN** -- All throughout. Alternate between Flow weeks and Systems weeks.
**WHERE/HOW** -- Leverage existing processes.

### Flow Weeks and Systems Weeks

- **Flow weeks (2-3 weeks):** Product teams solve user needs. DS team surfaces existing components.
- **Systems week (1 week out of every 4):** DS team audits newly created components for eligibility.

Eligibility criterion: "Can three other teams at this company use this component right now?"
Relaxed version for mature teams: "Can another team use this component this quarter?"

### Canon vs. Expanded Universe

- **Canon:** The design system's official components, patterns, and processes.
- **Expanded Universe:** Product-specific components that are "allowed and encouraged to exist even though they're not part of canon."

Not everything should go into the design system.

---

## Chapter 7: Roles and Responsibilities

### Front-End Engineering Is the Most Critical Skill

Front-end is "the coupling that connects design, engineering, and customers. Without that investment, you're missing the most critical part."

Brad Frost's distinction: "front-of-the-front-end" (HTML, CSS, presentational JS) vs. "back-of-the-front-end" (JS for application logic).

### Ecosystem Engineers

Named after Dr. Clive Jones's concept: beavers build dams for their own protection, but many other species benefit. "Ecosystem engineers are interested in building platforms."

### Three-Legged Stool

From Alex Schleifer (former VP Design, Airbnb): "Three elements define a product: the business, the code, and the pixels. Give each a voice in all product decisions."

### Team Growth Sequence

1. **Founding pair:** Designer + Engineer (minimum viable team)
2. **Add Product Owner:** Evangelizes while the founding pair does component work.
3. **More engineering and design help:** Additional staff as workload grows.
4. **Producer/Project Manager:** Manages work at the in-the-weeds level.
5. **Design and engineering hierarchy:** Junior/mid handle straightforward; senior handle complex.
6. **Full cross-disciplinary team:** Business analysis, content writing, QA, office hours, newsletters.

"Most organizations take between a year or two to grow to this point."

### Design System Annual Budgeting

"It takes $1-$1.5 million annually to sustain a mature design system practice."

ROI example: "$40 million savings from an initial investment of $4 million. That's a 1,000% return on investment."

---

## Chapter 8: Process and Workflow -- The Hot Potato Process

### Frameworks over Processes

A single question from Jonathan Rasmusson: "What would it take to deliver something of value to customers each and every week?" -- "That's the framework."

### The Hot Potato Workflow

Designer and engineer pass work back and forth rapidly:
1. Engineer starts in code -- plain text HTML for the page's jobs.
2. Engineer scaffolds -- structural HTML, basic layout.
3. Designer observes and redirects layout priorities.
4. Engineer implements adjustments immediately.
5. Designer creates an element collage (art direction, not full comps).
6. Engineer sets up design tokens from the element collage.
7. **"Dear Engineer, what do you need right now?"** -- Everyone in service of the engineer.
8. **"Kinda like this"** -- Use references instead of full comps.
9. **Spot comps** -- Only when no reference exists. 5-10 minutes, not full comps.
10. **Continuous exchange** until the screen is built.

"Hot Potato is about continually coming together to sync up and going apart to have heads-down independent work. The key is keeping the time apart as short as possible."

### Hot Potato at Scale

Does not scale to 100-person teams. "Instead of figuring out how to scale this process, try to reduce your scale." Split into teams of six.

---

## Chapter 9: Success Metrics for a Design System

### Efficiency and Consistency Indicators

Efficiency: reduction in design/engineering time, increase in speed to market, reduction in QA time, reduction in bugs.
Consistency: decrease in CSS redundancy, reduction in complaints, decrease in task completion time.

"All of these indicators point to one thing: adoption."

### Design System Coverage (80/20 Rule)

"Aim for up to 80% of any given page to be made of design system components and leave room for about 20% of the page to be custom."

Starting targets: 10% coverage working up to 80% over 1-2 years. Even simpler: get ONE component adopted by a few teams simultaneously.

### OKR Framework for Design Systems

- **Objective:** Vague, ambitious, somewhat comfortable. Usually starts with a verb.
- **Key Result:** Measurable, evaluated quarterly. Usually contains a number.

Target score: 60-80 out of 100. Not 100 (sandbagging). Not 0 (unrealistic).

For initial key results, use the Just Noticeable Difference: start with 15-20% changes.

Cadence: Friday data review, Monday team update, end-of-month stakeholder message.

### People Succeed, Not Systems

"The most important sentence in the book." Reframe from "15% increase in adoption of form components" to "Landing Pages teams complete their work one week faster."

### Design Systems as Relief

"The most impactful promise of a design system should be as a relief to the people who use it."

Relief metrics: turnover, job satisfaction, creativity, team engagement, psychological safety.

### Stables and Volatiles

From Michael Lopp: Stables rely on process, Volatiles drive innovation. "A healthy company needs to equally invest in both." Don't use the DS to build everything.

---

## Chapter 10: Evangelism Never Stops

### Lay Out Your Design System Like a Store

Andy's Brick Shop model: front of store shows assembled products, back has raw parts. "Most design systems do it completely opposite. Highlighting components first would be like putting a bin of heads at the front of the store."

Lead with examples of what can be made, not the component inventory.

### Make Your Design System About Its Users

Best-in-class: the Astro Design System (US Space Force) -- shows real products, breaks down page anatomy, links to live demos, provides source code.

---

## Conclusion: Key Lessons

1. "Real organizational value lies in truly connected design systems."
2. "Pilots are the easiest way to make a design system that's kindest for the teams making it and using it."
3. "True collaboration takes the most advantage of the promises of a great design system."
4. "You can serve people well if you understand what drives them."

The rebrand story: changing yellow buttons to purple. Estimated effort: 11 months, ~$10 million. With a connected design system: four weeks maximum.

---

## Note on Evolving the System

Mall's model for system evolution is continuous and emergent, not release-based:
- The Measuring Spoon Cycle is the evolution model.
- Canon/Expanded Universe as implicit sunset criteria.
- Connectedness enables safe evolution via package management.
- The piloting process surfaces what to retire each Systems week.

For explicit deprecation frameworks, see Source 1 (Frost) and Source 6 (Couldwell).

---


### Measuring Success at Startup Scale

- 3 product surfaces migrated to MUI using shared DS components by end of quarter
- Engineering time per migrated surface decreases by 15% from first to third pilot
- 0 regressions in clinical workflow completion during migration
- 1 designer writes CSS/tokens directly (Hot Potato adoption)
