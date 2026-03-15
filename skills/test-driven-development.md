# Skill: Test-Driven Development

> **THE IRON LAW:** No production code without a failing test first.
> **Trigger:** Before writing ANY production code — new features, bug fixes, refactoring, behavior changes.

---

## The RED-GREEN-REFACTOR Cycle

### RED — Write a Failing Test
1. Write ONE minimal test showing the desired behavior
2. Run it — watch it **fail**
3. Confirm it fails for the **right reason** (feature missing, not typo/import error)

### GREEN — Make It Pass
1. Write the **simplest** code that makes the test pass
2. No extra features, no "while I'm here" additions
3. Run the test — watch it **pass**
4. Run ALL tests — confirm nothing else broke

### REFACTOR — Clean Up
1. Improve code quality (extract helpers, rename, simplify)
2. Keep tests green during refactoring
3. Do NOT add new behavior during refactor — that needs a new RED phase
4. Commit when clean

### REPEAT
Next failing test → next behavior → next commit

---

## Enforcement Rules

| Situation | Action |
|-----------|--------|
| Code written before test | **Delete it. Start over.** |
| Test passes immediately | You're testing existing behavior. Fix the test. |
| Tests not written first | Stop. Write the test. Watch it fail. |
| Can't figure out what test to write | The design isn't clear. Go back to planning. |
| "Too simple to test" | Simple code breaks. Test takes 30 seconds. Write it. |

---

## Verification Checklist

Before declaring a TDD task complete:

- [ ] Every new function/method has a test
- [ ] Every test was seen failing before implementing
- [ ] Each test failed for the **expected reason** (missing feature, not typo)
- [ ] Wrote **minimal** code to pass each test
- [ ] ALL tests pass (not just "my" tests)
- [ ] Test output is pristine (no warnings, no skips)
- [ ] Tests use real code, not mocks (mocks only when unavoidable)
- [ ] Edge cases covered (empty input, null, boundaries, errors)

---

## Common Rationalizations (and Why They're Wrong)

| Rationalization | Reality |
|----------------|---------|
| "Too simple to test" | Simple code breaks. Test takes 30 seconds. |
| "I'll test after" | Tests passing immediately prove nothing about your code. |
| "Tests after achieve the same thing" | Tests-after verify what you remembered. Tests-first discover edge cases. |
| "Already manually tested" | Manual testing is ad-hoc and non-repeatable. |
| "Deleting code I already wrote is wasteful" | Keeping unverified code is technical debt. |
| "Just this once" | That's how every bad habit starts. |

---

## Red Flags — STOP and Start Over If:

- You wrote production code before a test
- A test passes immediately without implementation
- You can't explain WHY a test failed
- You're adding tests "later" or "after"
- You hear yourself say "just this once"

---

## Testing Anti-Patterns to Avoid

1. **Testing mock behavior** instead of real behavior
2. **Test-only methods** in production classes (code that exists only to make tests pass)
3. **Mocking without understanding** what you're replacing
4. **Incomplete mocks** that only have fields you know about
5. **Asserting implementation details** instead of outcomes
