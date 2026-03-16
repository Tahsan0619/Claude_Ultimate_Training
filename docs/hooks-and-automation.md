# Hooks & Automation Reference

> Hook lifecycle systems, codebase mapping, performance profiling, and automation patterns.

---

## Hook Lifecycle Tiers

Hooks fire at specific points in the AI assistant workflow. Organized by tier:

### Tier 1: Session Layer
| Hook | Fires | Use For |
|------|-------|---------|
| `SessionStart` | Session begins | Load lessons, check todo, refresh context |
| `SessionEnd` | Session closes | Save state, update lessons, commit work |
| `ContextCompact` | Before compaction | Archive state to files before context loss |
| `ContextRestore` | After compaction | Reload state from files |

### Tier 2: Tool Guard Layer
| Hook | Fires | Use For |
|------|-------|---------|
| `PreToolUse:Write` | Before file write | Validate changes, check plan compliance |
| `PreToolUse:Edit` | Before file edit | Read current state, warn about conflicts |
| `PreToolUse:Bash` | Before shell command | Security scan, block dangerous commands |
| `PostToolUse:Write` | After file write | Update progress, remind to test |
| `PostToolUse:Edit` | After file edit | Update codebase map |

### Tier 3: Transform Layer
| Hook | Fires | Use For |
|------|-------|---------|
| `ContextInject` | Before response | Add codebase map, inject reminder |
| `ThinkingValidate` | After thinking block | Verify reasoning quality |

### Tier 4: Continuation Layer
| Hook | Fires | Use For |
|------|-------|---------|
| `ConversationTurn` | Each message | Track turn count, warn about context fill |
| `MultiTurnFlow` | After 5+ turns | Suggest compaction or planning file update |

### Tier 5: Stop Layer
| Hook | Fires | Use For |
|------|-------|---------|
| `Stop` | Session completing | Verify all phases done, run final checks |

---

## Codebase Map Strategy

Maintain a lightweight map the AI reads before every major action:

### Auto-Generate Map
```bash
# Generate a codebase map (tree + key files)
find . -type f -name "*.ts" -o -name "*.tsx" -o -name "*.py" | head -100 | sort > .codebase-map.txt
```

### Map Format (~2K tokens max)
```markdown
# Codebase Map (auto-updated)
## Key Directories
- src/auth/     → Authentication (JWT, sessions, middleware)
- src/api/      → REST endpoints (Express routes)
- src/models/   → Database models (Prisma)
- src/utils/    → Shared utilities
- tests/        → Test files (mirrors src/ structure)

## Key Files
- src/auth/middleware.ts   → Auth middleware (guards all protected routes)
- src/api/router.ts        → Central route registration
- src/models/user.ts       → User model + relations
- prisma/schema.prisma     → Database schema (source of truth)

## Entry Points
- src/index.ts             → Server startup
- src/api/router.ts        → Route tree root

## Recently Changed
- src/auth/login.ts (today)
- src/models/user.ts (today)
```

### Two-Hook Strategy (from claudekit)
1. **SessionStart hook** → inject codebase map (~2K chars, forces focus)
2. **PostToolUse hook** → silently update map as files change

This keeps AI understanding current without cluttering the conversation.

---

## Hook Performance Profiling

Hooks should NEVER slow down the workflow. Monitor:

| Metric | Threshold | Action if Exceeded |
|--------|-----------|-------------------|
| Execution time | > 5 seconds | Optimize or disable hook |
| Output length | > 10K characters | Trim output, summarize |
| Estimated tokens | > 2K tokens | Compress or split |

### Self-Check Pattern
```bash
# Profile a hook's performance
time your-hook-command 2>&1 | wc -c
# Should be: < 5 seconds, < 10,000 characters
```

---

## Claude Code Hook Configuration

### `.claude/hooks.json`
```json
{
  "SessionStart": [{
    "matcher": "",
    "hooks": [{
      "type": "command",
      "command": "cat tasks/lessons.md 2>/dev/null; cat tasks/task_plan.md 2>/dev/null; echo '---session-loaded---'"
    }]
  }],
  "PreToolUse": [{
    "matcher": "Write|Edit",
    "hooks": [{
      "type": "command",
      "command": "head -30 tasks/task_plan.md 2>/dev/null || true"
    }]
  }],
  "PostToolUse": [{
    "matcher": "Write|Edit",
    "hooks": [{
      "type": "command",
      "command": "echo 'Remember: update tasks/progress.md with what changed'"
    }]
  }],
  "Stop": [{
    "matcher": "",
    "hooks": [{
      "type": "command",
      "command": "echo 'LANDING THE PLANE: Update todo, lessons, commit, push, verify clean git status'"
    }]
  }]
}
```

---

## Quality Gate Automation

### Pre-Commit Quality Chain
```bash
# Run in order — stop on first failure
lint → test → typecheck → format-check → security-scan → commit

# Example (Node.js)
npm run lint && npm test && npx tsc --noEmit && npm run format:check

# Example (Python)
ruff check . && pytest && mypy . && black --check .
```

### CI Pipeline Pattern
```yaml
quality-gate:
  steps:
    - lint
    - test (with coverage)
    - typecheck
    - security audit (npm audit / pip-audit)
    - format check
    - build (verify it compiles)
```

---

## Automation Patterns by Environment

| Environment | Hook System | Config File |
|-------------|-------------|-------------|
| Claude Code | Native hooks | `.claude/hooks.json` |
| Cursor | Rules + hooks | `.cursorrules` + `.cursor/hooks/` |
| VS Code | Tasks + settings | `.vscode/tasks.json` |
| Git | Git hooks | `.husky/` or `.git/hooks/` |
| CI/CD | Pipeline config | `.github/workflows/` |

### Universal Git Hooks (Work Everywhere)
```bash
# .husky/pre-commit
npm run lint
npm test
npx tsc --noEmit

# .husky/commit-msg
npx commitlint --edit $1
```
