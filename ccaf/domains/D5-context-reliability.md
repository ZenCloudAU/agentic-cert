# Domain 5: Context Management & Reliability
### Weight: 15% · Smallest domain — but failures here cascade everywhere

---

## What this domain tests

How context degrades under compression, why certain context positions matter, and which escalation triggers are reliable vs which the exam uses as distractors.

---

## Progressive summarisation trap

Summarisation compresses context. Compression loses precision.

| Original value | After summarisation |
|---|---|
| £4,832.17 | "approximately £5,000" |
| 14 March 2026 | "recently" |
| Order ID ORD-001234 | Dropped entirely |

For conversational context — acceptable. For transactional systems — fatal.

### The fix

Pull transactional facts into a **structured persistent block** that lives outside the summarised history:

```
CASE FACTS (do not summarise):
- Customer: CUST-001234
- Order: ORD-456789
- Amount in dispute: £4,832.17
- Date of purchase: 14 March 2026
- Escalation threshold: £2,000
```

This block gets included in every prompt regardless of what gets compressed in the conversational history. Values stay precise.

---

## Lost-in-the-middle effect

Models reliably process content at the **start** and **end** of long inputs. Content buried in the middle of a large context is more likely to be missed.

### Production implication

A 75K-token aggregated context with key findings buried at position 40K will miss them.

### The fix

- Put findings summary at the **top** of the prompt
- Use **explicit section headings** as navigation anchors through detail
- Never bury critical information in the middle of a long context

### Exam rule

If the question asks why a model is missing information that is present in the context, and the context is long — the answer is lost-in-the-middle.

---

## Tool output accumulation

Tool results accumulate in context fast.

`lookup_order` returns 40 fields. The coordinator needs 5.  
After 10 tool calls: 400 fields of tool output in context, 50 used.

### The fix: `PostToolUse` hook trimming

```python
# PostToolUse hook
def trim_order_result(result):
    return {
        "order_id": result["order_id"],
        "status": result["status"],
        "total": result["total"],
        "customer_id": result["customer_id"],
        "created_at": result["created_at"]
    }
```

Trim before the model sees it. Less noise, smaller footprint, same signal.

---

## Escalation triggers

### Reliable triggers (use these)

| Trigger | Why reliable |
|---------|-------------|
| Explicit customer request for a human | Stated preference — unambiguous |
| Policy gap the agent cannot interpret | Structural limitation — cannot be resolved by the model |
| Inability to make progress after N attempts | Observable, programmatic, not model-dependent |

### Unreliable triggers (exam uses these as wrong answers)

| Trigger | Why unreliable |
|---------|---------------|
| Sentiment analysis | Model-dependent, inconsistent across similar language |
| Model self-rated confidence scores | Model cannot reliably assess its own uncertainty |
| Automatic classifiers | Adds a layer over a symptom, not the cause |

### Exam scenario

> Which escalation triggers are reliable?
> A. Sentiment score below 0.3  
> B. Model confidence below 70%  
> C. Customer explicitly requests a human  
> D. Automatic frustration classifier fires

**Answer: C only.** A, B, and D are all model-dependent or classifier-dependent. C is a direct, programmatic signal.

---

## Context window management: key patterns

| Pattern | Use |
|---------|-----|
| Structured persistent blocks | Transactional facts that must survive compression |
| Tool output trimming via hooks | Reduce accumulation from verbose tool responses |
| Summary + detail architecture | Lead with summary, follow with detail — respects the middle |
| Domain-scoped context | Subagents receive only the context relevant to their task |

---

## Why this domain cascades

Context failures in D5 produce:
- **D1 failures:** Subagents with missing context produce wrong outputs
- **D2 failures:** Large skill outputs without `context: fork` eat session context
- **D3 failures:** Retry loops run against missing data (data was in context but got compressed out)
- **D4 failures:** Tool results accumulate and crowd out actionable information

Smallest weighting. Highest cascade potential.

---

## Key distinctions for D5

| Concept | Correct understanding |
|---|---|
| Progressive summarisation | Safe for conversation, dangerous for transactional values |
| Persistent blocks | Transactional facts outside summarisation scope |
| Lost-in-the-middle | Long contexts lose middle content — put critical info at the top |
| Tool output trimming | `PostToolUse` hooks. Trim before model sees it. |
| Reliable escalation | Explicit request, policy gap, no progress. Not sentiment or classifier. |

---

## Practice questions → [D5-practice.md](../practice-questions/D5-practice.md)

---

*Domain 5 of 5 · CCA-F Study Repository · ZenCloud Global Consultants · May 2026*
