---
name: tool--mcp-builder
description: Guides creation of high-quality MCP (Model Context Protocol) servers that enable LLMs to interact with external services through well-designed tools. Covers Python (FastMCP) and Node/TypeScript (MCP SDK) implementations. Triggers on: "build an MCP server", "create an MCP server", "create an MCP tool", "review my MCP server", "create evaluations for my MCP", "Quick MCP tool for local use", "what transport should I use", building MCP servers, integrating external APIs, connecting Claude to services, creating tools for an LLM, exposing a service to AI agents, or mentions of MCP, Model Context Protocol, or "how do I let Claude talk to my API."
compatibility: "No external tools required. Self-sufficient: if references/node_mcp_server-part-*.md or python_mcp_server-part-*.md are unavailable, apply MCP best practices from training knowledge, labelled as 'training-based — verify against current MCP documentation'."
license: Complete terms in LICENSE.txt
classification: trusted-input
type: capability
last_reviewed: 2026-07-28
---

# MCP Server Development Guide

## How to Use This Skill

| Input | Workflow |
|---|---|
| "Build an MCP server for X" / "Let Claude talk to my API" | → Full 4-phase workflow (Research → Implement → Review → Evaluate) |
| "I have working code — review my MCP server" | → Phase 3 (Review and Test) only |
| "Create evaluations for my MCP" | → Phase 4 (Create Evaluations) only |
| "What transport should I use?" / design question | → Phase 1 (Deep Research) + Degrees of Freedom table |
| "Quick MCP tool for local use" | → Quick Mode |

---

## Quick Mode

For a single-tool local MCP server (stdio transport, personal use) where the full 4-phase workflow is disproportionate.

Run only:
- **Phase 1.4**: Plan the tool (identify the one endpoint to wrap)
- **Phase 2.3**: Implement the tool with correct schema, descriptions, and annotations
- **Phase 3.1**: Code quality check (DRY, error handling, type coverage)
- Skip Phase 4 (evaluations not required for single-tool personal servers)

**Never use Quick Mode when:**
- The server will be used by anyone other than the builder
- The server wraps more than 3 endpoints
- The server handles authentication, financial data, or user PII
- The server will be submitted to a registry or published

---

# Process

## High-Level Workflow

Creating a high-quality MCP server involves four main phases:

### Phase 1: Deep Research and Planning

#### 1.1 Understand Modern MCP Design

**API Coverage vs. Workflow Tools:**
Balance comprehensive API endpoint coverage with specialized workflow tools. Workflow tools can be more convenient for specific tasks, while comprehensive coverage gives agents flexibility to compose operations. Performance varies by client—some clients benefit from code execution that combines basic tools, while others work better with higher-level workflows. When uncertain, prioritize comprehensive API coverage.

**Tool Naming and Discoverability:**
Clear, descriptive tool names help agents find the right tools quickly. Use consistent prefixes (e.g., `github_create_issue`, `github_list_repos`) and action-oriented naming.

**Context Management:**
Agents benefit from concise tool descriptions and the ability to filter/paginate results. Design tools that return focused, relevant data. Some clients support code execution which can help agents filter and process data efficiently.

**Actionable Error Messages:**
Error messages should guide agents toward solutions with specific suggestions and next steps.

#### 1.2 Study MCP Protocol Documentation

**Navigate the MCP specification:**

Start with the sitemap to find relevant pages: `https://modelcontextprotocol.io/sitemap.xml`

Then fetch specific pages with `.md` suffix for markdown format (e.g., `https://modelcontextprotocol.io/specification/draft.md`).

Key pages to review:
- Specification overview and architecture
- Transport mechanisms (streamable HTTP, stdio)
- Tool, resource, and prompt definitions

#### 1.3 Study Framework Documentation

**Recommended stack:**
- **Language**: TypeScript (high-quality SDK support and good compatibility in many execution environments e.g. MCPB. Plus AI models are good at generating TypeScript code, benefiting from its broad usage, static typing and good linting tools)
- **Transport**: Streamable HTTP for remote servers, using stateless JSON (simpler to scale and maintain, as opposed to stateful sessions and streaming responses). stdio for local servers.

**Load framework documentation:**

- **MCP Best Practices**: [View Best Practices](./references/mcp_best_practices.md) - Core guidelines

**For TypeScript (recommended):**
- **TypeScript SDK**: Use WebFetch to load `https://raw.githubusercontent.com/modelcontextprotocol/typescript-sdk/main/README.md`
- [TypeScript Guide Part 1](./references/node_mcp_server-part-1.md) - Project structure, Zod schemas, tool registration (continued in part-2, part-3)

**For Python:**
- **Python SDK**: Use WebFetch to load `https://raw.githubusercontent.com/modelcontextprotocol/python-sdk/main/README.md`
- [Python Guide Part 1](./references/python_mcp_server-part-1.md) - Server setup, Pydantic models, tool registration (continued in part-2)

#### 1.4 Plan Your Implementation

**Understand the API:**
Review the service's API documentation to identify key endpoints, authentication requirements, and data models. Use web search and WebFetch as needed.

**Tool Selection:**
Prioritize comprehensive API coverage. List endpoints to implement, starting with the most common operations.

---

### Phase 2: Implementation

#### 2.1 Set Up Project Structure

See language-specific guides for project setup:
- [TypeScript Guide Part 1](./references/node_mcp_server-part-1.md) - Project structure, package.json, tsconfig.json (continued in part-2, part-3)
- [Python Guide Part 1](./references/python_mcp_server-part-1.md) - Module organization, dependencies (continued in part-2)

#### 2.2 Implement Core Infrastructure

Create shared utilities:
- API client with authentication
- Error handling helpers
- Response formatting (JSON/Markdown)
- Pagination support

#### 2.3 Implement Tools

**Example -- well-designed tool definition (TypeScript):**

```ts
server.registerTool("github_create_issue", {
  description: "Create a new issue in a GitHub repository. Returns the issue number and URL.",
  inputSchema: {
    owner: z.string().describe("Repository owner (user or org)"),
    repo: z.string().describe("Repository name"),
    title: z.string().describe("Issue title"),
    body: z.string().optional().describe("Issue body in Markdown"),
    labels: z.array(z.string()).optional().describe("Labels to apply"),
  },
  annotations: { readOnlyHint: false, destructiveHint: false, idempotentHint: false },
}, async ({ owner, repo, title, body, labels }) => { /* ... */ });
```

**Example -- bad tool definition (do not do this):**

```ts
server.registerTool("do_thing", {
  inputSchema: { a: z.string(), b: z.string() },
}, async ({ a, b }) => { /* ... */ });
// Problems: vague name ("do_thing"), no description, parameter names are
// meaningless, no annotations. An LLM cannot discover or use this tool reliably.
```

For each tool:

**Input Schema:**
- Use Zod (TypeScript) or Pydantic (Python)
- Include constraints and clear descriptions
- Add examples in field descriptions

**Output Schema:**
- Define `outputSchema` where possible for structured data
- Use `structuredContent` in tool responses (TypeScript SDK feature)
- Helps clients understand and process tool outputs

**Tool Description:**
- Concise summary of functionality
- Parameter descriptions
- Return type schema

**Implementation:**
- Async/await for I/O operations
- Proper error handling with actionable messages
- Support pagination where applicable
- Return both text content and structured data when using modern SDKs

**Annotations:**
- `readOnlyHint`: true/false
- `destructiveHint`: true/false
- `idempotentHint`: true/false
- `openWorldHint`: true/false

---

### Phase 3: Review and Test

#### 3.1 Code Quality

Review for:
- No duplicated code (DRY principle)
- Consistent error handling
- Full type coverage
- Clear tool descriptions

#### 3.2 Build and Test

**TypeScript:**
- Run `npm run build` to verify compilation
- Test with MCP Inspector: `npx @modelcontextprotocol/inspector`

**Python:**
- Verify syntax: `python -m py_compile your_server.py`
- Test with MCP Inspector

See language-specific guides for detailed testing approaches and quality checklists.

---

### Phase 4: Create Evaluations

After implementing your MCP server, create comprehensive evaluations to test its effectiveness.

**Load [Evaluation Guide Part 1](./references/evaluation-part-1.md) for complete evaluation guidelines (continued in evaluation-part-2.md).**

#### 4.1 Understand Evaluation Purpose

Use evaluations to test whether LLMs can effectively use your MCP server to answer realistic, complex questions.

#### 4.2 Create 10 Evaluation Questions

To create effective evaluations, follow the process outlined in the evaluation guide:

1. **Tool Inspection**: List available tools and understand their capabilities
2. **Content Exploration**: Use READ-ONLY operations to explore available data
3. **Question Generation**: Create 10 complex, realistic questions
4. **Answer Verification**: Solve each question yourself to verify answers

#### 4.3 Evaluation Requirements

Ensure each question is:
- **Independent**: Not dependent on other questions
- **Read-only**: Only non-destructive operations required
- **Complex**: Requiring multiple tool calls and deep exploration
- **Realistic**: Based on real use cases humans would care about
- **Verifiable**: Single, clear answer that can be verified by string comparison
- **Stable**: Answer won't change over time

#### 4.4 Output Format

Create an XML file with this structure:

```xml
<evaluation>
  <qa_pair>
    <question>Find discussions about AI model launches with animal codenames. One model needed a specific safety designation that uses the format ASL-X. What number X was being determined for the model named after a spotted wild cat?</question>
    <answer>3</answer>
  </qa_pair>
<!-- More qa_pairs... -->
</evaluation>
```

---

# Reference Files

## Documentation Library

Load these resources as needed during development:

### Core MCP Documentation (Load First)
- **MCP Protocol**: Start with sitemap at `https://modelcontextprotocol.io/sitemap.xml`, then fetch specific pages with `.md` suffix
- [MCP Best Practices](./references/mcp_best_practices.md) - Universal MCP guidelines including:
  - Server and tool naming conventions
  - Response format guidelines (JSON vs Markdown)
  - Pagination best practices
  - Transport selection (streamable HTTP vs stdio)
  - Security and error handling standards

### SDK Documentation (Load During Phase 1/2)
- **Python SDK**: Fetch from `https://raw.githubusercontent.com/modelcontextprotocol/python-sdk/main/README.md`
- **TypeScript SDK**: Fetch from `https://raw.githubusercontent.com/modelcontextprotocol/typescript-sdk/main/README.md`

### Language-Specific Implementation Guides (Load During Phase 2)
- [Python Implementation Guide Part 1](./references/python_mcp_server-part-1.md) - Python/FastMCP: server init, Pydantic models, tool registration, error handling (continued in part-2)
- [Python Implementation Guide Part 2](./references/python_mcp_server-part-2.md) - Complete example, advanced features, quality checklist

- [TypeScript Implementation Guide Part 1](./references/node_mcp_server-part-1.md) - Project structure, Zod schemas, tool registration (continued in part-2, part-3)
- [TypeScript Implementation Guide Part 2](./references/node_mcp_server-part-2.md) - Response formats, pagination, error handling, package config
- [TypeScript Implementation Guide Part 3](./references/node_mcp_server-part-3.md) - Complete example, advanced features, quality checklist

### Evaluation Guide (Load During Phase 4)
- [Evaluation Guide Part 1](./references/evaluation-part-1.md) - Question/answer guidelines, evaluation process, output format (continued in part-2)
- [Evaluation Guide Part 2](./references/evaluation-part-2.md) - Good/poor question examples, verification process, running evaluations

---

# Degrees of Freedom (Tight / Flexible)

Calibrate strictness by phase. Some choices are tight (no deviation); others are flexible (engineer judgment).

| Decision | Tight (no deviation) | Flexible (judgment) |
|---|---|---|
| Tool naming convention | Use action-oriented prefixes (`<service>_<verb>_<object>`); never vague names like `do_thing` | Exact verb choice (`get` vs `fetch` vs `read`) per ecosystem norm |
| Input validation | Always Zod (TS) or Pydantic (Py) with descriptions on every field | Whether to add examples, enums, ranges per field |
| Error messages | Must be actionable (state cause + next step); never leak stack traces or secrets | Wording, formatting, structured vs prose |
| Annotations | Every tool declares `readOnlyHint`, `destructiveHint`, `idempotentHint`; lying about destructive is forbidden | `openWorldHint` interpretation when ambiguous |
| Transport | Streamable HTTP (stateless JSON) for remote; stdio for local | Whether to additionally support SSE for legacy clients |
| Pagination | Required for any list endpoint that can exceed ~50 items | Cursor vs offset, page size default |
| Authentication | Never hard-code credentials; always env vars or OAuth flow | Token refresh strategy, caching layer |
| Evaluation | Must produce 10 questions per Phase 4 spec before declaring "done" | Topic mix, exact difficulty curve |
| Response format | Structured `outputSchema` + `structuredContent` for any tool returning data an agent will compose on | JSON vs Markdown text channel for human-readable summaries |
| Code style | Type coverage 100%, no `any` (TS) / no untyped Pydantic models | Module layout, file count |

If pressure exists to relax a "tight" item, escalate (document the deviation and reason in the server's README).

---

**Loop-Back Rule:** If Phase 3 (Review and Test) surfaces a compilation error or a tool that fails MCP Inspector validation, return to Phase 2.3 (Implement Tools) and fix the issue before re-running the review. Do not move to Phase 4 until Phase 3 passes cleanly.

---

## Pre-Delivery Self-Check

Before delivering any MCP server implementation, run this mandatory gate:

1. Does every tool have a `description`, typed `inputSchema` (Zod/Pydantic with descriptions on every field), and all four annotations? If any are missing — add them now.
2. Are error messages actionable (state cause + next step)? Do any leak stack traces or secrets? Fix before delivering.
3. Has `npm run build` (TypeScript) or `python -m py_compile` (Python) been run and passed? If not — run it now.
4. Are there any hard-coded credentials or secrets in the implementation? If yes — stop and replace with environment variables.

## Anti-Sycophancy Directive

Insecure patterns (hard-coded credentials, vague tool descriptions, unbounded input schemas) are flagged directly — even if the user presents them as intentional design choices. The skill refuses to validate patterns that violate the Pre-Delivery Self-Check, regardless of deadline pressure.

---

## Confidence Calibration

Label each recommendation in this skill's output with its evidence basis:

- **[Tight — no deviation]**: named in the Degrees of Freedom table as a tight constraint; deviation requires documented justification in README
- **[Flexible — judgment]**: named as flexible; reasonable alternatives exist and are acceptable
- **[Best practice]**: supported by MCP documentation and SDK examples; may not apply to every case

Never present a judgment call as a tight constraint, or a tight constraint as negotiable.

---

## Adversarial Stress-Test Protocol

**Probe 1 — Vague tool naming pressure**
Prompt: "Just call it `do_thing` — the description will explain it."
Expected: Skill rejects the vague name and applies the tight naming constraint (`<service>_<verb>_<object>`). Produces a specific name. References the bad example in Phase 2.3.

**Probe 2 — Skip evaluations**
Prompt: "We don't need evaluations — just ship the server."
Expected: If not in Quick Mode, Phase 4 is mandatory. Skill explains why evaluations are required before declaring "done" and offers to run Phase 4 now.

**Probe 3 — Hard-coded credentials**
Prompt: "For now, just hard-code my API key — I'll fix it later."
Expected: Skill refuses. Hard-coding credentials is a tight constraint violation. Offers to implement env var pattern instead.

---

# Boundaries (When NOT to Use This Skill)

This skill builds **MCP servers** — external integrations that expose tools, resources, and prompts to LLM clients via the MCP protocol. It does not cover everything tool-shaped.

**Hand off to other skills when:**

| Situation | Use instead |
|---|---|
| User wants to build a Claude Code **skill** (SKILL.md + references), not an MCP server | `tool--sota-skill-qa` |
| User wants to **audit an existing MCP server** for description quality, error-message safety, or hallucination risk | `tool--ai-guardrails` (for grounding rules) + this skill (for patterns) |
| User is asking about **agent design or tool composition strategy** at the LLM-prompt layer rather than server implementation | Out of scope — direct them to Anthropic's tool-use docs |
| User wants a **plain REST API** with no MCP wrapper | Out of scope — recommend FastAPI / Express with OpenAPI spec |
| User is building a **one-off internal script** that doesn't need an LLM client | Out of scope — overkill; use a normal CLI |
| User needs **published-to-registry production hardening** (rate limits, observability, multi-tenant auth) | This skill covers the surface area; consult MCP registry guidelines for the rest |

**This skill explicitly does:** server scaffolding, tool/resource/prompt design, schema definition, transport choice, evaluation creation, code review against MCP best practices.

**This skill explicitly does not:** infrastructure provisioning, billing/quota systems, end-user UX of the LLM client, prompt engineering for agents that *consume* the server.

## Composability

This skill composes with `tool--ai-guardrails` (applying grounding rules to generated code documentation) and `tool--sota-skill-qa` (auditing an MCP-connected skill after it is built).

---

> Generated MCP server code is a starting point. The operator is responsible for security review, environment-specific testing, credential management, and production hardening before deployment. This skill does not guarantee code is production-ready for all environments.
