# CCA-F ↔ GH-600 Crosswalk
### Concept mapping between both certifications

---

## Shared concepts — learn once, apply twice

| Concept | CCA-F location | GH-600 location | Transfer notes |
|---------|---------------|-----------------|----------------|
| Agentic loop and stop reasons | D1 · Agent Architecture | D1 · Agent & SDLC | Identical concept. GH-600 surface: Copilot agent mode. |
| Context isolation between agents | D1 · Subagent isolation | D5 · Multi-Agent | Identical. GH-600: each trigger provides isolated context. |
| Tool descriptions as routing | D4 · Tool Design | D2 · Tool Use | Identical. GH-600: applies to MCP server tools. |
| Structured error responses | D4 · Tool Design | D2 · Tool Use | Identical pattern. GH-600: MCP tool errors. |
| Secrets via env var substitution | D4 · MCP config | D2 · MCP config | Identical principle. CCA-F: `${SECRET}`. GH-600: `${{ secrets.SECRET }}`. |
| Shared vs personal config | D2 · CLAUDE.md hierarchy | D2 · MCP config + D1 · custom instructions | Same principle, different files. |
| Progressive summarisation trap | D5 · Context | D3 · Memory & State | Identical. GH-600 fix: use PR/issue as persistent context. |
| Lost-in-the-middle | D5 · Context | D3 · Memory & State | Identical. |
| Pre-execution hooks | D1 · Hooks | D6 · Guardrails | Concept identical. GH-600 surface: branch protection rules. |
| Reliable vs unreliable escalation | D5 · Reliability | D6 · Guardrails | Identical. Both flag sentiment/confidence as unreliable. |
| Retry logic for missing data | D3 · Prompt Engineering | D4 · Evaluation | Identical. Retry against absent data = fabrication. |
| Least privilege | D4 · MCP | D2 · Tool Use | Identical principle. GH-600: GitHub permissions model. |
| HITL for high-consequence actions | D1 · Hooks | D6 · Guardrails | Identical. GH-600: branch protection + required reviewers. |
| Audit trails | Not directly tested | D6 · Guardrails | GH-600-specific. GitHub native audit log. |
| CI/CD integration | D2 · Claude Code | D1 · Agent & SDLC | Concept shared. CCA-F: `-p` flag. GH-600: Actions workflows. |

---

## GH-600-specific concepts (not in CCA-F)

| Concept | Domain | Notes |
|---------|--------|-------|
| `.github/copilot-instructions.md` | D1 | GitHub-specific custom instructions file |
| Copilot setup steps | D1 | Pre-execution environment configuration |
| GitHub Actions runner ephemerality | D3 | Runners are clean-slate per run |
| Workflow artifacts as state | D3 | GitHub-specific persistence mechanism |
| PR/issue as structured context | D3 | GitHub-specific durable context pattern |
| CODEOWNERS as HITL | D6 | GitHub-specific programmatic reviewer assignment |
| Branch protection as hard block | D6 | GitHub-specific enforcement mechanism |
| GitHub scan types (CodeQL, secret, dependency) | D4 | GitHub-specific evaluation artifacts |
| `.github/mcp.json` | D2 | GitHub-specific MCP configuration file |
| `${{ secrets.NAME }}` syntax | D2 | GitHub Actions secrets syntax (vs `${ENV_VAR}` in CCA-F) |
| Environment protection rules | D6 | GitHub-specific deployment gate |

---

## CCA-F-specific concepts (not in GH-600)

| Concept | Domain | Notes |
|---------|--------|-------|
| `stop_reason: tool_use / end_turn` | D1 | Anthropic API-specific signal |
| `CLAUDE.md` hierarchy (user/project/directory) | D2 | Claude Code-specific configuration |
| `-p` flag for non-interactive mode | D2 | Claude Code-specific CI/CD flag |
| `context: fork` in skill YAML | D2 | Claude Code-specific skill isolation |
| JSON schema `tool_use` structured output | D3 | Anthropic API-specific |
| Batch API (50% cost, 24h window) | D3 | Anthropic-specific API feature |
| `PreToolUse` / `PostToolUse` hooks | D1, D4 | Claude Code-specific hook system |
| MCP transport: stdio vs SSE | D4 | MCP implementation detail |
| Anthropic Fellows Program | Resources | Anthropic-specific professional development |

---

## Study sequence for both certifications

**Option A: CCA-F first**
1. `core/Agentic-Concepts.md` + `core/Mental-Models.md`
2. Full CCA-F study track
3. GH-600 domain breakdowns — focusing only on GitHub-specific concepts (the table above)
4. GH-600 Quick Reference Card + practice questions

**Option B: GH-600 first (beta window)**
1. `core/Agentic-Concepts.md` + `core/Mental-Models.md`
2. GH-600 domain breakdowns
3. GH-600 Quick Reference Card
4. Sit GH-600 beta (before May 31, 2026 for 80% discount)
5. Full CCA-F track — noting what transfers from GH-600

**Option C: Parallel (most efficient)**
1. `core/` files once
2. Study shared concepts once (use CCA-F breakdowns as primary — more detailed)
3. Use GH-600 breakdowns only for GitHub-specific surface
4. Two Quick Reference Cards — one per exam

---

*Phil Myint · ZenCloud Global Consultants · May 2026*
