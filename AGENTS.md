# AGENTS.md — PR Review Agent

## What This Agent Does

This agent reviews GitHub pull requests automatically. When triggered by a PR event,
it analyzes the diff, runs security and quality checks, and posts structured findings
as inline comments. It never approves its own reviews (fact-checker handles approval).

## How to Run

```bash
# Install gitagent CLI
npm install -g @shreyaskapale/gitagent

# Validate the agent definition
gitagent validate --compliance

# Export to system prompt (any LLM)
gitagent export --format system-prompt

# Run with Lyzr adapter
gitagent run . --adapter lyzr

# Run with Claude Code
gitagent export --format claude-code
```

## Inputs Expected

- `PR_DIFF` — The full unified diff of the pull request
- `PR_TITLE` — Title of the PR
- `PR_DESCRIPTION` — Body/description of the PR
- `REPO_LANGUAGE` — Primary programming language(s)
- `CHANGED_FILES` — List of files changed

## Outputs Produced

- Inline PR comments via GitHub API
- `findings.json` — Structured findings for the fact-checker
- `memory/runtime/dailylog.md` — Audit log entry

## Environment Variables Required

```
ANTHROPIC_API_KEY=your-key-here
GITHUB_TOKEN=your-github-token
GITHUB_REPO=owner/repo-name
```
