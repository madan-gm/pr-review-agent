---
name: code-review
description: Analyzes PR diffs for bugs, unhandled errors, N+1 queries, missing error handling, and missing test coverage. Produces structured findings with severity ratings and code fix suggestions.
license: MIT
allowed-tools: github-pr
metadata:
  version: "1.0.0"
  category: quality
---

# Code Review Skill

Analyze the provided PR diff for code quality issues. Only flag lines present in the diff — never flag pre-existing code.

## Checks to Perform

For each changed hunk, check for:
- Null/undefined dereferences without guards
- Unhandled promise rejections or missing try/catch
- Off-by-one errors in loops and array indexing
- N+1 query patterns (database calls inside loops)
- Missing input validation on public-facing functions
- Dead code (unreachable branches, unused variables)
- Missing error handling on I/O operations
- Race conditions in async code
- New public functions without test coverage

## Output Format

For each finding produce:
```json
{
  "file": "src/auth/login.js",
  "line": 42,
  "severity": "HIGH",
  "category": "error-handling",
  "message": "Promise rejection not handled — will crash Node process",
  "suggestion": "Wrap in try/catch or add .catch() handler",
  "code_fix": "try {\n  await loginUser(req.body);\n} catch (err) {\n  res.status(500).json({ error: err.message });\n}"
}
```

## Health Score Calculation

Start at 100, then:
- Subtract 30 per CRITICAL finding
- Subtract 15 per HIGH finding
- Subtract 5 per MEDIUM finding
- Subtract 1 per LOW finding
- Minimum score is 0

## Rules

- Only flag lines that appear in the diff hunks
- Always include a working code_fix for CRITICAL and HIGH findings
- Also flag ✅ things done well (good error handling, clear naming, solid tests)
- Cap total findings at 20 — consolidate if more exist
