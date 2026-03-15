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
