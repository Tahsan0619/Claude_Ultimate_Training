# HOW TO USE YOUR AGENT TEAM

**8 agents. One message. Every dispatched agent finishes.**

---

## What This Is

This is an AI coding team — 8 specialized agents that work together inside **VS Code Copilot Agent Mode**. You send **one message**, Copilot loops internally for free after that, and the Coordinator dispatches the agents your request needs. Not every request needs all 8 — a bug fix doesn't need UIUXSpecialist designing a color system. But whichever agents ARE dispatched, **every single one must fully complete its work** before the session ends. No agent gets skipped midway. No half-finished jobs. It can take hours. That's fine. You send one message and walk away.

---

## Where This Works

| Environment | Works? | Why |
|---|---|---|
| **VS Code — Agent Mode (Chat)** | **YES** | 1 premium request per message you send. Copilot loops internally for free — reads files, writes code, runs terminal, dispatches agents, all at no extra cost. This is the target environment. |
| GitHub Copilot Coding Agent (Issues/PRs) | No | That environment uses 1 premium request per full session via GitHub Issues. The agent model is different — agents don't run locally, they run in a cloud container. This team is designed for local VS Code execution. |

**How it works:** You open VS Code → open the Copilot Chat panel → select **Agent Mode** → paste the universal prompt below → press Enter → wait for SESSION COMPLETE.

---

## Your 8 Agents

| # | Agent | What It Does |
|---|---|---|
| 1 | **Coordinator** | Reads your request. Detects your tech stack. Plans the session. Dispatches every other agent in the right order. Enforces completion. Runs first, orchestrates everything. |
| 2 | **Architect** | Designs the system. Breaks the project into components. Writes the file map, data models, API routes, build order. Produces the spec that Builder follows. No code — just the blueprint. |
| 3 | **UIUXSpecialist** | Establishes the design system — colors, typography, spacing, component styles. Writes the visual spec for every screen. Ensures accessibility (WCAG). Runs before Builder (design) or after Builder (audit). |
| 4 | **Builder** | The coder. Writes failing tests first (TDD), then implements until tests pass. Follows Architect's spec and UIUXSpecialist's design system. Handles all files, all code, all terminal commands. |
| 5 | **SecurityAuditor** | Audits every file for OWASP Top 10 vulnerabilities, auth issues, injection risks, credential exposure, agent safety. Writes a severity-rated report. Builder fixes anything critical. |
| 6 | **PromptEngineer** | Only runs if the project involves AI/LLM features. Writes and audits system prompts, agent instructions, prompt templates. Ensures prompt quality using 27 frameworks. |
| 7 | **Verifier** | Runs the full test suite. Reviews all code through 6 lenses (developer, security, performance, QA, UX, accessibility). Binary PASS or FAIL — no partial credit. Sends Builder back if anything fails. |
| 8 | **MemoryKeeper** | Runs last. Updates todo.md and lessons.md. Commits all work to git. Writes the SESSION COMPLETE report. Archives session context for future sessions. |

**The full pipeline (for a complete build):**
```
Coordinator → Architect → UIUXSpecialist (design) → Builder → SecurityAuditor → PromptEngineer (if AI) → Verifier → (Builder fix loop if FAIL) → Verifier (re-check) → MemoryKeeper → SESSION COMPLETE
```

Not every request runs the full pipeline. Coordinator picks the right agents for the job. See "Which Agents Run When" below.

---

## Which Agents Run When

Coordinator decides automatically, but here's what to expect:

| Request Type | Agents Dispatched |
|---|---|
| **Full app from scratch** | All 8: Coordinator → Architect → UIUXSpecialist → Builder → SecurityAuditor → PromptEngineer → Verifier → MemoryKeeper |
| **New frontend feature** | Coordinator → Architect → UIUXSpecialist (design) → Builder → SecurityAuditor → Verifier → UIUXSpecialist (audit) → MemoryKeeper |
| **New backend feature** | Coordinator → Architect → Builder → SecurityAuditor → Verifier → MemoryKeeper |
| **Bug fix** | Coordinator → Builder → Verifier → MemoryKeeper |
| **Security audit** | Coordinator → SecurityAuditor → Builder (fixes) → Verifier → MemoryKeeper |
| **UI polish / redesign** | Coordinator → UIUXSpecialist (audit) → Builder (fixes) → Verifier → MemoryKeeper |
| **AI/LLM feature** | Coordinator → Architect → PromptEngineer → Builder → SecurityAuditor → Verifier → MemoryKeeper |
| **Writing/improving prompts or agents** | Coordinator → PromptEngineer → MemoryKeeper |

**The rule:** Whichever agents Coordinator dispatches, **every one of them must fully complete its workflow and write its handoff note** before the next agent starts. No agent is skipped once dispatched. MemoryKeeper always runs last — no session ends without SESSION COMPLETE.

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

You are the Coordinator. You have 8 agents at your disposal:
@architect @builder @verifier @memorykeeper @prompt-engineer @security-auditor @uiux-specialist

Execute the FULL pipeline:

1. Read CLAUDE.md, tasks/todo.md, and tasks/lessons.md — understand the system and any prior context
2. Detect or confirm the tech stack
3. Write the session plan to tasks/current_session_plan.md
4. Dispatch ALL agents in sequence:
   - Architect: design the system, write the spec and file map
   - UIUXSpecialist (DESIGN mode): establish design system, write visual specs for every component
   - Builder: implement everything with TDD (tests first, then code, then verify)
   - SecurityAuditor: audit all code for OWASP Top 10 and security issues
   - PromptEngineer: audit/write any AI-facing prompts (skip if project has no AI/LLM features)
   - Verifier: run full test suite, 6-lens code review, PASS or FAIL
   - If Verifier returns FAIL → send Builder back to fix → re-run Verifier
   - UIUXSpecialist (AUDIT mode): review the final UI for polish and accessibility
   - If UIUXSpecialist finds critical issues → send Builder back to fix
   - MemoryKeeper: update todo.md, update lessons.md, commit all work, write SESSION COMPLETE report
5. Do NOT stop until MemoryKeeper prints SESSION COMPLETE
6. Do NOT ask me any questions — make reasonable assumptions and document them in the session plan
7. If any agent fails 3 times on the same task, document the blocker in tasks/todo.md and move on — do not loop forever

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
│   COORDINATOR    │  Reads your request. Reads CLAUDE.md, todo.md, lessons.md.
│                  │  Detects tech stack. Writes session plan.
│                  │  Decides WHICH agents this request needs.
│                  │  Dispatches them in order.
└────────┬────────┘
         │
         ▼  (only if request needs design/planning)
┌─────────────────┐
│   ARCHITECT      │  Designs the system. Components, data models, API routes,
│                  │  file map, build order. Writes tasks/task_plan.md.
│                  │  ✅ Must complete fully before next agent.
└────────┬────────┘
         │
         ▼  (only if request has frontend UI)
┌─────────────────────┐
│  UIUX SPECIALIST    │  (DESIGN mode) Picks fonts, colors, spacing. Writes
│                     │  component specs. Writes tasks/design_system.md.
│                     │  ✅ Must complete fully before next agent.
└────────┬────────────┘
         │
         ▼  (almost always)
┌─────────────────┐
│    BUILDER       │  Writes failing tests → implements code → tests pass.
│                  │  Follows Architect's spec + UIUXSpecialist's design.
│                  │  Commits after each logical unit.
│                  │  ✅ Must complete fully before next agent.
└────────┬────────┘
         │
         ▼  (if auth, APIs, user input, or external data)
┌─────────────────────┐
│  SECURITY AUDITOR   │  Scans every file. OWASP Top 10 checklist. Auth,
│                     │  injection, XSS, CSRF, secrets. Severity-rated report.
│                     │  ✅ Must complete fully before next agent.
└────────┬────────────┘
         │
         ▼  (only if AI/LLM features)
┌─────────────────────┐
│  PROMPT ENGINEER    │  Reviews system prompts, agent instructions, prompt
│  (conditional)      │  templates. Rewrites if quality is low.
│                     │  ✅ Must complete fully before next agent.
└────────┬────────────┘
         │
         ▼  (almost always — if any code was written)
┌─────────────────┐
│    VERIFIER      │  Runs ALL tests. Reviews code through 6 lenses.
│                  │  PASS → continue. FAIL → Builder fixes → re-check.
│                  │  ✅ Must complete fully before next agent.
└────────┬────────┘
         │
         ▼  (if frontend UI was implemented)
┌─────────────────────┐
│  UIUX SPECIALIST    │  (AUDIT mode) Checks visual consistency, UX quality,
│                     │  accessibility (WCAG). Flags remaining issues.
│                     │  ✅ Must complete fully before next agent.
└────────┬────────────┘
         │
         ▼  (ALWAYS — every session ends here)
┌─────────────────────┐
│  MEMORY KEEPER      │  Updates tasks/todo.md. Updates tasks/lessons.md.
│                     │  Commits everything to git. Writes SESSION COMPLETE.
│                     │  ✅ ALWAYS runs. Session is not done without this.
└─────────────────────┘
         │
         ▼
    SESSION COMPLETE — every dispatched agent finished its job.
```

---

## Cost

**1 premium request per message you send.** That's it.

Everything Copilot does after your message — reading files, writing code, running terminal commands, dispatching agents, looping through the pipeline — is **free internal looping.** You pay for the message, not for the work.

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

You are the Coordinator. You have 8 agents at your disposal:
@architect @builder @verifier @memorykeeper @prompt-engineer @security-auditor @uiux-specialist

Execute the FULL pipeline:

1. Read CLAUDE.md, tasks/todo.md, and tasks/lessons.md — understand the system and any prior context
2. Detect or confirm the tech stack
3. Write the session plan to tasks/current_session_plan.md
4. Decide which agents this request needs and dispatch them in the correct order:
   - Architect — if the request needs system design, file planning, or component breakdown
   - UIUXSpecialist (DESIGN mode) — if the request has frontend UI that needs a design system or visual spec
   - Builder — if any code needs to be written or changed (almost always)
   - SecurityAuditor — if the request touches auth, APIs, user input, payments, or external data
   - PromptEngineer — if the request involves AI/LLM features, system prompts, or agent instructions
   - Verifier — if any code was written (almost always — runs tests + code review)
   - UIUXSpecialist (AUDIT mode) — if Builder implemented frontend UI (review for polish and accessibility)
   - MemoryKeeper — ALWAYS runs last (updates todo, lessons, commits, SESSION COMPLETE)
5. Every dispatched agent MUST fully complete its workflow and write its handoff note before the next agent starts
6. If Verifier returns FAIL → send Builder back to fix → re-run Verifier. Loop until PASS.
7. Do NOT stop until MemoryKeeper prints SESSION COMPLETE
8. Do NOT ask me any questions — make reasonable assumptions and document them in the session plan
9. If any agent fails 3 times on the same task, document the blocker in tasks/todo.md and move on — do not loop forever

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

[same execution instructions block as above]

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

[same execution instructions block as above]

===== END =====
```

### Example 4 — AI-powered feature (triggers PromptEngineer)

```
@coordinator

===== PROJECT REQUEST =====

Project: Add an AI recipe suggestion feature. Users describe what ingredients they have and dietary preferences, and the app suggests 3 recipes using OpenAI's API. Each suggestion shows recipe name, cook time, difficulty, and a "Generate Full Recipe" button that expands with full ingredients and step-by-step instructions.

Tech Stack: auto-detect

Special Requirements: This uses the OpenAI API — system prompts need to be reviewed by PromptEngineer. Rate limit to 10 suggestions per user per day. Cache suggestions for identical inputs.

Reference Files: See .env.example for the OPENAI_API_KEY variable.

===== EXECUTION INSTRUCTIONS =====

[same execution instructions block as above]

===== END =====
```

---

## Folder Structure

Drop this entire folder into your project root. The agents expect this layout:

```
your-project/
├── CLAUDE.md                       ← master instructions (agents read this first)
├── HOW_TO_USE.md                   ← this file (for you, the human)
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
├── agents/                         ← the 8 agent definition files
│   ├── coordinator.agent.md
│   ├── architect.agent.md
│   ├── builder.agent.md
│   ├── verifier.agent.md
│   ├── memorykeeper.agent.md
│   ├── prompt-engineer.agent.md
│   ├── security-auditor.agent.md
│   └── uiux-specialist.agent.md
└── src/                            ← your project source code
```

---

## FAQ

**Q: Do I need to send multiple messages to run different agents?**
No. One message triggers everything. Coordinator decides which agents are needed, dispatches them, and Copilot loops internally for free.

**Q: Does every request use all 8 agents?**
No. Coordinator picks the agents your request actually needs. A bug fix might only use Builder → Verifier → MemoryKeeper. A full app uses all 8. But whichever agents ARE dispatched, every single one must fully complete its work — no agent gets skipped once it starts.

**Q: Can I force specific agents to run or be skipped?**
Yes — in the Special Requirements field, you can write things like "Make sure SecurityAuditor runs" or "Skip UIUXSpecialist — this is backend only." Coordinator will respect that.

**Q: How long does it take?**
Depends on complexity. A bug fix might take 15-30 minutes. A full app can take 2-5 hours. Time doesn't matter — you send one message and walk away.

**Q: What if it gets stuck or errors out midway?**
Send a new message: `@coordinator Continue from where you left off. Read tasks/todo.md and tasks/current_session_plan.md to understand the current state. Do not restart — pick up.` That costs one more premium request but resumes the session.

**Q: Does this work with any tech stack?**
Yes. The agents detect your stack from package.json, requirements.txt, go.mod, Cargo.toml, etc. The embedded rules cover web (React, Next.js, Vue, Angular, Svelte), backend (Node, Python, Go, Rust), mobile (React Native, Flutter), and more.
