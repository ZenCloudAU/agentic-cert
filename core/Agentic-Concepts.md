# Agentic Concepts
### Platform-agnostic foundations · Applies to all certifications

---

> These concepts appear in every agentic AI certification exam regardless of platform.
> Learn them once here. Apply the platform-specific vocabulary in each certification folder.

---

## 1. The Agentic Loop

The fundamental mechanic of every agent system. An agent is not a single call — it is a loop.

```
Input → Model → Decision
                    │
            ┌───────┴────────┐
            │                │
        Tool call        Final answer
            │                │
        Execute          Return to user
            │
        Result → Model → Decision (loop continues)
```

### Stop conditions

Every well-designed agent loop has explicit stop conditions:

| Condition | Signal type | Reliability |
|-----------|------------|-------------|
| Task complete (`end_turn` / equivalent) | Programmatic | High |
| Max iterations reached | Programmatic | Medium (may cut off legitimate work) |
| Error threshold exceeded | Programmatic | High |
| Human escalation triggered | Programmatic or explicit | High |
| Model self-reports completion | Model-generated text | Low — never use alone |

**Rule across all platforms:** Never derive loop termination from model-generated text. Use programmatic signals.

---

## 2. Orchestration Patterns

### Hub-and-spoke (most common)

```
        COORDINATOR
       /     |     \
   Agent A  Agent B  Agent C
   (isolated context)
```

- Coordinator owns: decomposition, delegation, error handling, aggregation
- Subagents own: execution within their assigned scope
- Subagents share nothing — each receives only what the coordinator explicitly provides

### Pipeline (sequential)

```
Agent A → output → Agent B → output → Agent C → final output
```

- Each agent's output is the next agent's input
- Dependency ordering is the coordinator's responsibility
- A failure in Agent B terminates the pipeline unless error handling is explicit

### Parallel dispatch

```
Coordinator dispatches A, B, C simultaneously
Waits for all completions
Aggregates results
```

- Only valid when agents have no dependencies on each other
- Requires explicit handling of partial failures (some complete, some don't)

---

## 3. Context Isolation

**The most misunderstood property of multi-agent systems.**

In a multi-agent system, each agent runs in its own context. There is no shared memory between agents unless explicitly architected.

| What agents share | What agents do NOT share |
|---|---|
| Only what the coordinator explicitly provides | The coordinator's full context |
| Their assigned task | Other agents' outputs |
| Explicitly passed tool results | Session history from other agents |

**Consequence:** If the coordinator knows something and does not pass it to a subagent — the subagent does not know it. No exceptions.

**Production failure pattern:** Coordinator decomposes correctly, subagents execute correctly, final output is wrong in scope — because the coordinator's decomposition defined the wrong scope, not because subagents failed.

---

## 4. Tool Design Principles

Tools are the agent's interface to the world. Tool quality determines agent reliability.

### Descriptions are routing logic

The model uses tool descriptions to decide which tool to call. A poor description produces routing errors before any tool is executed.

**Minimum viable tool description:**
- What the tool does
- What input format it accepts (with examples)
- When to use it vs neighbouring tools
- What it explicitly does NOT do

### Structured errors enable recovery

| Error type | Generic response | Structured response |
|---|---|---|
| Rate limit | `"Request failed"` | `{category: "rate_limit", retryable: true, retryAfter: 30}` |
| Not found | `"Error"` | `{category: "not_found", retryable: false, query: "..."}` |
| Validation | `"Bad request"` | `{category: "validation", retryable: true, issue: "..."}` |

A coordinator receiving a generic error cannot recover. A coordinator receiving a structured error can decide — retry, annotate and continue, or escalate.

### Idempotency

Tools that take consequential actions must be idempotent where possible. A retried tool call must not create duplicate side effects.

---

## 5. Memory and Context Management

### Four types of agent memory

| Type | What it is | Risk |
|------|-----------|------|
| In-context | Current conversation/session | Lost on session end, degrades with length |
| External (database) | Persisted outside the model | Requires retrieval architecture |
| Cached | Pre-computed embeddings or summaries | Stale data risk |
| Structured state | Explicit fields passed between steps | Most reliable for transactional data |

### Progressive summarisation trap

Long conversations get compressed. Compression loses precision.

- Numbers → approximations
- Dates → relative references
- IDs → dropped entirely

**Fix:** Structured persistent blocks for any data that must survive compression. Keep these outside the summarisable conversation history.

### Lost-in-the-middle

Models process the start and end of long contexts more reliably than the middle.

**Fix:** Critical information at the top. Section headings as navigation anchors. Never bury key findings in the middle of a large context.

---

## 6. Guardrails and Enforcement

### The enforcement spectrum

```
Weakest ←————————————————————————→ Strongest

Prompt instruction → Output validation → Pre-execution hook → Hard block
```

| Method | Fires | Use for |
|--------|-------|---------|
| Prompt instruction | Most of the time | Soft preferences, style, non-critical behaviour |
| Output validation | After execution | Catching errors before returning to user |
| Pre-execution hook | Every time | Financial, legal, safety-critical constraints |
| Hard block | Every time | Absolute limits that cannot be overridden |

**Rule:** Match enforcement strength to consequence severity. Financial and safety constraints require pre-execution hooks, not prompt instructions.

### Human-in-the-loop (HITL)

Every production agent system needs defined escalation points where a human is required.

**Reliable escalation triggers:**
- User explicitly requests human intervention
- Agent cannot make progress after N attempts
- Policy gap the agent cannot interpret
- Action exceeds a defined consequence threshold

**Unreliable triggers (never use alone):**
- Sentiment analysis scores
- Model self-rated confidence
- Automatic classifiers without fallback

---

## 7. Evaluation and Reliability

### What evaluation means for agents

Agent evaluation is not a one-time benchmark. It is continuous monitoring of:
- Task completion rate
- Tool routing accuracy
- Error recovery rate
- Escalation trigger precision (not too sensitive, not too permissive)
- Context degradation over long sessions

### Feedback loops

A retry loop is only useful when the data exists and the agent failed to use it correctly. Retrying against missing data produces fabrication, not correction.

**Rule:** Diagnose the failure type before building a retry loop. Is the data there? Did the agent miss it? Or is the data absent?

---

## 8. The Architect's Diagnostic Framework

When an agentic system fails, diagnose in this order:

```
1. Where in the loop did it fail?
   → Decomposition / Delegation / Execution / Aggregation / Output

2. What was the root cause?
   → Missing context / Wrong tool / Bad description / Enforced incorrectly / Context degraded

3. What is the minimum fix?
   → Fix the root cause, not the symptom
   → Do not add infrastructure over a broken foundation
```

**The master principle — applies to every platform, every exam:**

> Fix the root cause. Do not add a layer on top of a broken design.
>
> Better descriptions before a routing classifier.
> Programmatic hooks before a stronger prompt.
> Explicit context passing before a shared memory layer.

---

*Phil Myint · ZenCloud Global Consultants · May 2026*
*Part of: Agentic AI Certification Hub*
