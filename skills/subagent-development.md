# Skill: Subagent-Driven Development

> **Purpose:** Execute plans by dispatching focused subagents with two-stage review.
> **Trigger:** When executing plans from `writing-plans.md`, especially for multi-task features.

---

## Why Subagents

- **Isolated context** — each agent gets only what it needs, no pollution
- **Coordinator stays clean** — main agent preserves context for orchestration
- **Parallel work** — independent tasks can run simultaneously
- **Quality gates** — two-stage review catches issues early

---

## Core Pattern

```
For each task in the plan:
    1. Dispatch IMPLEMENTER subagent
       → Full task context (don't assume it knows anything)
       → File contents it needs (provide them, don't say "go read X")
       → Clear success criteria
    
    2. Implementer does the work
       → Writes tests first (TDD)
       → Implements minimum code
       → Runs tests
       → Self-reviews
       → Commits
    
    3. Dispatch SPEC REVIEWER subagent
       → "Does this match the requirements?"
       → Skeptical verification against the spec
       → If issues → implementer fixes → reviewer re-checks
    
    4. Dispatch CODE QUALITY REVIEWER subagent
       → Only AFTER spec compliance is confirmed
       → Style, patterns, edge cases, performance
       → If issues → implementer fixes → reviewer re-checks
    
    5. Mark task complete, move to next
```

---

## Implementer Dispatch Template

```
You are implementing Task [N] of [Feature Name].

## Context
[Paste the full task from the plan — every line, every file path, every code sample]

## Project Context
[Key files content that the implementer needs — don't reference, paste them]

## Rules
1. Follow TDD: write failing test → implement → verify
2. Make ONLY the changes described in the task
3. Run ALL tests after implementation, not just your new one
4. Commit with message: [specific commit message]
5. Report back: DONE, DONE_WITH_CONCERNS, NEEDS_CONTEXT, or BLOCKED

## Success Criteria
[Paste from the plan — specific verification commands and expected output]
```

---

## Handling Implementer Status

| Status | Action |
|--------|--------|
| **DONE** | Proceed to spec review |
| **DONE_WITH_CONCERNS** | Read concerns. Fix if correctness-related. |
| **NEEDS_CONTEXT** | Provide the missing context, re-dispatch |
| **BLOCKED** | Assess: context problem? Wrong model? Task too large? Plan wrong? |

---

## Spec Reviewer Dispatch Template

```
You are reviewing Task [N] for SPEC COMPLIANCE only.

## Requirements
[Paste the original requirements/spec for this task]

## What Was Implemented
[Describe what the implementer did]

## Your Job
- Does the implementation match the requirements? (not more, not less)
- Are there requirements that were missed?
- Are there behaviors added that weren't specified? (over-building)
- Is the verification passing for the right reasons?

Report: APPROVED or ISSUES_FOUND with specific items
```

---

## Code Quality Reviewer Dispatch Template

```
You are reviewing Task [N] for CODE QUALITY only.
Spec compliance has already been verified ✅.

## Code Changes
[Provide diff or changed files]

## Your Job
- Code style matches project conventions
- Error handling is appropriate (not excessive)
- No dead code, no commented-out code
- Tests are meaningful (not testing mocks)
- Performance is reasonable
- Names are clear and descriptive

Report: APPROVED or ISSUES_FOUND with specific items
```

---

## Model Selection Guide

| Task Type | Model | Rationale |
|-----------|-------|-----------|
| Simple file search, exploration | Haiku (fast/cheap) | Low complexity, high volume |
| Mechanical implementation (clear spec) | Haiku/Sonnet | Well-defined, no ambiguity |
| Multi-file implementation | Sonnet (balanced) | Coordination needed |
| Code review, test writing | Sonnet | Requires understanding |
| Architecture, complex debugging | Opus (deep) | Multi-system reasoning |
| Security audit, novel algorithms | Opus | Can't afford to miss anything |

### Model Upgrade Triggers
- First attempt failed → try one tier higher
- Task spans 5+ files → Sonnet minimum
- Security-critical code → Opus always
- User explicitly asks for thoroughness → Opus

---

## Iterative Retrieval in Subagents

When a subagent needs to discover its own context:

```
Phase 1: DISPATCH → Broad search with initial keywords
Phase 2: EVALUATE → Score relevance of each result (0-1)
Phase 3: REFINE → Narrow search based on gaps
Phase 4: LOOP → Repeat if avg relevance < 0.8 (max 3 cycles)
```

Add to subagent dispatch prompt:
```
You may need to search for context. Use this approach:
1. Start with broad searches for the relevant module
2. Score each result's relevance (0-1)
3. Follow the most relevant leads deeper
4. Stop after 3 search cycles maximum
5. Report what you found and what gaps remain
```

---

## Parallel Agent Dispatch

Use when facing **3+ independent failures** in different subsystems:

1. Identify independent problem domains
2. Create focused agent tasks (one per domain)
3. Dispatch in parallel
4. Review and integrate results

**Do NOT parallelize** when:
- Failures might be related (fix one → fix others)
- Tasks share state or files
- You're still in exploratory debugging
