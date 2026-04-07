# Hook — Bootstrap

## On Agent Startup
1. Load `knowledge/code-review-guide.md`.
2. Check environment — verify `GITHUB_TOKEN` and `GEMINI_API_KEY` are set.
   - If missing, log the error to `memory/runtime/dailylog.md` and halt.
3. Initialize daily log.
4. Validate sub-agent — confirm `agents/fact-checker/agent.yaml` exists and is valid.
5. Load review history — read the last 5 entries from `memory/runtime/dailylog.md`.

## Warm-Up Check
Before reviewing the first PR of the day, silently verify:
- GitHub token has `pull_requests: write` permission
- Gemini API is reachable
- fact-checker agent can be invoked
"@ | Set-Content -Path hooks/bootstrap.md