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

---

## The Code Archaeologist (Before Modifying Unfamiliar Code)

Before changing code you don't fully understand, do a read-only exploration:

1. **Map the architecture** — draw the component structure (ASCII/Mermaid)
2. **Trace data flow** — follow data from input to output
3. **Identify dependencies** — what depends on this code?
4. **Check test coverage** — what's tested, what's not?
5. **Find patterns** — what conventions does this codebase use?

### Output Template
```markdown
## Code Archaeology Report: [module]
**Architecture:** [diagram or description]
**Data Flow:** [input → processing → output]
**Dependencies:** [what uses this, what this uses]
**Test Coverage:** [covered / not covered areas]
**Risk Areas:** [fragile code, missing tests, tight coupling]
**Recommendation:** [approach for the planned change]
```

---

## The Linus Principle — Data Structures Over Conditionals

When reviewing code, apply these questions:

1. **"Is this a real problem or made-up?"** — reject over-design
2. **"Is there a simpler way?"** — always seek the simplest solution
3. **"Will this break anything?"** — backward compatibility is sacred
4. **"Are there special cases?"** — special cases = design flaws; fix with better data structures
5. **"Can you reduce nesting?"** — if >3 levels of indentation, redesign

> "Bad programmers worry about code. Good programmers worry about data structures."

---

## Multi-Perspective Review

For critical features, run parallel reviews from different angles:

| Perspective | Questions |
|------------|-----------|
| **Developer** | Is this clean, maintainable, idiomatic? |
| **Security** | Any OWASP vulnerabilities? Data exposure? |
| **Performance** | N+1 queries? Memory leaks? Unnecessary re-renders? |
| **QA** | Edge cases covered? Error paths handled? |
| **UX** | Is the user experience logical and accessible? |
| **DevOps** | Will this deploy cleanly? Monitoring in place? |

### Severity Output Format

```
🔴 CRITICAL — [issue] → Must fix before merge
🟡 IMPORTANT — [issue] → Should fix before proceeding  
🟢 SUGGESTION — [issue] → Nice to have, note for later
```
