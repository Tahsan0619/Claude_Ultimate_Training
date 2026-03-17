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
Execute phases: BUILD → VERIFY → MEMORY
Bug fix only. Do NOT stop until SESSION COMPLETE.
===== END =====
```

**Phases:** Build (find & fix with TDD) → Verify (run tests) → Memory (update tasks, commit)
**What happens:** Coordinator finds and fixes the bug using TDD, confirms all tests pass, updates todo.md + lessons.md, and commits.

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
Execute phases: SECURITY → MEMORY
Security audit only. Do NOT stop until SESSION COMPLETE.
Write the report to tasks/security_audit_report.md.
===== END =====
```

**Phases:** Security (OWASP Top 10 scan) → Memory (update tasks, commit)
**What happens:** Coordinator scans every file against OWASP Top 10, writes a severity-rated report, updates todo.md + lessons.md, and commits.

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
Execute phases: SECURITY → BUILD → VERIFY → MEMORY
Audit → Fix → Verify. Do NOT stop until SESSION COMPLETE.
===== END =====
```

**Phases:** Security (audit) → Build (fix issues) → Verify (test) → Memory (commit)

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
Execute phases: DESIGN → MEMORY
UI/UX audit only. Do NOT stop until SESSION COMPLETE.
Write the report to tasks/uiux_audit_report.md.
===== END =====
```

**Phases:** Design (UI/UX audit + WCAG check) → Memory (commit)

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
Execute phases: DESIGN → BUILD → VERIFY → MEMORY
Audit → Fix → Verify. Do NOT stop until SESSION COMPLETE.
===== END =====
```

**Phases:** Design (UI/UX audit) → Build (fix issues) → Verify (test) → Memory (commit)

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
Execute phases: DESIGN → BUILD → SECURITY → VERIFY → MEMORY
Backend feature. Do NOT stop until SESSION COMPLETE.
===== END =====
```

**Phases:** Design (plan) → Build (TDD) → Security (audit) → Verify (test) → Memory (commit)

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
Execute ALL phases: DESIGN → BUILD → SECURITY → VERIFY → MEMORY
Full feature with UI. Do NOT stop until SESSION COMPLETE.
===== END =====
```

**Phases:** Design (architecture + UI spec) → Build (TDD) → Security (audit) → Verify (test) → Memory (commit)

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
Execute ALL phases: DESIGN → BUILD → SECURITY → VERIFY → MEMORY
Full build from scratch. Do NOT stop until SESSION COMPLETE.
===== END =====
```

**Phases:** ALL — Design (full architecture + UI system) → Build (TDD) → Security (OWASP audit) → Verify (full test suite) → Memory (commit)

---

### 🤖 "I want to add an AI/LLM feature"

```
@coordinator

===== PROJECT REQUEST =====
Project: [Describe the AI feature — what model, what it does, user interaction]
Tech Stack: auto-detect
Special Requirements: Review all system prompts for quality. [Other requirements]
Reference Files: [Existing prompt files, API files, or "none"]
===== EXECUTION INSTRUCTIONS =====
Execute phases: DESIGN → BUILD → SECURITY → VERIFY → MEMORY
AI feature with prompt review. Do NOT stop until SESSION COMPLETE.
===== END =====
```

**Phases:** Design (architecture + prompt review) → Build (TDD) → Security (audit) → Verify (test) → Memory (commit)

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
Execute phases: VERIFY → MEMORY
Quality check only. Do NOT stop until SESSION COMPLETE.
===== END =====
```

**Phases:** Verify (tests + 6-lens review) → Memory (commit)

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
Execute phases: VERIFY → BUILD → VERIFY → MEMORY
Test → Fix → Re-verify. Do NOT stop until SESSION COMPLETE.
===== END =====
```

**Phases:** Verify (find failures) → Build (fix) → Verify (re-check) → Memory (commit)

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
Execute phases: DESIGN → BUILD → VERIFY → MEMORY
Refactor only. Do NOT stop until SESSION COMPLETE.
===== END =====
```

**Phases:** Design (refactor plan) → Build (implement) → Verify (test) → Memory (commit)

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
Execute phases: BUILD → MEMORY
Prompt engineering only. Do NOT stop until SESSION COMPLETE.
===== END =====
```

**Phases:** Build (write/improve prompts) → Memory (commit)

---

### 🔍 "I want to validate the agent team itself"

```
@coordinator

===== PROJECT REQUEST =====
Project: Validate the entire agent team. Run synthetic tests against all 10 agents, then audit for conflicts/gaps.
Tech Stack: N/A
Special Requirements: none
Reference Files: All files in agents/
===== EXECUTION INSTRUCTIONS =====
Execute phases: VERIFY → MEMORY
Agent validation. Do NOT stop until SESSION COMPLETE.
===== END =====
```

**Phases:** Verify (test all agents + audit team) → Memory (commit)

---

## MIX AND MATCH — Build Your Own Pipeline

Don't see your exact scenario? Build a custom pipeline. Just list the phases you want in the `EXECUTION INSTRUCTIONS` block:

```
@coordinator

===== PROJECT REQUEST =====
Project: [Your task]
Tech Stack: auto-detect
Special Requirements: [Your requirements]
Reference Files: [Your references]
===== EXECUTION INSTRUCTIONS =====
Execute phases: [PHASE1] → [PHASE2] → [PHASE3] → MEMORY
Do NOT stop until SESSION COMPLETE.
===== END =====
```

### Phase Reference (pick what you need)

| Phase | What It Does | Use When... |
|-------|-------------|-------------|
| `DESIGN` | System architecture, component specs, file map, UI design | You need planning before building |
| `BUILD` | TDD implementation — failing tests first, then code | You need code written or changed |
| `SECURITY` | OWASP Top 10 scan, auth hardening, injection checks | You handle auth, APIs, user input |
| `VERIFY` | Runs full test suite + 6-lens code review | You need quality verification |
| `MEMORY` | Updates todo.md, lessons.md, git commit | **Always include last** |

### Rules for Custom Pipelines

1. **MEMORY always goes last** — it commits and closes the session
2. **DESIGN goes before BUILD** — plan before code
3. **VERIFY goes after BUILD** — verify what was built
4. **SECURITY goes after BUILD** — audit what was written
5. **You can skip any phase you don't need** — Coordinator executes only the phases you list

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
