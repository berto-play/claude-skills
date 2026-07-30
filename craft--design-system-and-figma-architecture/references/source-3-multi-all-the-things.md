# Source 3: The Multi-All-The-Things Organization — Brad Frost (2025)

Confidence: HIGH — Full article extracted and verified.

---

## Table of Contents

1. [The Core Tension](#the-core-tension)
2. [The Multi-All-The-Things Taxonomy](#the-multi-all-the-things-taxonomy)
   - 2.1 Multi-product
   - 2.2 Multi-brand
   - 2.3 Sub-brands
   - 2.4 Rebrands, refreshes, and redesigns
   - 2.5 White-labeling and campaigns
   - 2.6 Multi-product families
   - 2.7 Multiple color modes
   - 2.8 Multi-framework
   - 2.9 Multi-platform
3. [The Architectural Answer](#the-architectural-answer)
---

## The Core Tension

Organizations simultaneously need two competing things:

> "We simultaneously need the efficiency, quality, & scale that design systems provide AND the flexibility and agency to meet each product's unique needs."

The question Frost poses: **"Can we reap the benefits of using shared systems without forcing everything to look the same?"** The answer is yes, but it requires a nuanced, tiered architecture rather than a single monolithic system.

The common objection -- "design systems kill creativity" and "make everything look the same" -- contains a grain of truth. Systems do introduce constraints. But in exchange, teams receive: more cohesive UX, higher quality, increased velocity, shared language, a future-friendly foundation, and freedom to focus on higher-value work.

---

## The Multi-All-The-Things Taxonomy

Use this taxonomy during intake to classify what kind of "multi" challenge an organization faces. Most organizations are several of these at once.

### 2.1 Multi-product

The most common case. Two or more digital products sharing a single design system.

**Real-world example -- Caterpillar:** Maintains cat.com, a parts e-commerce store, a careers page, a dealer finder, and more. Their design system is named **Blocks**. It lets them deliver quality at scale across all these properties.

### 2.2 Multi-brand

Multiple brands in one portfolio, each targeting different audiences with distinct visual identities.

**Real-world example -- Marriott:** Portfolio organized by tier (Luxury, Premium, Select, Longer Stays). Each tier's brands (Ritz-Carlton, W Hotels, Courtyard, etc.) serve different audiences with distinct brand identities, visual languages, and messaging -- yet all under one parent organization.

### 2.3 Sub-brands

Brands that inherit from a parent but have their own distinct qualities -- typically differentiated by color, iconography, or typography.

**Real-world example -- DotDash Meredith / Verywell:** Brad Frost's team built the original Verywell health site. It later grew into three sub-brands: **Verywell Health**, **Verywell Fit**, and **Verywell Mind**. Each shares the parent structure but is differentiated by unique colors and iconography.

### 2.4 Rebrands, refreshes, and redesigns

Organizations periodically overhaul their visual language, from tiny tweaks to total transformation. The critical architectural challenge: supporting both **legacy** and **next-gen** designs simultaneously during rollout.

**Real-world example -- Verizon:** Updated from a predominantly black-and-white design to a bright yellow-and-red design. During transition, both the old and new visual languages had to coexist across products. Frost notes: "If the redesign isn't currently in progress, I promise you it's coming!"

### 2.5 White-labeling and campaigns

SaaS companies need customers to insert their own branding. E-commerce companies need seasonal/campaign themes.

**Real-world example -- Blend:** Creates mortgage application software used by banks. White-labeling ensures the experience feels like the bank's, not Blend's, so end users (mortgage applicants) trust they are dealing with their financial institution. Different bank logos, color themes, and branding are applied to the same underlying component set.

**Campaigns:** Separate from white-labeling -- think orange-and-black buttons for a Halloween theme or seasonal promotional styling.

### 2.6 Multi-product families

The same organization maintains fundamentally different product types: marketing websites (big typography, chunky cards, whitespace, illustrations) alongside enterprise software (dense data tables, modals upon modals, complex forms).

**Real-world example -- Salesforce:** Salesforce.com (the marketing site) has a completely different density, typography scale, and layout philosophy than Salesforce's enterprise application. Both must feel like "Salesforce" but serve radically different user tasks.

### 2.7 Multiple color modes

Three levels of complexity:

1. **Inverted/knockout only:** Ensuring text, icons, and components render correctly on dark backgrounds (baseline requirement for all systems).
2. **User preference:** Surfacing theme options in the UI. **Example -- GitHub:** Offers multiple light and dark theme options in its settings.
3. **OS preference:** Respecting `prefers-color-scheme` media query to match the operating system setting. **Example -- Dave Rupert's site and Piccalilli:** Both support OS-level color mode detection, with Piccalilli also offering a manual user toggle.

### 2.8 Multi-framework

One organization, many tech stacks. React, Angular, Vue, Svelte, Drupal, WordPress, AEM, and legacy systems can all coexist.

**The key insight:** "Users don't care that the homepage is powered by WordPress and the checkout is powered by Next.js; they just want an easy-to-use, cohesive experience." The design system must deliver consistency regardless of the rendering framework.

### 2.9 Multi-platform

Technology diversity beyond the web: native iOS, native Android, kiosks, point-of-sale systems, dashboards, digital signage.

**Real-world example -- Target:** Supports websites, native iOS app, native Android app, point-of-sale applications, dashboards, digital signage, and more. The design system must provide coherent design language across all of these fundamentally different platforms.

---

## The Architectural Answer

Frost's resolution has two parts:

### Part 1: Tiered layer-cake architecture

> "It would be a disaster if ALL user interface at an organization originated from a single design system."

Instead, the organization's **design system ecosystem** should establish a tiered layer-cake:

- **Foundation layer (shared):** Core infrastructure, primitives, and tokens shared across everything.
- **System layer (shared but configurable):** Components and patterns that can be themed and adapted.
- **Product layer (autonomous):** Product-specific customizations, overrides, and unique components.

This promotes shared infrastructure at the base while preserving flexibility and autonomy at the product level.

### Part 2: Design tokens as the mechanism

Design tokens **variabilize** style properties -- color, typography, borders, shadows, spacing -- to unlock the ability to serve different aesthetic expressions to different products from the same structural foundation.

Tokens are what allow organizations to accommodate their myriad products, brands, modes, product families, design generations, frameworks, and platforms -- all without sacrificing the benefits of shared systems.

