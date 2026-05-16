# GH-600 Quick Reference Card
### GitHub Certified: Agentic AI Developer · Exam day

---

## Domain weights

| Domain | % | Priority |
|--------|---|----------|
| D2 · Tool Use & Environment Interaction | 20–25% | ★★★★★ |
| D1 · Agent Architecture & SDLC | 15–20% | ★★★★ |
| D4 · Evaluation, Error Analysis & Tuning | 15–20% | ★★★★ |
| D5 · Multi-Agent Coordination | 15–20% | ★★★★ |
| D3 · Memory, State & Execution | 10–15% | ★★★ |
| D6 · Guardrails & Accountability | 10–15% | ★★★ |

**D2 + D1 + D4 + D5 = 65–85% of the exam.**

---

## The master principle

> Fix the root cause. Do not add a layer on top of a broken design.

- Wrong tool called → improve descriptions first, not routing classifier
- Agent ignores conventions → add to `.github/copilot-instructions.md`, not verbal instruction
- High-consequence action → branch protection, not Copilot instruction
- State lost across runs → write to artifact/external storage, not in-session memory

---

## D1 — Agent Architecture & SDLC

- Copilot in agent mode = full agentic loop. Not autocomplete.
- Setup steps = pre-execution environment configuration (not in the prompt)
- Custom instructions → `.github/copilot-instructions.md` · repository-wide · version-controlled
- New dev ignores conventions → conventions in personal settings, not `.github/copilot-instructions.md`
- Match agent autonomy to SDLC risk: read-only tasks → more autonomous; write to production → HITL required

---

## D2 — Tool Use & Environment

- `.github/mcp.json` → repository-wide MCP · shared · version-controlled
- Personal MCP settings → personal only · not shared
- Secrets → `${{ secrets.NAME }}` · never committed values
- Least privilege: minimum permissions for the task
- Tool called incorrectly → fix description first · not routing layer
- Generic MCP error → coordinator cannot recover · structured error required
- Permission denied error → `retryable: false` + `requiredPermission` field

---

## D3 — Memory, State & Execution

- Actions runner = ephemeral · does not persist across runs
- Workflow artifacts = durable · use for checkpointing
- PR/issue comments = durable structured context · use for task state
- Session context = lost on session end · not a persistence mechanism
- Multi-run task stores state in runner env var → root cause is runner ephemerality · fix is artifact

---

## D4 — Evaluation, Error Analysis & Tuning

- Scan output = evaluation artifact · high-severity findings need human review
- Retry when data exists and was missed · not when data is absent
- Absent data + retry = fabrication
- Minimum fix principle: fix root cause, verify, monitor
- Ongoing monitoring: tool usage + output quality + error patterns — not one-time benchmark

---

## D5 — Multi-Agent Coordination

- GitHub = coordinator · agents have isolated context per trigger
- Each agent receives only its trigger context · nothing flows between agents automatically
- Parallel dispatch risk: output conflicts when agents write to same target
- Fix: branch isolation, sequential dispatch, or locking
- Agent B needs Agent A's output → write to artifact/PR/issue · not in-session sharing

---

## D6 — Guardrails & Accountability

**Enforcement strength (hardest first):**
1. Branch protection rules (cannot be bypassed)
2. Required reviewers / CODEOWNERS (programmatic HITL)
3. Environment protection rules (deployment gates)
4. Safety scan requirements (code/secret/dependency)
5. Copilot custom instructions (best-effort)

**Reliable escalation:** Branch protection, CODEOWNERS, security scan above threshold, agent explicit flag  
**Unreliable:** Agent confidence score, sentiment of comments

- Audit trail: GitHub native logs cover all agent actions inside the platform
- Least privilege: per-task minimum, not org-wide defaults

---

## Scenario answer patterns

| Scenario signal | Answer direction |
|---|---|
| Agent ignores team conventions | Check `.github/copilot-instructions.md` vs personal settings |
| MCP tool called for wrong tasks | Improve tool description first |
| State lost between workflow runs | Runner is ephemeral · use artifacts |
| Agent merges without human review | Branch protection required · not an instruction |
| High-severity scan finding, agent auto-merges | HITL required at scan gate |
| Parallel agents overwrite each other | Branch isolation or sequential dispatch |
| Agent B misses context from Agent A | Context not passed explicitly · write to durable storage |
| Generic MCP error, agent stuck | Structured error response with retryability |
| New capability needs production access | Least privilege first · scope to task only |

---

## Beta exam notes

- Results not immediate — ~8 weeks after beta period closes
- Interactive components in the exam — not just multiple choice
- Code `GH600Flanders` = 80% off · first 100 candidates · deadline May 31, 2026

---

*Phil Myint · ZenCloud Global Consultants · May 2026*
