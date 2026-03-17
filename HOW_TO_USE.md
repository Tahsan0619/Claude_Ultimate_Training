# HOW TO USE YOUR AGENT TEAM

**10 agents. One message. Full session.**

> **Just want a quick prompt for a specific task?** Skip this file and go to [QUICK_START.md](QUICK_START.md) — it has ready-to-paste prompts for bug fixes, security audits, UI reviews, and more. Come back here for the full reference.

---

## What This Is

This is an AI coding team — 10 specialized agents that work inside **VS Code Copilot Agent Mode**. You send **one message**, and the Coordinator handles the full session: planning, coding, testing, verifying, and updating task memory. It works through phases internally — design, build, test, verify, memory update — drawing on the specialized knowledge from each agent file as needed.

Not every request needs all phases — a bug fix skips design, a security audit skips coding. But your `tasks/todo.md` and `tasks/lessons.md` files ALWAYS get updated at the end so your project builds memory over time.

---

## CRITICAL: Folder Setup

**The files from this repo must be at your project ROOT, not inside a subfolder.** VS Code Copilot discovers `.github/copilot-instructions.md` and `agents/*.agent.md` files from the workspace root. If they're nested inside a subfolder, Copilot can't find them and `@coordinator` won't work.

```
✅ CORRECT                           ❌ WRONG
your-project/                        your-project/
├── .github/                         └── ultimate-claude-prompt/
│   └── copilot-instructions.md          ├── .github/
├── agents/                              ├── agents/
├── skills/                              └── ...
├── docs/
├── tasks/
├── CLAUDE.md
└── src/  ← your code
```

Also make sure your VS Code workspace is open to the **project root** (the folder that contains `.github/`, `agents/`, etc.) — not to a parent folder or the prompt repo folder itself.

---

## Where This Works

| Environment | Works? | Why |
|---|---|---|
| **VS Code — Agent Mode (Chat)** | **YES** | 1 premium request per message you send. Copilot loops internally for free — reads files, writes code, runs terminal, all at no extra cost. This is the target environment. |
| GitHub Copilot Coding Agent (Issues/PRs) | No | Different agent model — runs in cloud containers, not local VS Code. |

**How it works:** You open VS Code → open the Copilot Chat panel → select **Agent Mode** → paste the universal prompt below → press Enter → wait for SESSION COMPLETE.

---

## Your 10 Agents

| # | Agent | What It Does |
|---|---|---|
| 1 | **Coordinator** | Reads your request. Detects your tech stack. Plans the session. Executes all work through phases (DESIGN → BUILD → SECURITY → VERIFY → MEMORY). Enforces completion. Runs first, does everything. |
| 2 | **Architect** | Designs the system. Breaks the project into components. Writes the file map, data models, API routes, build order. Produces the spec that Builder follows. No code — just the blueprint. |
| 3 | **UIUXSpecialist** | Establishes the design system — colors, typography, spacing, component styles. Writes the visual spec for every screen. Ensures accessibility (WCAG). Runs before Builder (design) or after Builder (audit). |
| 4 | **Builder** | The coder. Writes failing tests first (TDD), then implements until tests pass. Follows Architect's spec and UIUXSpecialist's design system. Handles all files, all code, all terminal commands. |
| 5 | **SecurityAuditor** | Audits every file for OWASP Top 10 vulnerabilities, auth issues, injection risks, credential exposure, agent safety. Writes a severity-rated report. Builder fixes anything critical. |
| 6 | **PromptEngineer** | Only runs if the project involves AI/LLM features. Writes and audits system prompts, agent instructions, prompt templates. Ensures prompt quality using 27 frameworks. |
| 7 | **Verifier** | Runs the full test suite. Reviews all code through 6 lenses (developer, security, performance, QA, UX, accessibility). Binary PASS or FAIL — no partial credit. Sends Builder back if anything fails. |
| 8 | **MemoryKeeper** | Runs last on project sessions. Updates todo.md and lessons.md. Commits all work to git. Writes the SESSION COMPLETE report. Archives session context for future sessions. |
| 9 | **TestRunner** | Validates that all agents produce expected outputs using synthetic test cases. Runs after any agent file edit or Opus upgrade. Catches broken agents before they waste a real session. |
| 10 | **AgentAuditor** | Cross-reads all agent files to detect conflicts, overlaps, gaps, and context limit risks. Runs after TestRunner. Ensures the team works together, not just individually. |

**The full pipeline (for a complete build):**
```
@coordinator executes ALL phases:
DESIGN → BUILD → SECURITY → VERIFY → MEMORY → SESSION COMPLETE
```

**The validation pipeline (for agent team health):**
```
@coordinator or @test-runner → @agent-auditor → update tasks/ → SESSION COMPLETE
```

Not every request runs all phases. The Coordinator picks the right phases for the job. See "Which Phases Run When" below.

---

## Which Phases Run When

The Coordinator decides automatically based on your request:

| Request Type | Phases Executed |
|---|---|
| **Full app from scratch** | DESIGN → BUILD → SECURITY → VERIFY → MEMORY |
| **New frontend feature** | DESIGN → BUILD → VERIFY → MEMORY |
| **New backend feature** | DESIGN → BUILD → SECURITY → VERIFY → MEMORY |
| **Bug fix** | BUILD → VERIFY → MEMORY |
| **Security audit** | SECURITY → MEMORY |
| **Security audit + fix** | SECURITY → BUILD → VERIFY → MEMORY |
| **UI polish / redesign** | BUILD → VERIFY → MEMORY |
| **AI/LLM feature** | DESIGN → BUILD → SECURITY → VERIFY → MEMORY |
| **Code review / tests** | VERIFY → MEMORY |
| **Refactor** | DESIGN → BUILD → VERIFY → MEMORY |

**The rule:** Every session ALWAYS ends with the MEMORY phase — `tasks/todo.md` and `tasks/lessons.md` get updated, work gets committed, and SESSION COMPLETE gets printed.

**Want to invoke a specific agent directly?** You can call `@security-auditor`, `@builder`, `@verifier`, etc. individually. Each agent reads and updates `tasks/todo.md` and `tasks/lessons.md` independently. See [QUICK_START.md](QUICK_START.md) for ready-to-paste prompts.

---

## The Universal Prompt

**This is the only prompt you need. Copy it. Fill in the 4 blanks. Send it in VS Code Agent Mode. That's it.**

```
@coordinator

===== PROJECT REQUEST =====

Project: [WHAT YOU WANT BUILT — describe the app, feature, or task in plain language. Be specific about what it should do. Examples: "A personal finance tracker where users can log expenses, categorize them, see monthly charts, and set budget limits" or "Add a dark mode toggle to the existing dashboard" or "Fix the login bug where users get 403 after password reset"]

Tech Stack: [YOUR PREFERRED STACK — or write "auto-detect" if the project already exists and you want Copilot to figure it out. Examples: "Next.js 14, TypeScript, Tailwind CSS, Prisma, PostgreSQL" or "Python, FastAPI, SQLAlchemy, React frontend" or "auto-detect"]

Special Requirements: [ANYTHING SPECIFIC — or write "none". Examples: "Must support dark mode" / "Needs to work offline" / "OAuth with Google and GitHub" / "Real-time updates with WebSockets" / "This is an AI app — needs system prompts reviewed" / "none"]

Reference Files: [IF YOU HAVE DESIGN MOCKUPS, WIREFRAMES, OR EXISTING SPECS — list them here. Or write "none". Examples: "See mockup.png in /designs" / "Follow the API spec in docs/api.md" / "none"]

===== EXECUTION INSTRUCTIONS =====

You are the Coordinator. Execute the FULL session yourself by working through these phases:

1. Read tasks/todo.md and tasks/lessons.md FIRST — understand prior context and avoid past mistakes
2. Read CLAUDE.md — understand the engineering rules
3. Detect or confirm the tech stack
4. Write the session plan to tasks/current_session_plan.md
5. Execute ALL phases yourself in order:
   - DESIGN: break the system into components, write spec and file map (skip for bug fixes)
   - BUILD: implement everything with TDD (write failing test first → code → pass → commit)
   - SECURITY: audit all new code for OWASP Top 10 and security issues (skip for non-security work)
   - VERIFY: run full test suite, lint, build — if anything fails, fix it and re-verify
   - MEMORY: update tasks/todo.md, update tasks/lessons.md, git commit, print SESSION COMPLETE
6. Do NOT stop until you print SESSION COMPLETE
7. Do NOT ask me any questions — make reasonable assumptions and document them
8. If the same issue fails 3 times, document the blocker in tasks/todo.md and move on

===== END =====
```

---

## How to Fill in the Blanks

### Project (required)
Write what you want in plain language. More detail = better result. Bad: "build me an app". Good: "Build a recipe sharing platform where users can sign up, post recipes with ingredients and steps, upload photos, search/filter by cuisine or dietary restriction, and save favorites. Landing page with featured recipes. User profile page showing their recipes."

### Tech Stack (required)
Tell it what to use, or say "auto-detect" for existing projects.
- **Full-stack web:** `Next.js 14, TypeScript, Tailwind CSS, Prisma, PostgreSQL`
- **Python backend:** `Python 3.12, FastAPI, SQLAlchemy, Alembic, PostgreSQL`
- **Mobile:** `React Native, Expo, TypeScript, Supabase`
- **Existing project:** `auto-detect` (Coordinator will read package.json / requirements.txt / etc.)

### Special Requirements (optional)
Anything that affects architecture or design decisions:
- Auth providers: "OAuth with Google, GitHub, and email/password"
- Realtime: "Live updates using WebSockets"
- AI features: "Uses OpenAI API for recipe suggestions — system prompts need review"
- Design: "Dark mode required" / "Must match Figma at /designs/mockup.fig"
- Deploy: "Must deploy to Vercel" / "Docker container for AWS ECS"
- Or just: "none"

### Reference Files (optional)
Point to anything already in the project that agents should read:
- "See designs/wireframe.png for layout reference"
- "Follow the existing API patterns in src/api/"
- Or just: "none"

---

## What Happens After You Press Enter

Here's what happens behind the scenes. You don't need to do anything — just wait.

```
YOU SEND ONE MESSAGE
        │
        ▼
┌─────────────────┐
│   COORDINATOR    │  Reads your request. Reads todo.md, lessons.md, CLAUDE.md.
│                  │  Detects tech stack. Writes session plan.
│                  │  Executes ALL phases itself (see below).
└────────┬────────┘
         │
         ▼  DESIGN PHASE (only if request needs design/planning)
┌─────────────────┐
│                  │  Designs the system. Components, data models, API routes,
│                  │  file map, build order. Writes tasks/task_plan.md.
└────────┬────────┘
         │
         ▼  BUILD PHASE (almost always)
┌─────────────────┐
│                  │  Writes failing tests → implements code → tests pass.
│                  │  TDD: Red → Green → Refactor.
│                  │  Commits after each logical unit.
└────────┬────────┘
         │
         ▼  SECURITY PHASE (if auth, APIs, user input, or external data)
┌─────────────────┐
│                  │  Scans code for OWASP Top 10. Auth, injection, XSS,
│                  │  CSRF, secrets. Fixes critical issues.
└────────┬────────┘
         │
         ▼  VERIFY PHASE (almost always — if any code was written)
┌─────────────────┐
│                  │  Runs ALL tests. Runs lint. Runs build.
│                  │  If anything fails → fix → re-verify.
└────────┬────────┘
         │
         ▼  MEMORY PHASE (ALWAYS — every session ends here)
┌─────────────────┐
│                  │  Updates tasks/todo.md with completed work.
│                  │  Updates tasks/lessons.md with insights.
│                  │  Git commits everything. Prints SESSION COMPLETE.
│                  │  ✅ ALWAYS runs. Session is not done without this.
└─────────────────┘
         │
         ▼
    SESSION COMPLETE — todo.md updated, lessons.md updated, work committed.
```

---

## Agent Validation Prompt

Run this after editing any agent file, adding a new agent, or after an Opus model upgrade. This validates that all 10 agents work individually (TestRunner) and as a team (AgentAuditor).

```
@coordinator

===== PROJECT REQUEST =====

Project: Validate the entire agent team. Run TestRunner against all 10 agents to verify each produces expected outputs. Then run AgentAuditor to detect conflicts, overlaps, gaps, and context limit risks between agents. Fix any issues found, then confirm the team is healthy.

Tech Stack: N/A — this is an agent validation session, not a code build.

Special Requirements: none

Reference Files: All files in agents/ directory

===== EXECUTION INSTRUCTIONS =====

You are the Coordinator. Execute these phases in order:
1. Test Runner phase — validate all 10 agents produce expected outputs using synthetic test cases
2. Agent Audit phase — cross-read all agent files, detect conflicts/overlaps/gaps, assess context limit risks
3. Memory phase — record results, update lessons.md, commit

Report results in:
- tasks/agent_test_results.md (TestRunner output)
- tasks/agent_audit_report.md (AgentAuditor output)

Do not ask questions. Do not stop until SESSION COMPLETE.

===== END =====
```

---

## Cost

**1 premium request per message you send.** That's it.

Everything Copilot does after your message — reading files, writing code, running terminal commands, executing phases, looping through the pipeline — is **free internal looping.** You pay for the message, not for the work.

One message. Full project. One premium request.

---

## Real Examples

### Example 1 — Full app from scratch

```
@coordinator

===== PROJECT REQUEST =====

Project: Build a team task management app. Users can sign up, create teams, invite members by email, create projects within teams, and manage tasks with a kanban board (drag and drop columns: To Do, In Progress, Done). Each task has a title, description, assignee, priority (low/medium/high/urgent), due date, and comments. Dashboard shows overdue tasks and team activity feed.

Tech Stack: Next.js 14, TypeScript, Tailwind CSS, Prisma, PostgreSQL

Special Requirements: OAuth with Google and GitHub. Real-time updates when teammates move tasks. Dark mode. Email notifications for task assignments and due dates.

Reference Files: none

===== EXECUTION INSTRUCTIONS =====

You are the Coordinator. Execute the FULL session yourself by working through these phases:

1. Read tasks/todo.md and tasks/lessons.md FIRST — understand prior context and avoid past mistakes
2. Read CLAUDE.md — understand the engineering rules
3. Detect or confirm the tech stack
4. Write the session plan to tasks/current_session_plan.md
5. Execute ALL phases yourself in order:
   - DESIGN: break the system into components, write spec and file map
   - BUILD: implement everything with TDD (write failing test first → code → pass → commit)
   - SECURITY: audit all new code for OWASP Top 10 and security issues
   - VERIFY: run full test suite, lint, build — if anything fails, fix it and re-verify
   - MEMORY: update tasks/todo.md, update tasks/lessons.md, git commit, print SESSION COMPLETE
6. Do NOT stop until you print SESSION COMPLETE
7. Do NOT ask me any questions — make reasonable assumptions and document them
8. If the same issue fails 3 times, document the blocker in tasks/todo.md and move on

===== END =====
```

### Example 2 — Add a feature to an existing project

```
@coordinator

===== PROJECT REQUEST =====

Project: Add a dark mode toggle to the existing dashboard. Should persist the user's preference in localStorage. Toggle button in the top-right navbar. All existing components need to support both themes. Use CSS custom properties for theming.

Tech Stack: auto-detect

Special Requirements: Must not break any existing functionality. Smooth transition animation between themes.

Reference Files: See src/components/Navbar.tsx for where the toggle should go.

===== EXECUTION INSTRUCTIONS =====

[same execution instructions block as Example 1]

===== END =====
```

### Example 3 — Bug fix

```
@coordinator

===== PROJECT REQUEST =====

Project: Fix the bug where logged-in users get a 403 error and redirect to /login after refreshing the page. The session token is valid but the middleware isn't reading it correctly on page refresh. Check the auth middleware and session handling.

Tech Stack: auto-detect

Special Requirements: none

Reference Files: Check src/middleware.ts and src/lib/auth.ts

===== EXECUTION INSTRUCTIONS =====

[same execution instructions block as Example 1]

===== END =====
```

### Example 4 — AI-powered feature (triggers PromptEngineer phase)

```
@coordinator

===== PROJECT REQUEST =====

Project: Add an AI recipe suggestion feature. Users describe what ingredients they have and dietary preferences, and the app suggests 3 recipes using OpenAI's API. Each suggestion shows recipe name, cook time, difficulty, and a "Generate Full Recipe" button that expands with full ingredients and step-by-step instructions.

Tech Stack: auto-detect

Special Requirements: This uses the OpenAI API — system prompts need to be audited. Rate limit to 10 suggestions per user per day. Cache suggestions for identical inputs.

Reference Files: See .env.example for the OPENAI_API_KEY variable.

===== EXECUTION INSTRUCTIONS =====

[same execution instructions block as Example 1]

===== END =====
```

---

## Folder Structure

Drop the contents of this folder into your project root (not as a subfolder). The agents expect this layout:

```
your-project/
├── .github/
│   └── copilot-instructions.md  ← Copilot reads this automatically to discover agents
├── CLAUDE.md                       ← master instructions (agents read this first)
├── HOW_TO_USE.md                   ← this file (for you, the human)
├── QUICK_START.md                  ← pick-your-use-case menu with ready prompts
├── README.md                       ← project readme
├── skills/                         ← 11 skill files (workflow disciplines)
├── docs/                           ← 11 doc files (reference standards)
├── tasks/                          ← working memory (agents read/write here)
│   ├── todo.md                     ← current task tracking
│   ├── lessons.md                  ← past mistakes and learnings
│   ├── current_session_plan.md     ← created by Coordinator each session
│   ├── task_plan.md                ← created by Architect
│   ├── design_system.md            ← created by UIUXSpecialist
│   └── archive/                    ← past session archives
├── agents/                         ← the 10 agent definition files
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
└── src/                            ← your project source code
```

---

## FAQ

**Q: Do I need to send multiple messages to run different agents?**
No. One message triggers everything. The Coordinator works through all phases (design, build, test, verify, memory) in a single session. Copilot loops internally for free.

**Q: Does every request use all phases?**
No. The Coordinator picks the right phases for your request. A bug fix skips design and goes straight to build → verify → memory. A full app runs all phases. But `tasks/todo.md` and `tasks/lessons.md` ALWAYS get updated at the end.

**Q: Can I force specific phases to run or be skipped?**
Yes — in the Special Requirements field, you can write things like "Run a security audit" or "Skip UI work — this is backend only." The Coordinator will adjust.

**Q: Why aren't tasks/todo.md and tasks/lessons.md updating?**
Make sure: (1) the files exist in `tasks/` folder, (2) VS Code is open to the project root (the folder containing `agents/`, `.github/`, etc.), and (3) you're using `@coordinator` in Agent Mode. The `.github/copilot-instructions.md` file mandates reading and updating these files on every conversation.

**Q: How long does it take?**
Depends on complexity. A bug fix might take 15-30 minutes. A full app can take 2-5 hours. You send one message and walk away.

**Q: What if it gets stuck or errors out midway?**
Send a new message: `@coordinator Continue from where you left off. Read tasks/todo.md and tasks/current_session_plan.md to understand the current state. Do not restart — pick up.` That costs one more premium request but resumes the session.

**Q: Does this work with any tech stack?**
Yes. The Coordinator detects your stack from package.json, requirements.txt, go.mod, Cargo.toml, etc. The embedded rules cover web (React, Next.js, Vue, Angular, Svelte), backend (Node, Python, Go, Rust), mobile (React Native, Flutter), and more.

**Q: What if I just want to use one specific agent (like security-auditor)?**
See [QUICK_START.md](QUICK_START.md) for ready-to-paste prompts for every use case. Or invoke any agent directly: `@security-auditor Audit all source files in src/ for OWASP Top 10 vulnerabilities.` Every agent reads `tasks/todo.md` and `tasks/lessons.md` at start and updates them when done.
