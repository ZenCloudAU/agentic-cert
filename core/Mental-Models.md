# Mental Models
### Diagnostic reasoning framework · Platform-agnostic

---

> Both the CCA-F and GH-600 exams test diagnostic reasoning, not recall.
> Every question puts you inside a broken production system.
> These mental models are the tools you use to find the answer.

---

## Mental Model 1: The Failure Layer Stack

When an agentic system produces wrong output, the failure is always in one of five layers. Diagnose top-down.

```
┌─────────────────────────────────────┐
│  5. OUTPUT LAYER                    │  ← Wrong format, missing fields, garbled synthesis
├─────────────────────────────────────┤
│  4. AGGREGATION LAYER               │  ← Coordinator assembled correct parts incorrectly
├─────────────────────────────────────┤
│  3. EXECUTION LAYER                 │  ← Agent ran the right task but produced wrong result
├─────────────────────────────────────┤
│  2. DELEGATION LAYER                │  ← Agent received wrong or insufficient context
├─────────────────────────────────────┤
│  1. DECOMPOSITION LAYER             │  ← Coordinator defined the wrong scope from the start
└─────────────────────────────────────┘
```

**Exam application:** When a scenario says subagents completed correctly but the final output is wrong — the failure is at layer 1 or 4. When context is missing — layer 2. When tools are called incorrectly — layer 3. Rarely layer 5.

---

## Mental Model 2: Root Cause vs Symptom

Every wrong answer in a certification exam is a symptom fix. Every correct answer is a root cause fix.

| Symptom | Symptom fix (wrong) | Root cause fix (correct) |
|---------|--------------------|-----------------------|
| Tool called incorrectly 40% of time | Add routing classifier | Expand and differentiate tool descriptions |
| Agent enforces limit inconsistently | Strengthen prompt instruction | Implement pre-execution hook |
| New team member ignores conventions | Document conventions more clearly | Move conventions to shared config file |
| Coordinator produces wrong scope | Add scope validation step | Fix decomposition logic |
| Model misses info in long context | Increase context window | Move critical info to top of context |
| Values go vague over long conversation | Retry with clarification | Use persistent structured block |

**The test:** Ask "does this fix make the broken thing work correctly, or does it add something on top of a still-broken thing?" If the latter — it is a wrong answer.

---

## Mental Model 3: The Enforcement Spectrum

Match enforcement strength to consequence severity. The exam always presents options across this spectrum.

```
SEVERITY OF CONSEQUENCE
Low                                                    High
│                                                        │
▼                                                        ▼

Prompt          Output           Pre-execution      Hard block
instruction  →  validation   →   hook           →   (absolute)

"Please don't    Check output     Fire before        Cannot be
do X"            after it runs    action executes    overridden
                                  every time
```

**Exam pattern:** Financial, legal, safety-critical → pre-execution hook. Style, preference, soft guidance → prompt instruction. The wrong answer always uses prompt instruction for high-consequence scenarios.

---

## Mental Model 4: Context Flows Explicitly

In every multi-agent system, context does not flow automatically. It must be explicitly passed.

```
COORDINATOR knows: [A, B, C, D, E]
                         │
              Explicitly packs: [A, C]
                         │
              SUBAGENT receives: [A, C]
              SUBAGENT does NOT receive: [B, D, E]
```

**Exam pattern:** Whenever a subagent produces an output that is missing something the coordinator knew — the root cause is missing context packing, not subagent capability.

**Corollary:** You cannot fix a context isolation problem by making the subagent "smarter." The subagent cannot access what it was not given.

---

## Mental Model 5: Programmatic vs Model-Dependent Signals

The exam consistently tests whether you can distinguish between signals that are programmatically reliable and signals that depend on model behaviour.

| Signal type | Examples | Reliability |
|-------------|---------|-------------|
| Programmatic | Stop reason, iteration count, threshold breach, explicit user request | High — fires every time |
| Model-dependent | Sentiment score, confidence rating, self-reported completion, classifier output | Low — varies with input phrasing and model state |

**Exam pattern:** Any escalation trigger or completion check that relies on the model assessing its own state is a wrong answer. Any trigger that fires based on an observable, countable, or explicitly stated condition is a correct answer.

---

## Mental Model 6: The Retry Test

Before building a retry loop, apply this test:

```
Is the information present in the source?
        │
      YES │                    NO
        ▼                      ▼
Did the agent        Retrying will produce
fail to extract it?  fabricated data.
        │            Build a fallback,
      YES │           not a retry loop.
        ▼
Retry loop is
appropriate.
```

**Exam pattern:** A retry loop scenario where the data is absent = fabrication, not correction. The correct answer addresses the missing data problem, not the retry mechanism.

---

## Mental Model 7: Description Quality = Routing Quality

Tool descriptions are not documentation. They are the decision mechanism the model uses to choose which tool to call.

**The description test:**

Read two tool descriptions side by side. Can you tell, without running the tools, exactly which one to call for any given scenario? If not — the descriptions are the problem.

```
POOR: get_account — Gets account data
POOR: get_order — Gets order data

Both accept IDs. Model cannot distinguish. Routing errors guaranteed.

GOOD: get_account — Retrieves account-level data by customer ID (format: CUST-XXXXXX).
      Use for: account status, contact details, payment history.
      Do NOT use for order-specific queries — use get_order instead.

GOOD: get_order — Retrieves order-specific data by order ID (format: ORD-XXXXXX).
      Use for: order status, line items, shipping, fulfilment.
      Do NOT use for account-level data — use get_account instead.
```

**Exam pattern:** Wrong tool called X% of the time → first action is always to improve descriptions. Never add a classifier first.

---

## Mental Model 8: The Cascade Map

Context and memory failures (lowest-weighted domain in most exams) cascade into every other domain.

```
Context failure
      │
      ├──→ Subagent receives wrong data (Orchestration failure)
      │
      ├──→ Tool output bloats context (Tool Design failure)
      │
      ├──→ Retry loop runs on absent data (Prompt Engineering failure)
      │
      └──→ Escalation triggers miss real failures (Reliability failure)
```

**Exam pattern:** A question about a multi-layer failure where the root cause is in context management will always have a correct answer that traces back to a context architecture decision — not a fix in the domain where the failure manifests.

---

## Applying the framework: exam question protocol

1. **Read the scenario.** What is failing and at what layer?
2. **Identify the root cause.** Decomposition? Context? Description? Enforcement?
3. **Eliminate symptom fixes.** Any answer that adds a layer over the broken thing.
4. **Select the root cause fix.** The one that makes the broken thing work correctly.
5. **Check the enforcement spectrum.** Is the consequence high? Match the enforcement strength.

**Time allocation:** 2 minutes per question. Spend 30 seconds on elimination. 90 seconds to confirm the correct answer against the root cause.

---

*Phil Myint · ZenCloud Global Consultants · May 2026*
*Part of: Agentic AI Certification Hub*
