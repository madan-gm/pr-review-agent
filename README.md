# 🤖 PR Review Agent — GitAgent Hackathon Submission

> **Reva** — A git-native AI code reviewer that lives in your repo, reviews every PR automatically, and enforces segregation of duties so the reviewer can never approve its own findings.

Built with the [gitagent](https://gitagent.sh) open standard for the **GitAgent Hackathon by Lyzr AI**.

---

## What It Does

Every time a pull request is opened or updated, Reva:

1. 🔐 **Scans for security issues first** — secrets, SQL injection, XSS, auth bypasses
2. 🐛 **Reviews code quality** — bugs, unhandled errors, N+1 queries, missing tests
3. 📝 **Posts structured inline comments** — with severity ratings and code fixes
4. 🤝 **Hands off to the fact-checker** — a separate approver agent that makes the final merge decision (SOD enforced)
5. 📋 **Logs everything** — full audit trail in `memory/runtime/dailylog.md`

## Why This Wins

| Feature | What We Use |
|--------|-------------|
| Full gitagent standard | `agent.yaml`, `SOUL.md`, `RULES.md`, `DUTIES.md`, `AGENTS.md` |
| Multi-skill workflow | `security-scan` → `code-review` → `review-summary` |
| Multi-agent SOD | `pr-review-agent` (reviewer) + `fact-checker` (approver) |
| SkillsFlow workflow | `workflows/pr-review-flow.yaml` with `depends_on` and conditions |
| MCP-compatible tools | `tools/github-pr.yaml`, `tools/bash-runner.yaml` |
| Lifecycle hooks | `hooks/bootstrap.md`, `hooks/teardown.md` |
| Persistent memory | `memory/runtime/dailylog.md` |
| Knowledge base | `knowledge/code-review-guide.md` |
| CI/CD integration | `.github/workflows/gitagent-ci.yml` |
| Compliance | `compliance/audit-checklist.md` |
| Calibration examples | `examples/sample-review.md` |

## Repo Structure

```
pr-review-agent/
├── agent.yaml                          # Manifest + SOD compliance config
├── SOUL.md                             # Reva's identity, personality, values
├── RULES.md                            # Hard constraints (must-always/never)
├── DUTIES.md                           # SOD policy — reviewer ≠ approver
├── AGENTS.md                           # Framework-agnostic run instructions
├── skills/
│   ├── code-review/SKILL.md            # Bug, quality, performance analysis
│   ├── security-scan/SKILL.md          # Secrets, injection, auth vulnerabilities
│   └── review-summary/SKILL.md        # Consolidate findings, post PR comment
├── tools/
│   ├── github-pr.yaml                  # GitHub PR API tool schema
│   └── bash-runner.yaml               # Shell command tool
├── workflows/
│   └── pr-review-flow.yaml            # SkillsFlow: scan → review → summarize → handoff
├── agents/
│   └── fact-checker/                  # Sub-agent: approver (SOD-separated)
│       ├── agent.yaml
│       ├── SOUL.md
│       └── DUTIES.md
├── knowledge/
│   └── code-review-guide.md           # Bug patterns, security patterns, good code signals
├── memory/runtime/
│   └── dailylog.md                    # Persistent audit trail
├── hooks/
│   ├── bootstrap.md                   # Startup: load knowledge, verify env
│   └── teardown.md                    # Shutdown: flush logs, clean up
├── compliance/
│   └── audit-checklist.md            # Full compliance verification list
├── examples/
│   └── sample-review.md              # Calibration: SQL injection + hardcoded secret example
├── .github/workflows/
│   └── gitagent-ci.yml               # CI: validate on push, review on PR
└── .gitignore                         # Keeps secrets and runtime state local
```

## Quick Start

```bash
# 1. Install gitagent CLI
npm install -g gitagent

# 2. Clone this agent
git clone https://github.com/your-username/pr-review-agent
cd pr-review-agent

# 3. Set up environment (never committed)
cp .env.example .env
# Edit .env with your ANTHROPIC_API_KEY and GITHUB_TOKEN

# 4. Validate the agent definition
gitagent validate --compliance

# 5. See agent info
gitagent info

# 6. Export to system prompt (works with any LLM)
gitagent export --format system-prompt

# 7. Run with Lyzr adapter
gitagent run . --adapter lyzr
```

## GitHub Actions Setup

Add these secrets to your repo (`Settings → Secrets → Actions`):
- `ANTHROPIC_API_KEY` — Your Anthropic API key
- `GITHUB_TOKEN` — Auto-provided by GitHub Actions

Then push a PR — Reva will review it automatically. ✅

## Segregation of Duties

```
PR Opened → Reva (reviewer) analyzes → fact-checker (approver) decides → Merge/Block
```

The reviewer **cannot** approve. The approver **cannot** analyze code.
This is enforced by `gitagent validate --compliance`. Deployment is blocked if violated.

## Built With

- [gitagent](https://gitagent.sh) — git-native agent standard
- [Lyzr AI](https://lyzr.ai) — agent runtime
- [Anthropic Claude](https://anthropic.com) — underlying LLM

---

*Submitted to the GitAgent Hackathon by Lyzr AI × HackCulture*
