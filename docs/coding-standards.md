# Coding Standards

> Read this when starting a new project or joining an existing one.
> Override any section by adding project-specific rules in your project's CLAUDE.md.

---

## Architecture Principles

### Keep It Simple
- Start with the simplest architecture that works
- Add complexity only when current design can't handle a proven requirement
- Don't design for hypothetical future needs (YAGNI)
- Prefer flat structures over deep nesting

### Separation of Concerns
- Each file/module has ONE clear responsibility
- Business logic never touches UI code directly
- Data access is isolated from business rules
- Configuration is separate from logic

### Dependency Direction
```
UI → Business Logic → Data Access → External Services
     (never the reverse)
```

---

## Code Style

### Naming
| Element | Convention | Example |
|---------|-----------|---------|
| Variables | Descriptive, no abbreviations | `userEmail`, not `ue` or `email2` |
| Functions | Verb + noun | `validateEmail()`, `fetchUserById()` |
| Booleans | is/has/should prefix | `isValid`, `hasPermission` |
| Constants | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT` |
| Classes | PascalCase | `PaymentService` |
| Files | Match export (kebab or camel per project) | `payment-service.ts` |

### Functions
- One function, one job
- Under 30 lines (extract if longer)
- Max 3-4 parameters (use an options object for more)
- Return early for error cases — avoid deep nesting
- Pure functions when possible (no side effects)

### Error Handling
- Handle errors at **system boundaries** (user input, API calls, file I/O)
- Don't wrap every line in try/catch — trust internal code
- Errors should be informative: what failed, why, and what to do
- Never silently swallow errors (`catch(e) {}` is banned)

```
// BAD — defensive paranoia
function add(a, b) {
  if (typeof a !== 'number') throw new Error('a must be number');
  if (typeof b !== 'number') throw new Error('b must be number');
  if (isNaN(a)) throw new Error('a is NaN');
  return a + b;
}

// GOOD — trust internal callers, validate at boundaries
function add(a, b) {
  return a + b;
}
```

### Imports
- Group imports: external → internal → relative
- Sort alphabetically within groups
- No unused imports
- Prefer named exports over default exports

---

## File Organization

```
project/
├── CLAUDE.md              ← AI instructions (you are here)
├── docs/                  ← Design specs, plans, architecture docs
│   ├── specs/             ← Design documents
│   └── plans/             ← Implementation plans
├── tasks/                 ← Task tracking
│   ├── todo.md            ← Current task list
│   └── lessons.md         ← Lessons learned log
├── src/                   ← Source code
│   ├── [module]/          ← Feature/domain modules
│   │   ├── index.ts       ← Public API
│   │   ├── [module].ts    ← Implementation
│   │   └── [module].test.ts ← Tests co-located
│   └── shared/            ← Cross-cutting utilities
├── tests/                 ← Integration/E2E tests
└── config/                ← Configuration files
```

---

## Testing Standards

### Test Structure
```
describe('[ModuleName]', () => {
  describe('[functionName]', () => {
    it('should [expected behavior] when [condition]', () => {
      // Arrange — set up test data
      // Act — call the function
      // Assert — verify the result
    });
  });
});
```

### Test Naming
- `should return user when valid ID provided`
- `should throw NotFoundError when user does not exist`
- `should handle empty input gracefully`

### What to Test
- Happy path (normal usage)
- Edge cases (empty, null, boundaries)
- Error cases (what should fail and how)
- Integration points (API calls, database queries)

### What NOT to Test
- Third-party library internals
- Private implementation details
- Configuration files
- Simple getters/setters with no logic

---

## Comments

- Don't comment WHAT the code does (the code says that)
- Comment WHY when the reason isn't obvious
- Comment workarounds with links to issues/docs
- Delete commented-out code — git remembers

```
// BAD
// Increment counter by 1
counter += 1;

// GOOD
// Rate limit resets at midnight UTC, so we retry after the boundary
await waitUntil(nextMidnightUTC());
```

---

## Security Baseline

- Never hardcode secrets, API keys, or credentials
- Validate and sanitize ALL user input
- Use parameterized queries — never concatenate SQL
- Escape output to prevent XSS
- Set appropriate CORS headers
- Use HTTPS for all external communication
- Keep dependencies updated
- See `docs/security-checklist.md` for the full list
