---
name: security-scan
description: Scans PR diffs for hardcoded secrets, SQL injection, XSS, command injection, auth bypasses, and weak cryptography. Always runs first and triggers immediate human escalation when critical issues are found.
license: MIT
allowed-tools: github-pr bash-runner
metadata:
  version: "1.0.0"
  category: security
  runs-first: "true"
---

# Security Scan Skill

Scan the PR diff for security vulnerabilities. This skill ALWAYS runs first — before code review — and can immediately halt the pipeline.

## Checks

### Secrets & Credentials
- Hardcoded API keys, passwords, tokens (AWS, GCP, Stripe, GitHub, etc.)
- Private keys or certificates committed to source
- `.env` files accidentally included in the diff

### Injection Vulnerabilities
- SQL injection via string concatenation in queries
- XSS via unescaped user input rendered as HTML
- Command injection via `exec()`, `shell_exec()`, `subprocess` with user input
- Path traversal via unvalidated file paths

### Authentication & Authorization
- Auth checks removed or commented out
- JWT secrets hardcoded in source
- Password hashing weakened (MD5, SHA1, plain text storage)
- Admin endpoints missing auth middleware

### Cryptography
- Weak algorithms: MD5, SHA1, DES, RC4
- Hardcoded IV or salt values
- Insecure random number generators used for security-sensitive operations

## Output Format

```json
{
  "escalate_immediately": false,
  "critical_count": 0,
  "secrets_found": false,
  "auth_changes_detected": false,
  "findings": []
}
```

## Escalation Rule

Set `escalate_immediately: true` if ANY of the following are found:
- Hardcoded secret or credential in any file
- SQL injection, XSS, or RCE vulnerability
- Auth or encryption logic removed or weakened
- Security tests or scanning rules disabled
