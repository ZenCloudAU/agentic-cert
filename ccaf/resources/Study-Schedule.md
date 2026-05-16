# Study Schedule — 4-Week Prep Plan
### Weighted by exam domain percentage

---

## Principles

- Time allocation mirrors domain weight. 27% of questions = 27% of study time.
- Each week closes with practice questions before moving forward.
- Week 4 is revision only. No new material.

---

## Week 1 — Domain 1: Agent Architecture (27%)

**Daily commitment:** 60–90 minutes

| Day | Task |
|-----|------|
| Mon | Read [D1-agent-architecture.md](../domain-breakdowns/D1-agent-architecture.md) in full |
| Tue | Build the agentic loop: write code that correctly handles `stop_reason: tool_use` and `stop_reason: end_turn` |
| Wed | Build a minimal hub-and-spoke coordinator: one coordinator, two subagents, explicit context packing |
| Thu | Implement a `PreToolUse` hook that blocks calls above a threshold. Compare against a prompt instruction doing the same. |
| Fri | Run [D1 practice questions](../practice-questions/D1-practice.md). Review every wrong answer. |
| Sat | Use the [D1 tutor prompt](../prompt-engineering/Tutor-Prompts-All-Domains.md) — work through all task statements |
| Sun | Rest or light review |

**Week 1 outcome:** You can explain coordinator decomposition failure, subagent context isolation, and hook-vs-prompt trade-offs without referring to notes.

---

## Week 2 — Domains 2 & 3: Claude Code + Prompt Engineering (40%)

**Daily commitment:** 60–90 minutes

| Day | Task |
|-----|------|
| Mon | Read [D2-claude-code.md](../domain-breakdowns/D2-claude-code.md) · Set up real CLAUDE.md hierarchy in a test project |
| Tue | Run Claude Code with `-p` flag in a script. Confirm non-interactive behaviour. |
| Wed | Read [D3-prompt-engineering.md](../domain-breakdowns/D3-prompt-engineering.md) |
| Thu | Design a JSON schema with correct required/optional/nullable handling. Test with a prompt that has missing source data. |
| Fri | D2 practice questions → review. D3 practice questions → review. |
| Sat | Build a few-shot prompt targeting an ambiguous classification case. Verify consistency across 10 inputs. |
| Sun | Rest or light review |

**Week 2 outcome:** You can correctly identify CLAUDE.md scope errors, run Claude Code non-interactively, and explain what schemas do and do not guarantee.

---

## Week 3 — Domains 4 & 5: Tool Design + Context (33%)

**Daily commitment:** 60–90 minutes

| Day | Task |
|-----|------|
| Mon | Read [D4-tool-mcp.md](../domain-breakdowns/D4-tool-mcp.md) · Write two tool descriptions for similar-function tools with full differentiation |
| Tue | Build a structured error response object covering all four error categories (rate limit, not found, validation, outage) |
| Wed | Configure a project-level MCP server in `.mcp.json` and a personal one in `~/.claude.json` |
| Thu | Read [D5-context-reliability.md](../domain-breakdowns/D5-context-reliability.md) · Build a persistent structured facts block for a simulated transaction conversation |
| Fri | D4 + D5 practice questions → review all wrong answers |
| Sat | Implement a `PostToolUse` hook that trims a verbose tool response to only needed fields |
| Sun | Rest |

**Week 3 outcome:** You can write tool descriptions that eliminate routing ambiguity, structure error responses that enable coordinator recovery, and protect transactional values from summarisation.

---

## Week 4 — Revision and Exam Readiness

**Daily commitment:** 45–60 minutes

| Day | Task |
|-----|------|
| Mon | Full read of [Quick Reference Card](Quick-Reference-Card.md) · Flag anything unfamiliar |
| Tue | Full read of [Anti-Patterns Index](Anti-Patterns-Index.md) · No new patterns should surprise you |
| Wed | Re-run practice questions from your weakest domain (lowest score in Weeks 1–3) |
| Thu | Run all practice questions across all domains in one session. Timed: 2 minutes per question. |
| Fri | Review any questions missed. Read the relevant domain breakdown section. |
| Sat | [Exam Day Checklist](Exam-Day-Checklist.md) · Light review only · No cramming |
| Sun | **Exam day** |

---

## Accelerated tracks

**2-week track:** Weeks 1 and 4 only. Read all domain breakdowns in Week 1. Revision and practice in Week 2. Requires 2 hours daily.

**1-week track:** Quick Reference Card + Anti-Patterns Index + all practice questions. Assumes existing hands-on Claude API experience.

**1-day track:** Quick Reference Card → Anti-Patterns Index → done. Know the patterns. Know the traps.

---

*Phil Myint · ZenCloud Global Consultants · May 2026*
