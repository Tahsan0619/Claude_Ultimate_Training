# Ultimate Claude Prompt — Usage Guide

> Your complete instruction set for getting production-quality results from Claude on every project.
> Portable across Claude Code, VS Code Copilot, Cursor, Windsurf, Claude API, and Claude.ai.

---

## What This Is

A synthesized, battle-tested collection of engineering best practices extracted from **40 top Claude & AI coding repositories**:

**Core Workflow & Skills:**
- **awesome-claude-code** — 300+ curated Claude Code resources and real CLAUDE.md examples
- **superpowers** — 14 disciplined development skills (TDD, debugging, planning, reviews)
- **everything-claude-code** — 65+ skills, 16 agents, eval-driven development, iterative retrieval
- **skills (Anthropic official)** — SKILL.md format, Anthropic-endorsed patterns
- **antigravity-awesome-skills** — 1,262+ universal skills across 90+ categories

**Agent & Orchestration:**
- **awesome-claude-agents** — 24 specialized agents across 4 categories
- **awesome-claude-code-subagents** — 127+ subagent implementations
- **agents (Anthropic plugins)** — 112 agents, 146 skills, 79 tools, 16 orchestrators
- **claude-squad** — Multi-instance parallel orchestration via git worktrees
- **ruflo** — Swarm orchestration with 3-tier routing
- **get-shit-done** — 15-agent orchestra, fresh-context-per-agent, thin orchestrator pattern
- **oh-my-openagent** — 11 specialized agents, 46 lifecycle hooks, 3-tier MCP
- **opcode** — Sub-agent phase decomposition, JSON agent configs

**Prompt Engineering:**
- **claude-skill-prompt-architect** — 27 research-backed frameworks, 7 intent categories
- **claude-prompt-engineering-guide** — XML structuring, thinking modes, role prompting
- **awesome-claude-prompts** — Curated effective prompt patterns

**System Prompts & Internals:**
- **claude-code-system-prompts** — 110+ categorized system prompts
- **system-prompts-and-models** — 30,000+ lines from 20+ AI platforms
- **claude-code-main** — Plugin architecture, extension system

**Memory & Context:**
- **claude-mem** — Persistent memory, cross-session context, transcript analysis
- **claude-code-router** — Smart model routing and cost optimization
- **planning-with-files** — File-based persistent planning for 16+ IDEs, session recovery
- **learn-claude-code** — 3-layer context compression, async mailbox coordination

**Quality & Safety:**
- **claudekit** — 195+ bash security patterns, hook performance profiling
- **beads** — "Landing the Plane" mandatory completion workflow, defensive agent patterns
- **claude-task-master** — AI complexity analysis, Nyquist verification auditor
- **AionUi** — Enterprise quality gates, i18n-first, AI signature prohibition

**Infrastructure & Tools:**
- **claude-code-templates** — 600+ component catalog, validation workflows
- **claude-starter-kit** — Symbol-driven reading, Serena LSP patterns
- **nanoclaw** — Container-based agent isolation, credential proxy
- **serena** — Symbol-level semantic code tools via LSP

**Design & UX:**
- **ui-ux-pro-max** — 67 UI styles, 161 color palettes, 57 font pairings, 99 UX guidelines
- **cherry-studio** — 300+ pre-configured assistants, 17+ languages

---

## How to Use

### Option 1: Drop Into Any Project (Recommended)
Copy the entire `ultimate-claude-prompt/` folder into your project root. Claude reads `CLAUDE.md` automatically and loads skills/docs on demand.

### Option 2: Cherry-Pick What You Need
Copy specific files:
- Just want TDD? → `skills/test-driven-development.md`
- Just want UI rules? → `docs/ui-ux-guidelines.md`
- Just want prompt frameworks? → `skills/prompt-architecture.md`
- Just want the workflow? → `CLAUDE.md` + `tasks/`

### Option 3: Reference from Your Own CLAUDE.md
```markdown
Follow the development workflow in ultimate-claude-prompt/CLAUDE.md
Use skills from ultimate-claude-prompt/skills/ when applicable
```

### Option 4: Use via API
Load CLAUDE.md + relevant skills as the system prompt. See `docs/environment-setup.md` for examples.

---

## Folder Structure

```
ultimate-claude-prompt/
├── CLAUDE.md                          ← START HERE — master instructions
├── README.md                          ← This file (usage guide)
│
├── skills/                            ← Workflow skills (11 files)
│   ├── brainstorming.md               ← Design before building
│   ├── writing-plans.md               ← Break work into executable steps
│   ├── task-planning.md               ← File-based persistent planning + session recovery
│   ├── test-driven-development.md     ← Red-Green-Refactor discipline
│   ├── debugging.md                   ← Systematic root cause + iterative retrieval
│   ├── verification.md               ← Prove it works + eval-driven development
│   ├── agent-orchestration.md         ← Multi-agent coordination, mailboxes, model routing
│   ├── subagent-development.md        ← Dispatch, review, iterate on subagent work
│   ├── code-review.md                ← Code archaeology + multi-perspective review
│   ├── prompt-architecture.md         ← 27 frameworks for crafting prompts
│   └── git-workflow.md               ← Branches, commits, merging
│
├── docs/                              ← Reference guides (10 files)
│   ├── coding-standards.md            ← Architecture, naming, style, testing
│   ├── ui-ux-guidelines.md            ← Typography, colors, components, responsive
│   ├── accessibility.md              ← WCAG, keyboard nav, screen readers, ARIA
│   ├── security-checklist.md          ← OWASP Top 10, agent safety, defensive patterns
│   ├── performance.md                ← Web vitals, loading, runtime optimization
│   ├── prompt-engineering.md          ← XML structuring, thinking modes, anti-sycophancy
│   ├── token-optimization.md          ← Model routing, symbol-driven reading, context compression
│   ├── continuous-learning.md         ← Session memory, file-based working memory, 2-action rule
│   ├── environment-setup.md           ← Setup for Claude Code, Cursor, VS Code, Windsurf, API
│   └── hooks-and-automation.md         ← Hook lifecycle tiers, codebase maps, quality gates
│
└── tasks/                             ← Living project tracking
    ├── todo.md                        ← Current task list (updated per session)
    └── lessons.md                     ← Lessons learned log (grows over time)
```

**Total: 24 files across 4 directories**

---

## The Workflow at a Glance

```
SESSION START
  → Read lessons + todo + task_plan (if exists)
  → Detect project stack automatically
  → Check for applicable skills
  ↓
NEW FEATURE                    BUG FIX                    REFACTOR
  → Brainstorm                   → Debug (systematic)       → Plan
  → Design spec                  → Root cause analysis      → Test existing behavior
  → Plan (file-based if complex) → Iterative retrieval      → Change code
  → TDD (red-green-refactor)     → Failing test             → Verify tests pass
  → Verify (+ evals)             → Fix                      → Verify (+ evals)
  → Multi-perspective review     → Verify                   → Review
  ↓                              ↓                           ↓
DONE → Update todo + lessons → Commit → LAND THE PLANE → Push
```

---

## Key Principles

1. **Plan before you code** — even 5 minutes of planning saves hours of rework
2. **Test before you implement** — TDD catches bugs before they exist
3. **Verify before you claim** — run the tests, read the output, then say "done"
4. **Debug systematically** — no guessing, no "quick fixes"
5. **Use the right model** — Haiku for simple tasks, Sonnet for most work, Opus for deep reasoning
6. **Fresh context per agent** — each subagent gets a clean window, no conversation rot
7. **File-based memory** — use the filesystem as persistent working memory
8. **Batch operations** — 1 message = all related operations (don't waste context)
9. **Land the plane** — every session ends with a mandatory completion checklist
10. **Design for users** — accessibility and UX are not optional
11. **Secure by default** — validate inputs, handle errors, protect data
12. **Learn from every session** — write lessons so you never repeat mistakes
13. **Works everywhere** — portable across Claude Code, Cursor, VS Code, Windsurf, API

---

## Environment Support

| Environment | Setup File | Auto-Reads CLAUDE.md? |
|-------------|-----------|----------------------|
| Claude Code (CLI) | Native | ✅ Yes |
| VS Code + Copilot | `.github/copilot-instructions.md` | Via reference |
| Cursor | `.cursorrules` | Via reference |
| Windsurf | `.windsurfrules` | Via reference |
| Claude API | System prompt | Manual load |
| Claude.ai | Project knowledge | Manual paste |

See `docs/environment-setup.md` for detailed setup instructions.

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
