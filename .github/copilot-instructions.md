# Copilot Instructions

> This project uses a 10-agent AI coding team. Copilot should follow these rules.

## Agent Discovery

All agents are defined in the `agents/` folder as `.agent.md` files with YAML frontmatter.
When the user calls `@coordinator`, `@builder`, `@security-auditor`, or any agent name — look in `agents/` for the matching `.agent.md` file and follow its full instructions.

## Available Agents

| Call With | File | Purpose |
|-----------|------|---------|
| `@coordinator` | `agents/coordinator.agent.md` | Session orchestrator — dispatches other agents |
| `@architect` | `agents/architect.agent.md` | System design, specs, file maps |
| `@builder` | `agents/builder.agent.md` | TDD implementation — all code writing |
| `@verifier` | `agents/verifier.agent.md` | Quality gate — tests + 6-lens code review |
| `@memorykeeper` | `agents/memorykeeper.agent.md` | Session wrap-up, git commits, lessons |
| `@prompt-engineer` | `agents/prompt-engineer.agent.md` | AI/LLM prompt writing and auditing |
| `@security-auditor` | `agents/security-auditor.agent.md` | OWASP Top 10 security audit |
| `@uiux-specialist` | `agents/uiux-specialist.agent.md` | Design system + accessibility |
| `@test-runner` | `agents/test-runner.agent.md` | Agent validation with synthetic tests |
| `@agent-auditor` | `agents/agent-auditor.agent.md` | Agent conflict/overlap detection |

## Master Instructions

Read `CLAUDE.md` at the start of every session — it contains the core engineering principles, workflow rules, and behavioral constraints that ALL agents follow.

## Session Memory

- `tasks/todo.md` — current task tracking (read at session start)
- `tasks/lessons.md` — past mistakes and learnings (read at session start, apply ALL lessons before working)

## Skills & Docs

- `skills/` — 11 workflow discipline files (brainstorming, TDD, debugging, verification, etc.)
- `docs/` — 11 reference standard files (coding standards, security, accessibility, performance, etc.)

Agents should load the relevant skill/doc files when their task falls within that domain.

## Key Rules

1. Every session starts by reading `tasks/todo.md` and `tasks/lessons.md`
2. Every session ends with MemoryKeeper printing `SESSION COMPLETE`
3. TDD is mandatory — write failing tests first, then implement
4. Never commit secrets, credentials, or `.env` files
5. Follow the 5-phase cycle: Research → Plan → Execute → Verify → Learn
