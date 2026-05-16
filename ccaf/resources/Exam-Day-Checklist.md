# Exam Day Checklist
### CCA-F · Claude Certified Architect – Foundations

---

## Before the exam

- [ ] Confirm exam appointment and access link via Anthropic Academy / Skilljar
- [ ] Confirm you are logged in as a Partner Network member
- [ ] Stable internet connection confirmed
- [ ] Quiet room, 120 minutes of uninterrupted time
- [ ] ID ready for proctoring if required
- [ ] Read the [Quick Reference Card](../study-guides/Quick-Reference-Card.md) one final time — then close it

---

## Mental model reset: what this exam tests

**Not recall. Diagnostic reasoning.**

Every question puts you inside a broken production system. Your job: identify the root cause and pick the fix that addresses it directly.

The wrong answers are always plausible. They are engineering decisions a reasonable person would make. The right answer fixes the actual problem — not the symptom.

---

## The master principle (memorise this)

> Fix the root cause. Do not add a layer on top of a broken design.

- Better tool descriptions **before** a routing classifier
- Programmatic hooks **before** a stronger prompt
- Explicit context packing **before** a shared memory layer
- Expand descriptions **before** merging tools

---

## When stuck: apply the elimination framework

1. **Which answer adds infrastructure over a broken foundation?** Eliminate it.
2. **Which answer is a workaround rather than a fix?** Eliminate it.
3. **Which answer addresses the symptom rather than the cause?** Eliminate it.
4. What remains is usually correct.

---

## Domain-specific mental triggers

**D1 (27%)** — If subagents are producing wrong output: check coordinator decomposition and context packing first.  
**D2 (20%)** — If a convention isn't being followed: check whether it's in user vs project CLAUDE.md. If CI/CD hangs: `-p` flag.  
**D3 (20%)** — If values are wrong despite correct structure: schema only guarantees syntax. If retry loop isn't working: the data probably isn't there.  
**D4 (18%)** — If tools are being called incorrectly: expand descriptions first. If coordinator can't recover from errors: structured error response required.  
**D5 (15%)** — If numbers are going vague: progressive summarisation trap. If model misses info: lost-in-the-middle. If escalation is unreliable: check whether you're using sentiment or confidence scores.

---

## Pacing

60 questions · 120 minutes = 2 minutes per question.

- First pass: answer what you know confidently
- Flag anything uncertain for review
- Second pass: apply elimination framework to flagged questions
- Never leave a question blank

---

## Passing score

720 / 1000

---

*Phil Myint · ZenCloud Global Consultants · May 2026*
