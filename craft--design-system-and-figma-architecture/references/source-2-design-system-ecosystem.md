# Source 2: The Design System Ecosystem — Brad Frost (2023)

Confidence: HIGH — Full article extracted and verified.

---

## Table of Contents

1. [Governing Principle: Gall's Law](#1-governing-principle-galls-law)
2. [The 5-Layer Architecture Overview](#2-the-5-layer-architecture-overview)
3. [Asset Types in the Ecosystem](#3-asset-types-in-the-ecosystem)
4. [Layer 1: Core Design System (Required)](#4-layer-1-core-design-system-required)
   - 4a. Design Tokens Layer
   - 4b. Foundations Design Library (Figma)
   - 4c. Icons Layer
   - 4d. UI Components
   - 4e. Reference Website
5. [Layer 2: Technology-Specific Implementation (Optional)](#5-layer-2-technology-specific-implementation-optional)
6. [Layer 3: Recipe Layer (Optional)](#6-layer-3-recipe-layer-optional)
7. [Layer 4: Smart Components (Optional)](#7-layer-4-smart-components-optional)
8. [Layer 5: Product Layer (Required)](#8-layer-5-product-layer-required)
9. [The Human Truth](#9-the-human-truth)

---

## 1. Governing Principle: Gall's Law

> "A complex system that works is invariably found to have evolved from a simple system that worked. A complex system designed from scratch never works and cannot be patched up to make it work. You have to start over with a working simple system."
> -- Gall's Law

Operational implication: the design system architecture must be **only as complex as it needs to be**. Add layers or complexity only when real needs arise. Growing a design system is a gradual process. Start with Layer 1 and prove value before expanding.

---

## 2. The 5-Layer Architecture Overview

```
Layer 1: Core Design System      [REQUIRED]  — tokens, icons, UI components, reference site
Layer 2: Technology-Specific      [OPTIONAL]  — framework wrappers, native implementations
Layer 3: Recipes                  [OPTIONAL]  — composed components for specific contexts
Layer 4: Smart Components         [OPTIONAL]  — UI + business logic + backend services
Layer 5: Product                  [REQUIRED]  — the actual websites and apps shipped to users
```

The relationship between these layers is bidirectional. Products inform and influence what is in the design system, and the design system informs and influences the products. Arrows connect each layer to every other layer; the ecosystem is interconnected, not purely top-down.

Nearly all layers beyond Layer 1 and Layer 5 are optional. Organizations start seeing value after implementing only a fraction of the full ecosystem.

---

## 3. Asset Types in the Ecosystem

Each layer is composed of combinations of these six asset types:

| Asset | What It Is | Example |
|---|---|---|
| **Design library** | Figma team library (published, subscribable) | Foundations library, UI component library |
| **Design file** | Figma project file (product-level, subscribes to libraries) | Product screen mockups |
| **Code repository** | Source code home | GitHub repo |
| **Front-end workshop** | Tool to build, view, test, review, and document coded UI | Storybook |
| **Code package** | Library published on a software registry | npm package |
| **Reference website** | Documentation storefront — part marketing, part docs, part support | Zeroheight or custom-built |

---

## 4. Layer 1: Core Design System (Required)

The core design system contains the common, organization-wide UI building blocks. It is a library of solved problems — the "boring stuff" like form controls, buttons, accordions. It tells the official story of how the organization designs and builds user interfaces, and provides the building materials to do it.

**Key assets in the core:** design tokens, icons, UI components, and a reference website.

### 4a. Design Tokens Layer

**What tokens are:** Low-level design definitions — color, typography, border radius, spacing, elevation — that constitute the design language. They are the sub-atomic particles applied to UI components to achieve a specific look and feel. Think of them as brand variables.

**Why tokens must be their own layer (separate from UI components):**
1. **Unlock theming** — support multiple brands, white-labeling, product families, redesigns, dark mode
2. **Separation of concerns** — decouple brand language from UI component structure
3. **Independent versioning** — version brand languages independently of UI component releases

**The 3-tier token architecture** is described as "the secret sauce" for making a design system support multiple brands, white-labeling, different product families, redesigns, dark mode, and more. (Detailed in Frost's companion article "Creating Themeable Design Systems.")

**Token layer assets:**

| Asset | Details |
|---|---|
| Foundations design library (Figma) | See Section 4b below |
| Design tokens repo | JSON files defining all theme values; Style Dictionary converts to CSS custom properties, Sass, iOS, Android, etc. |
| Design tokens package | Technology-specific formatted tokens published on a software registry (npm or similar) for consumption by web products, native apps, other environments |

### 4b. Foundations Design Library (Figma)

In the design discipline, tokens and associated styles are called "foundations." In Figma, these are defined as:

- **Variables** — for most token values (color, spacing, border radius, elevation, etc.)
- **Styles** — for typography-specific tokens (Figma does not yet support typography variables; this is on their roadmap)

The foundations must be managed as their own dedicated Figma library so that other files and libraries — including the UI component library — can subscribe to it.

**Theming consideration:** The library can contain definitions for each supported theme. Organizations supporting dozens or hundreds of themes should consider chunking into separate foundations libraries.

### 4c. Icons Layer

Icons are architecturally similar to tokens: they can exist as their own product, packaged and consumed by many environments.

**When to bundle vs. separate:**
- **Bundle with tokens (common):** When the organization is primarily web-based and icons do not need independent versioning.
- **Separate as own layer (recommended when):** Teams power web, native, AND other non-web software; icons need to be managed and versioned independently of tokens and components.

**Icon layer assets (when separated):**
- Icon Figma library
- Icon SVG repository
- Icon package (published on software registry)

### 4d. UI Components

The star of the design system. When people think "design system," they think of reusable UI components. This sub-layer has five distinct assets:

| Asset | Purpose |
|---|---|
| **Core UI Components Figma library** | Dedicated project where DS designers define component specs (layout, affordances, variants, states). Published as the library product designers subscribe to for drag-and-drop usage. |
| **Web Components library repo** | Source code for coded components. Frost recommends Web Components (using Lit or Stencil) as the core technology for interoperability across any web framework. |
| **Web Components Storybook** | Build, visualize, test, review, and document the coded library. Included in the repo as a dev dependency, published to a URL (via Netlify or similar). Each WIP branch gets its own published Storybook for review. |
| **Web component library package** | Built `dist` directory packaged and published on npm. Any web product — static site, React/Angular/Vue app, CMS site — can pull it in as a dependency. |
| **Reference website** | See Section 4e below. |

**Key architectural insight:** The true source of truth for a design system is the coded component library, not the Figma library. The Figma library serves designer efficiency; the coded library builds real products. "The only thing that really matters at the end of the day is the actual user experience human beings interact with."

### 4e. Reference Website

The reference website is the "storefront" — part marketing, part documentation, part support channel. It gathers all assets under one roof.

**Build vs. buy:** Can be custom-built or powered by a third-party tool like Zeroheight. Zeroheight pulls design assets from Figma and code assets from Storybook, maintaining each as a separate workshop but bringing them together for cross-disciplinary guidance.

---

## 5. Layer 2: Technology-Specific Implementation (Optional)

Translates the core design system into specific technical implementations. Three sub-categories:

**Framework wrapper layer:** Wraps Web Components in framework-specific syntax (e.g., `<ds-button>` becomes `<DsButton>`). Reasons to maintain:
- Some frameworks (especially older React) need massaging for Web Component compatibility
- Preserve hard-earned adoption in existing React/Angular/Vue libraries (replace guts with Web Component internals)
- Incrementally adopt new API naming standards while supporting legacy APIs
- Shield teams from unfamiliar Web Component conventions

**Framework layer assets:** framework code repo, framework Storybook, framework code package.

**Native layer:** iOS/Android component library repositories and packages. Challenges include diverse technologies (Jetpack Compose, Flutter, SwiftUI, UIKit), OS-level UI conventions, immature tooling, and siloed teams.

**Other non-web implementations:** Kiosks, ATMs, medical equipment, airplane seat-backs — same principle applies: dedicated repo for common UI code, deployed via software registry.

---

## 6. Layer 3: Recipe Layer (Optional)

### What Recipes Are

Recipes are composed UI components built from core design system ingredients for use in specific contexts (a product, product family, or business unit). They are not abstract enough or broadly used enough to belong in the core system.

Analogy: The core design system stocks the pantry with ingredients. Product teams take those ingredients and create product-specific compositions.

**Concrete example:** Core components include card, button, button group, heading, text passage, badge, key-value table. From these ingredients:
- E-commerce team composes a **product card recipe**
- Marketing team composes a **promo card recipe**
- Analytics team composes a **customer data card recipe**

### How Recipes Differ from Core Components

| Dimension | Core Component | Recipe |
|---|---|---|
| Scope | Organization-wide | Product or product-family specific |
| Ownership | Design system team | Product team |
| Pace | Slower, more considered | Faster, product-driven |
| Abstraction | High (generic, reusable) | Lower (context-specific composition) |
| Figma subscription | Subscribed to by recipe libraries | Subscribes to foundations + core UI library |

### The "Pressure Release Valve" Concept

The recipe layer serves as a critical pressure release valve for the UI ecosystem. Without it, DS teams frantically try to keep up with every UI-related product decision, running from meeting to meeting, capturing requests in an overcrowded backlog. This leads to bottlenecks and burnout.

With recipes:
- **Product designers** own their product-specific UI components and work at a fast pace
- **Design system designers** carry on working on core ingredients at a slower, more considered pace
- The DS team does not have to own, include, or oversee every bit of UI across the product landscape

This layer is essential for massive organizations managing many business units or sub-brands. IBM's Carbon team leaned into this concept because "business units needed extra capability to tailor how the design system was going to be consumed in their domain."

### How Recipes Graduate

The design system team monitors recipes. If a recipe proves to be broadly successful and reusable, it can be "graduated" into the core design system. Not every recipe is a candidate, but the recipe layer functions as a component incubator.

### Recipe Layer Assets

| Asset | Details |
|---|---|
| **Recipe design libraries** | Figma libraries ("cookbooks") containing product-specific composed components. Subscribe to both foundations and core UI component libraries. Published for downstream product designers. |
| **Recipe repositories** | Coded counterparts to Figma recipe libraries. Can be Web Components, React/Angular/Vue, or native — whatever is practical for the team. |
| **Recipe Storybooks** | Each recipe repo maintains and publishes its own Storybook. Should mirror the corresponding recipe Figma library. |
| **Recipe code packages** | Published on a software registry for downstream product developers to consume. |
| **Recipe reference websites** | Product-specific style guides. Example: if YouTube uses Material Design, the YouTube reference site details YouTube-specific recipes built on Material. Provides guidelines, rationale, configurations, and gotchas. |

---

## 7. Layer 4: Smart Components (Optional)

Design system UI components are intentionally dumb — they handle only presentation and basic functionality (e.g., accordion opens/closes on click). They contain no business logic and are not wired to backend services. This is by design for portability and interoperability.

Smart components wrap dumb DS components and recipes in logic to provide downstream teams with drop-in, ready-to-use functional components.

### Use Cases

- **Form submission and validation** — e.g., React Hook Form, React Final Form
- **Payment component** — processing credit card payments
- **Typeahead** — querying specific services/databases (company directory, product catalog)
- **Data tables** — sorting, filtering, searching logic (e.g., AG Grid)
- **Product grids** — sorting/filtering
- **Analytics wiring** — connecting analytics to UI components
- **CMS-ready components** — making DS and recipe components available to CMS editors

### Extension: Starter Kits

Some organizations extend smart components into full software starter kits — custom boilerplates: "Here is a NextJS environment with the DS tokens, components, and recipes linked as dependencies, all form fields wired up, and routes ready to go." Product teams spin up new projects and immediately build application logic instead of plumbing.

### Ownership

This layer is often maintained by the team that supports the underlying service, using DS components to deliver ready-to-roll solutions.

**Assets:** Smart component repositories and packages. Structure varies because they are adjacent to specific product architectures, but they should be managed as discrete products for clear usage, versioning, and maintenance.

---

## 8. Layer 5: Product Layer (Required)

The product layer is where the rubber meets the road. All infrastructure comes together to power real websites and apps used by real human beings. A design system's success is measured here.

### Product Design Files (Figma)

Product designers spin up Figma files that subscribe to:
1. **The Foundations library** — with the appropriate theme applied
2. **The UI component library**
3. **Any relevant recipe library**

With these subscriptions, designers design product-specific screens and flows.

### Product Codebases

Product codebases consume as dependencies:
- The appropriate framework flavor of the DS component library
- Any applicable recipe packages
- Any smart components

**Example `package.json`:**
```json
{
  "dependencies": {
    "@your-org/design-system-name": "^0.1.0",
    "@your-org/marketing-site-recipes": "^0.1.0",
    "@your-org/smart-form-components": "^0.1.0"
  }
}
```

**Example component usage:**
```jsx
import DsButton from "@your-org/design-system-name/Button";
import SiteHeader from "@your-org/marketing-site-recipes/SiteHeader";
import TextField from "@your-org/smart-form-components/TextField";
```

**Non-framework products** can consume Web Components directly via `<script>` tag — no build tooling required. The DS web component source of truth can power any web-based product irrespective of tech stack.

**Native products** pull in their own flavors of the component library as dependencies (e.g., via Swift Package Manager).

---

## 9. The Human Truth

> "Design systems are less about assets and their relationships to one another, but more about people and their relationships to one another."

Everything in this ecosystem defines ingredients and relationships between assets. It is human beings that hold it all together. The architecture is the skeleton; the humans are the muscle, the nervous system, and the will to keep it alive.
