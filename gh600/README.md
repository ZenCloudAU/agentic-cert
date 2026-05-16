# GitHub Certified: Agentic AI Developer (GH-600)
### Study Repository · Phil Myint · ZenCloud Global Consultants

---

> **Exam:** GitHub Certified: Agentic AI Developer (GH-600)
> **Issuer:** GitHub / Microsoft
> **Status:** Beta · Launched May 2026
> **Format:** 120 minutes · Proctored · Interactive components
> **Platform:** Pearson VUE via Microsoft Learn
> **Register:** https://learn.microsoft.com/en-us/credentials/certifications/agentic-ai-developer/
> **Beta discount:** Code `GH600Flanders` · 80% off · First 100 candidates · Deadline May 31, 2026

---

## What this exam tests

Expertise in operating, supervising, integrating, and governing AI agents inside production-grade SDLC workflows — using GitHub as the system of record and control plane.

The control plane distinction matters. This is not a generic agentic AI exam. It is specifically about how agents operate inside GitHub: Copilot, Actions, MCP servers, custom agents, and the SDLC workflow layer.

---

## Exam domain map

| # | Domain | Weight | Core focus |
|---|--------|--------|-----------|
| 1 | [Agent Architecture & SDLC Processes](./domain-breakdowns/D1-agent-sdlc.md) | 15–20% | Copilot agent setup, SDLC integration, architecture decisions |
| 2 | [Tool Use & Environment Interaction](./domain-breakdowns/D2-tool-environment.md) | 20–25% | MCP servers, tool configuration, permissions, environment scoping |
| 3 | [Memory, State & Execution](./domain-breakdowns/D3-memory-state.md) | 10–15% | State persistence, session management, execution environments |
| 4 | [Evaluation, Error Analysis & Tuning](./domain-breakdowns/D4-evaluation.md) | 15–20% | Agent output evaluation, error diagnosis, iterative improvement |
| 5 | [Multi-Agent Coordination](./domain-breakdowns/D5-multi-agent.md) | 15–20% | Orchestration, delegation, parallel execution, GitHub as coordinator |
| 6 | [Guardrails & Accountability](./domain-breakdowns/D6-guardrails.md) | 10–15% | Human-in-the-loop, audit trails, access controls, safety scans |

> **D2 + D1 + D4 + D5 = 65–85% of the exam.** D2 is the single heaviest domain.

---

## Repository structure

```
GH-600/
│
├── README.md                          ← This file
│
├── domain-breakdowns/
│   ├── D1-agent-sdlc.md               ← 15–20% · Copilot setup, SDLC architecture
│   ├── D2-tool-environment.md         ← 20–25% · MCP, tools, permissions, environments
│   ├── D3-memory-state.md             ← 10–15% · State, sessions, execution
│   ├── D4-evaluation.md               ← 15–20% · Evaluation, error analysis, tuning
│   ├── D5-multi-agent.md              ← 15–20% · Orchestration, delegation, GitHub
│   └── D6-guardrails.md               ← 10–15% · HITL, audit, access, safety
│
├── study-guides/
│   ├── Quick-Reference-Card.md        ← Exam-day one-pager
│   └── Anti-Patterns-GH600.md        ← GitHub-specific failure modes
│
├── practice-questions/
│   └── GH600-practice.md             ← Scenario questions across all 6 domains
│
└── resources/
    ├── Official-Links.md              ← Microsoft Learn, GitHub docs, exam guide
    ├── Study-Schedule.md              ← 3-week prep plan weighted by domain
    └── CCA-F-Crosswalk.md            ← Concept mapping between GH-600 and CCA-F
```

---

## Cross-reference: CCA-F concepts that directly apply

If you have studied or are studying CCA-F, the following concepts transfer directly. You are learning the GitHub surface, not the concept again.

| GH-600 domain | CCA-F equivalent | What transfers | What is new |
|---|---|---|---|
| D1 · Agent Architecture | CCA-F D1 | Agentic loop, decomposition, coordinator role | Copilot-specific setup, SDLC context |
| D2 · Tool Use | CCA-F D4 | Tool descriptions as routing, structured errors, MCP config | GitHub MCP specifics, Copilot tool scoping |
| D3 · Memory & State | CCA-F D5 | Context isolation, session persistence | GitHub-specific state patterns |
| D4 · Evaluation | CCA-F D3 | Error diagnosis, retry logic, feedback loops | GitHub scans, artifact evaluation |
| D5 · Multi-Agent | CCA-F D1 | Hub-and-spoke, context packing, parallel dispatch | GitHub as coordinator surface |
| D6 · Guardrails | CCA-F D1 (hooks) | Enforcement spectrum, HITL, escalation triggers | GitHub audit trails, branch protection |

**Study sequence recommendation:** Read `core/Agentic-Concepts.md` and `core/Mental-Models.md` first. They cover the shared foundations. Then work through each GH-600 domain breakdown for the GitHub-specific implementation layer.

---

## Beta exam note

The first 100 people who take Exam GH-600 (beta) on or before May 31, 2026, can get 80% off market price using code `GH600Flanders`. Scores will not be available immediately — results are released approximately eight weeks after the beta period concludes.

Beta exams are the best time to sit. Questions are being calibrated. The exam is harder to fail than a final release version, and you are contributing to the question pool that future candidates will use.

---

## Official preparation resources

| Resource | URL |
|----------|-----|
| Exam certification page | https://learn.microsoft.com/en-us/credentials/certifications/agentic-ai-developer/ |
| GH-600 study guide | https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/gh-600 |
| GitHub Learn certification page | https://learn.github.com/certification/AGENTIC |
| GitHub Copilot documentation | https://docs.github.com/en/copilot |
| GitHub MCP documentation | https://docs.github.com/en/copilot/using-github-copilot/using-extensions-to-integrate-external-tools |
| GitHub Actions documentation | https://docs.github.com/en/actions |
| Exam sandbox (look and feel) | https://ghcertdemo.starttest.com/ |
| Register via Pearson VUE | https://learn.microsoft.com/en-us/credentials/certifications/schedule-through-pearson-vue |

---

*Phil Myint · ZenCloud Global Consultants · May 2026*
