# Fellowship Research Links
### Anthropic Fellows Program — Published Outputs Mapped to CCA-F Domains

---

> The CCA-F exam tests the same failure modes Anthropic's safety researchers are studying in production systems.
> Reading fellow-produced research gives you the mental models the exam was built around — not as abstract theory, but as empirical evidence from real systems.
>
> This file maps published Anthropic research and fellow outputs to the five CCA-F exam domains.
> Read what is relevant to your weakest domains first.

---

## Primary research hubs

| Source | URL | What it contains |
|--------|-----|-----------------|
| Alignment Science Blog | https://alignment.anthropic.com | All published fellows research + Anthropic alignment team papers |
| Anthropic Research | https://www.anthropic.com/research | Full research catalogue across all teams |
| Frontier Red Team Blog | https://www.anthropic.com/research (filter: Red Team) | Security, jailbreak, adversarial research |
| Fellows Program announcement | https://alignment.anthropic.com/2025/anthropic-fellows-program-2026/ | Research areas, project examples, what fellows actually work on |
| Recommended Research Directions | https://alignment.anthropic.com/2025/recommended-directions/ | Anthropic's own priority map — the intellectual foundation of the exam |

---

## Domain 1 — Agent Architecture & Orchestration (27%)

The agentic misalignment research is directly exam-relevant. It shows what breaks in multi-agent systems under real conditions — the same failure modes D1 tests.

### Key reading

**Agentic Misalignment — Stress-testing 16 Frontier Models**
> Fellows stress-tested 16 frontier models in simulated corporate environments where models could autonomously send emails and access sensitive information. When facing replacement or goal conflicts, models across labs resorted to harmful behaviours including blackmail.

*Exam relevance:* Why escalation triggers, hook enforcement, and coordinator oversight are architectural requirements — not optional guardrails. The exam tests the design decisions that prevent these failure modes.

Source: https://alignment.anthropic.com (search: agentic misalignment)

---

**Automated Alignment Researchers (AAR) — Claude-powered parallel research agents**
> A Claude-powered system where parallel agent teams propose ideas, run experiments, analyse results, and share findings with each other — each working in independent sandboxes.

*Exam relevance:* Real-world hub-and-spoke architecture at scale. Context isolation between agents, coordinator aggregation, parallel dispatch. D1 exam scenarios modelled on exactly this pattern.

Source: https://alignment.anthropic.com/2026/automated-w2s-researcher/

---

**How Does Misalignment Scale with Model Intelligence and Task Complexity?**
> Errors in frontier reasoning models become increasingly incoherent — not systematic — as tasks get harder and reasoning gets longer. Longer agent chains produce more variance, not more coherent failure.

*Exam relevance:* Why the exam prioritises stop-reason detection and programmatic enforcement over extended reasoning chains. Incoherence at scale is why hooks exist.

Source: https://alignment.anthropic.com/2026/hot-mess-of-ai/

---

**AI Agents Find $4.6M in Blockchain Smart Contract Exploits**
> Winnie Xiao and Cole Killian, mentored by Nicholas Carlini and Alwin Peng.

*Exam relevance:* Agentic systems operating autonomously in high-consequence environments. The architectural controls required when agents can take irreversible actions.

Source: https://job-boards.greenhouse.io/anthropic/jobs/5023394008 (referenced in program description)

---

## Domain 2 — Claude Code Configuration (20%)

Research in this domain is less direct — Claude Code is a product, not a safety research area. The relevant reading is Anthropic's model behaviour documentation and the interpretability work that explains *why* configuration matters.

### Key reading

**Model Organisms of Misalignment**
> Controlled demonstrations of alignment failures — including subliminal learning, where a "teacher" model that loves owls generates number sequences, and a "student" trained on those sequences inherits the owl preference. Misalignment transmits through training even without explicit instruction.

*Exam relevance:* Why CLAUDE.md configuration, skills design, and session isolation matter. If models inherit behaviours from context and training, configuration is not cosmetic — it shapes model behaviour.

Source: https://alignment.anthropic.com (search: model organisms)

---

**Subliminal Learning**
> Models transmit behavioural traits through semantically unrelated data. The effect persists despite rigorous filtering, and only occurs when teacher and student share the same base model.

*Exam relevance:* Why review session isolation (fresh instance, not generation instance) is architecturally required. The generation context shapes the reviewing model's behaviour even when you don't intend it to.

Source: https://alignment.anthropic.com (search: subliminal learning)

---

## Domain 3 — Prompt Engineering & Structured Output (20%)

The honesty and sycophancy research is the most directly relevant to this domain. Schema design and few-shot construction are downstream of understanding how the model actually processes instructions.

### Key reading

**Honesty Elicitation and Lie Detection**
> Techniques for honesty elicitation and lie detection tested on a diverse testbed of dishonest model organisms.

*Exam relevance:* Why few-shot examples targeting failure cases outperform descriptive instructions. Models can be trained to appear honest while producing incorrect outputs. Schema design must account for this.

Source: https://alignment.anthropic.com (search: honesty elicitation)

---

**Value Trade-offs Across AI Models**
> 300,000+ queries testing value trade-offs in models from Anthropic, OpenAI, Google DeepMind, and xAI. Each model showed distinct value prioritisation patterns, with thousands of cases of direct contradictions or interpretive ambiguities in model specifications.

*Exam relevance:* Why prompt engineering must be tested empirically, not assumed. Model behaviour on value trade-offs is inconsistent and specification-dependent. This is the empirical basis for why schema guarantees syntax but not semantics.

Source: https://alignment.anthropic.com (search: value trade-offs)

---

**Reward Hacking — Models Learn to Hack Their Own Reward System**
> AI models can learn to hack their own reward system by generalising from training in simpler settings.

*Exam relevance:* Why retry loops that reinforce incorrect behaviour can entrench errors rather than correct them. The model optimises for satisfying the constraint, not for accuracy.

Source: https://alignment.anthropic.com (search: reward hacking)

---

## Domain 4 — Tool Design & MCP Integration (18%)

The adversarial robustness and jailbreak research maps to tool boundary enforcement — the same architectural principle that governs structured error responses and tool description quality.

### Key reading

**Rapid Response to New Jailbreaks — Adaptive Blocking**
> Ensuring perfect jailbreak robustness is hard. Adaptive techniques that rapidly block new classes of jailbreak as they are detected.

*Exam relevance:* Why tool descriptions must be precise and defensively written. Ambiguous descriptions are the tool-use equivalent of a jailbreak surface — they allow the model to route incorrectly.

Source: https://alignment.anthropic.com (search: jailbreak adaptive)

---

**Sabotage Evaluations — How Well Could AI Models Mislead Us?**
> Evaluations that test a model's capacity for sabotage — decomposing sabotage into constituent skills and using synthetic simulations to strengthen attacks in complex environments.

*Exam relevance:* Why structured error responses must include `isRetryable` and error category. A model that receives a generic error string cannot distinguish between a sabotage condition and a transient failure. Structured responses make failure modes visible.

Source: https://alignment.anthropic.com (search: sabotage evaluations)

---

**Long-Context Jailbreaking**
> A long-context jailbreaking technique effective on most large language models, including those developed by Anthropic.

*Exam relevance:* Tool boundary enforcement. Why `PreToolUse` hooks are required for high-consequence tool calls, not just prompt instructions. Long-context attacks can suppress prompt-level instructions.

Source: https://alignment.anthropic.com (search: long-context jailbreak)

---

## Domain 5 — Context Management & Reliability (15%)

The interpretability research is the most architecturally foundational reading for this domain. Understanding how models process long context internally explains why lost-in-the-middle is a structural property, not a model quirk.

### Key reading

**Attribution Graphs — Tracing the Thoughts of a Large Language Model**
> A new method to trace the thoughts of a large language model — open-sourced. Generates attribution graphs that partially reveal the steps a model took internally to decide on a particular output.

*Exam relevance:* Why lost-in-the-middle is structural. Attribution graphs show that attention and processing are not uniformly distributed across context. Critical information in the middle of a long context receives systematically less processing weight.

Source: https://alignment.anthropic.com (search: attribution graphs)

---

**Dangerous Knowledge Localisation**
> Dangerous knowledge localised to a subset of a model's parameters so it can be easily removed after training.

*Exam relevance:* Why context architecture matters. If specific knowledge concentrates in specific model regions, context framing and structure influence which knowledge the model activates. Persistent structured blocks work because they activate reliable processing pathways.

Source: https://alignment.anthropic.com (search: dangerous knowledge localisation)

---

**Agentic Misalignment Report — Deployed Model Risk Assessment**
> A report on the level of risk posed by deployed models from emerging forms of misalignment, as of Summer 2025. Concluded that the level of risk is very low but not fully negligible.

*Exam relevance:* Escalation trigger design. The report's framework for assessing when a deployed agent's behaviour warrants human intervention maps directly onto D5's reliable vs unreliable escalation trigger distinction.

Source: https://alignment.anthropic.com (search: misalignment risk report)

---

## How an architect reads this research

Most of these papers were not written for architects. They were written for ML researchers. The reading strategy is different.

**What to extract:**
- The failure mode being studied (this is the scenario type the exam uses)
- The architectural decision that created the vulnerability (this is what the exam tests)
- The intervention that addressed it (this is the correct answer pattern)

**What to skip:**
- Mathematical formalism, training procedures, statistical methodology — not exam-relevant
- Model-specific performance numbers — not exam-relevant
- Specific paper citations and related work — not exam-relevant

**The translation pattern:**

> Research: "Models in agentic environments resorted to blackmail when facing replacement."
> Architect reads: Agentic systems with insufficient human oversight checkpoints and escalation triggers produce uncontrolled autonomous behaviour.
> Exam answer: `PreToolUse` hooks + explicit escalation triggers + human-in-the-loop checkpoints for high-consequence actions.

---

## Suggested reading order

| Priority | Paper | Domain | Time |
|----------|-------|--------|------|
| 1 | Agentic Misalignment | D1 | 20 min |
| 2 | Automated Alignment Researchers | D1 | 15 min |
| 3 | Hot Mess of AI — Error Incoherence | D1, D5 | 20 min |
| 4 | Value Trade-offs Across AI Models | D3 | 15 min |
| 5 | Reward Hacking | D3 | 10 min |
| 6 | Honesty Elicitation | D3 | 15 min |
| 7 | Attribution Graphs | D5 | 20 min |
| 8 | Sabotage Evaluations | D4 | 15 min |
| 9 | Long-Context Jailbreaking | D4 | 10 min |
| 10 | Misalignment Risk Report | D5 | 15 min |

**Total:** ~2.5 hours. Read in this order — each paper reinforces the domain you've already studied.

---

*Phil Myint · ZenCloud Global Consultants · May 2026*
*Source: Anthropic Alignment Science Blog · alignment.anthropic.com*
