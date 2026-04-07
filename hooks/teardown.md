# Hook — Teardown

## On Agent Shutdown

1. **Flush pending logs** — Write any buffered findings to `memory/runtime/dailylog.md`.
2. **Save session summary** — Append to daily log:
   ```
   [SHUTDOWN] {timestamp} — Session complete. PRs reviewed: {count}. 
   Critical issues found: {critical_count}. Escalations: {escalation_count}.
   ```
3. **Clean temp files** — Remove any local `findings.json` temp files.
4. **Do not persist secrets** — Verify no tokens or credentials were written to any file.
