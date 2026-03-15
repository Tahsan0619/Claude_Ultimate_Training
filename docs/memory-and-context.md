# Memory & Context Management

> Strategies for maintaining context across sessions and managing long-running projects.

---

## Session Continuity Pattern

### Start of Every Session
```
1. Read tasks/lessons.md → apply past learnings
2. Read tasks/todo.md → pick up where you left off  
3. Check git log --oneline -10 → understand recent changes
4. Scan CLAUDE.md → refresh on project rules
```

### End of Every Session
```
1. Update tasks/todo.md → mark progress, note next steps
2. Update tasks/lessons.md → record any new insights
3. Commit all work → nothing left uncommitted
4. Leave a breadcrumb → brief note on what still needs to be done
```

---

## Progressive Disclosure for Context

Structure project knowledge in layers to minimize token usage:

### Layer 1: CLAUDE.md (always loaded)
- Core rules and principles (~200 words)
- Links to detail files
- Build/test commands

### Layer 2: Skill Files (loaded on demand)
- Read when a specific workflow applies
- Self-contained instructions
- Don't need CLAUDE.md context to work

### Layer 3: Doc Files (loaded on demand)
- Deep reference material
- Standards, checklists, guidelines
- Read only when doing that type of work

---

## Task Management with Memory

### tasks/todo.md — What to Do
```markdown
## [Feature Name]
**Status:** in-progress
**Branch:** feat/user-auth
**Context:** Implementing JWT auth per design spec in docs/specs/2025-01-15-auth-design.md

### Completed
- [x] Created user model and migration
- [x] Added login endpoint with tests

### Next
- [ ] Add token refresh endpoint
- [ ] Add middleware for protected routes
- [ ] Integration test the full auth flow
```

### tasks/lessons.md — What I Learned
```markdown
| Date | What Went Wrong | Rule |
|------|----------------|------|
| 2025-01-15 | Forgot to run full test suite after refactor | Always run ALL tests, not just the file I changed |
| 2025-01-16 | Used mock for DB where real DB was available | Prefer real implementations; mock only external services |
| 2025-01-17 | Spent 30min debugging a typo | Read error messages completely before investigating |
```

---

## Multi-Session Project Patterns

### Design Docs Persist Knowledge
Every significant decision → a design doc in `docs/specs/`:
```
docs/specs/2025-01-15-auth-design.md
docs/specs/2025-01-20-payment-integration.md
docs/specs/2025-02-01-caching-strategy.md
```
These are permanent project memory — any new session can read them.

### Plans Track Progress
Implementation plans in `docs/plans/`:
```
docs/plans/2025-01-15-auth-implementation.md
docs/plans/2025-01-20-payment-implementation.md
```
Tasks within plans are checked off as completed.

### Git History Is Memory
```bash
# What happened recently?
git log --oneline -20

# What changed in a specific area?
git log --oneline -- src/auth/

# What was the last thing worked on?
git log -1 --format="%s%n%b"
```

---

## Cross-Session Search (with claude-mem)

If using the claude-mem plugin:
1. **Search** past work: find relevant observations from previous sessions
2. **Timeline** view: see context around past decisions
3. **Fetch** details: retrieve specific observations that matter now

Token-efficient pattern: search → timeline → fetch (10x cheaper than reading full files)
