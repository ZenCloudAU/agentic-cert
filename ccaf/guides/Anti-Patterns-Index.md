# Anti-Patterns Index
### Every trap the CCA-F exam uses · Catalogued by domain

---

The exam does not test what correct looks like. It tests whether you recognise what failure looks like — and whether you pick the fix that addresses the root cause rather than the one that adds infrastructure over a broken design.

Every entry below is a pattern the exam uses to generate wrong-but-plausible answers.

---

## D1 — Agent Architecture Anti-Patterns

### AP-D1-01: Text-based completion detection
**Pattern:** Checking completion by parsing assistant message content ("if 'task complete' in response")  
**Why wrong:** Text parsing is fragile. Stop reasons are programmatic signals designed for this.  
**Correct:** Check `stop_reason`. `end_turn` = done. `tool_use` = continue loop.

### AP-D1-02: Arbitrary iteration limits
**Pattern:** "Stop the agent after 10 tool calls regardless of state"  
**Why wrong:** Cuts off legitimate multi-step work. Produces incomplete outputs without error signals.  
**Correct:** Monitor stop reasons. Implement token budgets or time limits as soft guardrails, not hard stops.

### AP-D1-03: Coordinator scope truncation
**Pattern:** Coordinator decomposes a broad question into a narrower subtopic set — outputs are complete and correct but wrong in scope  
**Why wrong:** The coordinator's decomposition defined the wrong scope. Subagents did exactly what they were told.  
**Correct:** Root cause is decomposition logic, not subagent execution or synthesis.

### AP-D1-04: Assuming subagent context inheritance
**Pattern:** Coordinator knows something. Subagent is expected to know it without being told.  
**Why wrong:** Subagents run in isolated context. They receive only what the coordinator packs into the Task.  
**Correct:** Every piece of information a subagent needs must be explicitly included in the Task prompt.

### AP-D1-05: Prompt-based enforcement for material consequences
**Pattern:** "Don't process refunds above £500" as a system prompt instruction  
**Why wrong:** Prompt instructions are best-effort. They work most of the time.  
**Correct:** Use `PreToolUse` hook to programmatically block the call. Hooks fire every time.

---

## D2 — Claude Code Anti-Patterns

### AP-D2-01: Personal config masquerading as team config
**Pattern:** Conventions added to `~/.claude/CLAUDE.md` instead of `.claude/CLAUDE.md`  
**Why wrong:** User-level config is not shared. New developers have no access to it.  
**Correct:** Team conventions go in `.claude/CLAUDE.md` — project-level, version-controlled, shared.

### AP-D2-02: Interactive mode in CI/CD
**Pattern:** Running Claude Code in a pipeline without `-p` flag  
**Why wrong:** Claude Code waits for interactive input. Pipeline job hangs indefinitely.  
**Correct:** `-p` flag. Non-interactive mode. Process, print, exit.

### AP-D2-03: Wrong flags for non-interactive mode
**Pattern:** `CLAUDE_HEADLESS=true`, `--batch`, stdin from `/dev/null`  
**Why wrong:** None of these are the correct solution. `CLAUDE_HEADLESS=true` is not a real flag.  
**Correct:** `-p`

### AP-D2-04: Review in generation session
**Pattern:** Using the same Claude instance that wrote code to review it  
**Why wrong:** Generation context causes the model to rationalise its own decisions.  
**Correct:** Fresh session for review. No generation context.

### AP-D2-05: Verbose skill output in main session
**Pattern:** Running analysis skills without `context: fork`  
**Why wrong:** Large output eats main session context window.  
**Correct:** `context: fork` in skill YAML frontmatter. Output stays in isolated subagent.

---

## D3 — Prompt Engineering Anti-Patterns

### AP-D3-01: Schema as accuracy guarantee
**Pattern:** "JSON schema ensures the response is correct"  
**Why wrong:** Schema guarantees syntax, not semantics. `stated_total` can be wrong. `satisfaction_score` can be invented.  
**Correct:** Schema ensures structure. Validation logic ensures accuracy.

### AP-D3-02: Required fields for conditional data
**Pattern:** `"satisfaction_score"` marked required when it may not exist in source data  
**Why wrong:** Required field + no source = model invents a value to satisfy the schema.  
**Correct:** Use `"type": ["string", "null"]` for fields that may be absent. Remove from required.

### AP-D3-03: Closed enums for open categories
**Pattern:** `"enum": ["positive", "negative", "neutral"]` — no escape valve  
**Why wrong:** Forces the model into a category that doesn't fit. Produces systematic misclassification.  
**Correct:** Add `"unclear"` and `"other"`. Let the model signal uncertainty.

### AP-D3-04: Retry loop for missing data
**Pattern:** Building a retry-with-feedback loop when information is simply not in the source document  
**Why wrong:** Retrying against missing data produces fabricated data.  
**Correct:** Retry only when the data exists and was mis-extracted. Not as a general error pattern.

### AP-D3-05: Batch API for blocking tasks
**Pattern:** Moving a pre-merge check to the Batch API to save costs  
**Why wrong:** Batch API has a 24-hour window. A developer cannot wait 24 hours for a merge gate.  
**Correct:** Batch for overnight reports, bulk processing. Synchronous for blocking tasks.

---

## D4 — Tool Design Anti-Patterns

### AP-D4-01: Minimal, non-differentiating tool descriptions
**Pattern:** `get_customer — Gets customer information` / `get_order — Gets order details`  
**Why wrong:** Nearly identical descriptions. Model cannot distinguish when to call which.  
**Correct:** Expand descriptions with input format, examples, and explicit cross-references ("Do not use for orders — use get_order instead").

### AP-D4-02: Routing classifier before fixing descriptions
**Pattern:** Adding a routing classifier to fix a tool selection problem  
**Why wrong:** Adds infrastructure over a broken foundation.  
**Correct:** Fix the descriptions first. Root cause before overlay.

### AP-D4-03: Generic error strings
**Pattern:** Returning `"Operation failed"` from an MCP tool  
**Why wrong:** Coordinator cannot determine failure type, retryability, or recovery path.  
**Correct:** Structured error with `errorCategory`, `isRetryable`, `attemptedQuery`, `partialResults`.

### AP-D4-04: Committed secrets in MCP config
**Pattern:** `"token": "ghp_xxxxxxxxxxxx"` in `.mcp.json`  
**Why wrong:** Version-controlled file. Secret is now in every developer's repo and git history.  
**Correct:** `"token": "${GITHUB_TOKEN}"` — environment variable substitution.

### AP-D4-05: Personal MCP in project config
**Pattern:** Adding a personal/experimental MCP server to `.mcp.json`  
**Why wrong:** `.mcp.json` is shared. Every team member gets a server intended for personal use.  
**Correct:** Personal MCP goes in `~/.claude.json`.

---

## D5 — Context Anti-Patterns

### AP-D5-01: Summarising transactional values
**Pattern:** Including order amounts, IDs, and dates in summarisable conversation history  
**Why wrong:** Summarisation turns £4,832.17 into "approximately £5,000" and drops IDs entirely.  
**Correct:** Persistent structured block outside summarisation scope.

### AP-D5-02: Burying critical information in long context
**Pattern:** 75K-token context with key findings at token 40K  
**Why wrong:** Lost-in-the-middle effect. Models reliably process start and end, not middle.  
**Correct:** Key findings summary at the top. Section headings as navigation anchors.

### AP-D5-03: Sentiment-based escalation
**Pattern:** Escalate when customer sentiment score drops below 0.3  
**Why wrong:** Model-dependent, inconsistent, and gameable.  
**Correct:** Explicit customer request for human, policy gap, or no progress after N attempts.

### AP-D5-04: Confidence-score escalation
**Pattern:** Escalate when model self-rates confidence below 70%  
**Why wrong:** Models cannot reliably assess their own uncertainty.  
**Correct:** Same as above — programmatic, observable triggers.

### AP-D5-05: Unfiltered tool output accumulation
**Pattern:** Returning full 40-field tool response to coordinator when 5 fields are needed  
**Why wrong:** Accumulates across tool calls. Context bloat crowds out actionable information.  
**Correct:** `PostToolUse` hook trims result before model sees it.

---

## Master anti-pattern principle

> Every wrong answer in the CCA-F is an engineering decision a reasonable person would make. The right answer is the one that fixes the root cause — not the one that adds a layer on top of a flawed design.

---

*Phil Myint · ZenCloud Global Consultants · May 2026*
