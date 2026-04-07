# Compliance Audit Checklist

Run `gitagent audit` to verify all items below.

## Agent Identity
- [x] `agent.yaml` present and valid
- [x] `SOUL.md` present with identity, values, communication style
- [x] `RULES.md` present with must-always and must-never constraints
- [x] `DUTIES.md` present with role definitions and conflict matrix

## Segregation of Duties
- [x] `reviewer` and `approver` roles defined
- [x] Conflict declared: `[reviewer, approver]`
- [x] `pr-review-agent` assigned only `reviewer` role
- [x] `fact-checker` assigned only `approver` role
- [x] Handoff defined for `pr_approval` requiring both roles
- [x] Enforcement mode: `strict`

## Security
- [x] No secrets in any committed file
- [x] `.env` listed in `.gitignore`
- [x] `GITHUB_TOKEN` marked `secret: true` in tool schema
- [x] `bash-runner` tool has allowed_commands allowlist

## Audit Trail
- [x] `memory/runtime/dailylog.md` initialized
- [x] Bootstrap hook writes startup entry
- [x] Teardown hook writes session summary
- [x] All review decisions logged with timestamp

## Workflow
- [x] `security-scan` runs before `code-review` (no conditions)
- [x] `code-review` has condition: `escalate_immediately == false`
- [x] Escalation path defined for critical findings
- [x] Error handling with notification channel defined

## Sub-Agent
- [x] `agents/fact-checker/agent.yaml` valid
- [x] `agents/fact-checker/SOUL.md` present
- [x] `agents/fact-checker/DUTIES.md` declares only `approver` role
