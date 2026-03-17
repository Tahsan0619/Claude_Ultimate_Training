# Ultimate Claude Prompt

> **Drop this folder into any project. Send one message. Get a full project back.**
>
> A portable AI coding instruction set with **10 specialized agents** that work as a team inside VS Code Copilot Agent Mode. One premium request. Full project — designed, coded, tested, secured, polished, and committed.

---

## What This Is

This is two things in one:

1. **A universal instruction set** — 11 skill files + 11 reference docs + a master CLAUDE.md that works in any AI coding environment (VS Code Copilot, Claude Code, Cursor, Windsurf, API). Drop it into any project and Claude/Copilot immediately follows battle-tested engineering practices.

2. **A 10-agent AI coding team** — specialized agents that work inside VS Code Copilot Agent Mode. You describe what you want, Coordinator reads task history, plans the session, and executes work through phases (DESIGN → BUILD → SECURITY → VERIFY → MEMORY). It updates `tasks/todo.md` and `tasks/lessons.md` every session. No supervision needed.

**Built from 40+ top AI coding repositories.** Every pattern, framework, and rule in this repo was extracted from real-world production use, not theory.

---

## Quick Start

### Step 1 — Copy the contents INTO your project root

**Don't nest the folder.** Copy the files so they sit at your project root:

```
your-project/
├── .github/
│   └── copilot-instructions.md   ← Copilot reads this automatically
├── agents/                        ← Copilot discovers @coordinator, @builder, etc.
│   ├── coordinator.agent.md
│   ├── builder.agent.md
│   └── ... (10 agent files)
├── skills/                        ← 11 workflow disciplines
├── docs/                          ← 11 reference standards
├── tasks/                         ← working memory (agents read/write here)
├── CLAUDE.md                      ← master instructions
├── src/                           ← your existing code
├── package.json                   ← your project
└── ...
```

> **Why at project root?** VS Code Copilot auto-discovers `.github/copilot-instructions.md` and `agents/*.agent.md` files from your workspace root. If they're nested inside a subfolder, Copilot can't find them and `@coordinator` won't work.

### Step 2 — Open VS Code → Copilot Chat → Agent Mode

### Step 3 — Pick your use case

**Want the full pipeline?** Paste the universal prompt from [HOW_TO_USE.md](HOW_TO_USE.md).

**Want something specific?** Use [QUICK_START.md](QUICK_START.md) — it has ready-to-paste prompts for every scenario:

| I want to... | Go to |
|---|---|
| Fix a bug | [QUICK_START.md](QUICK_START.md) — Bug Fix |
| Security audit only | [QUICK_START.md](QUICK_START.md) — Security Audit |
| Security audit + auto-fix | [QUICK_START.md](QUICK_START.md) — Security Audit + Fix |
| UI/UX review only | [QUICK_START.md](QUICK_START.md) — UI/UX Review |
| Add a backend feature | [QUICK_START.md](QUICK_START.md) — Backend Feature |
| Add a feature with UI | [QUICK_START.md](QUICK_START.md) — Feature with UI |
| Build a full app from scratch | [QUICK_START.md](QUICK_START.md) — Full App |
| Add an AI/LLM feature | [QUICK_START.md](QUICK_START.md) — AI Feature |
| Run tests + code review | [QUICK_START.md](QUICK_START.md) — Quality Check |
| Refactor existing code | [QUICK_START.md](QUICK_START.md) — Refactor |
| Write/improve AI prompts | [QUICK_START.md](QUICK_START.md) — Prompt Engineering |
| Mix and match agents | [QUICK_START.md](QUICK_START.md) — Custom Pipeline |

### Step 4 — Wait for SESSION COMPLETE

That's it. Copy a prompt, fill in the blanks, send it.

---

## The Agent Team

| Agent | Role | When It Runs |
|---|---|---|
| **Coordinator** | Reads your request, detects tech stack, plans session, executes all phases, enforces completion | Always first |
| **Architect** | System design — components, data models, API routes, file map, build order | New features, full builds |
| **UIUXSpecialist** | Design system (colors, typography, spacing), visual specs, WCAG accessibility | Frontend UI work |
| **Builder** | TDD implementation — failing tests first, then code, then verify | Almost always |
| **SecurityAuditor** | OWASP Top 10 audit, auth hardening, injection scanning, credential checks | Auth, APIs, user input |
| **PromptEngineer** | System prompt quality, agent instructions, prompt frameworks | AI/LLM features |
| **Verifier** | Full test suite, 6-lens code review (dev, security, perf, QA, UX, a11y), binary PASS/FAIL | After any code changes |
| **MemoryKeeper** | Updates todo/lessons, git commits, session archive, SESSION COMPLETE report | Always last |
| **TestRunner** | Validates all agents produce expected outputs using synthetic test cases | After agent edits, model upgrades |
| **AgentAuditor** | Detects conflicts, overlaps, gaps, and context limit risks between agents | After TestRunner, agent changes |

**How it works:** Copilot runs ONE agent per conversation. When you invoke `@coordinator`, it executes all needed phases itself (design, build, security, verify, memory). You can also invoke specialized agents directly (e.g. `@security-auditor` for a standalone security audit). Every agent reads `tasks/todo.md` + `tasks/lessons.md` at session start and updates them before finishing.

See [HOW_TO_USE.md](HOW_TO_USE.md) for the universal prompt template, real examples, and detailed pipeline documentation.

---

## Where This Works

| Environment | Agent Team? | Instruction Set? | Setup |
|---|---|---|---|
| **VS Code Copilot — Agent Mode** | **Yes** | **Yes** | Drop folder into project. Use universal prompt from HOW_TO_USE.md |
| Claude Code (CLI) | No (single agent) | **Yes** | Reads CLAUDE.md automatically |
| Cursor | No (single agent) | **Yes** | Reference via .cursorrules |
| Windsurf | No (single agent) | **Yes** | Reference via .windsurfrules |
| Claude API | No | **Yes** | Load CLAUDE.md as system prompt |
| Claude.ai | No | **Yes** | Paste into project knowledge |

The **agent team** (10 agents working through phases) only works in **VS Code Copilot Agent Mode** where Copilot loops internally for free after your message.

The **instruction set** (CLAUDE.md + skills + docs) works everywhere — it makes any AI coding tool follow better engineering practices regardless of environment.

---

## What's Inside

### Skills (11 files) — Workflow Disciplines

| File | What It Teaches |
|---|---|
| [skills/brainstorming.md](skills/brainstorming.md) | 2-3 approach framework, pros/cons matrix, structured ideation |
| [skills/writing-plans.md](skills/writing-plans.md) | Breaking work into executable steps, plan-before-build discipline |
| [skills/task-planning.md](skills/task-planning.md) | File-based persistent planning, session recovery, progress tracking |
| [skills/test-driven-development.md](skills/test-driven-development.md) | Red-Green-Refactor cycle, TDD enforcement, when to skip |
| [skills/debugging.md](skills/debugging.md) | Systematic root cause analysis, iterative retrieval, 3-Strike Rule |
| [skills/verification.md](skills/verification.md) | Gate function (binary PASS/FAIL), eval-driven development, Nyquist verification density |
| [skills/agent-orchestration.md](skills/agent-orchestration.md) | Multi-agent coordination, thin orchestrator, fresh context, async mailbox |
| [skills/subagent-development.md](skills/subagent-development.md) | Dispatch templates, two-stage review, model routing |
| [skills/code-review.md](skills/code-review.md) | Code archaeology, multi-perspective review, symbol-driven reading |
| [skills/prompt-architecture.md](skills/prompt-architecture.md) | 27 research-backed frameworks, 7 intent categories, XML structuring |
| [skills/git-workflow.md](skills/git-workflow.md) | Branch management, atomic commits, merge strategies |

### Docs (11 files) — Reference Standards

| File | What It Covers |
|---|---|
| [docs/coding-standards.md](docs/coding-standards.md) | Architecture, naming, style, component organization, testing |
| [docs/ui-ux-guidelines.md](docs/ui-ux-guidelines.md) | Typography, color tokens, spacing system, component patterns, responsive |
| [docs/accessibility.md](docs/accessibility.md) | WCAG 2.1, POUR principles, keyboard nav, screen readers, ARIA |
| [docs/security-checklist.md](docs/security-checklist.md) | OWASP Top 10, agent safety, credential isolation, defensive patterns |
| [docs/performance.md](docs/performance.md) | Web Vitals (LCP, FID, CLS), image optimization, loading strategies |
| [docs/prompt-engineering.md](docs/prompt-engineering.md) | XML structuring, thinking modes, role prompting, anti-sycophancy |
| [docs/token-optimization.md](docs/token-optimization.md) | Model routing, symbol-driven reading, 3-layer context compression |
| [docs/continuous-learning.md](docs/continuous-learning.md) | Session memory, instinct-based learning, 2-Action Rule |
| [docs/environment-setup.md](docs/environment-setup.md) | Setup for Claude Code, Cursor, VS Code, Windsurf, API |
| [docs/hooks-and-automation.md](docs/hooks-and-automation.md) | Hook lifecycle, codebase maps, quality gates, performance profiling |
| [docs/memory-and-context.md](docs/memory-and-context.md) | Persistent memory, cross-session context, transcript analysis |

### Agents (10 files) — The Team

| File | Agent |
|---|---|
| [agents/coordinator.agent.md](agents/coordinator.agent.md) | Coordinator — session recovery, phase execution, model routing, 3-Strike Rule |
| [agents/architect.agent.md](agents/architect.agent.md) | Architect — brainstorming framework, complexity assessment, structured file maps |
| [agents/builder.agent.md](agents/builder.agent.md) | Builder — TDD enforced, code archaeology, OWASP/a11y/UI rules inline |
| [agents/verifier.agent.md](agents/verifier.agent.md) | Verifier — gate function, 6-lens review, eval-driven verification |
| [agents/memorykeeper.agent.md](agents/memorykeeper.agent.md) | MemoryKeeper — 3-layer compression, instinct-based learning, Landing the Plane |
| [agents/prompt-engineer.agent.md](agents/prompt-engineer.agent.md) | PromptEngineer — 27 frameworks, 7 intent categories, quality scoring |
| [agents/security-auditor.agent.md](agents/security-auditor.agent.md) | SecurityAuditor — full OWASP checklists, agent safety, command blocklist |
| [agents/uiux-specialist.agent.md](agents/uiux-specialist.agent.md) | UIUXSpecialist — WCAG audit, font pairings, color tokens, pre-delivery checklist |
| [agents/test-runner.agent.md](agents/test-runner.agent.md) | TestRunner — synthetic test cases per agent, pass/fail validation, fix briefs |
| [agents/agent-auditor.agent.md](agents/agent-auditor.agent.md) | AgentAuditor — conflict detection, overlap/gap analysis, context limit risk assessment |

### Tasks (working memory)

| File | Purpose |
|---|---|
| [tasks/todo.md](tasks/todo.md) | Current task tracking — updated every session |
| [tasks/lessons.md](tasks/lessons.md) | Lessons learned — grows over time, read at session start |

---

## Folder Structure

```
ultimate-claude-prompt/
├── .github/
│   └── copilot-instructions.md        ← VS Code Copilot reads this automatically
├── CLAUDE.md                          ← Master instructions — read first
├── HOW_TO_USE.md                      ← Universal prompt + agent team guide
├── QUICK_START.md                     ← Pick-your-use-case menu with ready prompts
├── README.md                          ← This file
│
├── agents/                            ← 10 agent definition files
│   ├── coordinator.agent.md
│   ├── architect.agent.md
│   ├── builder.agent.md
│   ├── verifier.agent.md
│   ├── memorykeeper.agent.md
│   ├── prompt-engineer.agent.md
│   ├── security-auditor.agent.md
│   ├── uiux-specialist.agent.md
│   ├── test-runner.agent.md
│   └── agent-auditor.agent.md
│
├── skills/                            ← 11 workflow discipline files
│   ├── brainstorming.md
│   ├── writing-plans.md
│   ├── task-planning.md
│   ├── test-driven-development.md
│   ├── debugging.md
│   ├── verification.md
│   ├── agent-orchestration.md
│   ├── subagent-development.md
│   ├── code-review.md
│   ├── prompt-architecture.md
│   └── git-workflow.md
│
├── docs/                              ← 11 reference standard files
│   ├── coding-standards.md
│   ├── ui-ux-guidelines.md
│   ├── accessibility.md
│   ├── security-checklist.md
│   ├── performance.md
│   ├── prompt-engineering.md
│   ├── token-optimization.md
│   ├── continuous-learning.md
│   ├── environment-setup.md
│   ├── hooks-and-automation.md
│   └── memory-and-context.md
│
└── tasks/                             ← Living project memory
    ├── todo.md
    └── lessons.md
```

**Total: 37 files across 6 directories**

---

## The Workflow

Whether you use the agent team or the instruction set standalone, everything follows the same 5-phase cycle:

```
┌─────────────────────────────────────────────────────────┐
│                    SESSION START                         │
│  Read lessons.md → Read todo.md → Detect tech stack     │
│  Load applicable skills → Check for prior session state │
└────────────────────────┬────────────────────────────────┘
                         │
              ┌──────────┼──────────┐
              ▼          ▼          ▼
         NEW FEATURE   BUG FIX   REFACTOR
              │          │          │
              ▼          ▼          ▼
┌─────────────────────────────────────────────────────────┐
│  PHASE 1: RESEARCH                                      │
│  Read files, understand patterns, save findings         │
├─────────────────────────────────────────────────────────┤
│  PHASE 2: PLAN                                          │
│  Break into steps, write to tasks/, specify changes     │
├─────────────────────────────────────────────────────────┤
│  PHASE 3: EXECUTE                                       │
│  TDD: failing test → implement → pass → commit          │
├─────────────────────────────────────────────────────────┤
│  PHASE 4: VERIFY                                        │
│  Run ALL tests, read output, prove correctness          │
├─────────────────────────────────────────────────────────┤
│  PHASE 5: LEARN                                         │
│  Update lessons.md, update todo.md                      │
└────────────────────────┬────────────────────────────────┘
                         │
              ┌──────────┴──────────┐
              ▼                     ▼
         LAND THE PLANE       SESSION COMPLETE
         (mandatory)
         • Run lint + test + typecheck
         • Update todo.md
         • Update lessons.md
         • Git commit + push
         • Clean working tree
```

---

## Key Principles

| # | Principle | What It Means |
|---|---|---|
| 1 | **Plan before you code** | Even 5 minutes of planning saves hours of rework |
| 2 | **Test before you implement** | TDD — write the failing test first, always |
| 3 | **Verify before you claim** | Run the tests, read the output, then say "done" |
| 4 | **Debug systematically** | No guessing, no quick fixes — root cause only |
| 5 | **Fresh context per agent** | Each agent gets a clean context window — no conversation rot |
| 6 | **File-based memory** | Use the filesystem as persistent working memory across sessions |
| 7 | **Batch operations** | 1 message = all related operations. Never do sequentially what can be parallel |
| 8 | **Land the plane** | Every session ends with a mandatory completion checklist |
| 9 | **Accessibility is not optional** | WCAG AA minimum. Keyboard nav. Screen reader support |
| 10 | **Secure by default** | Validate inputs, handle errors, never commit secrets |
| 11 | **Learn from every session** | Write lessons so you never repeat the same mistake |
| 12 | **Simplicity first** | Minimal code. No over-engineering. No premature abstractions |
| 13 | **Never assume** | Verify paths, APIs, variables, state before using them |
| 14 | **Agent team health** | Validate agents with TestRunner + AgentAuditor after any agent change |

---

## Sources

Synthesized from **40+ top AI coding repositories** including:

**Core Workflow:** awesome-claude-code, superpowers, everything-claude-code, Anthropic official skills, antigravity-awesome-skills (1,262+ universal skills)

**Agent Systems:** awesome-claude-agents, awesome-claude-code-subagents, Anthropic agents (112 agents, 146 skills), claude-squad, ruflo, get-shit-done (15-agent orchestra), oh-my-openagent, opcode

**Prompt Engineering:** claude-skill-prompt-architect (27 frameworks), claude-prompt-engineering-guide, awesome-claude-prompts

**System Prompts:** claude-code-system-prompts (110+ prompts), system-prompts-and-models (30,000+ lines)

**Memory & Context:** claude-mem, claude-code-router, planning-with-files (16+ IDEs), learn-claude-code

**Quality & Safety:** claudekit (195+ bash patterns), beads (Landing the Plane), claude-task-master (Nyquist verification), AionUi (enterprise quality gates)

**Design & UX:** ui-ux-pro-max (67 UI styles, 161 palettes, 57 font pairings), cherry-studio

---

## Cost

**VS Code Copilot Agent Mode:** 1 premium request per message you send. Copilot loops internally for free — reading files, writing code, running terminal, executing phases. One message = full project delivered.

---

## License

MIT — use it however you want. Drop it into any project. Modify it. Share it. Build on it.
