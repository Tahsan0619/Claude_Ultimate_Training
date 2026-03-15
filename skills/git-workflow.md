# Skill: Git Workflow

> **Trigger:** Branch management, committing, merging, PR creation.

---

## Branch Strategy

```
main (or master)
  └── feat/feature-name      ← new features
  └── fix/bug-description     ← bug fixes
  └── refactor/what-changed   ← refactoring
  └── docs/what-documented    ← documentation
```

- Never push directly to `main`
- One branch per feature/fix
- Delete branches after merge

---

## Commit Messages

Format: `type(scope): description`

```
feat(auth): add JWT login endpoint
fix(api): handle null response from payment service  
refactor(db): extract query builder from user model
test(auth): add edge cases for expired tokens
docs(readme): add deployment instructions
chore(deps): update express to 4.19.2
```

**Rules:**
- Lowercase, no period at end
- Imperative mood ("add" not "added" or "adds")
- Under 72 characters
- Body for the "why", subject for the "what"

---

## Git Worktrees (Isolated Workspaces)

Use worktrees to work on features without switching branches:

```bash
# Create worktree
git worktree add .worktrees/feat-login -b feat/login

# Work in the worktree directory
cd .worktrees/feat-login

# Clean up after merge
git worktree remove .worktrees/feat-login
```

**Before creating worktrees:**
1. Verify `.worktrees/` is in `.gitignore`
2. Run project setup (npm install, pip install, etc.) in the worktree
3. Run tests to establish a clean baseline

---

## Finishing a Branch

Before merging or creating a PR:

1. **Run full test suite** — STOP if tests fail
2. **Determine base branch** — usually `main`
3. **Choose action:**
   - Merge locally → verify tests → delete branch → cleanup worktree
   - Push + create PR → `git push -u origin <branch>` then `gh pr create`
   - Keep branch as-is (for later)
   - Discard work (requires typed confirmation)

---

## Pre-Commit Checklist

- [ ] All tests pass
- [ ] No debug/temp code
- [ ] No secrets or credentials
- [ ] Commit message follows convention
- [ ] Changes are focused (one logical change per commit)
- [ ] Git diff reviewed — no unintended changes
