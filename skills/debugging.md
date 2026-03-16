# Skill: Systematic Debugging

> **THE IRON LAW:** No fixes without root cause investigation first.
> **Trigger:** ANY technical issue — test failures, bugs, unexpected behavior, performance problems, build failures.

---

## When to Use — ESPECIALLY When:

- Under time pressure ("just fix it quick")
- A "quick fix seems obvious"
- You've already tried multiple fixes
- A previous fix didn't work
- You don't fully understand the issue

---

## The 4-Phase Process

### Phase 1: Root Cause Investigation

1. **Read the error message COMPLETELY** — every line, every stack frame
2. **Reproduce consistently** — find exact steps to trigger
3. **Check recent changes** — what changed since it last worked?
4. **Trace the data flow** — follow data backward from symptom to source
5. **Add instrumentation** if manual tracing fails (log at each boundary)

> The root cause is almost never where the error appears.

### Phase 2: Pattern Analysis

1. **Find working examples** — where does similar code work in this codebase?
2. **Compare against references** — completely, not just the part you think matters
3. **Identify ALL differences** — any single difference could be the cause
4. **Understand dependencies** — what does the failing code depend on?

### Phase 3: Hypothesis & Testing

1. **Form a single hypothesis:** "I think [X] is the root cause because [evidence Y]"
2. **Test minimally** — change ONE variable at a time
3. **Verify the hypothesis** before implementing the fix
4. **Don't pretend to know** — if uncertain, say so

### Phase 4: Implementation

1. **Write a failing test** that reproduces the bug
2. **Implement a single fix** addressing root cause
3. **Verify the fix** — run the test, confirm it passes
4. **Run ALL tests** — confirm nothing else broke
5. **If <3 fixes failed:** return to Phase 1
6. **If ≥3 fixes failed:** STOP — question the architecture

---

## Root Cause Tracing Technique

```
Observe Symptom
    ↓
Find Immediate Cause ("this line threw the error")
    ↓
Ask: "What called this? What supplied the bad data?"
    ↓
Trace Up the Call Stack
    ↓
Keep asking "why?" until you find the ORIGINAL trigger
    ↓
That's your root cause
```

---

## Red Flags — STOP Immediately If:

| Thought | Reality |
|---------|---------|
| "Quick fix for now" | There is no "for now." Fix it right. |
| "Just try changing X" | Guessing is not debugging. |
| "I don't fully understand but it might work" | If you don't understand, you can't fix. |
| "One more attempt" (after 2+ failures) | Repeating failed approaches ≠ progress. |
| "It works on my machine" | Investigate the environmental difference. |

---

## Defense in Depth

After fixing a bug, add validation at multiple layers:

1. **Entry Point** — reject invalid data at the API boundary
2. **Business Logic** — validate data makes sense for the operation
3. **Environment Guards** — prevent dangerous operations in wrong context
4. **Debug Instrumentation** — capture context for future forensics

---

## Condition-Based Waiting

Replace arbitrary timeouts with condition polling:

```
BAD:  sleep(5000)  // hope it's ready
GOOD: waitFor(() => service.isReady(), "service startup", 10000)
```

- Poll every 10-50ms
- Always include a timeout
- Log what you're waiting for

---

## 3-Strike Rule

After 3 failed fix attempts:
1. STOP making changes
2. Question your understanding of the architecture
3. Read the surrounding code more broadly
4. Consider if the bug is a symptom of a deeper design problem
5. Ask the user for help — bring your analysis, not just the error

---

## Iterative Retrieval Pattern

When debugging in an unfamiliar codebase and you don't know what context you need:

```
DISPATCH → Broad search for the failing component
    ↓
EVALUATE → Score results by relevance (0-1 scale)
    ↓
REFINE → Narrow search based on gaps discovered
    ↓
LOOP → Repeat if relevance < 0.8 (max 3 cycles)
```

### Cycle Rules
- **Max 3 cycles.** If you haven't found root cause in 3 retrieval loops, escalate.
- **Each cycle narrows scope.** Never repeat the same search.
- **Score every result:** 0.8+ = directly relevant, 0.5-0.7 = tangential, <0.5 = skip next time.
- **Track what you've read.** Don't re-read files.

### Example Flow
```
Cycle 1: grep "PaymentError" → found in payment.ts, webhook.ts, types.ts
  → Relevance: payment.ts=0.9, webhook.ts=0.7, types.ts=0.3
  → Gap: Where is PaymentError constructed?

Cycle 2: grep "new PaymentError" → found in payment-validator.ts
  → Relevance: payment-validator.ts=0.95
  → Gap: What data flows into the validator?

Cycle 3: Read the caller of payment-validator.ts
  → Root cause found: currency field is undefined when locale is missing
```

---

## Code Archaeology (Pre-Debug)

Before debugging unfamiliar code, MAP THE TERRITORY:

1. **Architecture scan:** What are the top-level directories? What's the dependency graph?
2. **Entry point trace:** From the user action → through middleware → to the failing code
3. **Dependency map:** What does the failing module import? What imports it?
4. **Recent changes:** `git log --oneline -10 -- [failing file]` — was it recently touched?
5. **Test coverage:** Are there existing tests? What do they cover?

Only after mapping should you form hypotheses.
