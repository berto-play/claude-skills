# Anti-Patterns — Before & After

Common patterns that break skills, with concrete fixes.

---

## Contents

- D1: Identity Preamble
- D2: Filler Paragraphs
- Why This Matters
- D3: Duplication
- Extraction Rules
- Output Format
- Quality Standards
- Output Style
- D4: Dead Weight Sections
- Overview
- Purpose
- A4: Wrong Voice in Description
- B2: Missing or Generic Examples
- Example
- Example
- B3: Uniform Guardrails on Mixed-Risk Skill
- Rules
- Degrees of Freedom
- C1: Monolith (No Progressive Disclosure)

---

## D1: Identity Preamble

**Why it fails:** Identity assignment doesn't change AI behavior. Claude doesn't perform better because you tell it to be an expert. It wastes tokens on theatre instead of constraints.

**Before:**
```markdown
You are an expert financial analyst with 20 years of experience in emerging markets.
You have deep knowledge of macroeconomic indicators and geopolitical risk.
You always provide well-reasoned, data-driven analysis.
```

**After:**
```markdown
# Macro Analyst

Analyses macroeconomic regimes, geopolitical risk, and cross-asset themes.
```

---

## D2: Filler Paragraphs

**Why it fails:** Motivational text adds zero behavioral constraint. It inflates line count and dilutes signal. The test: "If I delete this, does the AI respond differently?" If no → delete.

**Before:**
```markdown
## Why This Matters

In today's fast-paced healthcare environment, accurate document extraction
is critical for maintaining compliance and ensuring patient safety. Teams
that fail to properly extract and validate regulatory content risk costly
penalties and compromised care quality. This skill exists to prevent those
outcomes and ensure every extraction meets the highest standards.
```

**After:**
```markdown
[Deleted entirely. The skill's workflows already enforce extraction accuracy.]
```

---

## D3: Duplication

**Why it fails:** Same instruction in multiple places means the AI reads it N times, wasting tokens. Worse, duplicates often drift apart during edits, creating contradictions.

**Before:**
```markdown
## Extraction Rules
- Always cite the source document and section.

## Output Format
- Every claim must include a source citation.

## Quality Standards
- Source attribution is mandatory for all extracted content.
```

**After:**
```markdown
## Output Style
- Cite the source document and section for every claim.
```

---

## D4: Dead Weight Sections

**Why it fails:** Sections with headers but no actionable constraints exist for organizational aesthetics, not behavior. They pad the skill without adding value.

**Before:**
```markdown
## Overview

This skill helps users analyze financial documents and extract key metrics.
It supports multiple document types and provides structured output.

## Purpose

The purpose of this skill is to ensure accurate financial analysis by
following a systematic extraction and validation process.
```

**After:**
```markdown
[Both deleted. The description already states what the skill does.
The workflows define how it does it. No overview or purpose section needed.]
```

---

## A4: Wrong Voice in Description

**Why it fails:** "Use this skill to..." addresses the user. But the description is read by Claude to decide whether to activate. Third person tells Claude what the skill DOES, which is what Claude needs for routing.

**Before:**
```yaml
description: >
  Use this skill whenever you need to analyze interviews or check compliance.
  It helps you find patterns across transcripts and cross-reference with
  regulatory frameworks.
```

**After:**
```yaml
description: >
  Analyses interviews, transcripts, and regulatory documents when uploaded.
  Triggers on: "analyse these interviews", "check compliance",
  "cross-reference with Agenas", "find patterns across transcripts."
```

---

## B2: Missing or Generic Examples

**Why it fails:** Without examples, the AI has to guess what the skill's output should look like. Generic examples ("user asks question, skill answers") teach nothing.

**Before:**
```markdown
## Example
The user uploads a document. The skill extracts key information and
presents it in a structured format.
```

**After:**
```markdown
## Example

**Input:** Operator uploads an ASL protocol stating RPM data must be
transmitted within 15 minutes of patient measurement.

**Output:**
- **Requirement**: Data transmission within 15 minutes of measurement.
- **Risk**: Current architecture batches every 30 minutes. Non-compliant.
- **Design implication**: Enrollment flow must capture ASL-specific SLA.
- **Open question**: What is the SLA outside business hours? Ask Mattia.
```

---

## B3: Uniform Guardrails on Mixed-Risk Skill

**Why it fails:** A skill that handles both regulatory compliance AND strategic brainstorming shouldn't apply the same rules to both. Over-constraining strategy kills usefulness. Under-constraining compliance creates risk.

**Before:**
```markdown
## Rules
- Always cite sources for every statement.
- Never speculate or infer.
- Only use information from provided documents.
```

**After:**
```markdown
## Degrees of Freedom

### Tight guardrails (compliance outputs)
- Cite specific regulatory source, section, and article.
- Never fabricate regulatory content. Ask for the source if unavailable.
- State when something is interpretation vs. direct requirement.

### Flexible guardrails (strategic outputs)
- Suggest framings and prioritizations based on available context.
- Propose options rather than directives. Operator decides.
- Use judgment about what stakeholders need to hear.
```

---

## C1: Monolith (No Progressive Disclosure)

**Why it fails:** A 450-line SKILL.md loads every time the skill activates. If 200 lines are reference taxonomy that's only needed 10% of the time, those tokens are wasted 90% of invocations.

**Before:**
```
SKILL.md (450 lines)
├── Workflows (150 lines) — needed every time
├── Hallucination taxonomy (120 lines) — needed rarely
├── Response templates (80 lines) — needed sometimes
└── Regulatory reference (100 lines) — needed sometimes
```

**After:**
```
SKILL.md (200 lines)
├── Workflows (150 lines)
├── Route: "See references/taxonomy.md"
├── Route: "See references/templates.md"
└── Route: "See references/regulatory.md"

references/
├── taxonomy.md (120 lines)
├── templates.md (80 lines)
└── regulatory.md (100 lines)
```
