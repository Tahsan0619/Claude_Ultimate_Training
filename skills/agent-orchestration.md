# Skill: Agent Orchestration

> **Purpose:** Coordinate multiple specialized agents for complex tasks.
> **Trigger:** Multi-step features, large refactors, full-stack work, parallel investigations.

---

## When to Use

- Task spans 3+ files or concerns (frontend + backend + tests + docs)
- Bug affects multiple subsystems independently
- Feature needs architecture + implementation + review + security
- You want parallel progress on independent work streams

---

## The Orchestration Pattern

```
1. ANALYZE  → Detect stack, scope, and complexity
2. ROUTE    → Assign agents by specialization
3. DISPATCH → Send tasks with full context (parallel when independent)
4. REVIEW   → Two-stage: spec compliance, then code quality
5. INTEGRATE → Merge results, verify everything works together
```

---

## Agent Selection Rules

1. **Prefer specific over generic** — `react-specialist` > `frontend-developer`
2. **Match technology to framework** — Django agent for Django, not generic Python
3. **Use universal agents as fallback** only when no specialist exists
4. **Always include reviewers** — code-reviewer + security-reviewer minimum

### Agent Roles by Task Type

| Task | Agents to Dispatch |
|------|-------------------|
| **New Feature** | planner → architect → implementer → tdd-guide → code-reviewer |
| **Bug Fix** | debugger → tdd-guide → implementer → code-reviewer |
| **Full-Stack Feature** | architect → backend-dev → frontend-dev → api-designer → tester |
| **Performance** | profiler → optimizer → tester → reviewer |
| **Security Audit** | security-reviewer → penetration-tester → compliance-auditor |
| **Refactor** | code-archaeologist → architect → implementer → tester → reviewer |
| **API Design** | api-designer → backend-dev → doc-writer → tester |

---

## Stack Auto-Detection

Before dispatching, detect the stack from config files:

| File Found | Stack | Agents to Prefer |
|------------|-------|-----------------|
| `package.json` + React | React/Node | react-specialist, node-backend |
| `package.json` + Next.js | Next.js | nextjs-expert, react-specialist |
| `requirements.txt` / `pyproject.toml` | Python | python-specialist, django/fastapi |
| `go.mod` | Go | go-specialist, go-reviewer |
| `Cargo.toml` | Rust | rust-specialist |
| `Gemfile` | Ruby/Rails | rails-specialist |
| `composer.json` | PHP/Laravel | laravel-specialist |
| `pom.xml` / `build.gradle` | Java/Kotlin | java-specialist |
| `pubspec.yaml` | Flutter/Dart | flutter-specialist |
| `Package.swift` | Swift/iOS | swift-specialist |

---

## Dispatch Template (Full Context)

```markdown
## Agent: [role-name]
**Task:** [specific, scoped task description]

### Context You Need
[Paste ALL relevant file contents — don't say "read file X"]
[Include: architecture overview, related code, tests, constraints]

### Your Deliverables
1. [Specific output 1]
2. [Specific output 2]

### Rules
- Follow TDD (write failing test first)
- Make ONLY the changes described
- Run ALL tests after changes
- Commit with message: [specific message]

### Success Criteria
- [Verification command]: [expected output]

### Report Back As
DONE | DONE_WITH_CONCERNS | NEEDS_CONTEXT | BLOCKED
```

---

## Parallel Dispatch Rules

### DO Parallelize When:
- Tasks are in **different files/modules** with no shared state
- Tasks are **independent** (fixing bug A has no effect on bug B)
- Tasks are **read-only investigations** (exploring different areas)

### DO NOT Parallelize When:
- Tasks share state or modify the same files
- One task's output is another task's input
- You're still in exploratory/uncertain debugging phase
- Fixes might be related (fixing one could fix others)

### Max Parallelism
- **2 agents maximum** running simultaneously (more = coordination overhead)
- Sequential between dependent tasks
- Pattern: Parallel → Sync → Parallel → Sync

---

## Model Routing by Task Complexity

| Complexity | Model | Use For |
|------------|-------|---------|
| **Low** | Haiku | File exploration, simple edits, docs, search |
| **Medium** | Sonnet | Multi-file implementation, code review, testing |
| **High** | Opus | Architecture decisions, security analysis, complex debugging |

**Default:** Sonnet for 90% of tasks. Upgrade to Opus only when:
- First attempt failed and task requires deeper reasoning
- Task spans 5+ files with complex dependencies
- Security-critical code that can't afford missed vulnerabilities

---

## The Code Archaeologist Pattern

Before modifying unfamiliar code, dispatch a **read-only** exploration agent:

```markdown
## Agent: code-archaeologist
**Task:** Map and analyze [module/feature] before we modify it.

### Deliverables
1. Architecture overview (ASCII or Mermaid diagram)
2. Data flow: how data moves through the system
3. Dependency graph: what depends on what
4. Test coverage: what's tested, what's not
5. Risk areas: fragile code, missing tests, complex coupling
6. Recommended approach for [the planned change]
```

This prevents blind modifications and surfaces hidden dependencies.

---

## Multi-Agent Review Pattern

After completing a feature, dispatch parallel reviewers:

1. **Spec Reviewer** → "Does this match requirements exactly? Not more, not less?"
2. **Code Quality Reviewer** → "Is this clean, maintainable, idiomatic?"
3. **Security Reviewer** → "Any vulnerabilities? OWASP issues? Data exposure?"
4. **Performance Reviewer** → "Any N+1 queries? Unnecessary re-renders? Memory leaks?"

Each reviewer reports independently. Fix critical issues, then re-review.
