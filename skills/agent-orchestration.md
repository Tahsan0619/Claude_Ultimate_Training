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

## Fresh Context Per Agent (Critical Pattern)

> Each spawned agent gets a **clean 200K token window**. This eliminates context rot.

```
Orchestrator (thin):
  1. Load minimal context from files
  2. Spawn Agent A with ONLY what it needs
  3. Agent A works in clean context → writes results to disk
  4. Orchestrator reads results from disk
  5. Spawn Agent B with Agent A's results
  
NEVER pass conversation history to subagents.
NEVER let the orchestrator accumulate work output.
Agents read from disk → work → write to disk.
```

### The Thin Orchestrator Principle
The coordinator NEVER does heavy lifting:
- Loads state from files (`tasks/task_plan.md`, `tasks/findings.md`)
- Dispatches agents with tight, focused prompts
- Collects results from disk artifacts
- Updates state files
- Dispatches next agent

If the orchestrator is writing code, something is wrong.

---

## Agent Role Registry

Specialized agents outperform generic ones. Name your agents by purpose:

| Agent | Purpose | Model |
|-------|---------|-------|
| **Archaeologist** | Map unfamiliar code before modifying | Haiku/Sonnet |
| **Architect** | Design systems, make structural decisions | Opus |
| **Implementer** | Write code following a spec | Sonnet |
| **Debugger** | Systematic root cause analysis | Sonnet/Opus |
| **Reviewer** | Code quality + spec compliance | Sonnet |
| **Security Auditor** | OWASP, vulnerability scanning | Opus |
| **Researcher** | Explore docs, APIs, patterns | Haiku |
| **Test Writer** | Write comprehensive test suites | Sonnet |
| **Optimizer** | Performance profiling + fixes | Sonnet |
| **Doc Writer** | Documentation generation | Haiku |

Each agent has different system prompts, risk tolerance, and model requirements.

---

## Phase Decomposition Pattern

Break complex tasks into sequential agent phases:

```
Phase 1: Archaeologist → map the codebase
   → outputs: architecture.md, dependency-graph.md

Phase 2: Architect → design the solution
   → reads: architecture.md
   → outputs: design-spec.md

Phase 3: Implementer(s) → build it (can parallelize per module)
   → reads: design-spec.md
   → outputs: code + tests

Phase 4: Reviewer → verify quality
   → reads: design-spec.md + code changes
   → outputs: review-report.md

Phase 5: Security Auditor → check for vulnerabilities
   → reads: code changes
   → outputs: security-report.md
```

Each phase gets a dedicated agent. Output flows to next phase via files.

---

## Async Mailbox Coordination

For multi-agent teams that need to communicate:

```
tasks/mailbox.jsonl  (append-only, one JSON object per line)

{"from": "researcher", "to": "planner", "type": "finding", "data": {...}}
{"from": "planner", "to": "implementer", "type": "task", "data": {...}}
{"from": "implementer", "to": "reviewer", "type": "ready", "data": {...}}
```

Rules:
- **Append-only** — never modify existing entries
- **One message per line** — easy to parse, grep, filter
- **Typed messages** — finding, task, ready, blocked, done
- **No imperative message passing** — just write to file, others read

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
