# DUTIES — Segregation of Duties Policy

## Purpose

This document defines the role boundaries for the PR Review multi-agent pipeline.
No single agent controls the full review-to-approval lifecycle.

## Roles

### `reviewer` — pr-review-agent (this agent)
- **Can:** Analyze code diffs, post inline comments, generate findings reports, flag issues, request changes
- **Cannot:** Approve PRs, merge branches, dismiss other reviewers' comments

### `approver` — fact-checker (sub-agent)
- **Can:** Read reviewer findings, verify critical findings, approve or reject PRs, escalate to humans
- **Cannot:** Perform code analysis, post new findings, modify reviewer output

### `auditor` — (logged to memory/)
- **Can:** Read all agent actions, write to audit log
- **Cannot:** Modify findings or approvals

## Conflict Matrix

| Role A | Role B | Conflict? |
|--------|--------|-----------|
| reviewer | approver | ✅ YES — same agent cannot do both |
| reviewer | auditor | ❌ No |
| approver | auditor | ❌ No |

## Handoff Protocol

```
PR Opened
    │
    ▼
pr-review-agent (reviewer)
    │  Runs: security-scan → code-review → review-summary
    │  Output: findings.json + inline PR comments
    │
    ▼
fact-checker (approver)
    │  Reads: findings.json
    │  Verifies: all CRITICAL findings acknowledged
    │  Decision: APPROVE / REQUEST_CHANGES / ESCALATE_HUMAN
    │
    ▼
Human Reviewer (if escalated)
    │  Required for: secrets found, auth changes, >2000 line diffs
    ▼
Merge / Block
```

## Enforcement

`enforcement: strict` — gitagent validate --compliance will block deployment
if the conflict matrix is violated.
