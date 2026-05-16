# Domain 2: Claude Code Configuration & Workflows
### Weight: 20% · Second priority

---

## What this domain tests

Whether you know where configuration files live, what scope they have, and how to run Claude Code in non-interactive environments. Configuration-heavy. Either you know where the files go or you don't.

---

## CLAUDE.md hierarchy

Three levels. Each with different scope and version-control behaviour.

| Level | Location | Scope | Version-controlled |
|-------|----------|-------|-------------------|
| User | `~/.claude/CLAUDE.md` | Personal — applies everywhere, only for you | No |
| Project | `.claude/CLAUDE.md` | Shared — applies to everyone on the project | Yes |
| Directory | `[directory]/.claude/CLAUDE.md` | Scoped to that directory tree | Yes |

### Critical rule

Personal conventions go in `~/.claude/CLAUDE.md`. Team conventions go in `.claude/CLAUDE.md`.

### Exam scenario (memorise this)

> A new developer joins the team and doesn't follow project coding conventions. The conventions are documented. What is the most likely cause?

**Answer:** Someone added the conventions to their own user-level CLAUDE.md (`~/.claude/CLAUDE.md`) instead of the project-level file (`.claude/CLAUDE.md`). The new developer has no access to another person's user config.

---

## Path-specific rules with `.claude/rules/`

Load rules conditionally based on file path using YAML frontmatter.

```yaml
---
paths:
  - "**/*.test.tsx"
---
Always use React Testing Library. Never use Enzyme.
```

```yaml
---
paths:
  - "src/api/**/*"
---
All API responses must include error codes and retry-after headers.
```

**Effect:** Rules load only when Claude Code operates on matching files. Context stays lean. Irrelevant conventions don't pollute every operation.

---

## The `-p` flag (CI/CD critical)

`-p` runs Claude Code **non-interactively**: process the prompt, print to stdout, exit.

```bash
claude -p "Run lint checks and report violations" 
```

**Without `-p`:** Claude Code waits for interactive input. A pipeline job hangs.

### Exam scenario (memorise this)

> A CI/CD pipeline job using Claude Code hangs indefinitely. Which flag fixes it?

Options:
- `CLAUDE_HEADLESS=true` — not a real flag
- Redirect stdin from `/dev/null` — a hack, not the solution
- `--batch` — not a valid flag
- **`-p`** — correct

**Answer: `-p`**. If you've run Claude Code in CI, free point. If not, it's a trap.

---

## Skill isolation with `context: fork`

Skills with `context: fork` in their YAML frontmatter run in an isolated subagent.

```yaml
---
context: fork
---
```

**Use case:** Verbose analysis tasks — dependency scans, coverage analysis, codebase maps. The output stays contained in the isolated subagent and does not eat the main session's context window.

**Rule:** Use `context: fork` for any skill that produces large, expensive output you don't need inline.

---

## Session context isolation for review

> The same Claude instance that generated code will rationalise its own decisions during review.

An independent instance — without the generation context — catches more issues. Code review in Claude Code should use a fresh session, not the generation session.

---

## Plan mode vs direct execution

| Mode | Behaviour | Use when |
|------|-----------|----------|
| Plan mode | Shows proposed changes before execution | High-stakes operations, unfamiliar codebases |
| Direct execution | Executes immediately | Routine tasks, trusted contexts |

---

## MCP in Claude Code

Project-level MCP servers → `.mcp.json` (version-controlled, shared)  
Personal/experimental MCP servers → `~/.claude.json` (not shared)  
Secrets → environment variable substitution: `${GITHUB_TOKEN}` (never committed values)

---

## Key distinctions for D2

| Concept | Correct understanding |
|---|---|
| User vs project CLAUDE.md | User = personal, not shared. Project = team, version-controlled. |
| `-p` flag | Non-interactive mode. Required for CI/CD. |
| `context: fork` | Isolates skill output from main session context. |
| Path-specific rules | YAML frontmatter with `paths:` array. Conditional loading. |
| Session isolation for review | Fresh instance catches more than generation instance. |

---

## Practice questions → [D2-practice.md](../practice-questions/D2-practice.md)

---

*Domain 2 of 5 · CCA-F Study Repository · ZenCloud Global Consultants · May 2026*
