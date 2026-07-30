# Typography Pairings Database

Curated font pairings organised by aesthetic mood. Each pairing specifies a Display face (headlines, hero text) and a Body face (running text, labels). All available on Google Fonts unless noted.

## How to Use

Select based on the aesthetic direction chosen in the Design Brief. Pairing number is a reference ID — cite it in the brief.

---

## Precision & Technical

| # | Display | Body | Mood | Best for |
|---|---|---|---|---|
| T01 | JetBrains Mono | IBM Plex Sans | Terminal-meets-corporate | Dev tools, CLI products, technical docs |
| T02 | Space Mono | Work Sans | Mechanical, structured | Data platforms, engineering dashboards |
| T03 | IBM Plex Serif | IBM Plex Sans | Authoritative, systematic | Enterprise B2B, legal tech, compliance |
| T04 | Fira Code | Source Sans 3 | Monospace precision | IDE themes, code-adjacent products |
| T05 | Overpass Mono | Overpass | Government-grade clarity | Public sector, infrastructure, utilities |

## Editorial & Magazine

| # | Display | Body | Mood | Best for |
|---|---|---|---|---|
| T06 | Playfair Display | Source Serif 4 | Classic editorial | News, publishing, long-form content |
| T07 | Fraunces | Libre Franklin | Warm, literary | Magazines, book platforms, cultural |
| T08 | Lora | Karla | Elegant, readable | Blogs, personal sites, storytelling |
| T09 | Cormorant Garamond | Nunito Sans | Refined, delicate | Art, culture, luxury editorial |
| T10 | Newsreader | Inter Tight | Newspaper, factual | Journalism, research, reports |

## Modern & Clean

| # | Display | Body | Mood | Best for |
|---|---|---|---|---|
| T11 | Sora | DM Sans | Geometric, contemporary | SaaS products, modern dashboards |
| T12 | Plus Jakarta Sans | Geist | Rounded, friendly-professional | Consumer SaaS, collaboration tools |
| T13 | Outfit | Instrument Sans | Crisp, no-nonsense | Fintech, productivity, task management |
| T14 | Bricolage Grotesque | Satoshi | Distinctive, modern | Creative tools, design platforms |
| T15 | General Sans | Cabinet Grotesk | Swiss-inspired, sharp | Agency sites, portfolios |

## Warm & Approachable

| # | Display | Body | Mood | Best for |
|---|---|---|---|---|
| T16 | Recoleta | Nunito | Friendly, rounded | Wellness, food, children, education |
| T17 | Quicksand | Hind | Playful, soft | Social apps, onboarding, lifestyle |
| T18 | Comfortaa | Rubik | Bubbly, modern | Gaming, entertainment, youth |
| T19 | Baloo 2 | Mukta | Rounded, cheerful | E-commerce, consumer products |
| T20 | Grandstander | Comic Neue | Hand-drawn, casual | Kids, casual gaming, playful brands |

## Bold & Expressive

| # | Display | Body | Mood | Best for |
|---|---|---|---|---|
| T21 | Clash Display | General Sans | High-impact, fashion | Fashion, music, creative agencies |
| T22 | Cabinet Grotesk | Switzer | Bold, confident | Startup marketing, product launches |
| T23 | Bebas Neue | Barlow | Tall, condensed, impactful | Sports, events, headlines |
| T24 | Anton | Archivo | Ultra-bold, in-your-face | Entertainment, streaming, gaming |
| T25 | Dela Gothic One | Zen Kaku Gothic New | Heavy, Japanese-influenced | Gaming, anime, bold brands |

## Luxury & Refined

| # | Display | Body | Mood | Best for |
|---|---|---|---|---|
| T26 | Tenor Sans | Cormorant | High-fashion, thin | Luxury retail, high-end brands |
| T27 | Marcellus | Raleway | Art Deco, elegant | Hotels, premium events, spirits |
| T28 | DM Serif Display | DM Sans | Balanced, sophisticated | Premium SaaS, upmarket B2B |
| T29 | Bodoni Moda | Montserrat | Classic luxury | Fashion houses, premium editorial |
| T30 | Cinzel | Fauna One | Stately, architectural | Museums, heritage, premium brands |

## Brutalist & Raw

| # | Display | Body | Mood | Best for |
|---|---|---|---|---|
| T31 | Archivo Black | Archivo | Block-heavy, industrial | Creative agencies, brutalist web |
| T32 | Chakra Petch | Azeret Mono | Technical, angular | Cybersecurity, dev tools, crypto |
| T33 | Bungee | Space Grotesk | Layered, experimental | Creative studios, experimental art |
| T34 | Orbitron | Rajdhani | Sci-fi, geometric | Gaming, space tech, futurism |
| T35 | Press Start 2P | VT323 | Pixel, retro-digital | Retro gaming, nostalgia, 8-bit |

## Data & Analytics

| # | Display | Body | Mood | Best for |
|---|---|---|---|---|
| T36 | Geist | Geist Mono | Vercel-native, clean | Analytics dashboards, data tools |
| T37 | Manrope | Fira Sans | Optical clarity, tabular | Financial dashboards, spreadsheets |
| T38 | Albert Sans | Red Hat Mono | Neutral, data-first | BI platforms, monitoring |
| T39 | Atkinson Hyperlegible | IBM Plex Mono | Maximum readability | Accessibility-critical, medical data |
| T40 | Lexend | Inconsolata | Readability-optimised | Education analytics, reading tools |

## Mobile-Optimised

| # | Display | Body | Mood | Best for |
|---|---|---|---|---|
| T41 | SF Pro Display | SF Pro Text | iOS native (system font) | iOS/macOS apps |
| T42 | Google Sans | Roboto Flex | Android native | Material Design apps |
| T43 | Nunito Sans | Nunito | Rounded, mobile-friendly | Cross-platform consumer apps |
| T44 | Urbanist | Red Hat Display | Geometric, mobile-modern | Fintech apps, mobile dashboards |
| T45 | Figtree | Figtree | Simple, high x-height | Mobile-first web, PWAs |

---

## Font Loading Rules

1. **Subset.** Only load the character sets you use. Latin-ext only if needed.
2. **Variable fonts.** Prefer variable weights over multiple static files. One file, all weights.
3. **Display swap.** Always use `font-display: swap` to prevent invisible text during load.
4. **Preload.** `<link rel="preload">` the primary body font. Display font can lazy-load.
5. **Fallback stack.** Always define a fallback that matches metrics: `font-family: 'Display', 'Georgia', serif`.
6. **Maximum 2 families.** Load exactly 2 font families per project. No exceptions.
