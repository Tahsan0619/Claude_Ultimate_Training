# Copilot Instructions

> **MANDATORY.** Copilot MUST follow these rules on EVERY conversation in this workspace. No exceptions.

---

## BEFORE YOU DO ANYTHING — READ THESE FILES

**On every conversation, your FIRST action MUST be reading these two files:**

1. **Read `tasks/todo.md`** — check what work is pending, in-progress, or done
2. **Read `tasks/lessons.md`** — check past mistakes so you don't repeat them

If these files don't exist, create them using the templates at the bottom of this file. Do NOT proceed to the user's request until you have read both files.

---

## AFTER YOU FINISH ANY WORK — UPDATE THESE FILES

**On every conversation where you made changes, your LAST actions MUST be:**

1. **Update `tasks/todo.md`** — mark completed tasks `[x]`, add new tasks `[ ]`, update statuses
2. **Update `tasks/lessons.md`** — add a row if you hit any error, made a correction, or learned something

**Format for todo.md updates:**
```markdown
## [Task Name]
**Status:** done
**Completed:** [today's date]

### What was done
- [Specific changes made]
```

**Format for lessons.md updates:**
```markdown
| [today's date] | [What went wrong or what was learned] | [Rule to follow next time] |
```

**This is NOT optional.** Do not end the conversation without updating both files if any work was done.

---

## AGENT SYSTEM

This workspace has specialized agents in the `agents/` folder. Each `.agent.md` file contains deep instructions for a specific role.

### How agents work in practice

VS Code Copilot runs **one agent at a time** per conversation. The agent named `@coordinator` is designed to handle full sessions by executing work in **phases** (not by "dispatching" separate agents — that doesn't work in Copilot).

When `@coordinator` is invoked, it:
1. Reads `tasks/todo.md` and `tasks/lessons.md`
2. Plans the work
3. Executes ALL phases itself: design → code → test → verify → update memory
4. Updates `tasks/todo.md` and `tasks/lessons.md` before finishing

When a specific agent like `@security-auditor` or `@builder` is invoked directly, that agent:
1. Reads `tasks/todo.md` and `tasks/lessons.md`
2. Does its specialized work
3. Updates `tasks/todo.md` and `tasks/lessons.md` before finishing

### Available Agents

| Call With | File | Use For |
|-----------|------|---------|
| `@coordinator` | `agents/coordinator.agent.md` | Full sessions — plans and executes everything |
| `@architect` | `agents/architect.agent.md` | System design, specs, file maps only |
| `@builder` | `agents/builder.agent.md` | Writing code with TDD |
| `@verifier` | `agents/verifier.agent.md` | Running tests + 6-lens code review |
| `@memorykeeper` | `agents/memorykeeper.agent.md` | Session wrap-up, git commits |
| `@prompt-engineer` | `agents/prompt-engineer.agent.md` | AI/LLM prompt writing |
| `@security-auditor` | `agents/security-auditor.agent.md` | OWASP Top 10 security audit |
| `@uiux-specialist` | `agents/uiux-specialist.agent.md` | Design system + accessibility |
| `@test-runner` | `agents/test-runner.agent.md` | Agent validation |
| `@agent-auditor` | `agents/agent-auditor.agent.md` | Agent conflict detection |

---

## MASTER INSTRUCTIONS

Read `CLAUDE.md` for extended engineering principles. Key rules enforced on every conversation:

1. **TDD is mandatory** — write failing tests first, then implement
2. **Never commit secrets, credentials, or `.env` files**
3. **Never ask the user questions** — make reasonable assumptions and proceed
4. **Always run tests before claiming done** — prove it works, don't just say it works
5. **Atomic commits** — one logical change per commit, clear message

---

## SKILLS & DOCS (load on demand)

- `skills/` — 11 workflow discipline files (TDD, debugging, verification, planning, etc.)
- `docs/` — 11 reference standard files (coding standards, security, accessibility, performance, etc.)

Load the relevant skill/doc file when the task falls in that domain. Don't load all of them.

---

## FILE TEMPLATES

### tasks/todo.md template
```markdown
# Tasks / Todo

> Current implementation plan. Updated as work progresses.

## Active

_No active tasks._
```

### tasks/lessons.md template
```markdown
# Lessons Learned

> Updated after every correction, failed attempt, or insight. Reviewed at every session start.

| Date | What Went Wrong | Rule to Prevent It |
|------|----------------|-------------------|
```
