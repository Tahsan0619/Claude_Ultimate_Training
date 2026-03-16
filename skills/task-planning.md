# Skill: File-Based Task Planning

> **Trigger:** Complex tasks (5+ steps), multi-session work, anything requiring persistent context.
> **Purpose:** Use the filesystem as working memory — survive context resets, session ends, and compactions.

---

## Why Files > Context Window

```
Context window = RAM → volatile, 200K tokens, fills up
Filesystem      = Disk → persistent, unlimited, always accessible

→ Important information goes to DISK
→ Claude reads from disk before major decisions
→ Sessions can restart without losing progress
```

---

## The Three-File System

Create in `tasks/` at project root:

### 1. `tasks/task_plan.md` — The Plan (Phases + Status)
```markdown
# Task Plan: [Feature Name]
**Goal:** [One sentence]
**Created:** YYYY-MM-DD

## Phase 1: [Name]
**Status:** pending | in_progress | complete
**Files:** [list]
**Steps:**
1. [Step with verification]
2. [Step with verification]
**Decisions:** [Any choices made during this phase]
**Errors:** [Any issues encountered]

## Phase 2: [Name]
**Status:** pending
...
```

### 2. `tasks/findings.md` — Research & Discoveries
```markdown
# Findings: [Feature Name]

## Technical Decisions
| Decision | Rationale | Source |
|----------|-----------|--------|
| JWT over sessions | Stateless, scales horizontally | RFC 7519 |
| Redis for blacklist | In-memory speed for token checks | benchmarks |

## Key Discoveries
- Express 5 changed middleware signature (breaking change)
- The auth module uses dependency injection — extend, don't modify
- Rate limiter already exists at src/middleware/rate-limit.ts

## Code Patterns Found
[Paste relevant code snippets found during research]
```

### 3. `tasks/progress.md` — Session Log
```markdown
# Progress: [Feature Name]

## Session 1 (YYYY-MM-DD)
- [x] Phase 1 complete: user model + migration
- [x] Phase 2 complete: login endpoint + tests
- [ ] Phase 3 started: token refresh (50% done)

### What Changed
- Created: src/auth/login.ts, src/auth/login.test.ts
- Modified: src/models/user.ts
- Tests: 12 passing, 0 failing

### Lessons This Session
- JWT library expects seconds not milliseconds for expiry
- Must run migration before tests in CI

## Session 2 (YYYY-MM-DD)
...
```

---

## The 2-Action Rule

> After every 2 search/read/browse operations → SAVE key findings to `tasks/findings.md`

This prevents knowledge loss when:
- Context window fills up
- You run `/clear` or `/compact`
- Session ends unexpectedly
- You switch branches or tasks

```
BAD:  Read 10 files → forget first 5 findings
GOOD: Read 2 files → save findings → read 2 more → save findings
```

---

## Landing the Plane (Session Completion)

> MANDATORY before ending any work session or pushing to remote.

```
1. File issues/TODOs for any remaining work
2. Run quality gates: lint → test → typecheck
3. Update task_plan.md phases with current status
4. Update progress.md with session summary
5. git pull --rebase && git push
6. Verify git status shows clean working tree
7. Leave breadcrumb: what to do first in next session
```

**Cannot skip.** Orphaned local work = invisible work = wasted work.

---

## Automatic Session Recovery

When starting a new session on an existing task:

```
1. Check if tasks/task_plan.md exists
   → Yes: Read it, find latest in_progress phase
   → No: Start fresh

2. Check tasks/progress.md
   → Find last session's final entry
   → Read what was completed, in-progress, remaining

3. Check tasks/findings.md
   → Load all technical decisions (don't re-research)
   → Load discovered patterns (don't re-discover)

4. Check git log --oneline -10
   → See what was committed since last session

5. Resume from the exact point where last session ended
```

---

## When to Use This vs. Simple todo.md

| Scenario | Use |
|----------|-----|
| Quick bug fix (1-3 steps) | `tasks/todo.md` |
| Single-session feature | `tasks/todo.md` |
| Multi-session feature | Three-file system |
| Complex refactor (5+ files) | Three-file system |
| Research-heavy task | Three-file system (findings.md is critical) |
| Multi-agent coordination | Three-file system (agents read/write to shared files) |

---

## Integration with Agents

When using subagents with file-based planning:

```
Coordinator reads task_plan.md → knows current state
Coordinator dispatches Agent A (with phase context from plan)
Agent A completes → writes results to findings.md or progress.md
Coordinator reads results → updates task_plan.md status
Coordinator dispatches Agent B (next phase)
```

Agents communicate through FILES, not conversation. This survives context resets.

---

## Complexity Assessment

Before writing a plan, score the task (1-10):

| Score | Complexity | Approach |
|-------|-----------|----------|
| 1-3 | Simple | `tasks/todo.md` checklist |
| 4-6 | Medium | Three-file system, 2-4 phases |
| 7-8 | Complex | Three-file system, subagent decomposition, 5+ phases |
| 9-10 | Very Complex | Architecture review first, then plan, then decompose to sub-plans |

**Factors that increase complexity:**
- Number of files to modify (5+ = complex, 10+ = very complex)
- Cross-module dependencies
- Unfamiliar codebase (first time touching this area)
- Security or data integrity implications
- Multiple integration points (APIs, databases, queues)
