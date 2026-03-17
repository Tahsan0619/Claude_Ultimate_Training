# QUICK START — Pick What You Need

> **Don't read the whole docs.** Find your situation below. Copy the prompt. Paste it in VS Code Copilot Agent Mode. Done.

---

## SETUP (Do This First — One Time Only)

### Step 1 — Copy this folder into your project

```
your-project/
├── .github/
│   └── copilot-instructions.md    ← Copilot reads this automatically
├── agents/                         ← 10 agent files (Copilot discovers these)
├── skills/                         ← workflow disciplines
├── docs/                           ← reference standards
├── tasks/                          ← working memory
├── CLAUDE.md                       ← master instructions
├── src/                            ← your code
├── package.json                    ← your project
└── ...
```

**Copy the entire contents of this folder** (agents/, skills/, docs/, tasks/, CLAUDE.md, and .github/) into your project's root directory. Merge if folders already exist.

### Step 2 — Open VS Code → Copilot Chat → Agent Mode

### Step 3 — That's it.

**Why it works automatically:** The `.github/copilot-instructions.md` file tells VS Code Copilot about the agent team. The `agents/*.agent.md` files have YAML frontmatter so Copilot registers each agent. When you type `@coordinator`, Copilot knows to look in `agents/coordinator.agent.md`.

---

## PICK YOUR USE CASE

### 🔧 "I want to fix a bug"

```
@coordinator

===== PROJECT REQUEST =====
Project: [Describe the bug — what happens vs what should happen]
Tech Stack: auto-detect
Special Requirements: none
Reference Files: [Files where the bug might be, or "none"]
===== EXECUTION INSTRUCTIONS =====
You are the Coordinator. Dispatch: @builder → @verifier → @memorykeeper
Bug fix pipeline only. Do NOT stop until SESSION COMPLETE.
===== END =====
```

**Agents used:** Coordinator → Builder → Verifier → MemoryKeeper
**What happens:** Builder finds and fixes the bug using TDD, Verifier confirms all tests pass, MemoryKeeper commits.

---

### 🔒 "I want a security audit only"

```
@coordinator

===== PROJECT REQUEST =====
Project: Run a full security audit on this project. Check all source code for OWASP Top 10 vulnerabilities, auth issues, injection risks, credential exposure, and insecure configurations. Write a severity-rated report.
Tech Stack: auto-detect
Special Requirements: Security audit only — no new features, no refactoring.
Reference Files: Scan all files in src/ (or your source directory)
===== EXECUTION INSTRUCTIONS =====
You are the Coordinator. Dispatch: @security-auditor → @memorykeeper
Security audit pipeline only. Do NOT stop until SESSION COMPLETE.
Write the report to tasks/security_audit_report.md.
===== END =====
```

**Agents used:** Coordinator → SecurityAuditor → MemoryKeeper
**What happens:** SecurityAuditor scans every file against OWASP Top 10, writes a severity-rated report, MemoryKeeper commits.

---

### 🔒+ "I want a security audit AND auto-fix the issues"

```
@coordinator

===== PROJECT REQUEST =====
Project: Run a full security audit, then fix all critical and high severity issues found.
Tech Stack: auto-detect
Special Requirements: Fix critical/high issues. Log medium/low issues in tasks/todo.md for later.
Reference Files: Scan all files in src/
===== EXECUTION INSTRUCTIONS =====
You are the Coordinator. Dispatch: @security-auditor → @builder → @verifier → @memorykeeper
Audit → Fix → Verify pipeline. Do NOT stop until SESSION COMPLETE.
===== END =====
```

**Agents used:** Coordinator → SecurityAuditor → Builder (fixes) → Verifier → MemoryKeeper

---

### 🎨 "I want a UI/UX review only"

```
@coordinator

===== PROJECT REQUEST =====
Project: Audit the existing UI for design consistency, visual polish, and WCAG accessibility compliance. Write a report with specific issues and fix recommendations.
Tech Stack: auto-detect
Special Requirements: Audit only — no code changes.
Reference Files: [Point to your UI components folder, or "none"]
===== EXECUTION INSTRUCTIONS =====
You are the Coordinator. Dispatch: @uiux-specialist → @memorykeeper
UI/UX audit pipeline only. Do NOT stop until SESSION COMPLETE.
Write the report to tasks/uiux_audit_report.md.
===== END =====
```

**Agents used:** Coordinator → UIUXSpecialist → MemoryKeeper

---

### 🎨+ "I want a UI/UX review AND auto-fix the issues"

```
@coordinator

===== PROJECT REQUEST =====
Project: Audit the UI for design, accessibility, and polish issues — then fix them all.
Tech Stack: auto-detect
Special Requirements: Fix all issues found. Ensure WCAG AA compliance.
Reference Files: [Point to your UI components folder, or "none"]
===== EXECUTION INSTRUCTIONS =====
You are the Coordinator. Dispatch: @uiux-specialist → @builder → @verifier → @memorykeeper
Audit → Fix → Verify pipeline. Do NOT stop until SESSION COMPLETE.
===== END =====
```

**Agents used:** Coordinator → UIUXSpecialist → Builder (fixes) → Verifier → MemoryKeeper

---

### ✅ "I want to add a feature (backend only, no UI)"

```
@coordinator

===== PROJECT REQUEST =====
Project: [Describe the feature you want added]
Tech Stack: auto-detect
Special Requirements: Backend only — no frontend changes needed.
Reference Files: [Relevant files, or "none"]
===== EXECUTION INSTRUCTIONS =====
You are the Coordinator. Dispatch: @architect → @builder → @security-auditor → @verifier → @memorykeeper
Backend feature pipeline. Do NOT stop until SESSION COMPLETE.
===== END =====
```

**Agents used:** Coordinator → Architect → Builder → SecurityAuditor → Verifier → MemoryKeeper

---

### ✅🎨 "I want to add a feature (with UI)"

```
@coordinator

===== PROJECT REQUEST =====
Project: [Describe the feature, including what the UI should look like]
Tech Stack: auto-detect
Special Requirements: [Any design requirements — dark mode, responsive, etc. Or "none"]
Reference Files: [Mockups, wireframes, existing components, or "none"]
===== EXECUTION INSTRUCTIONS =====
You are the Coordinator. Dispatch: @architect → @uiux-specialist → @builder → @security-auditor → @verifier → @memorykeeper
Full feature pipeline with UI. Do NOT stop until SESSION COMPLETE.
===== END =====
```

**Agents used:** Coordinator → Architect → UIUXSpecialist → Builder → SecurityAuditor → Verifier → MemoryKeeper

---

### 🏗️ "I want to build a full app from scratch"

```
@coordinator

===== PROJECT REQUEST =====
Project: [Describe the full app you want built — be detailed]
Tech Stack: [Your stack, e.g. "Next.js 14, TypeScript, Tailwind CSS, Prisma, PostgreSQL"]
Special Requirements: [Auth? Dark mode? AI features? Or "none"]
Reference Files: [Mockups, specs, or "none"]
===== EXECUTION INSTRUCTIONS =====
You are the Coordinator. You have all agents at your disposal:
@architect @builder @verifier @memorykeeper @prompt-engineer @security-auditor @uiux-specialist
Execute the FULL pipeline. Do NOT stop until SESSION COMPLETE.
===== END =====
```

**Agents used:** ALL project agents in sequence.

---

### 🤖 "I want to add an AI/LLM feature"

```
@coordinator

===== PROJECT REQUEST =====
Project: [Describe the AI feature — what model, what it does, user interaction]
Tech Stack: auto-detect
Special Requirements: System prompts need review by PromptEngineer. [Other requirements]
Reference Files: [Existing prompt files, API files, or "none"]
===== EXECUTION INSTRUCTIONS =====
You are the Coordinator. Dispatch: @architect → @prompt-engineer → @builder → @security-auditor → @verifier → @memorykeeper
AI feature pipeline. Do NOT stop until SESSION COMPLETE.
===== END =====
```

**Agents used:** Coordinator → Architect → PromptEngineer → Builder → SecurityAuditor → Verifier → MemoryKeeper

---

### 🧪 "I want to run tests and review code quality"

```
@coordinator

===== PROJECT REQUEST =====
Project: Run the full test suite, perform a 6-lens code review, and report quality status.
Tech Stack: auto-detect
Special Requirements: Review only — fix nothing unless tests are broken.
Reference Files: none
===== EXECUTION INSTRUCTIONS =====
You are the Coordinator. Dispatch: @verifier → @memorykeeper
Quality check pipeline only. Do NOT stop until SESSION COMPLETE.
===== END =====
```

**Agents used:** Coordinator → Verifier → MemoryKeeper

---

### 🧪+ "I want to run tests AND fix everything that fails"

```
@coordinator

===== PROJECT REQUEST =====
Project: Run the full test suite and code review. Fix all failing tests and code quality issues.
Tech Stack: auto-detect
Special Requirements: none
Reference Files: none
===== EXECUTION INSTRUCTIONS =====
You are the Coordinator. Dispatch: @verifier → @builder → @verifier → @memorykeeper
Test → Fix → Re-verify pipeline. Do NOT stop until SESSION COMPLETE.
===== END =====
```

**Agents used:** Coordinator → Verifier → Builder (fixes) → Verifier (re-check) → MemoryKeeper

---

### ♻️ "I want to refactor existing code"

```
@coordinator

===== PROJECT REQUEST =====
Project: [Describe what to refactor and why — "Extract API logic into service layer", "Split monolithic component into smaller ones", etc.]
Tech Stack: auto-detect
Special Requirements: Must not break any existing tests or functionality.
Reference Files: [Files to refactor, or "none"]
===== EXECUTION INSTRUCTIONS =====
You are the Coordinator. Dispatch: @architect → @builder → @verifier → @memorykeeper
Refactor pipeline. Do NOT stop until SESSION COMPLETE.
===== END =====
```

**Agents used:** Coordinator → Architect (plan) → Builder → Verifier → MemoryKeeper

---

### ✍️ "I want to write/improve AI prompts or agent instructions"

```
@coordinator

===== PROJECT REQUEST =====
Project: [Describe the prompt work — "Write a system prompt for a customer support chatbot", "Audit and improve existing agent instructions", etc.]
Tech Stack: N/A
Special Requirements: none
Reference Files: [Existing prompt files, or "none"]
===== EXECUTION INSTRUCTIONS =====
You are the Coordinator. Dispatch: @prompt-engineer → @memorykeeper
Prompt engineering pipeline only. Do NOT stop until SESSION COMPLETE.
===== END =====
```

**Agents used:** Coordinator → PromptEngineer → MemoryKeeper

---

### 🔍 "I want to validate the agent team itself"

```
@coordinator

===== PROJECT REQUEST =====
Project: Validate the entire agent team. Run TestRunner against all 10 agents, then AgentAuditor for conflicts/gaps.
Tech Stack: N/A
Special Requirements: none
Reference Files: All files in agents/
===== EXECUTION INSTRUCTIONS =====
You are the Coordinator. Dispatch: @test-runner → @agent-auditor → @memorykeeper
Agent validation pipeline. Do NOT stop until SESSION COMPLETE.
===== END =====
```

**Agents used:** Coordinator → TestRunner → AgentAuditor → MemoryKeeper

---

## MIX AND MATCH — Build Your Own Pipeline

Don't see your exact scenario? Build a custom pipeline. Just list the agents you want in the `EXECUTION INSTRUCTIONS` block:

```
@coordinator

===== PROJECT REQUEST =====
Project: [Your task]
Tech Stack: auto-detect
Special Requirements: [Your requirements]
Reference Files: [Your references]
===== EXECUTION INSTRUCTIONS =====
You are the Coordinator. Dispatch these agents in this exact order:
@[agent1] → @[agent2] → @[agent3] → @memorykeeper
Do NOT stop until SESSION COMPLETE.
===== END =====
```

### Agent Reference (pick what you need)

| Agent | What It Does | Use When... |
|-------|-------------|-------------|
| `@architect` | Designs system, writes spec + file map | You need planning before building |
| `@uiux-specialist` | Design system, visual specs, WCAG audit | You have frontend UI work |
| `@builder` | Writes code with TDD | You need code written or changed |
| `@security-auditor` | OWASP Top 10 scan + report | You handle auth, APIs, user input |
| `@prompt-engineer` | Writes/audits AI prompts | You have AI/LLM features |
| `@verifier` | Runs tests + 6-lens code review | You need quality verification |
| `@memorykeeper` | Commits, updates todo/lessons | **Always include last** |
| `@test-runner` | Validates agent files work | You edited agent files |
| `@agent-auditor` | Checks agent team for conflicts | After test-runner, or agent changes |

### Rules for Custom Pipelines

1. **`@memorykeeper` always goes last** — it commits and closes the session
2. **`@architect` goes before `@builder`** — design before code
3. **`@uiux-specialist` goes before `@builder`** (design mode) or after (audit mode)
4. **`@verifier` goes after `@builder`** — verify what was built
5. **`@security-auditor` goes after `@builder`** — audit what was written
6. **You can skip any agent you don't need** — Coordinator will only dispatch what you list

---

## TROUBLESHOOTING

### "Copilot doesn't recognize @coordinator"

1. Make sure the `agents/` folder is in your project root (not nested inside another folder)
2. Make sure each `.agent.md` file has YAML frontmatter at the top (the `---` block with name/description)
3. Make sure `.github/copilot-instructions.md` exists in your project
4. Restart VS Code if you just added the files

### "I copied the folder but nothing happens"

The files need to be in your **project root**, not inside a subfolder. Like this:

```
✅ CORRECT                          ❌ WRONG
your-project/                       your-project/
├── .github/                        └── ultimate-claude-prompt/
│   └── copilot-instructions.md         ├── .github/
├── agents/                             ├── agents/
├── skills/                             ├── skills/
├── docs/                               └── ...
├── tasks/
├── CLAUDE.md
└── src/
```

### "Session stopped midway / agent didn't finish"

Send this to resume:
```
@coordinator Continue from where you left off. Read tasks/todo.md and tasks/current_session_plan.md. Do not restart — pick up.
```

### "I want to use this on multiple projects"

Copy the entire set of files into each project. Each project gets its own `tasks/todo.md` and `tasks/lessons.md` that track that project's history.
