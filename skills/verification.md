# Skill: Verification Before Completion

> **THE IRON LAW:** No completion claims without fresh verification evidence.
> **Trigger:** Before ANY claim that something is "done", "working", "fixed", or "complete".

---

## The Gate Function

Before making ANY success claim, execute this sequence:

```
1. IDENTIFY → What command proves this works?
2. RUN     → Execute the FULL command (fresh, complete, not cached)
3. READ    → Read the FULL output (exit code, every line, count failures)
4. VERIFY  → Does the output actually confirm the claim?
5. CLAIM   → Only now can you say it's done
```

---

## What Is NOT Sufficient Evidence

| Not Evidence | Why |
|-------------|-----|
| A previous test run | State may have changed since |
| "It should pass" | Should ≠ does |
| A partial check | Part passing ≠ all passing |
| Linter passing | Linter ≠ compiler ≠ runtime |
| A subagent's success report | Verify independently |
| "I'm confident" | Confidence ≠ evidence |
| Manual inspection of code | Code looking right ≠ code working |

---

## Verification by Task Type

| Task Type | Required Verification |
|-----------|---------------------|
| Bug fix | Failing test → passes. All other tests still pass. |
| New feature | All new tests pass. All existing tests pass. |
| Refactor | All existing tests pass. Behavior unchanged. |
| Build/config change | Clean build from scratch. Tests pass. |
| UI change | Visual check at all breakpoints. Interactions work. |
| API change | Request/response matches spec. Error cases handled. |
| Database migration | Migrate up, verify schema, migrate down, migrate up again. |

---

## Red Flags — You're About to Lie If:

- You're using words like "should", "probably", "seems to"
- You feel satisfied before running the command
- You're about to commit/push/PR without running tests
- You're trusting output from a previous run
- You're about to say "done" but haven't seen green test output

---

## The Anti-Pattern Chain

```
"I made the change" 
    → "It should work now"      ← STOP HERE
    → "Let me just commit"       ← DANGER
    → "PR is ready"              ← YOU SKIPPED VERIFICATION
    → Hours wasted in review     ← CONSEQUENCE
```

## The Correct Chain

```
"I made the change"
    → Run the test suite
    → Read ALL output
    → 0 failures, 0 warnings
    → "Verified: all 47 tests pass, clean output"
    → Commit with confidence
```
