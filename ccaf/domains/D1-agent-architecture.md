# Domain 1: Agent Architecture & Orchestration
### Weight: 27% · Highest priority domain

---

## What this domain tests

Whether you can design, debug, and reason about agentic systems under production conditions. Not theory. Not concepts. Broken systems — and the decisions that broke them.

---

## Core mechanics: the agentic loop

```
User message → Claude → stop_reason: tool_use → Execute tool → Return result → Claude → ...
                                                                              ↓
                                                               stop_reason: end_turn → Done
```

**`stop_reason: tool_use`** — Execute the tool. Pack the result back into the conversation. Loop.  
**`stop_reason: end_turn`** — The agent has finished. Stop.

### Anti-patterns the exam tests directly

| Anti-pattern | What breaks |
|---|---|
| Checking completion by parsing assistant text | Text parsing is fragile. Stop reasons are programmatic signals. |
| Stopping after an arbitrary iteration count | Cuts off legitimate multi-step work. |
| Ignoring `stop_reason` and polling for output | Introduces race conditions and missed terminal states. |

**Rule:** Never derive agent state from message content. Derive it from stop reasons.

---

## Multi-agent orchestration: hub-and-spoke

```
                    ┌─────────────┐
                    │ COORDINATOR │
                    │             │
                    │ Decomposes  │
                    │ Delegates   │
                    │ Aggregates  │
                    │ Error-handles│
                    └──────┬──────┘
                           │
          ┌────────────────┼────────────────┐
          ↓                ↓                ↓
    [Subagent A]     [Subagent B]     [Subagent C]
    Isolated         Isolated         Isolated
    context          context          context
```

### Critical rule: subagent context isolation

Subagents receive **nothing** from the coordinator's context automatically. Every piece of information a subagent needs must be **explicitly packed into the Task prompt**.

If the coordinator knows something and does not include it in the Task — the subagent does not know it.

### Exam scenario (memorise this)

> A coordinator decomposes "AI impact on creative industries" into three subtasks: AI in digital art, AI in graphic design, AI in photography. Subagents complete correctly. The final report covers only visual art. Where is the failure?

**Answer: The coordinator.** It scoped a broad topic to visual-only subtasks. The subagents executed correctly. The aggregation step had nothing broader to aggregate. Root cause is decomposition, not execution.

---

## Coordinator responsibilities

The coordinator owns:
- Task decomposition (scope definition is here — get this wrong and everything downstream is wrong)
- Delegation (packing full context into every Task prompt)
- Error handling (interpreting subagent failures, deciding retry vs escalate vs annotate)
- Result aggregation (assembling outputs into a coherent whole)

The coordinator does **not** execute. It decomposes, delegates, and synthesises.

---

## Hooks vs prompt instructions

| Method | Reliability | Use when |
|--------|-------------|----------|
| Hooks (e.g. `PreToolUse`) | **Programmatic — fires every time** | Consequence of failure is financial, legal, or safety-critical |
| System prompt instruction | Best-effort — works most of the time | Soft guardrails, style guidance, preference signals |

### Exam rule

> "Don't process refunds above £500" in a system prompt: works most of the time.  
> A `PreToolUse` hook blocking `process_refund` calls above £500: fires every time.

When the consequence of failure is material — use hooks. Prompts are not guarantees.

---

## Task decomposition quality

Poor decomposition is the silent killer of multi-agent systems. The exam tests:

- Scope creep (coordinator adds tasks beyond what was requested)
- Scope truncation (coordinator narrows a broad question to a subset — the visual art scenario)
- Context starvation (coordinator gives subagents insufficient information to complete tasks)
- Dependency ordering (Task B requires Task A's output — failure to sequence breaks synthesis)

---

## Session resumption

Long-running agents need checkpointing. If an agent crashes mid-run:
- Without checkpointing: restart from zero
- With checkpointing: resume from last confirmed state

The exam tests whether you know that session context is not automatically persisted across interruptions.

---

## Key distinctions for D1

| Concept | Correct understanding |
|---|---|
| Orchestrator vs subagent | Orchestrator decomposes and delegates. Subagent executes in isolated context. |
| Context isolation | Subagents get nothing from coordinator unless explicitly included in Task. |
| Stop reason handling | Programmatic signal. Never parse assistant text to determine agent state. |
| Hooks vs prompts | Hooks are guarantees. Prompts are instructions. |
| Decomposition failure | Scope errors in decomposition produce complete but wrong outputs. |

---

## Practice questions → [D1-practice.md](../practice-questions/D1-practice.md)

---

*Domain 1 of 5 · CCA-F Study Repository · ZenCloud Global Consultants · May 2026*
