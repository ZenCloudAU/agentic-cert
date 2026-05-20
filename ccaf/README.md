# Claude Certified Architect — Foundations (CCA-F)
### Study Repository · Phil Myint · ZenCloud Global Consultants

---

> **Part of:** [Agentic AI Certification Hub](../../README.md)
> **Shared foundations:** See [`core/`](../../core/) for platform-agnostic concepts that underpin this exam.

---

> **Exam:** Claude Certified Architect – Foundations (CCA-F)
> **Launched:** March 12, 2026 · Anthropic
> **Format:** 60 questions · 120 minutes · Scenario-based · Proctored
> **Pass mark:** 720 / 1000
> **Level:** ~300 (production systems, not AI literacy)
> **Access:** Claude Partner Network members
> **Register:** https://anthropic.skilljar.com/claude-certified-architect-foundations-access-request

---

## Exam domain map

| # | Domain | Weight | Where people lose marks |
|---|--------|--------|------------------------|
| 1 | [Agent Architecture & Orchestration](./domains/D1-agent-architecture.md) | **27%** | Anti-patterns, coordinator logic, subagent context isolation |
| 2 | [Claude Code Configuration & Workflows](./domains/D2-claude-code.md) | **20%** | CLAUDE.md hierarchy, `-p` flag, skill isolation |
| 3 | [Prompt Engineering & Structured Output](./domains/D3-prompt-engineering.md) | **20%** | Schema limits, few-shot construction, retry logic |
| 4 | [Tool Design & MCP Integration](./domains/D4-tool-mcp.md) | **18%** | Tool descriptions, structured errors, config scoping |
| 5 | [Context Management & Reliability](./domains/D5-context-reliability.md) | **15%** | Summarisation traps, lost-in-middle, escalation triggers |

> **D1 + D2 + D3 = 67% of the exam.**

---

## Repository structure

```
CCA-F/
│
├── README.md                          ← This file
│
├── domain-breakdowns/
│   ├── D1-agent-architecture.md       ← 27% · Agentic loop, orchestration, hooks
│   ├── D2-claude-code.md              ← 20% · CLAUDE.md, CI/CD, session isolation
│   ├── D3-prompt-engineering.md       ← 20% · Schemas, few-shot, batch API
│   ├── D4-tool-mcp.md                 ← 18% · Tool descriptions, MCP config, errors
│   └── D5-context-reliability.md      ← 15% · Context compression, escalation
│
├── study-guides/
│   ├── Quick-Reference-Card.md        ← Exam-day one-pager
│   ├── Anti-Patterns-Index.md         ← Every CCA-F trap catalogued
│   └── Claude-Practitioner-Field-Guide.md  ← Ruben Hassid's 4-level usability curriculum
│
├── practice-questions/
│   ├── D1-practice.md                 ← 15 scenarios · Agent architecture
│   └── D2-D5-practice.md              ← 42 scenarios · Remaining four domains
│
├── prompt-engineering/
│   └── Tutor-Prompts-All-Domains.md   ← Claude-as-tutor prompts for each domain
│
└── resources/
    ├── Official-Links.md              ← Anthropic Academy, exam guide, docs
    ├── Study-Schedule.md              ← 4-week prep plan by domain weight
    ├── Exam-Day-Checklist.md          ← Pre-exam logistics and mental model reset
    └── Fellowship-Research-Links.md   ← Anthropic fellows research mapped to domains
```

---

## Where to start

**First time here:** Read `core/Agentic-Concepts.md` and `core/Mental-Models.md` first. They cover the foundations this exam tests.

**Have 4 weeks:** Follow the [4-week study schedule](./resources/Study-Schedule.md).

**Have 1 week:** [Quick Reference Card](./guides/Quick-Reference-Card.md) + [Anti-Patterns Index](./guides/Anti-Patterns-Index.md) + all practice questions.

**Have 1 day:** Quick Reference Card + Anti-Patterns Index. Know the traps.

---

## The exam's logic

Most certifications test recall. CCA-F tests diagnostic reasoning.

Every question puts you inside a production system that has already broken. Identify the root cause. Pick the fix that addresses it directly — not the one that adds a layer on top of a flawed design.

**The master principle:**
> Better tool descriptions before a routing classifier.
> Programmatic enforcement before a stronger prompt.
> Explicit context passing before a shared memory layer.

---

## Cross-certification reference

Studying GH-600 alongside CCA-F? See [`gh600/resources/CCA-F-Crosswalk.md`](../gh600/resources/CCA-F-Crosswalk.md) for the full concept mapping between both exams.

---

*Phil Myint · ZenCloud Global Consultants · May 2026*
