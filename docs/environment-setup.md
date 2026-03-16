# Environment Setup Guide

> How to use this prompt system in any AI coding environment.
> Copy the `ultimate-claude-prompt/` folder into your project root and follow the setup for your platform.

---

## Universal Setup (All Environments)

1. Copy this entire folder into your project root
2. Claude reads `CLAUDE.md` automatically at session start
3. Skills and docs load on demand (not all at once)
4. Create `tasks/todo.md` and `tasks/lessons.md` for session tracking

---

## Claude Code (Terminal CLI)

### Setup
```bash
# CLAUDE.md is auto-read from project root
# No additional setup needed

# Skills can also be placed in:
~/.claude/skills/          # Global (all projects)
.claude/skills/            # Project-specific
```

### Features Available
- Full file system access
- Bash/terminal commands
- Git operations
- Subagent dispatching (@agent syntax)
- Hooks (PreToolUse, PostToolUse, SessionStart, Stop)
- Slash commands (/plan, /explore, /tdd, etc.)

### Hooks Configuration
Create `.claude/hooks.json`:
```json
{
  "SessionStart": [{
    "matcher": "startup|resume|clear|compact",
    "hooks": [{
      "type": "command",
      "command": "cat tasks/lessons.md 2>/dev/null || echo 'No lessons yet'"
    }]
  }]
}
```

### Agent Files
Place agent definitions in `.claude/agents/`:
```yaml
---
name: code-reviewer
description: "Invoke after completing code to review quality and security"
tools: Read, Grep, Glob
---
Review the code changes for quality, security, and adherence to project standards.
```

---

## VS Code + GitHub Copilot

### Setup
- Place `CLAUDE.md` in project root (Copilot reads `.github/copilot-instructions.md` natively)
- Copy key instructions into `.github/copilot-instructions.md`
- Or reference: "Follow the instructions in CLAUDE.md"

### Adapt For Copilot
```markdown
# .github/copilot-instructions.md
Follow all instructions in CLAUDE.md at the project root.
When applicable, load and follow skills from the skills/ directory.
```

---

## Cursor

### Setup
- Place `CLAUDE.md` in project root
- Cursor reads `.cursorrules` natively — create one that references CLAUDE.md:

```markdown
# .cursorrules
Follow all instructions in CLAUDE.md at the project root.
For any coding task, follow the 5-phase workflow: Research → Plan → Execute → Verify → Learn.
Always follow TDD: write failing test first, then implement.
```

### Agent Files
Place in `.cursor/agents/` with same YAML format.

---

## Windsurf

### Setup
- Place `CLAUDE.md` in project root
- Create `.windsurfrules` referencing the instruction set:

```markdown
Follow all instructions in CLAUDE.md in the project root.
Load applicable skills from skills/ directory on demand.
```

---

## Claude API (Direct)

### System Prompt
When using Claude via API, include CLAUDE.md content in the system prompt:

```python
import anthropic

client = anthropic.Anthropic()

# Load the master instructions
with open("CLAUDE.md") as f:
    system_prompt = f.read()

# Optionally append relevant skills
with open("skills/test-driven-development.md") as f:
    system_prompt += "\n\n" + f.read()

response = client.messages.create(
    model="claude-opus-4-6-20250310",
    max_tokens=8192,
    system=system_prompt,
    messages=[{"role": "user", "content": "Build a login API endpoint"}]
)
```

### Thinking Configuration
```python
response = client.messages.create(
    model="claude-opus-4-6-20250310",
    max_tokens=16384,
    thinking={"type": "adaptive"},     # Auto-calibrated reasoning
    # OR for manual control:
    # output_config={"effort": "max"},  # Deepest reasoning (Opus only)
    system=system_prompt,
    messages=[...]
)
```

---

## Claude.ai Web Interface

### Setup
- Enable Skills in Settings > Capabilities
- Paste key instructions from CLAUDE.md into the project instructions
- Or start each conversation with: "Follow these instructions: [paste CLAUDE.md]"

### Pro Tip
For recurring projects, create a Claude Project and set CLAUDE.md as the project knowledge base.

---

## Multi-Agent Environments

### Claude Squad (Parallel Instances)
```bash
# Each instance gets its own git worktree
cs  # Launch Claude Squad

# Create focused sessions for parallel work
# Each session operates in isolation
# Review diffs before committing
```

### Git Worktrees (Manual Parallelism)
```bash
# Create isolated workspace per feature
git worktree add .worktrees/feat-auth -b feat/auth
git worktree add .worktrees/feat-payments -b feat/payments

# Run Claude in each
cd .worktrees/feat-auth && claude
```

---

## Folder Structure When Installed

```
your-project/
├── CLAUDE.md                    ← Master instructions (auto-read)
├── skills/                      ← Workflow disciplines
│   ├── brainstorming.md
│   ├── writing-plans.md
│   ├── task-planning.md         ← NEW: File-based persistent planning
│   ├── test-driven-development.md
│   ├── debugging.md
│   ├── verification.md
│   ├── agent-orchestration.md
│   ├── subagent-development.md
│   ├── code-review.md
│   ├── prompt-architecture.md
│   └── git-workflow.md
├── docs/                        ← Reference guides
│   ├── coding-standards.md
│   ├── ui-ux-guidelines.md
│   ├── accessibility.md
│   ├── security-checklist.md
│   ├── performance.md
│   ├── prompt-engineering.md
│   ├── token-optimization.md
│   ├── continuous-learning.md
│   ├── environment-setup.md     ← This file
│   └── hooks-and-automation.md  ← NEW: Hook lifecycle, quality gates
├── tasks/                       ← Living tracking
│   ├── todo.md
│   └── lessons.md
└── [your project files]
```

---

## Customizing for Your Project

Add a project-specific section at the TOP of CLAUDE.md:

```markdown
## Project: [Your Project Name]
**Stack:** React 18 + Next.js 14 + PostgreSQL + Prisma
**Test command:** `npm test`
**Build command:** `npm run build`  
**Lint command:** `npm run lint`
**Deploy:** Vercel (auto from main branch)

### Project-Specific Rules
- Use server components by default, client only when needed
- All API routes in app/api/ with Zod validation
- Database changes need migration: `npx prisma migrate dev`
- Component naming: PascalCase, co-located .test.tsx files
```

User instructions ALWAYS override template defaults.
