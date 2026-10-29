---
name: github-workflow
description: Use when the user says "start" followed by a task, a LOC-N id or an issue number, and again when they say "commit", "merge" or "close". Runs one task from GitHub issue to a verified, reported result without pushing or closing anything on its own.
---

# One task, start to finish

## "start <task>"

1. **Find the task.** An issue number: `gh issue view <n> --comments`. A `LOC-N` id:
   read its line in `.temp/tasks/` (grep for the id, not the whole folder).
2. **Agree on "done".** Write the acceptance criteria in 2 to 4 lines. Ask only
   if something is genuinely unclear.
3. **Open the issue now, not earlier**, if there isn't one yet:
   `gh issue create --title "<what>" --body "<scope, checks, deploy note>"`.
   Then replace `LOC-N` with `#<issue>` in its line in the week file and mark it 🔧.
4. **Branch:** `git switch -c <fix|feat>/<issue>-<short-slug>`.
5. **Plan** in `.temp/tasks/issue-<issue>.md`: the criteria, the risk level
   (low / medium / high, from CLAUDE.md) and a checklist of small steps.
6. **Build in small steps.** Run the narrowest test after each one.
   - Review-first mode: the user writes each step; review it before the next.
   - Implement mode: write each step yourself.
7. **Verify by risk level** (CLAUDE.md, "Verification"). For high risk, use an
   isolated local copy and write `.temp/checks/<issue>-<slug>/`:
   - `README.md`: each scenario and its expected result (happy path,
     failures, old code on the new database, rollback);
   - `result/verdict.md`: pass or fail per scenario, with the real numbers.
8. **Report and stop.** What changed, what was checked (with real output) and
   anything the user should know. Never start the next task.

## "commit"

- Stage the task's files by path; never `git add -A` or `git add .`.
- Message: `#<issue> <type>: <summary>` (under 72 characters), plus at most 2 short lines.
- Never write `fixes #<n>`, `closes #<n>` or `resolves #<n>`: GitHub would close the
  issue on push, before it's checked in production.
- Stay on the branch.

## "merge"

- `git switch main && git merge --ff-only <branch>`. If that fails, rebase the
  branch onto main first, rerun the tests, then merge.
- Delete the local branch.
- `gh issue comment <n> --body "<commit, what was checked, deploy steps>"`.
- Don't push. The user pushes.

## "close" (only after the user checked it in production)

- `gh issue close <n>`, then mark the task ✅ in its week file.
