# Skill: Writing Plans

> **Trigger:** After a design is approved, before any implementation begins.
> **Purpose:** Break an approved spec into bite-sized, executable tasks.

---

## When to Use

- After brainstorming/design approval
- Any implementation with 3+ steps
- When coordinating work across multiple files
- Before dispatching work to subagents

## The Rules

1. **Fresh context for each reader** — the plan must work for someone with zero codebase knowledge
2. **One action per step** — write test, run it, implement, verify, commit
3. **Exact verification** — specific commands with expected output, not "make sure it works"
4. **Complete code** — not "add validation here" but the actual code to write
5. **File map first** — list all files and their responsibilities BEFORE defining tasks

---

## Plan Document Structure

```markdown
# [Feature Name] Implementation Plan

> For agentic workers: Use subagent-driven-development or executing-plans skill

**Goal:** [One sentence]
**Architecture:** [2-3 sentences]
**Tech Stack:** [Key technologies]
**Design Spec:** [Link to design doc]

---

## File Map

| File | Responsibility | New/Modified |
|------|---------------|-------------|
| `src/auth/login.ts` | Login handler | Modified |
| `src/auth/login.test.ts` | Login tests | New |
| ... | ... | ... |

---

## Tasks

### Task 1: [Short description]
**Files:** `src/auth/login.test.ts`
**Steps:**
1. Create test file with failing test for [specific behavior]
2. Run: `npm test -- login.test.ts` → expect 1 failure
3. Implement minimum code in `src/auth/login.ts` to pass
4. Run: `npm test -- login.test.ts` → expect 0 failures
5. Run: `npm test` → expect all green
6. Commit: `feat(auth): add login handler with tests`

### Task 2: [Short description]
...
```

---

## Scope Check

Before writing the plan, ask:
- Does this spec cover multiple independent subsystems?
- If yes → break into separate plans (one per subsystem)
- Each plan should be independently implementable and testable

## Task Granularity Guide

| Too Big | Just Right | Too Small |
|---------|-----------|-----------|
| "Implement the auth system" | "Add login endpoint with JWT" | "Import the jwt library" |
| "Build the UI" | "Create login form component" | "Add a div" |
| "Set up database" | "Create users migration + model" | "Add one column" |

---

## After Writing

- Review the plan critically — would a new developer understand every step?
- Check: does every task have a verification command?
- Check: are tasks in dependency order? (no step references something not yet built)
- Save to `docs/plans/YYYY-MM-DD-<feature-name>.md`
- Begin execution with `test-driven-development.md` discipline

---

## Task Complexity Assessment

Before writing a plan, assess complexity (1-10 scale):

| Score | Complexity | Recommended Approach |
|-------|-----------|---------------------|
| 1-3 | Simple | `tasks/todo.md` checklist, no formal plan |
| 4-6 | Medium | This planning skill, 2-4 phases |
| 7-8 | Complex | File-based planning (`skills/task-planning.md`), subagent decomposition |
| 9-10 | Very Complex | Architecture review first, then plan, then decompose to sub-plans |

### Factors That Increase Complexity
- **Files to modify:** 5+ = complex, 10+ = very complex
- **Cross-module dependencies:** changes in module A affect module B
- **Unfamiliar codebase:** first time touching this area
- **Security implications:** auth, payments, PII handling
- **Multiple integration points:** APIs, databases, queues, external services
- **State management complexity:** shared mutable state, race conditions

### Recommended Subtask Count
| Complexity | Subtasks |
|-----------|----------|
| 1-3 | 1-3 tasks, inline |
| 4-6 | 4-8 tasks, formal plan |
| 7-8 | 8-15 tasks, file-based, subagents |
| 9-10 | 15+ tasks across sub-plans |

### Verification Density (Nyquist Principle)
Test coverage per phase must match estimated requirements:
- **Critical phases** (auth, payments): 1 verification per step
- **Standard phases** (CRUD, UI): 1 verification per 2-3 steps
- **Low-risk phases** (docs, formatting): 1 verification at phase end

If test coverage is below required density, the plan has verification gaps.
