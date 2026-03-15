# Skill: Brainstorming

> **Trigger:** "Let's build X", "Create Y", "Add feature Z", any creative or design work.
> **HARD GATE:** No code, no scaffolding, no implementation until design is approved.

---

## When to Use

- Starting any new feature, project, or component
- User describes something to build (even if it "sounds simple")
- Exploring multiple approaches to a problem
- Any work that touches architecture or user experience

## Process

### Step 1: Explore Context
- Read project files, docs, recent commits
- Understand the existing architecture and patterns
- Identify constraints and dependencies

### Step 2: Ask Clarifying Questions
- Ask focused questions (batch them, don't drip-feed)
- "What problem does this solve for the user?"
- "Are there constraints I should know about?"
- "Should this integrate with existing X or be standalone?"

### Step 3: Propose 2-3 Approaches
Present each approach with:
- **How it works** (architecture summary)
- **Pros** (what it does well)
- **Cons** (tradeoffs and risks)
- **Complexity estimate** (low / medium / high)
- **Recommendation** with reasoning

### Step 4: Design for Isolation
Whatever approach is chosen:
- Clear boundaries between components
- Well-defined interfaces (inputs/outputs)
- Independently testable units
- Minimal coupling to existing code

### Step 5: Write Design Doc
Save to `docs/specs/YYYY-MM-DD-<topic>-design.md`:
```markdown
# [Feature Name] Design Spec
**Goal:** One sentence
**Approach:** Chosen approach with rationale
**Architecture:** Component diagram or description
**Data Flow:** How data moves through the system
**Edge Cases:** Known edge cases and how they're handled
**Open Questions:** Anything still unresolved
```

### Step 6: Get Approval
- Present the design to the user for review
- Resolve any feedback
- Only after approval → invoke `writing-plans.md`

---

## Anti-Patterns

| Temptation | Reality |
|-----------|---------|
| "This is too simple to need a design" | Simple things become complex. Design anyway. |
| "I already know how to build this" | Knowing ≠ designing. Write it down. |
| "Let me just start coding and we'll see" | You'll rebuild it twice. Design first. |
| "The user just wants it done fast" | Fast without design = slow with bugs. |

---

## Output

A written design spec that:
- Fits on one page
- Anyone could implement from it
- Has clear success criteria
- Is approved by the user
