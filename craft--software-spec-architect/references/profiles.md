# Product Profiles

Pick one primary profile before generating. It decides which template sections are
filled in full, which are marked N/A with a reason, and which edge lens to sweep. This
is how the skill stays lean: a spec never carries sections its product type does not
need. When two profiles both apply (a web app with a public API), pick the primary and
pull the second profile's extra sections in as needed.

## Activation table

| Section | UI (app/web/mobile) | API / integration | CLI / utility | Data pipeline / batch | Cloud / distributed |
|---------|:---:|:---:|:---:|:---:|:---:|
| 4 Lifecycle | fresh-device + returning | first-use + steady-state | first-use + steady-state | first-run + steady-state | deploy + steady-state |
| 6 Screen states | full | N/A | N/A (use CLI I/O below) | N/A | N/A |
| 7 Behavioral matrix | full | full | full | full | full |
| 11 Data model | if it persists | usually | if it persists | yes | yes |
| 12 System shape + contracts | if multi-part | contracts required | usually N/A | stages required | required |
| 10 Security sub-block | if auth/sensitive | usually | rarely | if sensitive | required |
| 17 Install/deploy | device install | deploy + config | binary + install | schedule + deploy | deploy + config |

Sections not listed (vision, problem, users, stories, errors, NFRs, non-goals, metrics,
open questions) are filled for every profile.

## Per-profile edge lens

Sweep these in the behavioral matrix (section 7) on top of the 8 hunting grounds.

- **UI:** double-tap and rapid re-tap, back/forward and deep links, rotation and resize,
  keyboard and screen-reader focus order, offline and reconnect, session expiry mid-form,
  interrupted navigation, partial data render.
- **API / integration:** malformed and oversized payloads, missing/invalid auth, expired
  token, rate-limit hit, idempotency of retries, partial success, versioning and unknown
  fields, downstream timeout and its retry/backoff, webhook redelivery and ordering.
- **CLI / utility:** missing or conflicting flags, piped stdin vs interactive, huge input
  stream, non-zero exit codes for each failure class, quiet vs verbose, signal (Ctrl-C)
  mid-run, no color / non-TTY, environment variable overrides.
- **Data pipeline / batch:** empty and single-row inputs, schema drift, poison record,
  partial batch failure and resume, exactly-once vs at-least-once, backfill and
  re-processing, watermark/late data, idempotent writes, dead-letter handling.
- **Cloud / distributed:** node or region loss, network partition, clock skew, thundering
  herd, cache stampede, eventual-consistency read-after-write, cascading failure and
  circuit breaking, tenant isolation breach.

## Opt-in complex / regulated overlay

Off by default. Turn it on only when the software is multi-tenant, handles health or
financial data, faces a regulator, or the operator asks for full rigor. Offer it; do not
switch it on silently. It adds four things on top of the normal pack.

**1. Requirement IDs + traceability.** Give each requirement a stable ID and a one-line
record (source, rationale, priority, status, verification method, related acceptance
criteria). Prefixes: `FR-###` functional, `NFR-###` quality, `SEC-###` security,
`DATA-###` data, `INT-###` integration, `OPS-###` operational. Then a traceability map:
goal -> requirement -> story -> acceptance criterion -> test -> component. This is the
biggest lift; only worth it when auditability is required. Standard: ISO/IEC/IEEE 29148.

**2. Machine-readable contracts.** For every interface, name the contract and where it
lives, and require it be validated against primary docs (record version + retrieval date):
- HTTP APIs -> OpenAPI: https://spec.openapis.org/oas/latest.html
- Event / message interfaces -> AsyncAPI: https://www.asyncapi.com/docs/reference/specification/v3.0.0
- Structured payloads -> JSON Schema.

**3. Split security spec.** Promote the security sub-block to its own section: assets and
threat actors, authn/authz, access-control matrix, tenant isolation, secrets management,
trust boundaries, abuse cases, rate limiting, audit events, encryption boundaries,
dependency/supply-chain risk, vulnerability response.
- OWASP ASVS 5.0: https://owasp.org/www-project-application-security-verification-standard/
- NIST SSDF (SP 800-218): https://csrc.nist.gov/pubs/sp/800/218/final and https://csrc.nist.gov/projects/ssdf

**4. Architecture views (C4, selectively).** System context and container views; add a
deployment view and trust boundaries; add component views only where one earns its place.
- C4 model: https://c4model.com/introduction · diagrams: https://c4model.com/diagrams · checklist: https://c4model.com/diagrams/checklist

**Expanded quality attributes** (pair with section 9 when this overlay is on): availability
(target with measurement), reliability and data integrity, recoverability with RTO and RPO,
scalability, observability, maintainability, deployability. Each still needs a measurable
quality scenario (stimulus + condition + response + measurement).

Accessibility standard for UI work, any profile: WCAG 2.2, name a target level.
- WCAG 2.2: https://www.w3.org/TR/WCAG22/ · overview: https://www.w3.org/WAI/standards-guidelines/wcag/

Agent Skills format reference: https://agentskills.io/specification · https://agentskills.io/home

## Overlay QA additions

When the overlay is on, the self-QA gate also checks: every requirement has a unique ID
and a verification method; every acceptance criterion traces to a requirement and every
requirement to at least one acceptance criterion; every named interface has a contract;
every architecture decision is recorded; no placeholder or vague adverb survives. Report
these alongside the base gate result.
