---
name: resume
description: "what do you build? - Pick up a paused wdyb run: read wdyb-<PR-number>.json, continue asking from the first unanswered hunk, then grade and write the report. Use when the user types /wdyb:resume <PR number>, or wants to continue a PR reading session they paused earlier."
---

# What do you build? - Resume

Continue a run that was paused with `pause` in the `question` skill. Everything about how to ask, grade, and report lives in that skill — read `../question/SKILL.md` and follow it. This skill only covers getting back to the right place.

## Hard rules

The same ones apply, and they matter more here because the saved answers are sitting in front of you:

- **Stop and wait.** After asking about a hunk, end your turn immediately. The user's next message is the answer
- **Never answer for the user.** No example answers, no hints
- **Never show the saved answers back.** Not as a recap, not as a reminder of where they left off. They are graded later and seeing them now shapes the remaining answers
- No feedback during collection. Take the answer and move on

## Steps

1. **Load the run** — read `wdyb-<PR-number>.json` in the working directory. If it is missing, say so and offer to start over with `/wdyb:question <PR-number>`. If `"status"` is `complete`, say the run is already graded and ask whether to start over
2. **Re-fetch the diff** — run `gh pr diff` again and match it against the saved hunks. If the PR has changed since, say which hunks no longer match and ask whether to keep the answers that still apply or start over
3. **Find the place** — the first hunk with no `interpretation` is next. Say where you are (`Resuming at 6/14`) and ask that hunk. Nothing else
4. **Carry on** — collect the rest exactly as `question` does, accepting `skip` / `back` / `stop` / `pause`
5. **Finish** — at the end, grade and write the report as `question` does, and set `"status": "complete"` in the JSON
