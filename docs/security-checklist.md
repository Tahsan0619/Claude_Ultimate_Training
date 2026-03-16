# Security Checklist

> Read before deploying, handling user data, or touching authentication.

---

## OWASP Top 10 — Quick Reference

### 1. Broken Access Control
- [ ] Every endpoint checks authorization (not just authentication)
- [ ] Users can only access their own data
- [ ] Admin functions are gated behind role checks
- [ ] Directory listing is disabled
- [ ] CORS is configured to specific origins (not `*`)

### 2. Cryptographic Failures
- [ ] Passwords hashed with bcrypt/argon2 (never MD5/SHA1)
- [ ] Sensitive data encrypted at rest
- [ ] HTTPS enforced for all traffic
- [ ] No secrets in code, logs, or error messages
- [ ] API keys stored in environment variables or secret manager

### 3. Injection
- [ ] SQL: parameterized queries only (no string concatenation)
- [ ] XSS: all user content escaped before rendering
- [ ] Command injection: no `exec()` with user input
- [ ] Path traversal: validate file paths, no `../` allowed
- [ ] Template injection: use safe templating engines

### 4. Insecure Design
- [ ] Rate limiting on authentication endpoints
- [ ] Account lockout after failed attempts
- [ ] Input validation at system boundaries
- [ ] Principle of least privilege (minimum permissions)

### 5. Security Misconfiguration
- [ ] Debug mode disabled in production
- [ ] Default credentials changed
- [ ] Error messages don't leak stack traces
- [ ] Unnecessary features/ports/services disabled
- [ ] Security headers set (CSP, X-Frame-Options, etc.)

### 6. Vulnerable Components
- [ ] Dependencies are up to date
- [ ] No known CVEs in dependency tree
- [ ] Lock files committed (package-lock.json, etc.)
- [ ] Regular dependency audits scheduled

### 7. Authentication Failures
- [ ] Strong password requirements enforced
- [ ] MFA available for sensitive operations
- [ ] Sessions expire after inactivity
- [ ] Tokens are properly invalidated on logout
- [ ] Password reset doesn't reveal if account exists

### 8. Data Integrity Failures
- [ ] CI/CD pipeline has integrity checks
- [ ] Dependencies verified (checksums, signatures)
- [ ] No deserialization of untrusted data
- [ ] Code reviews required before merge

### 9. Logging & Monitoring
- [ ] Authentication events logged (success and failure)
- [ ] Authorization failures logged
- [ ] Input validation failures logged
- [ ] Logs don't contain sensitive data (passwords, tokens)
- [ ] Alerting configured for suspicious patterns

### 10. Server-Side Request Forgery (SSRF)
- [ ] URL inputs validated and sanitized
- [ ] Internal network access restricted
- [ ] Allowlists for external service URLs
- [ ] No user-controlled redirect targets without validation

---

## Quick Rules

| Principle | Application |
|-----------|------------|
| **Never trust user input** | Validate, sanitize, escape |
| **Least privilege** | Minimum permissions always |
| **Defense in depth** | Multiple layers of protection |
| **Fail securely** | Errors should deny access, not grant it |
| **Keep it simple** | Complex security = broken security |

---

## Agent Safety Rules (for AI-Assisted Development)

### Defensive Shell Flags
When running commands in AI agent contexts, use non-interactive flags to prevent hangs:
```bash
cp -f source dest        # NOT: cp source dest (may prompt for overwrite)
rm -f file               # NOT: rm file (may prompt for confirmation)
mv -f source dest        # NOT: mv source dest
scp -o BatchMode=yes     # Prevents SSH password prompts
git merge --no-edit      # Skip editor for merge commits
```

### Credential Isolation
- **Never expose secrets to agent tools** — use a credential proxy or env vars
- Agents should request auth tokens via a secure service, never handle raw credentials
- API keys in environment variables, not in `.env` files committed to git
- Container-based agents: mount secrets as read-only volumes, never bake into images

### API Rate-Limit Budgeting
When agents interact with external APIs:
```
1. Check available quota: gh api rate_limit
2. Budget: 3-5 API calls total per CI wait
3. Strategy: sleep 10-15 min → check ONCE → sleep → check again
4. NEVER poll in tight loops — burns quota instantly
```

### Agent Command Blocklist
Block or gate these in CI/hooks:
- `rm -rf /` or any recursive delete on root
- `git push --force` without explicit user approval
- `DROP TABLE`, `DELETE FROM` without WHERE clause
- Any command containing secrets in arguments (visible in process list)

---

## Secrets Management

```
NEVER:
  - Hardcode in source code
  - Commit to git
  - Put in client-side code
  - Log to console/files
  - Send in error messages

ALWAYS:
  - Use environment variables
  - Use secret managers (Vault, AWS Secrets, etc.)
  - Rotate regularly
  - Audit access
  - .env in .gitignore
```
