# Hook — Bootstrap

## On Agent Startup

1. **Load knowledge base** — Read `knowledge/code-review-guide.md` into context.
2. **Check environment** — Verify `GITHUB_TOKEN` and `ANTHROPIC_API_KEY` are set.
   - If missing, log error to `memory/runtime/dailylog.md` and halt.
3. **Initialize daily log** — Append startup entry to `memory/runtime/dailylog.md`:
   ```
   [STARTUP] {timestamp} — PR Review Agent initialized. Ready for review requests.
   ```
4. **Validate sub-agent** — Confirm `agents/fact-checker/agent.yaml` exists and is valid.
5. **Load review history** — Read last 5 entries from `memory/runtime/dailylog.md`
   to understand recent PR patterns in this repo.

## Warm-Up Check

Before reviewing the first PR of the day, silently verify:
- GitHub token has `pull_requests: write` permission
- Anthropic API is reachable
- fact-checker agent can be invoked
