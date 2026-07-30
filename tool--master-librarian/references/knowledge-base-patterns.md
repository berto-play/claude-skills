# Knowledge Base Design Patterns

Reference for Workflow 6 (Knowledge Base Design), Workflow 7 (Acquisition Strategy), and AI Corpus Mode. How to design a corpus for a purpose, spec it, and format it for agent consumption.

---

## The five corpus components

Every purpose-built knowledge base decomposes into the same five components. Design each explicitly; skipping one is how corpora fail silently.

| Component | What it holds | Failure if missing |
|---|---|---|
| **Core doctrine** | The field's settled knowledge — canonical texts, standards, established frameworks | Agent reinvents basics, contradicts the field |
| **Methods** | How work actually gets done — procedures, templates, worked examples | Agent knows *about* the domain but can't produce in it |
| **Reference data** | Facts, taxonomies, regulations, standards, terminology | Agent hallucinates specifics (names, thresholds, codes) |
| **Edge cases & failure modes** | What goes wrong, exceptions, disputed territory | Agent is confidently wrong exactly where stakes are highest |
| **Critique / adversarial** | The serious challenges to the standard view | Agent presents contested claims as settled |

The critique component is the one designers skip most and the one that most improves output honesty — it is the corpus-level version of the anti-sycophancy directive.

## Criticality tiers

Tier every source. The tier drives acquisition order (Workflow 7) and what gets cut when the corpus must shrink.

| Tier | Test | Acquisition rule |
|---|---|---|
| **Critical** | Agent produces wrong or dangerous output without it | Acquire before the agent ships. Blockers. |
| **Important** | Output quality visibly drops without it | Acquire in the first working cycle |
| **Nice-to-have** | Marginal depth or style improvement | Acquire only on demonstrated need |

**Lean corpus rule:** the smallest corpus that covers the purpose wins. Every source must earn its tier. If a source can't be assigned a failure mode it prevents, it doesn't belong in the spec.

## The corpus spec format

Deliver Workflow 6 output as this table (one row per source):

| Field | Content |
|---|---|
| ID | Short handle (`core-01`, `method-03`) |
| Source | Title, author, year — citation-verified per the Universal protocol |
| Component | doctrine / methods / data / edge-cases / critique |
| Tier | critical / important / nice-to-have |
| Why | One line: the failure mode this source prevents |
| Access | Best path from the Workflow 2 hierarchy; cost-value line if paid |
| Format | PDF / EPUB / HTML / dataset / API |
| Extraction | Route to `tool--doc-to-markdown` mode: RAG chunks / context window / knowledge base |

Follow the table with: **acquisition order** (tier, then unlock order) and **open gaps** (components with no adequate source found — never paper over a gap with a weak source).

## Metadata schema (AI Corpus Mode)

When the corpus feeds a RAG pipeline or agent, tag every ingested document with:

```yaml
source_id: core-01
title: ""            # verified exact title
author: ""           # verified
year:
authority: canonical | peer-reviewed | practitioner | gray-literature | contested
recency: current | dated-but-valid | superseded-in-part
domain_tags: []      # field taxonomy, 2-5 tags
confidence: high | medium | low   # librarian's confidence in source reliability
component: doctrine | methods | data | edge-cases | critique
tier: critical | important | nice-to-have
```

`authority` and `confidence` are separate on purpose: a canonical text can be low-confidence for a specific use (dated edition), and gray literature can be high-confidence (a regulator's own FAQ).

## Chunking guidance (hand-off, not implementation)

The librarian specs; `tool--doc-to-markdown` extracts. But the spec should state per source:

- **Chunk by structure, not size, when structure exists** — statutes by article, textbooks by section, papers by heading. Fall back to size-based only for unstructured prose.
- **Reference data wants small chunks** (a table row or definition per chunk); **doctrine wants larger ones** (an argument needs its context).
- **Never split a normative statement from its exceptions** — a rule chunked away from its exemptions is a hallucination generator.

## Worked example (compact)

**Purpose:** agent that screens job applications for a design role, EU market.

| ID | Source | Component | Tier | Why | Access | Format | Extraction |
|---|---|---|---|---|---|---|---|
| data-01 | EU employment-law summaries (official EURES / national labor agency pages) | data | critical | Prevents illegal screening criteria | Free (official portals) | HTML | knowledge base |
| doctrine-01 | Structured-interview meta-analyses (Schmidt & Hunter line of work) | doctrine | critical | Prevents gut-feel scoring; validated methods only | Unpaywall → ILL | PDF | RAG chunks |
| edge-01 | Bias-in-hiring research (audit studies) | edge-cases | critical | Prevents proxy discrimination the operator can't see | Google Scholar OA versions | PDF | RAG chunks |
| method-01 | Role taxonomy + leveling frameworks for design roles | methods | important | Consistent leveling language | Free (published frameworks) | HTML/PDF | context window |
| critique-01 | Critiques of algorithmic screening (e.g., HireVue controversy literature) | critique | important | Agent states limits of its own screening | Free (press + OA papers) | HTML/PDF | RAG chunks |
| data-02 | Salary benchmark dataset (paid — run cost-value check) | data | nice-to-have | Only if compensation advice is in scope | Paid (cost-value verdict first) | dataset | knowledge base |

Gaps flagged: no adequate open dataset for EU design-role salaries → named as a gap, not filled with a weak proxy.

---

*All sources named in a spec pass Universal Citation Verification. A corpus spec with an invented source is worse than an empty one.*
