# Skill: Code Review

> **Trigger:** After completing a task, feature, or before merge/PR.
> **Core Principle:** Review early, review often. Don't save it for the end.

---

## When to Request Review

- After each completed task (in subagent-driven workflow)
- After a major feature implementation
- Before merging to main branch
- After any "this was tricky" implementation

---

## Requesting Review (as author)

Provide the reviewer with:

```markdown
## Review Request

**What was implemented:** [1-2 sentence summary]
**Plan/Requirements:** [link or paste the relevant spec]
**Files changed:** [list of changed files]
**How to verify:** [specific commands to run]
**Areas of concern:** [anything you're unsure about]
```

---

## Receiving Review (responding to feedback)

### The Process

1. **READ** all feedback completely without reacting
2. **UNDERSTAND** each item — restate in your own words (or ask for clarification)
3. **VERIFY** against the codebase — is the reviewer's concern valid?
4. **EVALUATE** — is this technically sound for THIS project?
5. **RESPOND** technically (not emotionally)
6. **IMPLEMENT** one item at a time, test after each

### Forbidden Responses
- "You're absolutely right!"
- "Great point!"
- "Let me implement that now" (before understanding)

### Correct Responses
- "Fixed. [one-line description]"
- "Good catch — fixed in [file:line]."
- Push back with reasoning when the reviewer is wrong

### When to Push Back

- Suggestion breaks existing functionality
- Reviewer lacks full context
- Violates YAGNI (building unused features)
- Technically incorrect for this stack
- Conflicts with user's architectural decisions

---

## Issue Severity

| Level | Action |
|-------|--------|
| **Critical** | Must fix immediately. Blocks merge. |
| **Important** | Should fix before proceeding. |
| **Suggestion** | Nice to have. Note for later. |

---

## Self-Review Checklist (Before Requesting External Review)

- [ ] All tests pass (ran the full suite)
- [ ] No debug code left (console.log, print, TODO hacks)
- [ ] No commented-out code
- [ ] Variable/function names are clear
- [ ] Error handling exists at system boundaries
- [ ] No hardcoded secrets, URLs, or credentials
- [ ] Changes match the spec — nothing more, nothing less
- [ ] Git diff makes sense as a coherent change
