# What do you build? - Question

A Claude Code skill that walks you through a GitHub pull request **one hunk at a time** and asks: *what do you think this does?*

You write your own interpretation of every hunk. Only after you've answered them all does Claude grade you — pointing out what you misread, with evidence from the actual code — and saves the result to `wdyb-<PR-number>.md`.

Reading a diff with an AI usually means the AI explains it to you. That's fast, but you never find out what you *didn't* understand. This flips it: you explain, then get corrected.

## Install

```
/plugin marketplace add kaleidot725/wdyb
/plugin install wdyb@wdyb
```

Requires [GitHub CLI](https://cli.github.com/), authenticated (`gh auth login`).

## Usage

```
/wdyb:question 123
```

Takes a PR number, a PR URL, or nothing (resolves the PR for the current branch). While answering: `skip` to pass on a hunk, `back` to revisit the previous one, `stop` to grade early.

Add `--json` for a machine-readable report following [`report-schema.json`](plugins/wdyb/skills/question/references/report-schema.json), or `--html` for a standalone page with syntax-colored diffs and a score bar. Pass several to write several.

## Example output

```markdown
# wdyb #123 — Add retry to API client

8 correct / 3 close / 2 wrong / 1 unanswered (14 hunks)

## Summary

You consistently catch added error handling, but tend to miss changes
in the ordering of async operations.

## 2. src/api/client.ts `@@ -40,3 +48,9 @@` — ⚠️ Close

**Your interpretation**
> Adding a retry.

**What it actually does**
Adds a retry *and* an exponential backoff delay between attempts.
```

## License

MIT
