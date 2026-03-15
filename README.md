# Ultimate Claude Prompt — Usage Guide

> Your complete instruction set for getting production-quality results from Claude on every project.

---

## What This Is

A synthesized, battle-tested collection of engineering best practices extracted from 4 top Claude Code repositories:

- **awesome-claude-code** — 300+ curated Claude Code resources, patterns, and real CLAUDE.md examples
- **superpowers** — 14 disciplined development skills (TDD, debugging, planning, subagents, reviews)
- **claude-mem** — Persistent memory and context management across sessions
- **ui-ux-pro-max** — 67 UI styles, 161 color palettes, 57 font pairings, 99 UX guidelines

---

## How to Use

### Option 1: Drop Into Any Project
Copy the entire `ultimate-claude-prompt/` folder into your project root. Claude will read `CLAUDE.md` automatically at session start and follow the linked skill/doc files on demand.

### Option 2: Cherry-Pick What You Need
Copy only specific files into your project:
- Just want TDD? → Copy `skills/test-driven-development.md`
- Just want UI rules? → Copy `docs/ui-ux-guidelines.md`
- Just want the workflow? → Copy `CLAUDE.md` + `tasks/`

### Option 3: Reference from Your Own CLAUDE.md
Add to your existing project's CLAUDE.md:
```markdown
## Workflow
Follow the development workflow in ultimate-claude-prompt/CLAUDE.md

## Skills
Use skills from ultimate-claude-prompt/skills/ when applicable
```

---

## Folder Structure

```
ultimate-claude-prompt/
├── CLAUDE.md                          ← START HERE — master instructions
├── README.md                          ← This file (usage guide)
│
├── skills/                            ← Workflow skills (read on demand)
│   ├── brainstorming.md               ← Design before building
│   ├── writing-plans.md               ← Break work into executable steps
│   ├── test-driven-development.md     ← Red-Green-Refactor discipline
│   ├── debugging.md                   ← Systematic root cause analysis
│   ├── verification.md               ← Prove it works before claiming done
│   ├── subagent-development.md        ← Dispatch and review subagent work
│   ├── code-review.md                ← Give and receive code feedback
│   └── git-workflow.md               ← Branches, commits, merging
│
├── docs/                              ← Reference guides (read when needed)
│   ├── coding-standards.md            ← Architecture, naming, style, testing
│   ├── ui-ux-guidelines.md            ← Typography, colors, components, responsive
│   ├── accessibility.md              ← WCAG, keyboard nav, screen readers, ARIA
│   ├── security-checklist.md          ← OWASP Top 10, secrets, validation
│   ├── performance.md                ← Web vitals, loading, runtime optimization
│   ├── prompt-engineering.md          ← How to write effective AI instructions
│   └── memory-and-context.md          ← Session management, cross-session knowledge
│
└── tasks/                             ← Living project tracking
    ├── todo.md                        ← Current task list (updated per session)
    └── lessons.md                     ← Lessons learned log (grows over time)
```

---

## The Workflow at a Glance

```
SESSION START
  → Read lessons + todo
  → Check for applicable skills
  ↓
NEW FEATURE                    BUG FIX                    REFACTOR
  → Brainstorm                   → Debug (systematic)       → Plan
  → Design spec                  → Root cause               → Test existing behavior
  → Plan                         → Failing test              → Change code
  → TDD (red-green-refactor)     → Fix                      → Verify tests pass
  → Verify                       → Verify                    → Verify
  → Review                       → Lesson learned            → Review
  ↓                              ↓                           ↓
DONE → Update todo + lessons → Commit
```

---

## Key Principles

1. **Plan before you code** — even 5 minutes of planning saves hours of rework
2. **Test before you implement** — TDD catches bugs before they exist
3. **Verify before you claim** — run the tests, read the output, then say "done"
4. **Debug systematically** — no guessing, no "quick fixes"
5. **Design for users** — accessibility and UX are not optional
6. **Secure by default** — validate inputs, handle errors, protect data
7. **Learn from mistakes** — write it down so you never repeat it

---

## Customizing for Your Project

Add a project-specific section at the top of CLAUDE.md:

```markdown
## Project: [Your Project Name]
**Stack:** [React + Node.js + PostgreSQL]
**Test command:** `npm test`
**Build command:** `npm run build`
**Lint command:** `npm run lint`

### Project-Specific Rules
- [Any rules specific to this project]
- [Component naming conventions]
- [API patterns]
```

User instructions always take priority over the template rules.
