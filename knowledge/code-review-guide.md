# Code Review Knowledge Base

## Common Bug Patterns by Language

### JavaScript / TypeScript
- `== null` vs `=== null` — use strict equality
- `async` functions without `await` inside — silent no-ops
- Missing `.catch()` on Promises used as fire-and-forget
- `parseInt()` without radix argument
- `typeof` checks for functions that may not exist at runtime

### Python
- Mutable default arguments (`def f(x=[])`) — shared across calls
- `except Exception:` swallowing all errors silently
- `is` vs `==` for value comparison
- Missing `encoding` param on `open()` calls
- `time.sleep()` in async code blocking the event loop

### Java
- `NullPointerException` from unchecked method return values
- `==` for String comparison instead of `.equals()`
- Raw types instead of generics
- Catching `Exception` or `Throwable` too broadly
- Resource leaks (streams, connections not in try-with-resources)

## Performance Red Flags
- Database queries inside `for` loops (N+1)
- Sorting inside a loop that runs O(n) times
- `String` concatenation in a loop (use `StringBuilder`)
- Loading entire datasets into memory instead of streaming
- Missing database indexes on frequently queried columns

## Security Patterns to Always Flag
- `eval()`, `exec()`, `Function()` with dynamic input
- `innerHTML` assignment with user-controlled data
- `pickle.loads()` on untrusted data (Python)
- `ObjectInputStream` on untrusted data (Java)
- Regex with catastrophic backtracking potential

## Good Code Signals (Worth Calling Out ✅)
- Comprehensive error handling with specific error types
- Input validation at API boundaries
- Immutable data structures where appropriate
- Clear variable names that explain intent
- Tests that cover edge cases, not just happy path
