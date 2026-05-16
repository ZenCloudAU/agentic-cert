# Anti-Patterns Master Index
### All agentic AI failure modes · Platform-agnostic · Mapped to both certifications

---

> This is the vendor-neutral version. Every anti-pattern here appears in both the CCA-F and GH-600 exams under different surface implementations but identical root causes.
> Platform-specific anti-patterns are in each certification's own Anti-Patterns file.

---

## Category 1: Loop and Termination Anti-Patterns

### AP-LOOP-01: Text-based completion detection
**Pattern:** Checking whether the agent is done by parsing its response text for phrases like "task complete" or "I have finished."
**Root cause:** Using a model-generated signal for a programmatic decision.
**Fix:** Use stop reasons (`end_turn`, workflow status flags, or explicit completion callbacks).
**Appears in:** CCA-F D1 · GH-600 D1

### AP-LOOP-02: Arbitrary iteration limits as completion signals
**Pattern:** Stopping after N steps regardless of whether the task is done.
**Root cause:** Confusing resource limits with completion signals.
**Fix:** Use stop reasons for completion. Use iteration limits as safety rails only, with explicit partial-result handling.
**Appears in:** CCA-F D1 · GH-600 D1

### AP-LOOP-03: No session resumption
**Pattern:** Long-running agent crashes and restarts from zero.
**Root cause:** No checkpointing between steps.
**Fix:** Checkpoint completed steps explicitly. Resume from last confirmed state.
**Appears in:** CCA-F D1 · GH-600 D3

---

## Category 2: Orchestration Anti-Patterns

### AP-ORCH-01: Scope truncation in decomposition
**Pattern:** Coordinator decomposes a broad question into a subset of its actual scope. Subagents execute correctly. Final output is complete but wrong in scope.
**Root cause:** Decomposition logic, not subagent execution.
**Fix:** Review and correct the coordinator's decomposition logic. The subagents are not the problem.
**Appears in:** CCA-F D1 · GH-600 D5

### AP-ORCH-02: Assumed context inheritance
**Pattern:** Coordinator knows something and expects subagents to know it without being told.
**Root cause:** Misunderstanding of context isolation.
**Fix:** Explicitly pack every piece of information a subagent needs into its task prompt. No exceptions.
**Appears in:** CCA-F D1 · GH-600 D5

### AP-ORCH-03: Parallel dispatch of dependent agents
**Pattern:** Agent B requires Agent A's output. Both are dispatched simultaneously.
**Root cause:** Coordinator does not model task dependencies.
**Fix:** Sequence dependent agents. Dispatch B only after A completes and its output is packed into B's task.
**Appears in:** CCA-F D1 · GH-600 D5

### AP-ORCH-04: Coordinator executing instead of orchestrating
**Pattern:** Coordinator takes on execution tasks rather than delegating them.
**Root cause:** Unclear separation of concerns between coordinator and agents.
**Fix:** Coordinator decomposes, delegates, handles errors, aggregates. It does not execute.
**Appears in:** CCA-F D1 · GH-600 D5

---

## Category 3: Tool and Integration Anti-Patterns

### AP-TOOL-01: Minimal, non-differentiating tool descriptions
**Pattern:** Two tools with nearly identical descriptions. Model routes incorrectly.
**Root cause:** Descriptions are not sufficient for the model to distinguish tools.
**Fix:** Expand descriptions with input format, examples, explicit cross-references, and scope exclusions.
**Appears in:** CCA-F D4 · GH-600 D2

### AP-TOOL-02: Routing classifier before fixing descriptions
**Pattern:** Adding a classifier to fix a tool selection problem caused by poor descriptions.
**Root cause:** Adding infrastructure over a broken foundation.
**Fix:** Fix descriptions first. Only add routing infrastructure if ambiguity persists after descriptions are correct.
**Appears in:** CCA-F D4 · GH-600 D2

### AP-TOOL-03: Generic error responses
**Pattern:** Tool returns `"Operation failed"` or equivalent.
**Root cause:** No structured error design.
**Fix:** Return structured errors with category, retryability flag, attempted input, and partial results.
**Appears in:** CCA-F D4 · GH-600 D2

### AP-TOOL-04: Unfiltered verbose tool output
**Pattern:** Tool returns 40 fields. Coordinator needs 5. All 40 accumulate in context across multiple calls.
**Root cause:** No output trimming at the tool boundary.
**Fix:** Trim tool output to needed fields before returning to the orchestrator. Post-execution hook or in-tool filtering.
**Appears in:** CCA-F D4/D5 · GH-600 D2/D3

### AP-TOOL-05: Committed secrets in configuration
**Pattern:** API tokens or credentials hardcoded in version-controlled configuration files.
**Root cause:** Secrets management not designed into the tool configuration.
**Fix:** Environment variable substitution. Never commit values.
**Appears in:** CCA-F D4 · GH-600 D2

---

## Category 4: Context and Memory Anti-Patterns

### AP-CTX-01: Progressive summarisation of transactional values
**Pattern:** Numbers, IDs, and dates live in summarisable conversation history. They degrade over long conversations.
**Root cause:** No separation between conversational context and transactional state.
**Fix:** Persistent structured block for transactional facts, outside the summarisation scope.
**Appears in:** CCA-F D5 · GH-600 D3

### AP-CTX-02: Critical information buried in long context
**Pattern:** Key findings at position 40K of a 75K-token context. Model misses them.
**Root cause:** Lost-in-the-middle — models process start and end more reliably than middle.
**Fix:** Move critical information to the top. Use section headings as navigation anchors.
**Appears in:** CCA-F D5 · GH-600 D3

### AP-CTX-03: No state persistence across sessions
**Pattern:** Agent loses all progress if the session ends or the process crashes.
**Root cause:** State stored only in-context, not persisted externally.
**Fix:** Checkpoint state to external storage at defined intervals.
**Appears in:** CCA-F D1 · GH-600 D3

---

## Category 5: Enforcement Anti-Patterns

### AP-ENF-01: Prompt instruction for high-consequence constraints
**Pattern:** "Do not process refunds over $500" as a system prompt instruction.
**Root cause:** Using a best-effort mechanism for a must-not-fail constraint.
**Fix:** Pre-execution hook that fires every time, regardless of model behaviour.
**Appears in:** CCA-F D1 · GH-600 D6

### AP-ENF-02: Sentiment-based escalation
**Pattern:** Escalate when sentiment score drops below threshold.
**Root cause:** Model-dependent signal used as a programmatic trigger.
**Fix:** Explicit user request, policy gap, or measurable inability to progress.
**Appears in:** CCA-F D5 · GH-600 D6

### AP-ENF-03: Model confidence-based escalation
**Pattern:** Escalate when model self-rates confidence below threshold.
**Root cause:** Models cannot reliably assess their own uncertainty.
**Fix:** Same as above — programmatic, observable triggers only.
**Appears in:** CCA-F D5 · GH-600 D6

### AP-ENF-04: No human-in-the-loop for irreversible actions
**Pattern:** Agent executes irreversible actions (send email to 10K users, delete records, initiate payments) without human confirmation.
**Root cause:** Human oversight not architected into the action boundary.
**Fix:** HITL checkpoint before any irreversible action above a defined consequence threshold.
**Appears in:** CCA-F D1 · GH-600 D6

---

## Category 6: Evaluation Anti-Patterns

### AP-EVAL-01: One-time benchmark as ongoing evaluation
**Pattern:** Agent evaluated once at deployment. No ongoing monitoring.
**Root cause:** Treating agent evaluation like model evaluation.
**Fix:** Continuous monitoring of task completion rate, tool routing accuracy, error recovery rate.
**Appears in:** GH-600 D4

### AP-EVAL-02: Retry loop for absent data
**Pattern:** Retry-with-feedback loop runs when the required information is not in the source.
**Root cause:** Retry loop built as a general error handler, not a targeted data recovery mechanism.
**Fix:** Diagnose before building the loop. Retry only when data exists and was mis-extracted.
**Appears in:** CCA-F D3 · GH-600 D4

### AP-EVAL-03: Review in generation context
**Pattern:** The same agent/session that generated code also reviews it.
**Root cause:** Generation context causes rationalisation of own decisions.
**Fix:** Fresh context for review. No generation history.
**Appears in:** CCA-F D2 · GH-600 D4

---

## Cross-certification pattern summary

| Anti-pattern category | CCA-F domains | GH-600 domains |
|---|---|---|
| Loop and termination | D1 | D1, D3 |
| Orchestration | D1 | D5 |
| Tool and integration | D4 | D2 |
| Context and memory | D5 | D3 |
| Enforcement | D1, D5 | D6 |
| Evaluation | D3 | D4 |

---

*Phil Myint · ZenCloud Global Consultants · May 2026*
*Part of: Agentic AI Certification Hub*
