# Agentic AI Certification Hub
### Phil Myint · ZenCloud Global Consultants · Brisbane

---

Agentic AI Certification Hub is a structured learning and reference repository for agentic AI concepts, Claude, GitHub, architecture foundations, practice questions, and cross-certification study.

## Purpose

This repo exists to separate durable agentic AI architecture concepts from vendor-specific exam vocabulary. The core material covers foundations such as orchestration, context, tools, memory, guardrails, and evaluation. The certification folders then apply those foundations to specific platforms and exam surfaces.

## Who It Is For

- Architecture practitioners preparing for agentic AI certifications.
- Enterprise and solution architects who need a practical grounding in agentic AI systems.
- Builders using Claude, GitHub Copilot, MCP, and AI-assisted delivery workflows.
- StudioSix and Velocity Architecture practitioners who need a learning layer for agentic architecture concepts.

## Ecosystem Fit

This repository supports the Velocity Architecture / StudioSix ecosystem by strengthening the AI-assisted delivery learning layer. It is not the core framework and it is not a product repo; it is a study and reference hub that helps practitioners understand the concepts behind agentic architecture work.

## Current Status

Usable study repository. Current content covers Claude Certified Architect foundations and GitHub agentic AI certification preparation, with shared cross-certification foundations in `core/`.

---

## Certifications tracked

| Certification | Issuer | Exam | Status | Domain focus |
|---|---|---|---|---|
| [Claude Certified Architect – Foundations](./ccaf/) | Anthropic | CCA-F | Live · March 2026 | Claude API, Claude Code, MCP |
| [GitHub Certified: Agentic AI Developer](./gh600/) | GitHub / Microsoft | GH-600 | Beta · May 2026 | GitHub Copilot, SDLC, Copilot agents |

---

## Repo structure

```
agentic-ai-certification/
│
├── README.md                                    ← This file
│
├── core/                                        ← Platform-agnostic foundations
│   ├── Agentic-Concepts.md                      ← Orchestration, context, tools, memory
│   ├── Anti-Patterns-Master.md                  ← All failure modes, vendor-neutral
│   └── Mental-Models.md                         ← Diagnostic reasoning framework
│
└── certifications/
    │
    ├── CCA-F/                                   ← Anthropic · Claude Certified Architect
    │   ├── README.md                            ← CCA-F overview and domain map
    │   ├── domain-breakdowns/                   ← D1–D5 deep dives
    │   ├── study-guides/                        ← Quick ref, anti-patterns, field guide
    │   ├── practice-questions/                  ← 57 scenario questions across all domains
    │   ├── prompt-engineering/                  ← Tutor prompts, system prompt templates
    │   └── resources/                           ← Links, schedule, fellowship research
    │
    └── GH-600/                                  ← GitHub · Agentic AI Developer
        ├── README.md                            ← GH-600 overview and domain map
        ├── domain-breakdowns/                   ← D1–D6 deep dives
        ├── study-guides/                        ← Quick ref, anti-patterns
        ├── practice-questions/                  ← Scenario questions per domain
        └── resources/                           ← Links, beta discount, study schedule
```

---

## Where to start

**New to agentic AI architecture:** Start in `core/`. Read Agentic-Concepts.md, then Mental-Models.md. These are the foundations every certification exam tests from different angles.

**Targeting CCA-F first:** Go directly to `certifications/CCA-F/README.md`. Everything is built and ready.

**Targeting GH-600 first:** Go to `certifications/GH-600/README.md`. Study core/ alongside it — GH-600 is GitHub-surface agentic architecture, and the core concepts are identical.

**Studying both simultaneously:** Read `core/` once. Then use each certification folder for platform-specific vocabulary, tooling, and exam scenarios. The overlapping domains reinforce each other.

---

## Cross-certification domain map

The same architectural concepts appear in both exams under different names and through different tooling.

| Concept | CCA-F Domain | GH-600 Domain |
|---------|-------------|---------------|
| Agent loop and stop reason handling | D1 · Agent Architecture (27%) | D1 · Agent Architecture & SDLC (15–20%) |
| Multi-agent orchestration | D1 · Agent Architecture (27%) | D5 · Multi-Agent Coordination (15–20%) |
| Tool design and routing | D4 · Tool Design & MCP (18%) | D2 · Tool Use & Environment (20–25%) |
| Context and memory management | D5 · Context & Reliability (15%) | D3 · Memory, State & Execution (10–15%) |
| Guardrails and enforcement | D1 · Hooks vs prompts | D6 · Guardrails & Accountability (10–15%) |
| Evaluation and error analysis | D3 · Prompt Engineering (20%) | D4 · Evaluation, Error & Tuning (15–20%) |
| CI/CD and SDLC integration | D2 · Claude Code (20%) | D1 · Agent Architecture & SDLC (15–20%) |

**Study pattern:** Learn the concept in whichever exam you're tackling first. When you move to the second exam, you're learning the platform surface — not the concept again.

---

## Beta opportunity — GH-600

The first 100 people who take Exam GH-600 (beta) on or before May 31, 2026 can get 80% off market price. Use discount code `GH600Flanders` at registration. Spots are limited and first-come, first-served. The beta exam is not available in Turkey, Pakistan, India, or China.

> Australia is eligible. Register via Pearson VUE through Microsoft Learn.

---

*Phil Myint · ZenCloud Global Consultants · May 2026*
