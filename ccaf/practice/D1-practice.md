# Domain 1 Practice Questions
### Agent Architecture & Orchestration · 15 Scenarios

---

**Instructions:** Scenario-based. Each question has four options. Select the best answer. Explanations follow each question — read them even when correct.

---

**Q1.** A multi-agent system processes customer support tickets. The coordinator assigns tickets to specialised subagents. Logs show that subagents consistently fail to apply the correct escalation policy for premium customers, even though the policy is documented in the coordinator's system prompt. What is the most likely root cause?

A. The subagents' system prompts are too long  
B. The escalation policy is not being explicitly packed into the Task prompt sent to each subagent  
C. The coordinator is using the wrong orchestration pattern  
D. The subagents need access to the coordinator's conversation history  

**Answer: B.** Subagents run in isolated context. They receive only what the coordinator explicitly packs into the Task. The coordinator knowing the policy means nothing if the subagents don't receive it.

---

**Q2.** An agentic system is checking completion by parsing the assistant's response for the phrase "task completed successfully." In production, the system occasionally hangs on tasks that are actually done. What is the correct fix?

A. Make the completion phrase more distinctive to reduce false negatives  
B. Add a timeout that forces completion after 30 seconds  
C. Replace text parsing with `stop_reason: end_turn` detection  
D. Add a validation step that confirms completion independently  

**Answer: C.** `stop_reason` is a programmatic signal designed for this purpose. Text parsing is fragile and unreliable. This is the root cause — fix it directly.

---

**Q3.** A coordinator decomposes the task "Analyse the competitive landscape for our enterprise software product" into three subtasks: pricing comparison, feature comparison, and UI comparison. Subagents complete all three correctly. The final synthesis is technically accurate but misses market positioning, distribution strategy, and partnership analysis. Where did the system fail?

A. The synthesis layer did not aggregate correctly  
B. The subagents produced incomplete analysis within their assigned domains  
C. The coordinator's task decomposition scoped the analysis too narrowly  
D. The coordinator did not provide enough context to the synthesis step  

**Answer: C.** The coordinator defined the scope. Competitive landscape analysis includes more than product features and UI. The subagents did exactly what they were told — the decomposition was wrong.

---

**Q4.** A financial services agent is configured with a system prompt instruction: "Do not initiate wire transfers above $10,000 without supervisor approval." During a penetration test, a tester constructs a prompt that causes the agent to initiate a $50,000 transfer. What architectural change would have prevented this?

A. Strengthen the system prompt instruction to be more explicit  
B. Add a validation layer that checks the transfer amount before submission  
C. Implement a `PreToolUse` hook that blocks the `initiate_transfer` tool when amount > $10,000  
D. Use a separate model to audit all transfer requests  

**Answer: C.** A `PreToolUse` hook fires every time the tool is called, regardless of prompt. Prompt instructions are best-effort. For financial consequences, programmatic enforcement is required.

---

**Q5.** An agent runs a five-step research workflow. A network error occurs at step 3. Without any session management, what happens when the system restarts?

A. The agent resumes from step 3 using cached results  
B. The agent restarts from step 1 and repeats all work  
C. The agent skips to step 4 and works forward  
D. The agent returns the partial results from steps 1–2  

**Answer: B.** Without checkpointing, context does not persist across restarts. The agent has no record of completed steps.

---

**Q6.** A coordinator assigns three research subagents the task "Research AI trends." Subagents return results on AI in healthcare, AI in finance, and AI in manufacturing. The final report covers only those three sectors. A stakeholder notes the report missed AI in education, AI in transportation, and AI in retail. What is the failure?

A. Subagents should have proactively expanded their scope  
B. The synthesis step should have identified coverage gaps  
C. The coordinator's task decomposition did not cover the full domain  
D. Subagents should have communicated with each other to avoid gaps  

**Answer: C.** Coordinator owns scope definition. The decomposition assigned three specific sectors. The result is correct for those three — but incomplete for the intended question.

---

**Q7.** An agent system uses `stop_reason: tool_use` as the signal to continue looping. A developer argues that this is unnecessary overhead and proposes checking for tool call patterns in the assistant's message content instead. What is the correct response?

A. The developer is correct — message parsing is more flexible  
B. The developer is incorrect — `stop_reason` is a programmatic signal; message parsing is fragile and should never be used for flow control  
C. Both approaches are equally valid; choose based on system requirements  
D. Message parsing is acceptable for simple tool patterns but not for complex ones  

**Answer: B.** `stop_reason` is deterministic and designed for this exact purpose. Message parsing introduces fragility that breaks in production.

---

**Q8.** A hub-and-spoke system has a coordinator and four subagents. Subagent B's task requires the output from Subagent A. The coordinator dispatches all four subagents simultaneously. What breaks?

A. Nothing — subagents share a message queue and will sequence naturally  
B. Subagent B will fail or produce incorrect results because it cannot access Subagent A's output  
C. The coordinator will automatically retry Subagent B once A completes  
D. Subagent B will request the output from Subagent A directly  

**Answer: B.** Subagents run in isolated context. They cannot access each other's outputs. Dependency ordering must be managed by the coordinator — dispatch Subagent A first, collect its output, then dispatch Subagent B with A's output included in the Task.

---

**Q9.** A `PreToolUse` hook blocks calls to `send_email` when the recipient count exceeds 1,000. A developer argues this could be handled more flexibly with a prompt instruction. When is the developer's argument valid?

A. Never — hooks should always be used for tool restrictions  
B. When the email restriction is a preference, not a compliance or cost requirement  
C. When the system is in a development environment only  
D. When the email count threshold is configurable  

**Answer: B.** Hooks are required when the consequence of failure is material (financial, legal, compliance). For soft guardrails and preferences where occasional exceptions are acceptable, prompt instructions are appropriate.

---

**Q10.** A coordinator assigns a subagent the task: "Summarise the customer complaint." The subagent returns a generic summary. The coordinator escalates. The actual complaint was about a billing error on invoice #INV-00234 for $847. Why is the subagent's output insufficient?

A. The subagent's summarisation capability is too limited  
B. The coordinator did not include the complaint details in the Task prompt  
C. The subagent needed access to the billing system  
D. The task was too vague for effective summarisation  

**Answer: B.** If the coordinator did not pack the complaint details into the Task, the subagent had nothing to summarise. Explicit context packing is the coordinator's responsibility.

---

**Q11.** Which of the following is a reliable method to detect when an agentic loop should terminate?

A. The assistant's response contains no tool call JSON  
B. The iteration count reaches a predefined limit  
C. `stop_reason` equals `end_turn`  
D. The response length drops below a threshold  

**Answer: C.** Only `stop_reason: end_turn` is a reliable programmatic terminal signal.

---

**Q12.** A multi-agent document processing system has a coordinator and specialist subagents for legal, financial, and technical content. A document contains all three content types. The coordinator assigns the whole document to each subagent. What is the architectural problem?

A. Subagents should not process the same document simultaneously  
B. The coordinator is not decomposing the task — it is delegating the whole task to three specialists instead of assigning each relevant section  
C. The subagents will produce conflicting outputs  
D. The coordinator should process financial content itself  

**Answer: B.** Effective decomposition segments the work. Giving the full document to each specialist is duplication, not orchestration.

---

**Q13.** A `PostToolUse` hook is implemented to block certain responses from reaching the coordinator. Compared to a system prompt instruction to "ignore tool results that contain X," what is the key advantage of the hook?

A. Hooks are faster than prompt processing  
B. Hooks fire programmatically on every tool result, regardless of model behaviour  
C. Hooks can access external systems  
D. Hooks produce better error messages  

**Answer: B.** The prompt instruction depends on the model reading and following it. The hook fires unconditionally.

---

**Q14.** A coordinator successfully decomposes a research task into five subtasks and dispatches five subagents. Four subagents return results. One times out. What should the coordinator do?

A. Fail the entire task and restart  
B. Return results from the four successful subagents and discard the gap  
C. Annotate the gap, include partial results in the synthesis, and flag the missing domain to the output  
D. Retry the failed subagent indefinitely until it completes  

**Answer: C.** Coordinator owns error handling. Partial results with annotated gaps are more useful than a full restart or silent omission.

---

**Q15.** An agent is checking its own task completion by asking itself "Have I completed everything requested?" and using the response to determine whether to continue. What is wrong with this approach?

A. The agent is wasting tokens on self-assessment  
B. The agent's self-assessment cannot be trusted — it will tend to confirm completion even when work remains  
C. The check introduces a loop that could run indefinitely  
D. The approach is valid for simple tasks but not complex ones  

**Answer: B.** Model self-assessment of task completion is unreliable. Use programmatic stop reasons and explicit completion criteria, not model self-reporting.

---

*Domain 1 Practice · CCA-F Study Repository · ZenCloud Global Consultants · May 2026*
