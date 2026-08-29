# What do you build? - Question

A Claude Code skill that walks you through a GitHub pull request **one hunk at a time** and asks: *what do you think this does?*

You write your own interpretation of every hunk. Only after you've finished all of them does Claude grade your answers — pointing out what you misread, with evidence from the actual code — and saves the result to `wdyb-<PR-number>.md`.

Made for pre-review reading, onboarding onto an unfamiliar codebase, and practicing code reading.

## Why

Reading a diff with an AI assistant usually means the assistant explains it to you. That's fast, but you don't find out what you *didn't* understand. wdyb flips it: you explain, then get corrected.

## Features

- **No spoilers.** During the collection phase, Claude gives no hints, no feedback, not even an "exactly". You read it yourself.
- **Hunk-level granularity.** Finer than per-file, so you can see precisely where your reading went wrong.
- **Evidence-based feedback.** Claude reads the surrounding code — callers, callees, type definitions, tests — and cites files and line numbers.
- **A record you keep.** `wdyb-123.md` stays in your working directory so you can look back at your patterns over time.

## Install

In Claude Code:

```
/plugin marketplace add kaleidot725/wdyb
/plugin install wdyb@wdyb
```

## Usage

```
/wdyb:question 123
```

| Argument | Example |
| --- | --- |
| PR number | `/wdyb:question 123` |
| PR URL | `/wdyb:question https://github.com/owner/repo/pull/123` |
| None | `/wdyb:question` — resolves the PR for the current branch |

While answering:

| Input | Effect |
| --- | --- |
| `skip` | Skip a hunk you can't read (recorded as unanswered) |
| `back` | Go back to the previous hunk |
| `stop` | End collection early and grade what you have so far |

Lock files, generated files, binaries, and rename-only diffs are excluded automatically.

## Requirements

- [Claude Code](https://claude.com/claude-code)
- [GitHub CLI (`gh`)](https://cli.github.com/), authenticated (`gh auth login`)

## Example output

`wdyb-123.md`

```markdown
# wdyb #123 — Add retry to API client

- Score: 8 correct / 3 close / 2 wrong / 1 unanswered (14 hunks)

## Summary

You consistently catch added error handling, but tend to miss changes in
the ordering of async operations.

---

## 2. src/api/client.ts `@@ -40,3 +48,9 @@` — ⚠️ Close

**Your interpretation**

> Adding a retry.

**What it actually does**

Adds a retry *and* an exponential backoff delay between attempts. ...
```

## License

MIT
