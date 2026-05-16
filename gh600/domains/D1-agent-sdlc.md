# GH-600 Domain 1: Agent Architecture & SDLC Processes
### Weight: 15–20%

---

## What this domain tests

Whether you can design and configure agent workflows that operate correctly inside GitHub's SDLC — not just standalone agents, but agents embedded in the software development lifecycle with appropriate controls, access, and SDLC awareness.

---

## GitHub Copilot as an agentic system

Copilot in agent mode is not autocomplete. It is an agentic loop:

1. Receives a task (issue, PR comment, workflow trigger)
2. Plans steps
3. Uses tools (file read/write, search, terminal, MCP servers)
4. Executes and iterates
5. Produces output (code changes, PR comments, summaries)

**Architecture decision at this layer:** What does the agent have access to? What triggers it? What does it produce? These are SDLC design decisions, not configuration details.

---

## Copilot setup steps

Copilot setup steps run before the agent begins its task. They configure the environment the agent will operate in.

**Use cases:**
- Install dependencies the agent will need
- Set environment variables
- Configure access tokens for tools
- Verify pre-conditions before agent execution begins

**Exam principle:** Setup steps are where environment preparation belongs. Not in the agent's system prompt. Not inline in the workflow.

---

## Custom instructions

Custom instructions define how Copilot behaves — its conventions, constraints, and context — across sessions and users.

**File location:** `.github/copilot-instructions.md`  
**Scope:** Repository-wide  
**Version-controlled:** Yes

**Exam scenario:** A new developer joins the team and Copilot does not follow the team's naming conventions. The conventions are documented. Most likely cause: conventions were added to a personal Copilot settings file, not to `.github/copilot-instructions.md`.

---

## SDLC integration points

Agents in GitHub operate at specific SDLC integration points:

| Integration point | What the agent does | Trigger |
|---|---|---|
| Issue triage | Categorise, label, assign | Issue created/updated |
| PR review | Review code, flag issues, suggest fixes | PR opened/updated |
| CI/CD pipeline | Generate tests, fix failures, update docs | Workflow run |
| Documentation | Update docs from code changes | Merge to main |
| Security scanning | Identify vulnerabilities, suggest patches | Push / schedule |

**Architecture decision:** Which integration points are appropriate for autonomous action vs human-in-the-loop review?

---

## Key distinctions for D1

| Concept | Correct understanding |
|---|---|
| Setup steps | Pre-execution environment configuration. Not in the prompt. |
| Custom instructions | `.github/copilot-instructions.md`. Repository-wide. Version-controlled. |
| Agent mode vs autocomplete | Agent mode is a full agentic loop. Fundamentally different surface. |
| SDLC integration points | Each has different risk profile and appropriate oversight level. |

---

*Domain 1 of 6 · GH-600 Study Repository · ZenCloud Global Consultants · May 2026*
