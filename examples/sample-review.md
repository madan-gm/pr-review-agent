# Example — Sample PR Review Interaction

## Input (PR Diff)

```diff
--- a/src/auth/login.js
+++ b/src/auth/login.js
@@ -10,6 +10,14 @@
 const express = require('express');
 const router = express.Router();
 
+const DB_PASSWORD = "supersecret123";
+
+router.post('/login', (req, res) => {
+  const query = "SELECT * FROM users WHERE username = '" + req.body.username + "'";
+  db.query(query, (err, result) => {
+    res.json(result);
+  });
+});
+
 module.exports = router;
```

---

## Expected Agent Output

### Security Scan Result
```json
{
  "skill": "security-scan",
  "escalate_immediately": true,
  "critical_count": 2,
  "secrets_found": true,
  "findings": [
    {
      "file": "src/auth/login.js",
      "line": 13,
      "severity": "CRITICAL",
      "category": "hardcoded-secret",
      "message": "Hardcoded database password found in source code",
      "suggestion": "Move to environment variable: process.env.DB_PASSWORD"
    },
    {
      "file": "src/auth/login.js",
      "line": 16,
      "severity": "CRITICAL",
      "category": "sql-injection",
      "message": "SQL injection vulnerability — user input concatenated directly into query",
      "suggestion": "Use parameterized queries",
      "code_fix": "const query = 'SELECT * FROM users WHERE username = ?';\ndb.query(query, [req.body.username], (err, result) => {"
    }
  ]
}
```

### PR Comment Posted

> ## 🤖 Reva's PR Review
>
> **PR Health Score: 10/100** | 2 🔴 Critical · 0 🟠 High · 0 🟡 Medium · 0 🔵 Low
>
> ### ⛔ Escalated to Human Review
> Two critical security issues were found. This PR is blocked pending human review.
>
> ### Required Before Merge
> - [ ] 🔴 Remove hardcoded `DB_PASSWORD` — use `process.env.DB_PASSWORD`
> - [ ] 🔴 Fix SQL injection in login query — use parameterized queries
>
> ---
> *Review by Reva · Approval decision by: fact-checker (SOD enforced)*

### Fact-Checker Decision
```
Decision: REQUEST_CHANGES
Reason: 2 CRITICAL findings open. Cannot approve.
Escalation: Human reviewer notified.
```
