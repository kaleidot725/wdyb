---
name: question
description: "what do you build? - Walk through a GitHub PR one hunk at a time, collect the user's own interpretation of each hunk, then grade them against the real code and save the result to wdyb-<PR-number>.md. Use when the user types /wdyb:question <PR number>, wants to check how well they understand a PR, or wants to practice code reading."
---

# What do you build? - Question

Present a PR's diff one hunk at a time and let the user write, in their own words, what each hunk does. Grade only after every hunk has been answered.

**This measures understanding. Giving the answer away defeats the purpose.**

## Hard rules

- During collection, never signal right or wrong. No feedback, no acknowledgement, no hints — take the answer and move on
- One hunk at a time. Never batch them
- Record the user's interpretation verbatim. Do not summarize or rewrite it
- Back every judgement with the real code. Never grade on a guess

## Steps

1. **Split into hunks** — get the diff with `gh pr diff`, split on `@@`, and number them. Exclude lock files, generated files, and binaries. State the count before starting
2. **Collect an answer per hunk** — show the full diff, ask "what do you think this does?", take the answer, move on. Accept `skip` / `back` / `stop`
3. **Grade once all are answered** — read the real code to confirm what each hunk actually does, compare against the interpretation, and judge: `✅ Correct` / `⚠️ Close` / `❌ Wrong` / `➖ Unanswered`
4. **Save the report** — write `wdyb-<PR-number>.md` and give the path

Write the report in the language the user has been writing in.

## Output format

Markdown by default. `--json` and `--html` write `wdyb-<PR-number>.json` / `.html` instead; pass several to write several. Start from the matching file in `references/` and replace its content, keeping its structure.

- **Markdown** — `report-template.md`. One `###` section per hunk
- **JSON** — `report-schema.json`: one object with `pr`, `date`, `score`, `summary`, `excluded`, and a `hunks` array whose entries carry `interpretation`, `actual`, `notes`, and a `verdict` of `correct` / `close` / `wrong` / `unanswered`. Check the result parses
- **HTML** — `report-template.html`. Keep the CSS as is. One `<article class="hunk">` per hunk; in the diff, one `<i>` per line with class `a` (added), `d` (removed), `h` (the `@@` header), or none for context, and escape `<`, `>`, `&`. Size the `.bar` spans as percentages of the hunk count. Self-contained — no external assets
