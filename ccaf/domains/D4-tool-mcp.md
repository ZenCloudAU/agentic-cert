# Domain 4: Tool Design & MCP Integration
### Weight: 18% · Fourth priority

---

## What this domain tests

Whether you understand that tool descriptions are the routing mechanism — and that structured error responses are the difference between a coordinator that can recover and one that cannot.

---

## Tool descriptions are routing logic

The LLM uses tool descriptions to decide which tool to call. Descriptions are not documentation. They are the decision mechanism.

### Minimum viable description

A tool description must answer:
- What does this tool do?
- What inputs does it accept? (format, type, examples)
- When should I use this tool over its neighbour?
- What edge cases should I know about?

### Anti-pattern

```
get_customer — Gets customer information
get_order — Gets order details
```

Both accept similar-looking inputs. No differentiation. The model guesses.

### Correct pattern

```
get_customer — Retrieves account-level customer information by customer ID (format: CUST-XXXXXX). 
Use this tool when you need account status, contact details, or payment history. 
Do not use for order-specific queries — use get_order instead.
Example inputs: CUST-001234, CUST-987654.

get_order — Retrieves order-specific details by order ID (format: ORD-XXXXXX).
Use this tool when you need order status, line items, shipping details, or fulfilment history.
Do not use for account-level customer data — use get_customer instead.
Example inputs: ORD-456789, ORD-000001.
```

### Exam scenario (memorise this)

> Logs show `get_customer` is being called for order queries 45% of the time. Tool descriptions are minimal and nearly identical. What is the first corrective step?

Options:
- Build a routing classifier
- Merge the two tools
- Expand and differentiate the descriptions
- Add a pre-tool validation step

**Answer: Expand the descriptions.** Fix the root cause before adding infrastructure on top of broken routing.

---

## Structured error responses

### Anti-pattern: generic error string

```json
"Operation failed"
```

A coordinator receives this. It cannot determine:
- What kind of failure occurred
- Whether to retry
- Whether to try an alternative approach
- Whether to annotate the gap and continue

### Correct pattern: structured error object

```json
{
  "errorCategory": "rate_limit",
  "isRetryable": true,
  "attemptedQuery": "SELECT * FROM orders WHERE customer_id = 'CUST-001234'",
  "partialResults": [...],
  "retryAfterSeconds": 30,
  "message": "Rate limit exceeded for orders endpoint"
}
```

A coordinator receiving this can:
- Determine it's a rate limit (not a data error)
- Know it's retryable
- Wait the specified interval
- Continue with partial results in the interim

### Four error categories to know

| Category | `isRetryable` | Coordinator action |
|----------|--------------|-------------------|
| Rate limit | True | Wait and retry |
| Not found | False | Annotate gap, continue |
| Validation | False | Fix query, retry |
| Upstream outage | Depends | Escalate or annotate |

---

## MCP configuration scoping

| Location | File | Scope | Version-controlled |
|----------|------|-------|--------------------|
| Project-level | `.mcp.json` | Shared across team | Yes |
| Personal/experimental | `~/.claude.json` | Local only | No |
| Secrets | Environment variables | `${SECRET_NAME}` substitution | Never committed |

### Exam scenario (memorise this)

> A developer adds a personal MCP server for local testing. Where does the configuration go?

**Answer: `~/.claude.json`** — personal config, not version-controlled, not shared.

> A team MCP server needs to be available to all developers. Where does it go?

**Answer: `.mcp.json`** — project config, version-controlled, shared.

---

## MCP primitives

| Primitive | Purpose |
|-----------|---------|
| Tools | Functions the model can call |
| Resources | Data the model can read |
| Prompts | Reusable prompt templates |
| Sampling | Model calling model (advanced) |

### Transport mechanisms

| Transport | Use case |
|-----------|---------|
| stdio | Local MCP servers |
| SSE (Server-Sent Events) | Remote MCP servers |

---

## Tool interface design principles

1. **One tool, one responsibility.** A tool that does three things is a tool with three description problems.
2. **Input validation at the tool boundary.** Don't let the model pass invalid input downstream.
3. **Output trimming before return.** If a tool returns 40 fields and the coordinator needs 5, trim at the tool level. Less noise, smaller context footprint.
4. **Idempotent tools where possible.** Retryable tools must not create duplicate side effects.

---

## Key distinctions for D4

| Concept | Correct understanding |
|---|---|
| Tool descriptions | Routing logic, not documentation. Ambiguous descriptions = routing errors. |
| Generic vs structured errors | Generic = coordinator cannot recover. Structured = coordinator can decide. |
| `.mcp.json` vs `~/.claude.json` | Project-shared vs personal-only. |
| Secrets management | Environment variable substitution. Never committed values. |
| Error categories | Rate limit (retryable), not found (not retryable), validation (fix and retry), outage (escalate). |

---

## Practice questions → [D4-practice.md](../practice-questions/D4-practice.md)

---

*Domain 4 of 5 · CCA-F Study Repository · ZenCloud Global Consultants · May 2026*
