# Claude Practitioner Field Guide
### For architects who need to articulate the tool — not just use it

---

> An architect who cannot explain Claude's capabilities to practitioners is an incomplete architect.
> This guide maps every major Claude surface and workflow so you can speak to them with precision.

---

## How to use this guide

Four levels. Each builds on the previous. As an architect, you do not need to complete every level — you need to be able to *describe* what each level contains, *why* it matters, and *when* to direct practitioners to it.

Read each section. Note the architectural relevance. Use the "Architect's articulation" lines when briefing teams, clients, or stakeholders.

---

## Level 1 — The Basics (24 minutes)
*What every practitioner needs before anything else*

---

### Claude Setup
**Resource:** http://how-to-claude.ai  
**What it covers:** Account setup, interface orientation, model selection, basic settings.

**Practitioner value:** Eliminates setup friction. Gets someone productive in under 10 minutes.

**Architect's articulation:**
> "Before we talk about workflows or integrations, everyone on the team needs a consistent baseline setup. This covers that in under 10 minutes."

---

### Claude For Dummies
**Resource:** https://ruben.substack.com/p/claude-for-dummies  
**What it covers:** What Claude is, what it is not, how to write a basic prompt, what to expect.

**Practitioner value:** Resets wrong mental models — particularly the "search engine" and "autocomplete" misconceptions that produce bad prompts.

**Architect's articulation:**
> "Most prompt quality problems are mental model problems. This resets the model before bad habits form."

---

### Architectural note on Level 1

The gap between a Level 1 practitioner and a Level 2 practitioner is not knowledge — it is mental model. Architects who onboard teams without addressing mental models first will spend significant time debugging prompt quality issues downstream.

Mandate Level 1 completion before any team member is handed a workflow.

---

## Level 2 — Real Workflows (1 hour)
*Where individual productivity becomes team capability*

---

### Claude Cowork
**Resource:** http://claude-co.work  
**What it covers:** The Cowork interface — collaborative Claude usage, shared sessions, project context.

**Practitioner value:** Shifts Claude from a personal tool to a shared working environment.

**Architect's articulation:**
> "Cowork is where Claude stops being something individuals use and starts being something teams use together."

---

### Claude for Teams
**Resource:** http://how-claude.team  
**What it covers:** Team plans, shared Projects, admin controls, usage visibility.

**Practitioner value:** Governance and access control for enterprise Claude deployment.

**Architect's articulation:**
> "This is the enterprise surface. Usage policies, team Projects, admin visibility — the controls an organisation needs before Claude goes into production workflows."

---

### Cowork + Projects
**Resource:** https://ruben.substack.com/p/claude-cowork-project  
**What it covers:** How Cowork and Projects work together — persistent context, shared instructions, team memory.

**Practitioner value:** Projects give Claude persistent context across sessions. This is where the tool shifts from reactive to proactive.

**Architect's articulation:**
> "Projects are the closest thing Claude has to institutional memory within a team. Set up correctly, Claude starts every session knowing the team's context, conventions, and active work."

---

### Claude Design
**Resource:** http://claudedesign.free  
**What it covers:** Using Claude for visual and design workflows — briefs, critique, ideation.

**Practitioner value:** Extends Claude's relevance to non-technical practitioners.

**Architect's articulation:**
> "Not every Claude user is an engineer. Design workflows are a legitimate use case and a common adoption blocker when ignored."

---

### Claude for Slides
**Resource:** http://how-to-gamma.ai  
**What it covers:** Claude + Gamma integration for slide deck generation.

**Practitioner value:** Presentation production as a Claude output — not just text.

**Architect's articulation:**
> "Output format matters. If practitioners only know Claude produces text, they will underuse it. Slides are one of the highest-frequency deliverables in most organisations."

---

### Claude Skills
**Resource:** http://claude-skills.free  
**What it covers:** The Skills system — SKILL.md files, reusable instructions, practitioner-defined capability extensions.

**Practitioner value:** Moves practitioners from one-off prompts to repeatable, shareable capability packages.

**Architect's articulation:**
> "Skills are how individual prompt knowledge becomes team infrastructure. A well-designed skill encodes what would otherwise live only in one person's head."

---

### Architectural note on Level 2

Level 2 is where the architect's design decisions start to matter. Projects, Skills, and team configuration are not practitioner decisions — they are architecture decisions. An architect who delegates these to individual users will end up with inconsistent context, duplicated effort, and no governance visibility.

Own the Level 2 layer. Design it before deployment.

---

## Level 3 — The Pro Moves (3.5 hours)
*Where practitioners become high-leverage users*

---

### Avoid Sycophancy
**Resource:** https://ruben.substack.com/p/i-love-to-be-right  
**What it covers:** How Claude's training creates agreement bias, and how to prompt against it.

**Practitioner value:** Dramatically improves the quality of Claude's critical feedback, analysis, and devil's advocate responses.

**Architect's articulation:**
> "Claude is trained to be helpful, which can mean agreeable. Any practitioner using Claude for analysis, review, or decision support needs to know how to counteract this. Unchecked, it produces false validation."

---

### Stop Prompting
**Resource:** https://ruben.substack.com/p/stop-prompting-claude  
**What it covers:** Moving from ad hoc prompting to structured instructions — system prompts, persistent context, memory patterns.

**Practitioner value:** The shift from typing prompts to designing interactions. Higher leverage, lower repetition.

**Architect's articulation:**
> "Ad hoc prompting is the lowest-leverage way to use Claude. This covers how to move to structured, repeatable interactions — which is where real productivity lives."

---

### Stop Hitting Claude Limits
**Resource:** https://ruben.substack.com/p/how-to-stop-hitting-claude-usage  
**What it covers:** Context window management, conversation structure, usage limit patterns and workarounds.

**Practitioner value:** Eliminates the most common frustration point for intermediate Claude users.

**Architect's articulation:**
> "Usage limits and context overflow are the most common points where practitioners disengage. This prevents both."

---

### Claude 101 (Anthropic Academy)
**Resource:** https://anthropic.skilljar.com/claude-101  
**What it covers:** Anthropic's official foundational course — model behaviour, capabilities, limitations, responsible use.

**Practitioner value:** The authoritative source. Everything else is derivative.

**Architect's articulation:**
> "This is Anthropic's own curriculum. For any practitioner who needs a verifiable, vendor-backed foundation — this is it. Particularly relevant for regulated industries or client-facing contexts."

---

### Claude Code
**Resource:** http://claudecode.free  
**What it covers:** Claude Code — the agentic coding CLI. Installation, configuration, CLAUDE.md, basic workflows.

**Practitioner value:** Entry point for engineers moving from Claude.ai to the agentic coding surface.

**Architect's articulation:**
> "Claude Code is a fundamentally different surface to Claude.ai. It is an agentic system with its own configuration layer, file access, and execution capabilities. Practitioners who treat it as a chat interface will underuse it and misunderstand its risks."

---

### Architectural note on Level 3

Level 3 is where practitioners stop being consumers of Claude and start being designers of Claude interactions. Sycophancy awareness, structured prompting, and context management are not optional for anyone using Claude in a professional or client-facing context.

For teams deploying Claude in analysis, advisory, or decision-support roles — Level 3 completion should be a baseline requirement, not an optional extension.

---

## Level 4 — Expert Mode (8 hours)
*Where practitioners become builders*

---

### Claude Computer
**Resource:** https://ruben.substack.com/p/claude-computer  
**What it covers:** Computer use — Claude operating a desktop environment, browser, and applications autonomously.

**Practitioner value:** The most expansive Claude capability surface. Automation of tasks that require screen interaction.

**Architect's articulation:**
> "Computer use moves Claude from generating content to taking action. This requires a different risk framework — practitioners at this level need to understand what Claude can do autonomously and where human oversight is required."

---

### Build with the Claude API (Anthropic Academy)
**Resource:** https://anthropic.skilljar.com/claude-with-the-anthropic-api  
**What it covers:** The Anthropic API — messages, tool use, structured output, system prompts, streaming, model selection.

**Practitioner value:** The technical foundation for building Claude-powered applications.

**Architect's articulation:**
> "This is the boundary between using Claude and building with Claude. Anyone integrating Claude into a product, pipeline, or automated workflow needs this foundation. It is also the prerequisite knowledge for the CCA-F certification."

---

### Architectural note on Level 4

Level 4 practitioners are no longer users — they are builders. Their work creates the infrastructure that other practitioners run on. An architect's role at this level shifts from education to design review, integration oversight, and production readiness assessment.

Level 4 capability without architectural governance is where the most significant production risks emerge.

---

## Architect's Summary

| Level | Time | What it produces | Architect's role |
|-------|------|-----------------|-----------------|
| 1 · Basics | 24 min | Correct mental model, basic fluency | Mandate before anything else |
| 2 · Workflows | 1 hr | Team capability, shared context | Design the Projects and Skills layer |
| 3 · Pro moves | 3.5 hrs | High-leverage individual users | Set as baseline for professional use |
| 4 · Expert | 8 hrs | Builders and integrators | Govern the output, not the input |

---

## What an architect needs to be able to say

About the tool surface:
> "Claude operates across four surfaces: the web interface, mobile, Claude Code (agentic CLI), and the API. Each has different capability profiles, configuration options, and governance requirements."

About team deployment:
> "Projects give teams persistent context. Skills give teams reusable capability packages. Admin controls give architects governance visibility. These three layers need to be designed before individual practitioners are given access."

About risk:
> "The risk profile changes at each level. Chat is low-risk. Agentic coding requires configuration governance. Computer use requires human oversight protocols. API integration requires production architecture review."

About the CCA-F:
> "Ruben's curriculum covers usability fluency. The certification tests production architecture. They are complementary, not equivalent. Usability fluency is how you explain the tool. Architecture knowledge is how you build with it safely."

---

*Phil Myint · ZenCloud Global Consultants · May 2026*  
*Companion to: CCA-F Study Repository*
