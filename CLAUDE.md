# CLAUDE.md

<!-- Filled in by the project-setup skill. Keep it short: Claude reads this file
     at the start of every session, so every line here costs context every time. -->

## Project

<What it is, who uses it, and whether production holds real user data.>

- Stack: <language, framework, database, hosting>
- Layout: <where the app, shared code, tests and migrations live>
- Decisions: `docs/decisions/` (one file per decision that's worth keeping)

## Commands

- Install: `<command>`
- Run locally: `<command>`
- Check everything: `<typecheck && lint && test>`
- One test file: `<command> <path>`
- Production build: `<command>`

## Working mode

**Review-first** <!-- or: Claude implements -->

- Review-first: I write the code. Claude plans, reviews each step and explains
  problems plainly (correctness, then design, then style). Claude writes code
  only when I ask for it.
- Claude implements: Claude writes the code after I say "start", in small steps,
  and I review the result.

## Trigger words

| I say | Claude does |
|---|---|
| "start <task>" | the `github-workflow` skill: issue, branch, plan, build, verify, report |
| "commit" | commits on the task branch, nothing more |
| "merge" | merges the branch into main locally and comments on the issue |
| "close" | closes the issue, only after I've checked it in production |
| "just tell me" | answers only, with no edits |

## Approval gates

- Never `git push`, merge a PR, or close or delete an issue on your own. I push;
  Claude merges and closes only when I say "merge" or "close".
- Stage files by path; never `git add -A` or `git add .`.
- Never start the next task without asking.
- Never run migrations against a shared or production database.
- Never print secrets or tokens. Read env files without echoing their values.
- Stop every server or process you start before ending your turn.

## Verification (scaled to risk)

- **Low** (UI, copy, small fixes): tests, plus a quick look at the result.
- **Medium** (new API behaviour): tests, plus a run of the real route or command.
- **High** (data, migrations, auth, payments): test in an isolated local copy,
  never in production. Write the scenarios and a verdict in `.temp/checks/<issue>-<slug>/`.
- Verify where the user enters (the real page, route or CLI), not an inner function.

## Risky areas in this project

<Listed by project-setup: e.g. billing, auth, migrations, anything that deletes data.>

## Conventions

- <Two or three rules Claude can't infer from the code, e.g. where SQL lives.>
- Comments: one or two lines, the non-obvious reason only.
- Match the surrounding code's naming and style.

## Task files

Plans and notes live in `.temp/` (gitignored, local only). The `task-planning`
skill creates and maintains it. The durable record of a task is its GitHub issue,
and of a decision, `docs/decisions/`.
