# Continuous Learning & Session Memory

> Strategies for learning from sessions, maintaining cross-session knowledge, and building project intelligence over time.

---

## Session Continuity Protocol

### Start of Every Session
```
1. Read tasks/lessons.md       → apply past learnings
2. Read tasks/todo.md          → pick up where you left off
3. git log --oneline -10       → understand recent changes
4. Scan CLAUDE.md              → refresh on project rules
5. Check for applicable skills → invoke with even 1% relevance
```

### End of Every Session
```
1. Update tasks/todo.md        → mark progress, note next steps
2. Update tasks/lessons.md     → record any new insights
3. Commit all work             → nothing left uncommitted
4. Leave breadcrumbs           → brief note on state for next session
```

---

## The Lessons Log (tasks/lessons.md)

Record insights immediately after any:
- Correction from the user
- Failed attempt (what didn't work and why)
- Surprising behavior
- New pattern discovered
- Approach that worked especially well

### Format
```
| Date | What Happened | Rule to Follow |
|------|--------------|----------------|
| 2025-03-15 | Used mock DB when real DB was available | Prefer real implementations; mock only external services |
| 2025-03-16 | Spent 30min on what was a typo | Read error messages completely before investigating |
```

---

## Instinct-Based Learning (Advanced)

When you notice a repeated pattern, record it as an instinct:

### Instinct Template
```yaml
trigger: "when [specific situation]"
confidence: 0.7    # 0.3=tentative, 0.5-0.7=reliable, 0.9=certain
scope: project     # or "global"
action: "[what to do in this situation]"
evidence: "[what observations led to this]"
```

### Confidence Rules
- **0.3** — Seen once, tentative pattern
- **0.5** — Seen 2-3 times, likely real
- **0.7** — Seen consistently, reliable pattern
- **0.9** — Confirmed by user feedback, near-certain

### Scope Rules
- **Project-scoped:** React patterns stay in React projects, Django in Django
- **Global:** Universal patterns (always validate input, always read before editing)
- **Promotion:** When a project pattern appears in 2+ projects → promote to global

---

## Design Docs as Persistent Memory

Every significant design decision → a spec in `docs/specs/`:

```
docs/specs/YYYY-MM-DD-auth-design.md
docs/specs/YYYY-MM-DD-payment-integration.md
docs/specs/YYYY-MM-DD-caching-strategy.md
```

These persist across sessions. Any new session can read them to understand past decisions.

---

## Plans Track Progress

Implementation plans in `docs/plans/`:

```
docs/plans/YYYY-MM-DD-auth-implementation.md
docs/plans/YYYY-MM-DD-payment-implementation.md
```

Tasks within plans are checked off as completed. Next session picks up where the last one stopped.

---

## Git History as Memory

```bash
# What happened recently?
git log --oneline -20

# What changed in a specific area?
git log --oneline -- src/auth/

# What was the last thing worked on?
git log -1 --format="%s%n%b"

# Full context of a recent change
git show HEAD
```

---

## File-Based Working Memory

For complex tasks, use the filesystem as persistent memory (survives context resets):

### Three-File System
```
tasks/task_plan.md  → Phases with status (pending/in_progress/complete)
tasks/findings.md   → Research discoveries + technical decisions with rationale
tasks/progress.md   → Session log + actions taken + lessons
```

See `skills/task-planning.md` for the full discipline.

### The 2-Action Rule
> After every 2 search/read/browse operations → SAVE key findings to `tasks/findings.md`

This prevents knowledge loss when:
- Context window fills up
- You run `/clear` or `/compact`
- Session ends unexpectedly
- You switch to a different area of the codebase

### Automatic Session Recovery
When resuming work on an existing task:
1. Check `tasks/task_plan.md` → find latest in_progress phase
2. Read `tasks/progress.md` → see last session's final state
3. Read `tasks/findings.md` → load all previous research (don't re-research!)
4. Check `git log --oneline -10` → see committed changes
5. Resume from exact point where last session ended

---

## Session State File (For Context Compaction)

Before compacting or ending a complex session, save state:

```markdown
# Session State: [Feature Name]
**Date:** YYYY-MM-DD
**Branch:** feat/feature-name

## Completed
- [x] Created user model and migration
- [x] Added login endpoint with tests

## In Progress
- [ ] Token refresh endpoint (50% done — tests written, implementation started)

## Key Decisions Made
- Using JWT over sessions because [reason]
- Redis for token blacklist because [reason]

## Files Modified
- src/auth/login.ts (new)
- src/auth/login.test.ts (new)
- src/models/user.ts (modified)

## What Remains
- Token refresh endpoint completion
- Auth middleware for protected routes
- Integration tests for full auth flow

## Gotchas Discovered
- Express 5 changed error handling middleware signature
- JWT library expects seconds, not milliseconds for expiry
```

Save to `tasks/session-state.md`. Next session loads this to rebuild context.

---

## Cross-Session Search

If using persistent memory tools (claude-mem or similar):
1. **Search** past work: find relevant observations from previous sessions
2. **Timeline** view: see context around past decisions
3. **Fetch** details: retrieve specific observations that matter now
4. Use progressive disclosure: search → timeline → fetch (10x cheaper than reading full files)
