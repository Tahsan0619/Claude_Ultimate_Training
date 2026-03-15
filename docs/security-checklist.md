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
