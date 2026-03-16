# Skill: Prompt Architecture

> **Purpose:** Transform vague prompts into expert-level, structured prompts using proven frameworks.
> **Trigger:** Writing prompts, crafting instructions, creating system prompts, or improving any prompt.

---

## The 7 Intent Categories

Every prompt falls into one category. Identify intent first, then select the right framework.

| Intent | Signal | Best Framework |
|--------|--------|---------------|
| **RECOVER** | "I have output but lost/need the prompt" | RPEF (Reverse Prompt Engineering) |
| **CLARIFY** | "I know roughly what I want but can't specify" | Reverse Role Prompting (AI Interview) |
| **CREATE** | "Generate something new from scratch" | CO-STAR, RISEN, TIDD-EC (see table below) |
| **TRANSFORM** | "Improve, convert, or rewrite existing content" | BAB, Self-Refine, Chain of Density |
| **REASON** | "Solve a problem requiring logic/calculation" | Chain of Thought, Tree of Thought, Plan-and-Solve |
| **CRITIQUE** | "Stress-test, verify, or attack output" | Devil's Advocate, Pre-Mortem, CAI Critique-Revise |
| **AGENTIC** | "Task requires tools with iterative reasoning" | ReAct (Reasoning + Acting) |

---

## CREATE Frameworks (Most Common)

Pick based on task complexity:

| Framework | Best For | Structure |
|-----------|----------|-----------|
| **APE** | Ultra-minimal one-off prompts | Action → Purpose → Expectation |
| **RTF** | Simple expertise tasks | Role → Task → Format |
| **CTF** | Context-driven tasks | Context → Task → Format |
| **RACE** | Expert with role + context | Role → Action → Context → Expectation |
| **CO-STAR** | Content creation & writing | Context → Objective → Style → Tone → Audience → Response |
| **RISEN** | Multi-step procedures | Role → Instructions → Steps → End goal → Narrowing |
| **TIDD-EC** | High-precision with constraints | Task → Instructions → Do → Don't → Examples → Context |
| **CARE** | Constraint-driven tasks | Context → Ask → Rules → Examples |
| **BROKE** | Business deliverables with KPIs | Background → Role → Objective → Key results → Examples |

---

## REASON Frameworks

| Framework | Best For | How It Works |
|-----------|----------|-------------|
| **Chain of Thought** | Step-by-step reasoning | "Think step by step" → explain each step |
| **Tree of Thought** | Multiple solution paths | Generate 3+ approaches → evaluate → select best |
| **Plan-and-Solve** | Numerical/logical problems | Plan steps → solve each → verify |
| **Least-to-Most** | Multi-hop compositional problems | Solve simplest subproblem first → build up |
| **Step-Back** | Abstract reasoning | Abstract the principle first → apply to specific |
| **RCoT** | Verify reasoning | Reverse-check: did the answer satisfy ALL conditions? |

---

## TRANSFORM Frameworks

| Framework | Best For |
|-----------|----------|
| **BAB** (Before-After-Bridge) | Rewrite/refactor/convert content |
| **Self-Refine** | Iterative quality improvement |
| **Chain of Density** | Compress or densify text |
| **Skeleton of Thought** | Outline-first, then expand sections |

---

## CRITIQUE Frameworks

| Framework | Best For |
|-----------|----------|
| **Devil's Advocate** | Strongest opposing argument |
| **Pre-Mortem** | Assume failure, identify causes |
| **CAI Critique-Revise** | Principle-based critique + revision |
| **RCoT** | Verify reasoning didn't miss conditions |

---

## Quality Scoring (Self-Assessment)

Score any prompt across 5 dimensions (1-10 each):

| Dimension | Question |
|-----------|---------|
| **Clarity** | Is the goal unambiguous? |
| **Specificity** | Are requirements detailed enough? |
| **Context** | Is necessary background provided? |
| **Completeness** | Are all elements present? |
| **Structure** | Is the prompt well-organized? |

- **Score < 4:** Use a structured framework (RISEN, TIDD-EC)
- **Score 4-7:** Enhance with missing elements
- **Score > 7:** Refine and optimize

---

## XML Structuring for Complex Prompts

Use XML tags to organize complex instructions:

```xml
<task>
  <objective>Create a financial report</objective>
  <constraints>
    <length>500-750 words</length>
    <format>Prose only, no bullet lists</format>
    <tone>Professional but accessible</tone>
  </constraints>
</task>

<thinking>
Before responding:
1. What are the key components?
2. What assumptions am I making?
3. What are potential solutions?
4. What are the trade-offs?
</thinking>

<output_format>
  Provide reasoning first, then the answer.
</output_format>
```

---

## Extended Thinking Triggers

| Phrase | Effect |
|--------|--------|
| `think` | Standard reasoning activation |
| `think hard` | Deeper reasoning with more exploration |
| `ultrathink` | Maximum reasoning depth (Opus only) |

---

## Role Prompting (Most Powerful Technique)

```
You are a [role] with [years] of experience in [domain].
You specialize in [specific expertise].
Your audience is [who you're speaking to].
[Specific situational context that shapes the response.]
```

**Example:**
```
You are a senior security engineer with 15 years of experience 
auditing production systems. You specialize in OWASP Top 10 
vulnerabilities. Your audience is a development team that needs 
specific, actionable remediation steps. The codebase is a 
Node.js/Express API handling payment processing.
```

---

## Anti-Sycophancy Rules

When asking Claude to evaluate or critique:
- Tell it to give honest, independent judgment
- Tell it to push back when it disagrees
- Don't reward agreement — reward accuracy
- "I want the truth, not what I want to hear"

---

## Quick Prompt Improvement Checklist

- [ ] Does it say WHO Claude is? (role)
- [ ] Does it say WHAT to do? (task)
- [ ] Does it say HOW to format output? (format)
- [ ] Does it provide CONTEXT? (background)
- [ ] Does it set CONSTRAINTS? (boundaries)
- [ ] Does it include EXAMPLES? (few-shot)
- [ ] Does it specify what NOT to do? (anti-patterns)
