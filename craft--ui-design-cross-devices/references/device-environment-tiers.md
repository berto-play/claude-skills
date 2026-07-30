# Device & Environment Tiers

The content surface (see `surface-archetypes.md`) determines WHAT is being designed. The device/environment tier determines HOW it adapts. These are independent axes — don't conflate them.

## Core tiers (most requests only need these)

| Tier | Width/context | Input | Governing constraint | Notes |
|---|---|---|---|---|
| **Ultra-small / glanceable** | Watch, embedded panels, browser widgets | Touch (tiny), crown, voice | One primary task, one piece of information | Web/browser-based only — do not promise full watchOS app parity; watchOS is a native surface, not a reliable web target |
| **Small handheld** | ~320-479px | Touch, thumb-reach | Functional baseline from 320px (WCAG 1.4.10 floor) | Single-column priority, bottom-reachable actions, software-keyboard resilience |
| **Standard mobile** | ~480-767px | Touch | Thumb zone, one-handed use | Portrait + landscape both expected |
| **Large mobile / foldable** | ~768-1023px | Touch + occasional pointer | Don't just stretch mobile layout | Use container queries; support tabletop/split configurations where exposed |
| **Tablet** | ~768-1024px (overlaps foldable) | Touch-first, keyboard-capable | Two-handed, both orientations common | Split panes, stylus-compatible where relevant |
| **Laptop / desktop web** | ~1024-1439px | Pointer + keyboard | Precision pointing, hover available | Multi-column, persistent nav, higher density tolerated |
| **Large desktop / ultrawide** | 1440px+ | Pointer + keyboard | More space ≠ permission to stretch content uncontrolled | Constrained reading width even at large sizes; extra space serves context (auxiliary panels), not just enlargement |
| **10-foot UI (TV/projector)** | 8-12 ft viewing distance | D-pad/remote, voice, gesture — no pointer | Large safe-margin text, spatial focus navigation | Overscan-safe margins (5-10% inset), no hover, high contrast for variable room lighting, low content density (lean-back) |

## Thumb-zone geometry (Josh Clark, *Designing for Touch*, A Book Apart)

Touch target *size* (44pt/48dp, see `canonical-sources.md`) says nothing about *where on screen* a thumb can comfortably and accurately reach — that's a separate, real constraint this book quantifies directly:

- **Phone (one-handed)**: only about a third of the screen is truly effortless — a bottom arc, on the side opposite the thumb (bottom-left arc for a right-hand hold). Comfort and accuracy diverge: the most *accurate* tapping zone is a smaller fan-shaped region within the comfort arc; top corners (especially top-right for right-handed users) are both the hardest to reach and the least accurate. Top-vs-bottom matters more than left-vs-right — screen bottom is comfortable on every phone regardless of hand; screen top always demands a stretch. Put core, frequent actions at bottom or screen-middle (where both hand orientations overlap), never assume a single top-corner action is easy to reach.
- **Phablet / large mobile**: the one-handed thumb zone is *smaller* than on a regular phone, not larger — past a size threshold, the grip shifts to add pinky support underneath, which reduces thumb range rather than extending it. Don't assume "bigger screen = bigger reachable area."
- **Tablet**: no consistent thumb zone at all — grip varies by stance/posture (held at side edges, propped on a surface, two-handed). Design tablet interfaces around flexible reach rather than a fixed zone; this is why tablet layouts benefit from spreading primary actions rather than anchoring everything to one screen edge.
- **Hybrid/laptop touchscreens**: thumb zone is defined by a hands-on-corners grip — targets near where hands already rest avoid forcing an arm-raise to reach the touchscreen.
- **Practical rule**: when a component's adaptation strategy (reflow/reveal/replace) changes across phone → phablet → tablet, re-derive the reachable zone for each — don't assume the "safe" zone from one tier still applies at the next.

## Extended tiers (only surface these when the request actually needs them)

| Tier | Context | Key requirement |
|---|---|---|
| **Kiosk / digital signage** | Fullscreen, unattended | Inactivity reset, large targets, offline/connection-loss states, burn-in-aware presentation |
| **Print / export** | PDF, printed report, invoice | Print stylesheet, page-break handling, hidden interactive chrome, expanded URLs, stable tables/figures |
| **Vision Pro browser window** | Conventional site, displayed spatially | Large comfortable targets, strong hover/selection feedback, no tiny dense controls |
| **Vision Pro / WebXR immersive** | Explicit immersive mode | Gaze/hand/controller adaptation, comfortable motion, mandatory non-immersive fallback, explicit exit control |
| **Embedded browser / webview** | Auth webviews, in-app panels | Unknown viewport assumptions, no browser-chrome assumptions, graceful degradation for unavailable APIs |

## Platform boundaries — say this explicitly, don't overclaim

- **Apple Watch**: web/browser support exists only within an embedded web view inside a native app — there is no general-purpose Safari-on-watchOS target. If a request implies "the website works on Apple Watch," correct that to "an ultra-small responsive mode works in an embedded web view; a true watch experience needs a native watchOS app."
- **Apple TV**: same boundary — Apple's tvOS guidance centers on native apps and media playback, not general Safari browsing. Call the 10-foot tier "ten-foot browser / remote-controlled display" support, not "Apple TV support," unless a native tvOS shell is explicitly in scope.

## Input modes to document per component (not just per tier)

Keyboard, mouse, trackpad, touch, stylus, coarse pointer, fine pointer, remote/D-pad, gamepad, device motion/orientation, gaze, hand tracking, voice, screen reader/switch control. A component's adaptation strategy should name which of these it supports — "responsive" alone doesn't specify input, and getting input wrong (hover-only on a touchscreen laptop) is a more common failure than getting layout wrong.

## Cross-cutting presentation modes (apply across any tier, not tier-specific)

Light/dark, high contrast, increased text size, browser zoom, reduced motion, reduced transparency, low bandwidth, offline, installed-PWA mode, portrait/landscape, split-screen, fullscreen, HDR/wide-color media. Media queries can key off pointer type, hover capability, and user preferences — not just viewport width; treat width-only responsiveness as incomplete.
