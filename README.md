# Claude Skills

A public catalog of 46 Claude Code skills I've built. Each folder is one skill: a `SKILL.md` (the instructions Claude reads) plus `references/`.

Grouped by namespace: `mode--` (behavioral enforcement that holds for a session), `tool--` (task utilities), `craft--` (design & product), `fun--` (finance).

---

## Modes — behavioral enforcement (11)

- **[`mode--00-failsafe`](mode--00-failsafe/SKILL.md)** — The router for the eight failsafe modes (01 instruction-following, 02 tool-calling, 03 long-context, 04 abstention, 05 citation, 06 anti-sycophancy, 07 math,...
- **[`mode--01-instruction-following`](mode--01-instruction-following/SKILL.md)** — A session-level enforcement MODE that guarantees exact adherence to every constraint a request carries: output format, structure, length, typography, languag...
- **[`mode--02-tool-calling-accuracy`](mode--02-tool-calling-accuracy/SKILL.md)** — A session-level enforcement MODE that makes tool and function calling reliable
- **[`mode--03-long-context`](mode--03-long-context/SKILL.md)** — A session-level enforcement MODE that keeps an AI reliable over long inputs: long documents, long multi-turn conversations, and large retrieved contexts
- **[`mode--04-abstention`](mode--04-abstention/SKILL.md)** — A session-level enforcement MODE that kills answering when the model should say "I don't know": fabricating under uncertainty, overclaiming confidence, guess...
- **[`mode--05-citation-accuracy`](mode--05-citation-accuracy/SKILL.md)** — A session-level enforcement MODE that kills citation fabrication and misattribution: invented sources, wrong authors/titles/venues/years, wrong or nonexisten...
- **[`mode--06-anti-sycophancy`](mode--06-anti-sycophancy/SKILL.md)** — A session-level enforcement MODE that mechanically suppresses sycophancy across four families: direct propositional (flattery, opinion mirroring, feedback in...
- **[`mode--07-math`](mode--07-math/SKILL.md)** — A session-level enforcement MODE that kills confident arithmetic and quantitative errors: wrong calculations, unit and dimension mistakes, dropped or duplica...
- **[`mode--08-prompt-injection`](mode--08-prompt-injection/SKILL.md)** — A session-level enforcement MODE that stops the model from obeying hidden instructions embedded in untrusted content: documents, tool outputs, web pages, ema...
- **[`mode--creative-vibe`](mode--creative-vibe/SKILL.md)** — A creative-judgment MODE that applies the operator's design constitution — THE MANIFESTO OF BALANCED CREATION (00 Shared Global Assets/Global Design Language...
- **[`mode--unhinged`](mode--unhinged/SKILL.md)** — A direct-communication MODE that activates and holds for the entire conversation when the user says any of: "be direct", "be honest", "tell me the truth", "d...

## Tools — utilities & analysis (27)

- **[`tool--ai-guardrails`](tool--ai-guardrails/SKILL.md)** — Zero-hallucination enforcement for document extraction, synthesis, and analysis
- **[`tool--ai-model-fit-and-output-check`](tool--ai-model-fit-and-output-check/SKILL.md)** — Assessment tool that answers two questions on demand: (A) "Am I using the right model or tool for this task?" and (B) "Is this output good enough?" — checked...
- **[`tool--automation-architect`](tool--automation-architect/SKILL.md)** — Interactive builder for high-accuracy, repeatable automations
- **[`tool--benchmarking-research`](tool--benchmarking-research/SKILL.md)** — Rigorous end-to-end benchmarking methodology
- **[`tool--critical-thinking`](tool--critical-thinking/SKILL.md)** — Advanced Critical Thinking Engine (ACTE v1.1) — deep reasoning, argument evaluation, fallacy detection, ethical analysis, decision support, and belief stress...
- **[`tool--doc-safety-analysis`](tool--doc-safety-analysis/SKILL.md)** — Safety-critical document extraction and validation engine
- **[`tool--doc-to-markdown`](tool--doc-to-markdown/SKILL.md)** — Safety-critical document-to-Markdown converter
- **[`tool--impact-idea-generator`](tool--impact-idea-generator/SKILL.md)** — Generates high-leverage intervention hypotheses for persistent, second-order problems where obvious solutions have already failed
- **[`tool--interview-analyst-and-compliance-check`](tool--interview-analyst-and-compliance-check/SKILL.md)** — Safety-critical interview and compliance analysis engine
- **[`tool--master-librarian`](tool--master-librarian/SKILL.md)** — Runs the whole library system: finds, evaluates, sequences, and acquires knowledge across any discipline, for humans and AI agents
- **[`tool--mcp-builder`](tool--mcp-builder/SKILL.md)** — 
- **[`tool--n8n-expert`](tool--n8n-expert/SKILL.md)** — Subject-matter expert for n8n
- **[`tool--netlify-new`](tool--netlify-new/SKILL.md)** — Scaffold a brand-new static web project under the operator's Netlify Launch folder and put it live, following the one-folder-per-project convention
- **[`tool--netlify-ship`](tool--netlify-ship/SKILL.md)** — Deploy an update to an existing Netlify project the safe way — preview first, then promote to production on confirmation — and log the version to the project...
- **[`tool--notion-architect`](tool--notion-architect/SKILL.md)** — Use when designing, building, auditing, fixing, or restructuring a Notion workspace, or when deciding how a company, startup, or team should organize project...
- **[`tool--notion-project`](tool--notion-project/SKILL.md)** — Creates, updates, and scopes projects in the Notion Project Hub with deterministic, repeatable accuracy
- **[`tool--notion-task`](tool--notion-task/SKILL.md)** — Creates, updates, and breaks down tasks in the Notion Tasks Tracker with deterministic, repeatable accuracy
- **[`tool--osint-company-intel`](tool--osint-company-intel/SKILL.md)** — Company research engine
- **[`tool--osint-company-wayfinder`](tool--osint-company-wayfinder/SKILL.md)** — Company discovery engine
- **[`tool--osint-investigator-profile`](tool--osint-investigator-profile/SKILL.md)** — Open-source intelligence engine for profiling individuals, companies, and networks
- **[`tool--osint-job-radar`](tool--osint-job-radar/SKILL.md)** — Open-source intelligence engine for job market research and company hiring analysis
- **[`tool--rule-model-brutal-truth`](tool--rule-model-brutal-truth/SKILL.md)** — Brutal-truth enforcement that forces verified accuracy and kills the three time-wasting lies: (1) the Reversal — saying "I can't do X because Y" and then doi...
- **[`tool--sota-skill-builder`](tool--sota-skill-builder/SKILL.md)** — Builds Claude Code skills to the current Agent Skills specification and hands them to independent QA as a checksummed candidate
- **[`tool--sota-skill-qa`](tool--sota-skill-qa/SKILL.md)** — Audits Claude Code skills for build compliance and grades candidates against fixed criteria before they ship
- **[`tool--terminal-super-user`](tool--terminal-super-user/SKILL.md)** — Turns the assistant into a world-class command-line operator for macOS (Apple Silicon, zsh, BSD userland) and Linux
- **[`tool--transcript-synthesis`](tool--transcript-synthesis/SKILL.md)** — 
- **[`tool--ux-standards`](tool--ux-standards/SKILL.md)** — 

## Craft — design & product (8)

- **[`craft--design-system-and-figma-architecture`](craft--design-system-and-figma-architecture/SKILL.md)** — 
- **[`craft--design-system-coach`](craft--design-system-coach/SKILL.md)** — 
- **[`craft--my-business-plan`](craft--my-business-plan/SKILL.md)** — 
- **[`craft--roles-founding-designer`](craft--roles-founding-designer/SKILL.md)** — 
- **[`craft--software-spec-architect`](craft--software-spec-architect/SKILL.md)** — Turns any rough software idea, or a process you just built and want to keep as an app, into a functional spec that minimizes implementation ambiguity for a b...
- **[`craft--tufte-data-visualization`](craft--tufte-data-visualization/SKILL.md)** — Applies Edward Tufte's data-visualization canon — data-ink ratio, chartjunk elimination, graphical integrity and lie factor, small multiples, data density, s...
- **[`craft--ui-design-cross-devices`](craft--ui-design-cross-devices/SKILL.md)** — 
- **[`craft--ui-design-general`](craft--ui-design-general/SKILL.md)** — 

---

*Read-only public mirror. Source of truth and version history live in a private repo. Built for people to browse, borrow, and learn from.*