# CLAUDE.md — Master Instructions

> Drop this entire folder into any project root. Claude reads CLAUDE.md first, then follows links to specialized docs.

---

## SESSION START PROTOCOL

1. Read `tasks/lessons.md` — apply all lessons before touching anything
2. Read `tasks/todo.md` — understand current state
3. If neither exists, create them before starting
4. Scan for applicable skills in `skills/` — invoke any with even 1% relevance
5. Check `docs/coding-standards.md` for project conventions

---

## INSTRUCTION PRIORITY (highest → lowest)

1. **User's explicit instructions** (direct requests, project CLAUDE.md overrides)
2. **This instruction set** (skills, workflows, standards)
3. **Default system prompt behavior**

---

## CORE PRINCIPLES

| Principle | Rule |
|-----------|------|
| **Simplicity First** | Touch minimal code. No over-engineering. |
| **No Laziness** | Root causes only. No temp fixes. No band-aids. |
| **Never Assume** | Verify paths, APIs, variables, state before using. |
| **Evidence Over Claims** | Never say "should work" — run it and prove it. |
| **Ask Once** | One question upfront if unclear, never interrupt mid-task. |
| **Honesty Is Core** | Never claim success without verification. |
| **Demand Elegance** | For non-trivial changes: is there a more elegant solution? |

---

## WORKFLOW — THE 5-PHASE CYCLE

> Every non-trivial task follows this cycle. Skip nothing.

### Phase 1: RESEARCH
- Understand the full scope before writing a single line
- Read related files, docs, tests, recent commits
- Invoke `skills/brainstorming.md` for creative/design work
- Ask clarifying questions (one batch, not one-at-a-time drip)

### Phase 2: PLAN
- Enter plan mode for any task with 3+ steps
- Write the plan to `tasks/todo.md` before implementing
- Break into bite-sized steps (2-5 min each, one action per step)
- Each step must specify: files to touch, exact changes, verification command
- See `skills/writing-plans.md` for the full planning discipline

### Phase 3: EXECUTE
- Follow the plan step-by-step — no freestyling
- Write failing test FIRST → implement → verify (TDD cycle)
- Commit after each meaningful step
- If something goes wrong: STOP and re-plan, never push through
- See `skills/test-driven-development.md`

### Phase 4: VERIFY
- Run the full test suite — not just "the test I wrote"
- Check exit codes, read full output, count failures
- Never mark complete without fresh verification evidence
- See `skills/verification.md`

### Phase 5: LEARN
- After any correction: update `tasks/lessons.md`
- Format: `[date] | what went wrong | rule to prevent it`
- Review lessons at every session start

---

## SUBAGENT STRATEGY

- Use subagents to keep main context clean
- One focused task per subagent
- Provide full context in the dispatch (don't assume subagent knows anything)
- Throw more compute at hard problems — dispatch parallel agents for independent failures
- Two-stage review: spec compliance FIRST, then code quality
- See `skills/subagent-development.md`

---

## TASK MANAGEMENT

| Step | Action |
|------|--------|
| **Plan** | Write to `tasks/todo.md` |
| **Verify** | Confirm approach before implementing |
| **Track** | Mark steps complete as you go |
| **Explain** | High-level summary each step |
| **Learn** | Write to `tasks/lessons.md` after corrections |

---

## WHAT TO DO WHEN STUCK

```
1. STOP — don't try one more thing
2. Re-read the error message COMPLETELY
3. Check tasks/lessons.md for similar past issues
4. Use skills/debugging.md systematic process
5. If 3+ attempts failed: question the architecture, not the fix
6. Ask the user — but bring your analysis, not just the error
```

---

## LINKED DOCS (read on demand, not all at once)

| File | When to Read |
|------|-------------|
| `skills/brainstorming.md` | Starting any new feature or creative work |
| `skills/writing-plans.md` | Before implementing anything with 3+ steps |
| `skills/test-driven-development.md` | Before writing any production code |
| `skills/debugging.md` | When any test fails or bug appears |
| `skills/verification.md` | Before claiming anything is "done" |
| `skills/subagent-development.md` | When dispatching work to subagents |
| `skills/code-review.md` | After completing a feature or PR |
| `skills/git-workflow.md` | Branch management and merging |
| `docs/coding-standards.md` | Code style, architecture, conventions |
| `docs/ui-ux-guidelines.md` | Any frontend/UI work |
| `docs/security-checklist.md` | Before deploying or handling user data |
| `docs/performance.md` | Optimization work or performance issues |
| `docs/accessibility.md` | Any user-facing UI work |

---

## QUALITY STANDARD

> Ask: "Would a staff engineer approve this?"

- Every function has a test
- Every test was seen failing before passing
- No dead code, no commented-out code
- Error handling at system boundaries only (not defensive paranoia)
- Names are clear — no abbreviations, no `temp`, no `data2`
- Git history tells a story — atomic commits with clear messages
