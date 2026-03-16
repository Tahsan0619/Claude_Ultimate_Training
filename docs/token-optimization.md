# Token Optimization & Model Routing

> Read when working on large projects, managing costs, or choosing models for sub-tasks.

---

## 3-Tier Model Routing

| Tier | Model | Latency | Cost | Use When |
|------|-------|---------|------|----------|
| **1 (Fast)** | Haiku | ~200ms | Cheapest | Simple searches, single-file edits, docs, exploration |
| **2 (Balanced)** | Sonnet | ~1-2s | Medium | Multi-file implementation, code review, testing (DEFAULT) |
| **3 (Deep)** | Opus | ~3-5s | Expensive | Architecture, security audit, complex debugging, multi-system reasoning |

### When to Upgrade to Opus
- First attempt with Sonnet failed
- Task spans 5+ files with complex interdependencies
- Security-critical code that can't miss vulnerabilities
- Architecture decisions affecting the whole system
- Complex debugging after 2+ failed fixes

### When Haiku Is Enough
- File exploration and search
- Simple single-file edits with clear instructions
- Documentation generation
- Code formatting / linting fixes
- Boilerplate scaffolding

---

## Context Window Management

### Current Limits (Claude Models)
| Model | Context | Max Output |
|-------|---------|-----------|
| Opus 4.6 | 200K tokens (1M beta) | 128K tokens |
| Sonnet 4.6 | 200K tokens | 128K tokens |
| Haiku 4.5 | 200K tokens | 16K tokens |

### Strategies to Stay Within Budget

**1. Progressive Disclosure**
Don't load everything at once. Structure information in layers:
- Layer 1: CLAUDE.md — always loaded (~500 tokens)
- Layer 2: Skills — loaded when relevant (~2-5K tokens each)
- Layer 3: Docs — loaded for specific tasks (~3-5K tokens each)

**2. Targeted File Reads**
```
BAD:  Read entire 500-line file when you need line 42
GOOD: Read lines 35-55 to get the function you need
```

**3. Smart Search**
- Use `grep` with specific patterns (not read entire files)
- Use glob patterns to narrow file search
- Use `mgrep` (multi-grep) when available — ~50% fewer tokens

**4. Subagent Isolation**
- Each subagent gets ONLY the context it needs
- Don't pass full conversation history to subagents
- Paste relevant content directly instead of saying "read file X"

**5. Strategic Compaction**
- Compact at logical break points (between features, not mid-task)
- Save session state to file before compacting
- Use `tasks/todo.md` to preserve progress across compactions

---

## Cost Optimization Patterns

### The 90/10 Rule
Sonnet handles 90% of coding tasks well. Only use Opus for the 10% that needs deep reasoning.

### Batch Independent Operations
```
EXPENSIVE (3 messages):
  Message 1: Read file A
  Message 2: Read file B  
  Message 3: Read file C

CHEAP (1 message):
  Message 1: Read files A, B, C simultaneously
```

### Use Thinking Efficiently
| Keyword | Depth | Token Cost | When |
|---------|-------|-----------|------|
| `think` | Standard | Low | Most tasks |
| `think hard` | Deep | Medium | Multi-step problems |
| `ultrathink` | Maximum | High | Architecture, security (Opus only) |

### Avoid Token Waste
- Don't repeat the question back before answering
- Don't explain what you're "about to do" — just do it
- Use concise variable names in generated code
- Avoid verbose comments that restate the code
- Don't generate unnecessary boilerplate

---

## Iterative Retrieval Pattern

When a subagent doesn't know what context it needs, use this 4-phase loop (max 3 cycles):

```
Phase 1: DISPATCH → Broad search (patterns, keywords)
Phase 2: EVALUATE → Score results by relevance (0-1)
Phase 3: REFINE → Narrow search based on what's missing
Phase 4: LOOP → Repeat if relevance < 0.8 (max 3 cycles)
```

**Relevance Scoring:**
- 0.8-1.0: Directly implements target → use
- 0.5-0.7: Related patterns → consider
- 0.2-0.4: Tangential → skip
- 0.0-0.2: Not relevant → exclude from next cycle
