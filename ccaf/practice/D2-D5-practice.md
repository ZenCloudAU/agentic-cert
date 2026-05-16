# Domain 2–5 Practice Questions
### Claude Code · Prompt Engineering · Tool Design · Context & Reliability

---

# Domain 2: Claude Code Configuration (12 questions)

**Q1.** A team of five developers shares a Claude Code configuration for code style conventions. A new developer joins and doesn't follow the conventions, despite them being documented. What is the most likely cause?

A. The conventions were documented in a README, not a CLAUDE.md file  
B. The conventions were added to a developer's `~/.claude/CLAUDE.md` instead of the project's `.claude/CLAUDE.md`  
C. The new developer's editor doesn't support CLAUDE.md  
D. The conventions need to be added to each developer's personal config manually  

**Answer: B.** User-level CLAUDE.md is personal and not shared. Team conventions must be in the project-level file which is version-controlled and shared with all developers.

---

**Q2.** A CI/CD pipeline step runs `claude "Review this PR for security issues"` and hangs indefinitely. What is the correct fix?

A. Set `CLAUDE_HEADLESS=true` environment variable  
B. Add `--batch` flag to the command  
C. Add `-p` flag to the command  
D. Redirect stdin from `/dev/null`  

**Answer: C.** `-p` enables non-interactive mode. Claude Code processes the prompt, prints to stdout, and exits. Without it, Claude Code waits for interactive input.

---

**Q3.** A SKILL.md file contains the following frontmatter. When does this skill load?

```yaml
---
paths:
  - "src/api/**/*"
---
```

A. For all files in the project  
B. Only for files inside `src/api/` and its subdirectories  
C. Only for files named `api.*`  
D. For all TypeScript files in the project  

**Answer: B.** The glob pattern `src/api/**/*` matches all files within `src/api/` and any subdirectory thereof.

---

**Q4.** A skill performs a comprehensive codebase dependency analysis that returns 15,000 words of output. This output is appearing in the main session context and crowding out subsequent work. What is the correct fix?

A. Summarise the skill output before returning it  
B. Add `context: fork` to the skill's YAML frontmatter  
C. Increase the context window allocation for the session  
D. Split the skill into smaller sub-skills  

**Answer: B.** `context: fork` runs the skill in an isolated subagent. The verbose output stays contained and does not consume the main session's context.

---

**Q5.** A developer wants to add an experimental MCP server for personal testing without affecting the team. Where does the configuration go?

A. `.mcp.json` in the project root  
B. `.claude/CLAUDE.md` in the project  
C. `~/.claude.json` in the developer's home directory  
D. `~/.claude/CLAUDE.md`  

**Answer: C.** Personal and experimental MCP servers go in `~/.claude.json`. This is not version-controlled and not shared.

---

**Q6.** A team MCP server provides access to the internal knowledge base. All developers need access. Where does the configuration go?

A. `~/.claude.json` for each developer  
B. `.mcp.json` in the project root  
C. `.claude/CLAUDE.md`  
D. The server must be globally installed on each developer's machine  

**Answer: B.** `.mcp.json` is the project-level MCP configuration. It is version-controlled and shared with all team members.

---

**Q7.** A developer needs to store a GitHub token for an MCP server. Which configuration is correct?

A. `"token": "ghp_xxxxxxxxxxxxxxxx"` directly in `.mcp.json`  
B. `"token": "${GITHUB_TOKEN}"` in `.mcp.json` with the actual token set as an environment variable  
C. The token should be stored in `.claude/CLAUDE.md` as a comment  
D. Tokens cannot be used with MCP servers  

**Answer: B.** Environment variable substitution keeps secrets out of version-controlled files.

---

**Q8.** A developer runs the same Claude Code instance that generated a module to perform code review on that module. What problem does this introduce?

A. The review will take longer than a fresh instance  
B. The model will rationalise its own generation decisions rather than providing objective review  
C. The review context will interfere with future generation tasks  
D. Claude Code cannot review code it has generated  

**Answer: B.** Generation context causes the model to defend its own choices. A fresh session without generation context provides more objective review.

---

**Q9.** Which CLAUDE.md file location is NOT version-controlled?

A. `.claude/CLAUDE.md`  
B. `src/.claude/CLAUDE.md`  
C. `~/.claude/CLAUDE.md`  
D. `.claude/rules/api-conventions.md`  

**Answer: C.** `~/.claude/CLAUDE.md` is in the user's home directory. It is personal, not in the project repository, and not version-controlled.

---

**Q10.** A pipeline needs Claude Code to process a list of 500 files and return a structured JSON report. The pipeline must complete within 5 minutes. Which flag enables this?

A. `--async`  
B. `--json`  
C. `-p`  
D. `--no-input`  

**Answer: C.** `-p` enables non-interactive mode. The pipeline won't hang waiting for user input.

---

**Q11.** A developer wants different style rules to apply when editing test files versus source files. What is the correct implementation?

A. Maintain two separate CLAUDE.md files and switch between them manually  
B. Add both rule sets to the project CLAUDE.md with comments marking which applies where  
C. Create two skill files in `.claude/rules/` with different `paths:` arrays in their YAML frontmatter  
D. Claude Code does not support path-conditional rules  

**Answer: C.** `.claude/rules/` with YAML frontmatter and `paths:` arrays enables path-conditional rule loading.

---

**Q12.** A new developer sets up their development environment and adds their personal editor preferences to `.claude/CLAUDE.md`. What is the problem?

A. Editor preferences are not supported in CLAUDE.md  
B. `.claude/CLAUDE.md` is version-controlled and shared — their personal preferences will affect all team members  
C. The preferences will be overridden by the user-level CLAUDE.md  
D. There is no problem  

**Answer: B.** Project-level CLAUDE.md is shared. Personal preferences belong in `~/.claude/CLAUDE.md`.

---

# Domain 3: Prompt Engineering & Structured Output (12 questions)

**Q1.** A JSON schema marks `customer_satisfaction_rating` as required. Source documents are customer emails that do not include satisfaction ratings. After deployment, all records show a satisfaction rating of 7/10. What is happening?

A. The model is defaulting to a neutral score  
B. The model is fabricating values to satisfy the required field constraint  
C. The schema validation is malfunctioning  
D. The source documents contain implicit satisfaction signals  

**Answer: B.** Required field + no source data = fabrication. Mark the field as nullable or remove it from required.

---

**Q2.** A classification system uses a schema with `"enum": ["complaint", "inquiry", "compliment"]`. Logs show that customers expressing neutral feedback or mixed messages are being classified as "complaint." What is the fix?

A. Add more examples of each category  
B. Add `"unclear"` and `"other"` to the enum values  
C. Remove the enum constraint and let the model classify freely  
D. Add a confidence score field  

**Answer: B.** Closed enums force the model into the nearest category. `"unclear"` and `"other"` provide escape valves for edge cases.

---

**Q3.** A document processing pipeline uses a retry-with-feedback loop to improve extraction quality. The loop runs 5 times and still returns incorrect values. What is the most likely cause?

A. The feedback instructions are not specific enough  
B. The required information is not present in the source document  
C. 5 iterations is insufficient — increase to 10  
D. The model needs a different temperature setting  

**Answer: B.** Retry loops work when data exists but was mis-extracted. If the information isn't in the document, retrying fabricates it.

---

**Q4.** An engineering manager proposes moving both a blocking PR review check and a nightly dependency report to the Batch API to reduce costs by 50%. What is the correct recommendation?

A. Move both — 50% savings is worth the latency trade-off  
B. Move neither — Batch API latency is incompatible with engineering workflows  
C. Move the nightly report; keep the PR review synchronous  
D. Move the PR review; the nightly report is already async  

**Answer: C.** The Batch API has a 24-hour window. A developer cannot wait up to 24 hours for a PR to merge. The nightly report has no blocking dependency on latency.

---

**Q5.** A model correctly structures all JSON output. An auditor finds that `invoice_total` values don't match the sum of `line_items` in 12% of records. What does this indicate?

A. The JSON schema has a bug  
B. The model is applying the schema incorrectly  
C. The schema guarantees syntax not semantic accuracy — mathematical verification requires additional validation logic  
D. The line_items field definition is ambiguous  

**Answer: C.** Schemas ensure valid JSON structure. They cannot enforce mathematical relationships between fields.

---

**Q6.** A few-shot prompt for ticket classification includes 5 examples, all from clear-cut cases. The system underperforms on ambiguous tickets. What should be changed?

A. Increase the number of examples to 20  
B. Replace the examples with more detailed rule descriptions  
C. Target the examples specifically at ambiguous cases — the ones where the model currently fails  
D. Add a confidence score to the output schema  

**Answer: C.** Few-shot examples should cover failure cases, not easy cases. The model already handles clear-cut tickets.

---

**Q7.** A chain-of-thought instruction says "Think step by step before answering." The model sometimes follows this and sometimes doesn't. What is a more reliable implementation?

A. Use a stronger instruction: "You MUST think step by step"  
B. Add the instruction to the first few-shot example  
C. Include a `"reasoning"` field in the JSON schema before the `"conclusion"` field — the model must populate it  
D. Use a different model  

**Answer: C.** Structuring the schema to require a `reasoning` field forces the model to produce intermediate reasoning before the conclusion field. This is more reliable than an instruction alone.

---

**Q8.** A schema defines `order_date` as `"type": "string"` and required. A document mentions "sometime last week." What value will the model produce?

A. null  
B. An error  
C. A specific date fabricated from context  
D. The string "sometime last week"  

**Answer: C.** Required string + vague source = model resolves the vagueness to produce a specific value. Use nullable type and remove from required when date may be imprecise.

---

**Q9.** A prompt describes five classification categories in detail. Accuracy on borderline cases is inconsistent. What is the most effective intervention?

A. Expand the descriptions further  
B. Add few-shot examples specifically targeting the borderline cases  
C. Reduce the number of categories  
D. Add a preamble explaining why accurate classification matters  

**Answer: B.** Descriptions create interpretation variance. Examples create pattern recognition. Borderline cases need examples, not longer descriptions.

---

**Q10.** The Batch API is best suited for which of the following use cases?

A. A real-time customer chat system  
B. A pre-deployment safety check that blocks releases  
C. Nightly generation of 10,000 product descriptions  
D. A fraud detection system requiring sub-second response  

**Answer: C.** The Batch API is designed for high-volume, non-time-sensitive work. 50% cost savings, up to 24-hour window.

---

**Q11.** A schema marks `refund_reason` as required with `"enum": ["defective", "wrong_item", "changed_mind", "other"]`. Customer submits: "I ordered this as a gift but the recipient already has one." Which value will the model select?

A. `"defective"` — closest category  
B. `"other"` — catch-all  
C. `"changed_mind"` — most semantically similar  
D. null  

**Answer: B.** This is why `"other"` is essential in enums. None of the specific categories fits. Without `"other"`, the model would be forced into the nearest category, producing a misclassification.

---

**Q12.** A developer wants to reduce JSON output errors. They are choosing between `tool_use` with a JSON schema and a prompt instruction to "output only valid JSON." Which is more reliable and why?

A. The prompt instruction — it's more flexible  
B. `tool_use` with a schema — it enforces structure at the API level, not at the instruction level  
C. Both are equally reliable  
D. Neither — always use a JSON validator  

**Answer: B.** `tool_use` with a schema is enforced by the API. A prompt instruction is best-effort.

---

# Domain 4: Tool Design & MCP Integration (10 questions)

**Q1.** Two tools are defined: `get_account(customer_id)` and `get_transaction(transaction_id)`. Both accept alphanumeric ID strings. Logs show `get_account` is called for transaction lookups 38% of the time. What is the first corrective action?

A. Add a routing classifier that inspects the ID format  
B. Merge the two tools into one with a type parameter  
C. Expand and differentiate the tool descriptions with input format examples and cross-references  
D. Add a pre-call validation step  

**Answer: C.** Root cause is description ambiguity. Fix it before adding infrastructure.

---

**Q2.** An MCP tool returns `"Database query failed"` when an error occurs. A coordinator receives this. What can the coordinator determine from this response?

A. The error type and whether to retry  
B. The attempted query for debugging  
C. Whether partial results are available  
D. Nothing actionable — the response contains no structured information  

**Answer: D.** A generic string gives the coordinator no basis for recovery decisions.

---

**Q3.** A production MCP server requires a database password. Where should this credential be stored?

A. Hardcoded in `.mcp.json`  
B. In a `.env` file committed to the repository  
C. As an environment variable referenced by `${DB_PASSWORD}` in the configuration  
D. In the server's CLAUDE.md  

**Answer: C.** Environment variable substitution keeps secrets out of version-controlled files.

---

**Q4.** An MCP tool returns 60 fields from a customer record. The coordinator only uses 8 of them. After 20 tool calls, the coordinator's context is overwhelmed. What is the correct fix?

A. Increase the context window  
B. Implement a `PostToolUse` hook that trims the response to the 8 needed fields before the coordinator sees it  
C. Split the tool into multiple smaller tools  
D. Ask the coordinator to ignore irrelevant fields  

**Answer: B.** `PostToolUse` hook trimming. Less noise, smaller footprint, same signal.

---

**Q5.** A developer is configuring a new MCP server for the whole team to use. Where does the server configuration go?

A. `~/.claude.json`  
B. `.claude/CLAUDE.md`  
C. `.mcp.json` in the project root  
D. `~/.claude/CLAUDE.md`  

**Answer: C.** Team-shared MCP servers go in `.mcp.json` — version-controlled and shared.

---

**Q6.** An MCP tool for processing refunds should return a structured error when the refund amount exceeds the allowable limit. Which response is most useful to a coordinator?

A. `"Refund denied"`  
B. `{"error": "limit_exceeded"}`  
C. `{"errorCategory": "policy_limit", "isRetryable": false, "attemptedAmount": 850.00, "maxAllowed": 500.00, "message": "Refund amount exceeds policy limit"}`  
D. `{"success": false}`  

**Answer: C.** Structured error with category, retryability, attempted values, and limit gives the coordinator everything it needs to decide the next step.

---

**Q7.** Two tools have identical descriptions except for their names. Which consequence is most likely?

A. The model will call the tools in alphabetical order  
B. The model will randomly select between them  
C. The model will systematically prefer one tool for both use cases based on name familiarity  
D. The model will request clarification before calling either  

**Answer: C.** With identical descriptions, the model uses other signals — including name familiarity and training distribution — to select. This produces systematic routing errors.

---

**Q8.** An MCP server is remote and uses SSE transport. Another MCP server is local and uses stdio. Which scenario is correct for each?

A. Remote/stdio, Local/SSE  
B. Remote/SSE, Local/stdio  
C. Both use stdio  
D. Both use SSE  

**Answer: B.** SSE (Server-Sent Events) is for remote MCP servers. stdio is for local processes.

---

**Q9.** A tool description currently reads: "Searches the product catalog." A coordinator calls it for both product searches and inventory checks, causing errors. What is the minimum effective improvement?

A. Rename the tool to `search_product_catalog`  
B. Add a parameter list to the description  
C. Expand the description to specify what the tool does, what inputs it accepts, what it does NOT do, and when to use it vs alternatives  
D. Add a second tool for inventory and point both descriptions at each other  

**Answer: C.** Full description expansion with scope, inputs, exclusions, and cross-references.

---

**Q10.** Which MCP primitive allows a model to call another model?

A. Tools  
B. Resources  
C. Prompts  
D. Sampling  

**Answer: D.** Sampling is the MCP primitive for model-calling-model patterns.

---

# Domain 5: Context Management & Reliability (8 questions)

**Q1.** A claims processing agent uses progressive summarisation across a multi-turn conversation. After 20 turns, the agent references the claim amount as "several thousand dollars" instead of the original £4,832.17. What happened?

A. The model lost access to the original value  
B. Progressive summarisation compressed the precise value into an approximation  
C. The schema is missing the amount field  
D. The model rounded the figure  

**Answer: B.** Progressive summarisation converts precise values to approximations. Transactional facts must be stored in persistent structured blocks outside the summarisation scope.

---

**Q2.** A document analysis system processes 80,000-token research papers. Key conclusions are located around token 50,000. The model consistently misses them. What is the architectural cause?

A. The model's context window is too small  
B. Lost-in-the-middle effect — models process the start and end of long contexts reliably; middle content is frequently missed  
C. The conclusions are not formatted correctly  
D. The model needs a different temperature setting  

**Answer: B.** Lost-in-the-middle. Move conclusions to the top of the document.

---

**Q3.** A customer support agent escalates when its sentiment analysis classifier scores below 0.4. An audit finds that frustrated customers with composed writing styles are not being escalated, while calm customers using emotionally charged language are. What is the root cause?

A. The sentiment threshold is too high  
B. The sentiment classifier is unreliable as an escalation trigger  
C. The model needs more training data  
D. The classifier should use a different NLP model  

**Answer: B.** Sentiment-based escalation is unreliable. Replace with explicit customer requests, policy gaps, or measurable inability to progress.

---

**Q4.** Which of the following is a reliable escalation trigger?

A. Customer message sentiment score below 0.3  
B. Model self-rated confidence below 65%  
C. Customer explicitly states "I want to speak to a human"  
D. Automatic frustration detection classifier fires  

**Answer: C.** Explicit customer request is a direct, programmatic, unambiguous signal. A, B, and D are all model-dependent or classifier-dependent.

---

**Q5.** A multi-turn invoice dispute conversation requires precise tracking of invoice numbers, amounts, and dates. What architectural pattern prevents these values from being degraded by summarisation?

A. Increase summarisation frequency to prevent context overflow  
B. Store transactional facts in a structured persistent block included in every prompt, separate from summarisable conversation history  
C. Use a longer context window to avoid summarisation  
D. Reference external storage for the values on each turn  

**Answer: B.** Persistent structured block. Values stay precise regardless of what gets compressed in the conversation history.

---

**Q6.** A coordinator receives tool results averaging 35 fields per call. It needs 6 fields per result. After 15 calls, context is saturated. What is the correct intervention?

A. Summarise tool results before adding to context  
B. Increase the context window allocation  
C. Implement a `PostToolUse` hook that trims results to the 6 needed fields before the coordinator sees them  
D. Reduce the number of tool calls  

**Answer: C.** `PostToolUse` hook. Trim before the model sees it. Reduces noise without losing needed signal.

---

**Q7.** An agent processes a 60,000-token legal brief. The summary and key holdings are placed at token 45,000. The model's analysis misses three of five key holdings. What change would most improve results?

A. Use a model with a larger context window  
B. Move the summary and key holdings to the beginning of the document  
C. Reduce the brief to under 32,000 tokens  
D. Add explicit section markers at token 45,000  

**Answer: B.** Lost-in-the-middle. Critical content must be at the top where processing is most reliable.

---

**Q8.** A Domain 5 failure causes incorrect values to be passed to a subagent's Task prompt. Which domain is experiencing a cascade failure?

A. Domain 2  
B. Domain 3  
C. Domain 1  
D. Domain 4  

**Answer: C.** Domain 1 (Agent Architecture). Subagent context isolation means subagents depend entirely on what the coordinator packs in the Task. If context degradation corrupts those values, the subagent operates on wrong data.

---

*Domains 2–5 Practice · CCA-F Study Repository · ZenCloud Global Consultants · May 2026*
