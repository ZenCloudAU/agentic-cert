# CCA-F Quick Reference Card
### Exam day · Print or bookmark

---

## Domain weights

| Domain | % | Priority |
|--------|---|----------|
| D1 · Agent Architecture & Orchestration | 27% | ★★★★★ |
| D2 · Claude Code Configuration | 20% | ★★★★ |
| D3 · Prompt Engineering & Structured Output | 20% | ★★★★ |
| D4 · Tool Design & MCP Integration | 18% | ★★★ |
| D5 · Context Management & Reliability | 15% | ★★★ |

**D1 + D2 + D3 = 67% of the exam.**

---

## The exam's master principle

> Fix the root cause. Do not add a layer on top of a broken design.

Better tool descriptions before a routing classifier.  
Programmatic enforcement (hooks) before a stronger prompt.  
Explicit context passing before a shared memory layer.

---

## D1 — Agent Architecture

- `stop_reason: tool_use` → execute and loop. `stop_reason: end_turn` → done.
- Never derive agent state from message text. Use stop reasons.
- Coordinator owns: decomposition, delegation, error handling, aggregation.
- Subagents get **nothing** from coordinator unless explicitly packed in Task.
- Hooks fire **every time**. Prompt instructions work **most of the time**.
- Wrong output on correct subtasks → root cause is coordinator decomposition scope.

---

## D2 — Claude Code

- `~/.claude/CLAUDE.md` → personal, not shared, not version-controlled
- `.claude/CLAUDE.md` → project, shared, version-controlled
- `-p` flag → non-interactive. Required for CI/CD pipelines.
- `context: fork` → skill runs in isolated subagent. Verbose output stays contained.
- Code review → fresh session. Not the generation session.
- `.mcp.json` → project-level MCP. `~/.claude.json` → personal MCP.

---

## D3 — Prompt Engineering

- JSON schema → syntax guarantee only. Not semantic. Not accuracy.
- Required field + no source data → model invents a value.
- Optional fields: `"type": ["string", "null"]`
- Enums: always include `"unclear"` and `"other"`
- Few-shot beats descriptive instructions for ambiguous/overlapping categories.
- Retry loops → only when data exists. Not for missing information.
- Batch API → 50% cost, 24h window. Never for blocking tasks.

---

## D4 — Tool Design & MCP

- Minimal descriptions → routing errors. Ambiguous tools → wrong calls.
- `get_customer` called for orders 45% of time → expand descriptions first.
- Generic error (`"Operation failed"`) → coordinator cannot recover.
- Structured error (category + isRetryable + partialResults) → coordinator can decide.
- `.mcp.json` → team-shared. `~/.claude.json` → personal only.
- Secrets → `${ENV_VAR}` substitution. Never committed.

---

## D5 — Context & Reliability

- Progressive summarisation: numbers → "approximately". Dates → "recently". Dangerous for transactional facts.
- Fix: structured persistent block outside summarisation scope.
- Long context → lost-in-the-middle. Put critical info at the **top**.
- `PostToolUse` hook → trim verbose tool output before model sees it.

**Reliable escalation triggers:**
- Explicit customer request for human
- Policy gap agent cannot interpret
- No progress after N attempts

**Unreliable (exam distractors):**
- Sentiment score
- Model confidence score
- Automatic classifiers

---

## Scenario answer patterns

| Scenario signal | Answer direction |
|---|---|
| Output correct but wrong scope | Coordinator decomposition failure |
| Pipeline job hangs | Missing `-p` flag |
| New dev not following conventions | User-level CLAUDE.md instead of project-level |
| Tool called for wrong queries | Expand descriptions first |
| Generic error, coordinator stuck | Structured error response |
| Values going vague over conversation | Progressive summarisation trap — use persistent block |
| Model missing info in long context | Lost-in-the-middle — move to top |
| Retry loop not resolving | Information not in source — retrying invents data |

---

*Phil Myint · ZenCloud Global Consultants · May 2026*
