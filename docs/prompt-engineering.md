# Prompt Engineering Reference

> Techniques for getting the best output from Claude in any context.

---

## Instruction Design Principles

### 1. Be Specific, Not Vague
```
BAD:  "Make it better"
GOOD: "Reduce function to under 20 lines by extracting the validation logic into a separate helper"
```

### 2. Show the Structure You Want
```
BAD:  "Write documentation"
GOOD: "Write a README with these sections: ## Overview, ## Installation, ## Usage, ## API Reference"
```

### 3. Use Constraints to Focus Output
```
BAD:  "Explain this code"
GOOD: "In 3 sentences, explain what this function does and what edge case it handles"
```

### 4. Provide Context, Not Just Instructions
```
BAD:  "Fix the bug"
GOOD: "Users report a 500 error when submitting the form with empty email. The endpoint is /api/register in src/auth/register.ts. Expected: validation error returned. Actual: unhandled null reference."
```

---

## CLAUDE.md Best Practices

### Structure Your CLAUDE.md
```markdown
# Project Context
[What this project does, who uses it, what matters most]

# Build & Run Commands
[Exact commands — don't make Claude guess]

# Architecture & Conventions
[Patterns, naming, file organization rules]

# Testing
[How to run tests, what framework, what coverage target]

# Known Issues / Gotchas
[Things that will trip up an AI if not warned]
```

### The Key Insight
Claude reads CLAUDE.md at the start of every session. Put the most important rules first. Link to details instead of putting everything in one file.

---

## Context Engineering

### Progressive Disclosure
Don't dump everything at once. Structure information in layers:
1. **Top level:** The rule (1-2 lines)
2. **Linked doc:** The full explanation (read on demand)
3. **Examples:** In the linked doc, not the top level

### Token Efficiency
- Brief bullet points > long paragraphs
- Tables > prose for structured data
- Links to detail files > inline essays
- Frequently-loaded instructions < 200 words total

### Skill Design Pattern
```yaml
name: descriptive-name
description: "Use when [trigger condition] — NOT 'this skill does X'"
```
The description should be about WHEN TO USE, not what it does.

---

## Effective Prompting Patterns

### Chain of Thought
"Think step by step about [problem]. For each step, explain your reasoning before giving the answer."

### Role Assignment
"You are a senior security engineer reviewing this code. Look for OWASP Top 10 vulnerabilities."

### Constraint Setting
"Respond in valid JSON only. No markdown, no explanation."

### Few-Shot Examples
"Here are 3 examples of good commit messages:
- feat(auth): add JWT refresh token rotation
- fix(api): handle timeout in payment webhook
- refactor(db): extract query builder pattern

Now write a commit message for [change]."

### Self-Verification Prompt
"After writing this code, review it and identify any bugs, edge cases, or improvements. Fix them before presenting the final version."

---

## Anti-Patterns to Avoid

| Pattern | Problem | Better Approach |
|---------|---------|----------------|
| Wall of text instructions | Claude loses focus | Structured headers + bullet points |
| Contradictory rules | Claude picks randomly | Resolve conflicts, set priority |
| "Be smart about it" | Ambiguous guidance | Specify exact behavior wanted |
| Re-explaining every session | Wastes context window | Put it in CLAUDE.md once |
| Too many rules at once | Diminishing returns | Focus on top 5-10 most important |

---

## XML Tag Structuring

XML tags are Claude's native structuring format — use them to separate concerns:

```xml
<instructions>
  High-level behavioral rules
</instructions>

<context>
  Background information, project details
</context>

<examples>
  <example>Input → Output pairs Claude should follow</example>
</examples>

<constraints>
  Hard limits: length, format, what NOT to do
</constraints>

<output_format>
  Exact structure of expected response
</output_format>
```

### Nesting for Complex Prompts
```xml
<task>
  <objective>Build REST API for user management</objective>
  <requirements>
    <requirement priority="high">JWT authentication</requirement>
    <requirement priority="medium">Rate limiting</requirement>
  </requirements>
  <constraints>
    <constraint>No external auth libraries</constraint>
    <constraint>PostgreSQL only</constraint>
  </constraints>
</task>
```

### Why XML Beats Markdown for System Prompts
- Claude's training data has heavy XML exposure → better parsing
- Tags create unambiguous boundaries (no markdown heading ambiguity)
- Nesting communicates hierarchy that flat bullet points can't
- Easier to programmatically generate and validate

---

## Extended Thinking Control

### Trigger Words (Budget Keywords)
| Keyword | Depth | Use When |
|---------|-------|----------|
| `think` | Standard | Default for most tasks |
| `think step by step` | Methodical | Logical reasoning chains |
| `think hard` | Deep | Multi-step problems, debugging |
| `think harder` | Very deep | Architecture, complex bugs |
| `ultrathink` / `megathink` | Maximum | Security audits, novel algorithms (Opus only) |

### When to Force Deep Thinking
- Mathematical proofs or formal reasoning
- Security vulnerability analysis
- Architecture decisions with long-term consequences
- Debugging after 2+ failed attempts
- Code that handles money, auth, or PII

### When Standard Thinking Is Fine
- File reading and search operations
- Simple CRUD implementation
- Documentation generation
- Code formatting and style fixes

---

## System Prompt Patterns (from 110+ real prompts)

### The Authority Pattern
Give Claude a specific identity and domain mastery:
```
You are a principal engineer at [domain]. You have 15 years of experience with [technology]. You treat every code review as if it ships to production tomorrow.
```

### The Constraint-First Pattern
Lead with what NOT to do:
```
NEVER:
- Generate code without tests
- Skip error handling
- Use deprecated APIs
- Hardcode secrets

ALWAYS:
- Validate all external input
- Use parameterized queries
- Log at decision boundaries
- Return structured errors
```

### The Persona Stack
Layer multiple perspectives for comprehensive output:
```
Review this code as:
1. A security engineer (OWASP Top 10)
2. A performance engineer (Big-O, memory)
3. A junior dev reading it for the first time (clarity)
4. A QA engineer writing edge case tests
```

---

## Anti-Sycophancy Techniques

Claude tends to agree with the user. Counter this:

```markdown
## Critical Rules
- If my approach has issues, tell me DIRECTLY — don't sugarcoat
- If a request conflicts with best practices, push back with reasoning
- Never say "great idea" unless it genuinely is
- If you're uncertain, say "I'm not sure" — don't fabricate confidence
- Challenge assumptions when evidence suggests they're wrong
```

### Force Honest Evaluation
```
Rate this code 1-10 with specific justification. A score above 7 requires citing specific strengths. A score below 5 requires specific fixes.
```

---

## Multi-Window / Long Task Guidance

For tasks that span many messages:

### State Tracking Pattern
```markdown
Before each response, output a brief status block:

## Current State
- Phase: [Planning | Implementation | Testing | Review]
- Progress: [X of Y tasks complete]
- Blockers: [none | describe]
- Next: [immediate next action]
```

### Checkpoint Pattern
After every 3-5 messages of work, output:
```markdown
## Checkpoint
✅ Completed: [list]
🔄 In Progress: [current task + status]
📋 Remaining: [list]
⚠️ Issues Found: [any problems discovered]
```

---

## Force Web Search (When Available)

When Claude has web access and needs current information:
```
Search the web for [specific query]. Do NOT rely on training data for this — I need current information as of today.
```

Or force freshness:
```
What is the latest version of [library]? Check the web, don't guess from memory.
```
