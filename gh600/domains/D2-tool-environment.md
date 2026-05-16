# GH-600 Domain 2: Tool Use & Environment Interaction
### Weight: 20–25% · Highest priority domain

---

## What this domain tests

Tool configuration, MCP server setup, permission scoping, and environment design for agents operating inside GitHub. The heaviest domain — and the one most directly mapped to production failures.

---

## MCP servers in GitHub Copilot

MCP (Model Context Protocol) servers extend what Copilot agents can do. They expose tools, resources, and prompts that the agent can call.

### Configuration locations

| Location | Scope | Version-controlled |
|----------|-------|--------------------|
| `.github/mcp.json` | Repository-wide, shared | Yes |
| User settings in Copilot UI | Personal, not shared | No |

### Secrets and credentials

Never commit credentials. Use GitHub secrets with environment variable substitution:

```json
{
  "mcpServers": {
    "my-api": {
      "url": "https://api.example.com/mcp",
      "token": "${{ secrets.API_TOKEN }}"
    }
  }
}
```

**Exam pattern:** MCP token hardcoded in `.github/mcp.json` → wrong. Secret reference via `${{ secrets.TOKEN }}` → correct.

---

## Tool permissions and scoping

Agents should have the minimum permissions required for their task. Over-permissioned agents are a production risk.

### Permission principles

| Principle | Application |
|-----------|------------|
| Least privilege | Grant only the permissions the agent's task requires |
| Scope to context | Repository-scoped permissions, not organisation-wide, unless required |
| Separate read from write | Agents that only need to read should not have write permissions |
| Time-bound where possible | Permissions for one-off tasks should not persist indefinitely |

**Exam scenario:** An agent for PR code review has write access to the repository's main branch. What is the problem? Write access to main is not required for code review — and creates unnecessary risk. Scope to PR-only write permissions or read-only.

---

## Environment interaction

Agents in GitHub interact with the environment through:

| Interaction type | Examples | Risk level |
|---|---|---|
| File read | Inspect code, read docs | Low |
| File write | Edit code, update docs | Medium |
| Terminal execution | Run tests, build, lint | Medium–High |
| External API calls (via MCP) | Create issues, post Slack messages, update databases | High |
| Repository operations | Merge PRs, push to protected branches | High |

**Architecture decision:** Each interaction type requires explicit permission grant. Higher risk interactions should require human confirmation.

---

## Tool descriptions in GitHub context

The same principle from CCA-F D4 applies: the agent uses tool descriptions to decide which tool to call. Ambiguous descriptions produce routing errors.

**GitHub-specific:** Copilot's built-in tools (file read/write, search, terminal) have fixed descriptions. MCP server tools have descriptions you write. These are your routing mechanism.

**Exam pattern:** If a Copilot agent is calling the wrong MCP tool — the fix is to improve the MCP tool description, not to add a routing layer.

---

## Structured error handling for MCP tools

Same pattern as CCA-F D4. Generic errors prevent agent recovery. Structured errors enable it.

| Error type | Required fields |
|---|---|
| Rate limit | `retryable: true`, `retryAfterSeconds` |
| Not found | `retryable: false`, `attemptedQuery` |
| Permission denied | `retryable: false`, `requiredPermission` |
| Service unavailable | `retryable: true`, fallback guidance |

---

## Key distinctions for D2

| Concept | Correct understanding |
|---|---|
| `.github/mcp.json` | Repository-wide MCP config. Version-controlled. Team-shared. |
| User MCP settings | Personal only. Not shared. Not version-controlled. |
| Secrets in MCP config | `${{ secrets.NAME }}` substitution. Never committed values. |
| Least privilege | Minimum permissions for the task. Not organisation-wide defaults. |
| Tool descriptions | Routing logic. Ambiguous = routing errors. Fix descriptions first. |

---

*Domain 2 of 6 · GH-600 Study Repository · ZenCloud Global Consultants · May 2026*
