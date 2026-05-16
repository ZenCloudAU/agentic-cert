# Domain 3: Prompt Engineering & Structured Output
### Weight: 20% · Third priority

---

## What this domain tests

Not whether you can write prompts. The exam assumes you can. It tests whether you understand the limits of what prompts and schemas can guarantee — and what they cannot.

---

## Structured output: what schemas do and do not guarantee

### What `tool_use` with a JSON schema **guarantees**

- Syntactically valid JSON
- Required fields are present
- Field types match the schema
- Enum values are from the defined set

### What schemas **do not guarantee**

- Semantic correctness
- Mathematical accuracy (e.g. `stated_total` matching sum of `line_items`)
- Values grounded in source data (a required field with no source data → model fabricates)
- Logical consistency between fields

### Exam rule

> A schema eliminates syntax errors. It does not eliminate semantic errors.

A response can be perfectly valid JSON with perfectly wrong values.

---

## Schema design principles

| Design choice | Rule |
|---|---|
| Required fields | Only mark required if the information is **always present** in the source |
| Missing data | Required field + no source data = model invents a value |
| Optional fields | Use `"type": ["string", "null"]` for fields that may be absent |
| Enums | Add `"unclear"` and `"other"` — never force the model into a category that doesn't fit |
| Field specificity | Specific field names beat vague ones — reduces interpretation variance |

### Anti-pattern

```json
{
  "required": ["customer_name", "order_date", "satisfaction_score"]
}
```

If `satisfaction_score` isn't in the source document, the model fills it in. Mark it nullable or omit it from required.

---

## Few-shot examples

**Few-shot beats descriptive instructions** for consistent output. Rules create interpretation variance. Examples create pattern recognition.

### Construction principles

- Target examples at cases where the model **actually gets it wrong** — ambiguous inputs, overlapping categories
- Include the **rationale**, not just the answer — "classified as X because..." helps more than just "X"
- Cover edge cases explicitly — if the edge case isn't in the examples, it won't be handled consistently
- Keep examples structurally consistent — input format and output format should match exactly what production will see

---

## Retry-with-feedback loops

**Work when:** Data is present but mis-extracted. The model made an error, not a knowledge gap.  
**Do not work when:** The information is not in the source document. Retrying against missing data produces invented data.

### Exam rule

> Build a retry loop only when the information exists and the model failed to find it. Not as a general error-handling pattern.

---

## Batch API

| Property | Value |
|----------|-------|
| Cost | 50% savings vs synchronous |
| Latency | Up to 24 hours |
| SLA | No latency guarantee |

**Right for:** Overnight reports, bulk processing, non-blocking analysis  
**Wrong for:** Any task where someone is waiting — blocking pre-merge checks, real-time responses

### Exam scenario (memorise this)

> A manager proposes moving both a blocking pre-merge check and a nightly tech-debt report to the Batch API to save costs. What is the correct recommendation?

**Answer:** Move the nightly report. Keep the pre-merge check synchronous. A developer cannot wait 24 hours for a merge gate.

---

## System prompt structure

| Element | Purpose |
|---------|---------|
| Role framing | Sets the model's operating context and persona |
| Task definition | What the model is doing |
| Constraints | What the model must not do |
| Output format | Structure and format of the response |
| Examples | Concrete demonstrations of expected behaviour |

**Rule:** Structure system prompts in this order. Role → Task → Constraints → Format → Examples. Reversing this order reduces reliability.

---

## Chain-of-thought patterns

- Explicit: "Think step by step before giving your answer"
- Implicit: Structure the output to require intermediate reasoning (e.g. `"reasoning"` field before `"conclusion"` field in schema)
- Implicit CoT via schema is more reliable than explicit instruction — the model is forced to populate the field, not just told to think

---

## Key distinctions for D3

| Concept | Correct understanding |
|---|---|
| Schema guarantees | Syntax only. Not semantics. Not accuracy. Not groundedness. |
| Required fields | Only if always present. Required + absent source = invented value. |
| Few-shot vs instructions | Examples beat rules for consistency on ambiguous inputs. |
| Retry loops | Only useful when data exists. Not for missing information. |
| Batch API | 50% saving, 24-hour window. Incompatible with blocking tasks. |

---

## Practice questions → [D3-practice.md](../practice-questions/D3-practice.md)

---

*Domain 3 of 5 · CCA-F Study Repository · ZenCloud Global Consultants · May 2026*
