# GH-600 Domain 3: Memory, State & Execution
### Weight: 10–15%

---

## What this domain tests

How agents persist state, manage execution environments, and handle long-running or interrupted tasks inside GitHub's infrastructure.

---

## State persistence patterns

GitHub agents are session-bound by default. State does not persist automatically across sessions or workflow runs.

| State type | How to persist | Risk |
|---|---|---|
| Conversation context | GitHub Copilot session memory (limited) | Lost on session end |
| Task progress | GitHub Issues, PR comments, workflow artifacts | Durable |
| Structured data | External storage via MCP tools | Most reliable |
| Execution checkpoints | Workflow artifacts or step outputs | Durable across runs |

**Exam principle:** If a task may span multiple sessions or workflow runs, state must be written to durable storage explicitly. It cannot live only in the agent's context.

---

## Execution environments

| Environment | Use case | Key property |
|---|---|---|
| GitHub Actions runner | CI/CD tasks, automated workflows | Ephemeral — clean state per run |
| Codespaces | Interactive development | Persistent within a session |
| Local with Copilot | Developer machine | Developer-controlled environment |

**Exam scenario:** An agent that runs a multi-step data migration across multiple workflow runs stores progress in an Actions runner environment variable. The migration fails partway through and restarts from zero on retry. Root cause: runner is ephemeral — environment variables do not persist across runs. Fix: checkpoint progress to a workflow artifact or external storage.

---

## Context management in GitHub agents

Same principles as CCA-F D5 apply — GitHub surface:

- Long Copilot conversations degrade precision (progressive summarisation)
- Critical information placed late in a long context is missed (lost-in-the-middle)
- MCP tool outputs accumulate and crowd context — trim before returning

**GitHub-specific fix:** Use PR/issue descriptions and comments as structured persistent context. These are durable, readable, and version-controlled — more reliable than in-session memory.

---

## Key distinctions for D3

| Concept | Correct understanding |
|---|---|
| Actions runner state | Ephemeral. Does not persist across runs. |
| Workflow artifacts | Durable. Use for checkpointing between runs. |
| Session context | Lost on session end. Not a persistence mechanism. |
| PR/issue as state | Durable, structured, version-controlled. Use for task state. |

---

*Domain 3 of 6 · GH-600 Study Repository · ZenCloud Global Consultants · May 2026*

---
---

# GH-600 Domain 4: Evaluation, Error Analysis & Tuning
### Weight: 15–20%

---

## What this domain tests

How to assess whether agents are performing correctly, diagnose failures, and improve agent behaviour iteratively — using GitHub's scan and artifact infrastructure.

---

## Agent evaluation framework

Agent evaluation in GitHub is not a benchmark — it is ongoing monitoring across three layers:

| Layer | What to measure | How |
|---|---|---|
| Output quality | Does the agent produce correct, useful output? | Code review, PR feedback, acceptance rates |
| Tool usage | Are tools being called correctly and efficiently? | Logs, MCP server telemetry |
| Error patterns | What types of failures occur? How often? | Workflow logs, failure annotations |

---

## GitHub scans as evaluation input

GitHub's built-in scanning tools produce structured output agents can use for self-evaluation and tuning:

| Scan type | Output | Agent use |
|---|---|---|
| Code scanning (CodeQL) | Vulnerability alerts | Agent reviews, patches, or flags |
| Secret scanning | Detected secrets | Agent annotates, does not fix autonomously |
| Dependency scanning | Vulnerable dependencies | Agent proposes updates |
| Custom scan rules | Domain-specific issues | Agent applies team-defined quality rules |

**Exam principle:** Scan output is an evaluation artifact. An agent that ignores scan results is operating without feedback. An agent that acts on scan results without human review for high-severity findings is over-autonomous.

---

## Error diagnosis patterns

Same core pattern as CCA-F D3 retry logic — GitHub surface:

**Step 1:** Categorise the error
- Tool failure (MCP server error, permission denied)
- Model error (wrong output, missed instruction)
- Environment error (runner timeout, dependency missing)
- Data error (source does not contain required information)

**Step 2:** Apply the retry test
- Data exists, agent missed it → retry with improved context
- Data absent → retry fabricates, find the data or annotate the gap
- Tool error → retry if retryable, escalate if not
- Environment error → fix the environment, not the agent prompt

---

## Iterative tuning

The cycle for improving agent behaviour:

```
Observe failure → Categorise root cause → Apply minimum fix → Verify → Monitor
```

**Minimum fix principle:** Same as CCA-F. Fix the root cause. Do not add a layer over a broken foundation.

| Root cause | Minimum fix |
|---|---|
| Wrong tool called | Improve tool description |
| Agent ignores team conventions | Add to `.github/copilot-instructions.md` |
| Output format incorrect | Add format example to custom instructions |
| Agent over-writes when read sufficient | Reduce permissions scope |

---

## Key distinctions for D4

| Concept | Correct understanding |
|---|---|
| Scan output | Evaluation artifact. High-severity findings need human review. |
| Retry for missing data | Fabricates. Find the data or annotate the gap. |
| Iterative tuning | Root cause first. Minimum fix. Verify before moving on. |
| Ongoing monitoring | Not a one-time benchmark. Continuous across tool use, output, and error patterns. |

---

*Domain 4 of 6 · GH-600 Study Repository · ZenCloud Global Consultants · May 2026*

---
---

# GH-600 Domain 5: Multi-Agent Coordination
### Weight: 15–20%

---

## What this domain tests

Orchestrating multiple agents or Copilot instances to work on related tasks — with GitHub as the coordination layer.

---

## GitHub as coordinator

In the GH-600 context, the coordinator is often GitHub itself — the platform that triggers agents, routes tasks, and aggregates outputs via Issues, PRs, and Actions workflows.

```
GitHub (coordinator)
    │
    ├── Triggers Copilot agent A (issue triage)
    ├── Triggers Copilot agent B (PR review)
    └── Triggers Copilot agent C (documentation update)
         │
         Each agent:
         - Has isolated context (only what the trigger provides)
         - Produces output (PR comment, label, file change)
         - Does not share state with other agents
```

---

## Context isolation in GitHub multi-agent workflows

Each agent trigger provides only the context of that trigger:

| Trigger | Context the agent receives |
|---|---|
| Issue created | Issue title, body, labels, assignee |
| PR opened | PR title, description, changed files, base branch |
| Workflow dispatch | Workflow inputs, repository context |
| Comment on PR | Comment text, PR context, thread |

**Exam principle:** If Agent B needs information that Agent A produced, that information must be explicitly written somewhere Agent B can access — a PR comment, an issue update, a workflow artifact, or a shared MCP resource. It does not flow automatically.

---

## Parallel vs sequential dispatch

| Pattern | Use when | Risk |
|---|---|---|
| Parallel | Agents are independent — no dependencies | Output conflicts if both agents write to the same target |
| Sequential | Agent B depends on Agent A's output | Agent B must wait; requires orchestration logic |

**Exam scenario:** Two agents are dispatched in parallel. Both modify the same file. The second agent's changes overwrite the first's. Root cause: parallel dispatch of dependent agents without conflict resolution. Fix: sequence them or use branch isolation.

---

## Output conflict resolution

When multiple agents produce outputs that could conflict:

- **Branch isolation:** Each agent works on its own branch. A human or coordinator merges.
- **Locking:** One agent locks a resource while working on it.
- **Sequential dispatch:** Second agent runs only after first completes and its output is merged.

---

## Key distinctions for D5

| Concept | Correct understanding |
|---|---|
| GitHub as coordinator | Platform triggers, routes, and aggregates — agents have isolated context. |
| Context isolation | Each agent receives only its trigger context. Nothing flows between agents automatically. |
| Parallel dispatch risk | Output conflicts when agents write to the same target. |
| Sequential fix | Explicit ordering. Second agent receives first agent's output via durable storage. |

---

*Domain 5 of 6 · GH-600 Study Repository · ZenCloud Global Consultants · May 2026*

---
---

# GH-600 Domain 6: Guardrails & Accountability
### Weight: 10–15%

---

## What this domain tests

Human-in-the-loop design, audit trails, access controls, and safety scans — the governance layer that makes autonomous agents safe to run in production.

---

## Human-in-the-loop (HITL) in GitHub

GitHub has built-in HITL mechanisms that map to the enforcement spectrum from CCA-F:

| Mechanism | Enforcement strength | Use for |
|---|---|---|
| Branch protection rules | Hard block — cannot be bypassed | Protect main, require review |
| Required reviewers | Programmatic — fires every time | High-consequence changes |
| CODEOWNERS | Programmatic — auto-assigns review | Domain-specific review requirements |
| PR required before merge | Programmatic — fires every time | All production changes |
| Environment protection rules | Programmatic — gates deployment | Production deployments |
| Copilot custom instructions | Best-effort — works most of the time | Agent behaviour guidance |

**Exam principle:** Match enforcement strength to consequence. An agent that can merge to main without human review is an unacceptable production risk. Branch protection is the architectural control, not a Copilot instruction.

---

## Reliable escalation triggers (GitHub surface)

| Trigger | Reliable | Why |
|---|---|---|
| Review required by branch protection | Yes | Programmatic, fires every time |
| CODEOWNERS match requiring approval | Yes | Programmatic, fires every time |
| Security scan finding above severity threshold | Yes | Structured output, observable |
| Agent explicitly flags for human review | Yes | Direct, stated signal |
| Agent confidence score below threshold | No | Model-dependent, unreliable |
| Sentiment of PR comments | No | Model-dependent, unreliable |

---

## Audit trails

Every agent action in GitHub creates an audit trail:

- **GitHub audit log:** All Actions runs, permission changes, repository events
- **PR history:** All comments, reviews, changes — including agent-generated ones
- **Commit history:** Who made what change and when (including agent commits)
- **Workflow run logs:** Complete execution trace for every Actions run

**Exam principle:** An agent that takes consequential actions without producing an audit trail is unacceptable for enterprise deployment. GitHub's native audit infrastructure covers this if agents operate within the platform — not via external tools that bypass the audit log.

---

## Access control principles

| Principle | Application |
|---|---|
| Least privilege | Agents get minimum permissions for their task |
| Role separation | Different agents for different risk levels (read-only triage vs write-access PR review) |
| Time-bound tokens | Short-lived tokens for agent actions where possible |
| No standing admin access | Agents should not have organisation-admin permissions |

---

## Safety scans as guardrails

GitHub's scanning infrastructure can gate agent actions:

- Code scanning must pass before merge (branch protection rule)
- Secret scanning alerts block commits containing credentials
- Dependency scanning gates can prevent vulnerable dependencies from being introduced

**Exam principle:** Safety scans are programmatic guardrails when combined with branch protection. An agent cannot merge code that fails a required scan — no matter what the agent's instructions say.

---

## Key distinctions for D6

| Concept | Correct understanding |
|---|---|
| Branch protection | Hardest enforcement. Fires every time. Not bypassable by the agent. |
| CODEOWNERS | Programmatic HITL. Auto-assigns required reviewers. |
| Reliable escalation | Programmatic, observable signals — not sentiment or confidence. |
| Audit trail | GitHub's native logs cover agent actions if agents operate within the platform. |
| Least privilege | Per-task minimum. Not organisation-wide defaults. |

---

*Domain 6 of 6 · GH-600 Study Repository · ZenCloud Global Consultants · May 2026*
