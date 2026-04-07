---
name: review-summary
description: Consolidates findings from security-scan and code-review into a structured GitHub PR comment and a findings.json handoff for the fact-checker approver agent.
license: MIT
allowed-tools: github-pr
metadata:
  version: "1.0.0"
  category: reporting
---

# Review Summary Skill

Consolidate all findings from the security-scan and code-review skills into a human-readable PR comment and a machine-readable JSON handoff for the fact-checker approver agent.

## Summary Comment Format

```markdown
## 🤖 Reva's PR Review

**PR Health Score: {score}/100** | {critical} 🔴 Critical · {high} 🟠 High · {medium} 🟡 Medium · {low} 🔵 Low

### Summary
{2-3 sentence plain-English summary of what was found and overall assessment}

### Required Before Merge
- [ ] {each CRITICAL and HIGH finding listed as an actionable checkbox}

### Suggestions (Non-blocking)
- {each MEDIUM and LOW finding as a bullet point}

---
*Review by [Reva](.) · Powered by [gitagent](https://gitagent.sh) · Approval by: fact-checker (SOD enforced)*
```

## Findings JSON (for fact-checker handoff)

```json
{
  "pr_health_score": 72,
  "merge_blocked": false,
  "critical_count": 0,
  "high_count": 2,
  "medium_count": 3,
  "low_count": 1,
  "all_findings": []
}
```

## Rules

- Sort findings: CRITICAL → HIGH → MEDIUM → LOW
- Post inline comment on the specific diff line for each finding
- Cap at 20 inline comments — consolidate if more
- Call out ✅ things done well alongside the issues
- Append an audit entry to `memory/runtime/dailylog.md`
- Write `findings.json` to workspace root for fact-checker
