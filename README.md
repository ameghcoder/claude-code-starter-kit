# Claude Code starter kit for solo developers

A small, opinionated setup that turns Claude Code into a careful teammate.
Every task becomes a GitHub issue, gets planned, built in small steps, and
verified according to its risk. You stay in charge of the steps that matter:
pushing, merging and closing.

It comes from a real product built solo with Claude Code over two months:
177 issues, a weekly release cycle and real customers. It has been cut down
to the parts that held up.

## What's inside

```
CLAUDE.md                                  # project facts, rules and trigger words (template)
.gitignore                                 # keeps .temp/ and personal settings out of git
.claude/
└── skills/
    ├── project-setup/SKILL.md             # fills in CLAUDE.md for your project (run once)
    ├── github-workflow/SKILL.md           # one task: issue → branch → build → verify → report
    └── task-planning/SKILL.md             # backlog, one file per week, archive
docs/decisions/0000-template.md            # template for recording a decision
```

The **skills** load only when they're needed, so they don't fill Claude's
context in every session. `CLAUDE.md` is read every session, so it stays short.

## Requirements

- [Claude Code](https://claude.com/claude-code)
- git, and the GitHub CLI `gh` (logged in: `gh auth login`)
- A GitHub repository for your project (issues are the task record)

Nothing to install beyond that, and it works the same on macOS, Linux and Windows.

## Where to put it

At the **root of your project**, next to your `package.json`, `pyproject.toml`
or equivalent. `CLAUDE.md` and `.claude/` must sit at the root for Claude Code
to find them.

## Setup: new project

1. Copy everything from this kit into your project's root folder.
2. Start Claude Code in the project folder and run `/project-setup`.
   It asks a few questions (what you're building, stack, commands, what's
   risky, working mode), then shows you the filled-in `CLAUDE.md`. Nothing
   is written until you say OK.
3. Commit the kit: `CLAUDE.md`, `.claude/`, `.gitignore`, `docs/`.

## Setup: existing project

1. Copy `.claude/` and `docs/decisions/0000-template.md` into your project.
2. Add the lines from this kit's `.gitignore` to yours.
3. **Already have a `CLAUDE.md`?** Keep yours, and let setup merge the kit's
   sections into it. Otherwise copy the kit's `CLAUDE.md`.
4. Run `/project-setup`. On an
   existing repo it reads your manifests, scripts and CI first, and only asks
   what it can't find out.
5. Review the result and commit it.

## How to use it day to day

You drive Claude with a handful of words:

| You say | What happens |
|---|---|
| "plan this week" / "add these tasks" | `task-planning` updates `.temp/tasks/` |
| "start LOC-3" or "start #12" | `github-workflow`: opens the issue if needed, branches, plans, builds, verifies, reports, then **stops** |
| "commit" | commits on the task branch |
| "merge" | merges into main locally and comments on the issue |
| "close" | closes the issue, after you've checked it in production |
| "just tell me" | Claude answers without changing anything |

**A typical task:**

```
You:    start LOC-4
Claude: opens issue #12, creates fix/12-login-redirect, writes a plan in
        .temp/tasks/issue-12.md, builds it step by step, runs the tests,
        checks the real login page, then reports what changed and what it checked.
You:    (read the report, look at the diff) commit
You:    merge
You:    (git push, deploy, check it live) close
```

### Working mode

Set in `CLAUDE.md`:

- **Review-first** (the default): you write the code; Claude plans, reviews
  each step and explains problems. Good if you want to keep sharpening your
  own skills.
- **Claude implements**: Claude writes the code; you review the result.

### Task files (`.temp/`)

`task-planning` creates this folder the first time you use it. It's
gitignored, so it stays local:

```
.temp/tasks/backlog.md               # ideas and tasks not planned yet (LOC-1, LOC-2…)
.temp/tasks/week-2026-11-02.md       # this week's plan, named by its Monday
.temp/tasks/archive/                 # finished weeks
.temp/tasks/issue-12.md              # one task's plan and notes
.temp/checks/12-login-redirect/      # verification record for a risky change
.temp/discussions/  .temp/for-later.md  .temp/scratch/
```

Two ideas to know:

- **`LOC-N` vs `#NUM`.** A planned task gets a local id (`LOC-4`). The GitHub
  issue is opened only when work starts, and then `LOC-4` becomes `#12`. You
  never guess an issue number ahead of time, so the two never drift apart.
- **Weekly files.** Normal work only reads the current week and the backlog,
  which keeps Claude's context small. Old weeks go to the archive.

### Verification scales with risk

- **Low** (copy, UI, small fixes): tests, plus a look at the result.
- **Medium** (new API behaviour): tests, plus a run through the real route.
- **High** (data, migrations, auth, payments): test against an isolated local
  copy of your stack (never production), and write down each scenario and its
  verdict in `.temp/checks/`.

"Done" means deployed and checked in production, not "committed".

## Tips

- **Models and effort.** Use a stronger model with higher effort for
  architecture, migrations and risky changes, and a lighter model for copy,
  docs and small fixes. Switch with `/model` and `/effort`.
- **Long sessions.** One session per project is fine. Run `/compact` when the
  context gets long, or let auto-compact do it. Start a fresh session with
  `/clear` when you switch to unrelated work.
- **Keep `CLAUDE.md` short.** Add a rule only after Claude gets something
  wrong that it couldn't infer from the code, and write the reason next to it.
- **Record decisions** in `docs/decisions/` (copy the template). Notes in
  `.temp/` are temporary; decisions are kept.

## Approval gates

The gates live as plain rules in `CLAUDE.md`, so you can read exactly what
Claude has been asked to do and not do:

- Claude never pushes. You push, after you've reviewed the work.
- Claude never merges or closes on its own: only when you say "merge" or "close".
- Claude stages files by path, never everything at once.

That's how the project behind this kit ran for two months: in its logs,
Claude never once ran `git push`.

*Optional:* if you want a hard block on top, Claude Code's settings can deny
specific commands, with no script, on any OS. For example, in
`.claude/settings.json`: `{"permissions": {"deny": ["Bash(git push:*)"]}}`.
Check the permissions page of the Claude Code docs for the exact syntax of
your version.

## Customize

- **Another approval gate:** add one line to `CLAUDE.md`'s "Approval gates".
- **A rule for one area** (for example "migrations only add"): add it to
  `CLAUDE.md`, or as a path-scoped rule in `.claude/rules/` once the project is big.
- **Removing the kit:** delete `.claude/`, the kit's `CLAUDE.md` sections,
  and the `.gitignore` lines.

## License

MIT. See [LICENSE](LICENSE).
