---
name: task-planning
description: Use when the user plans a week, asks for this week's tasks, hands over new tasks or ideas to record, or finishes a week. Creates and maintains the local .temp/ task files (backlog, one file per week, archive).
---

# Task files in .temp/ (local, gitignored)

## First use: create the structure

If `.temp/` doesn't exist, create it and make sure `.gitignore` has a `.temp/` line:

```
.temp/
├── tasks/
│   ├── backlog.md        # LOC-N tasks not planned into a week yet
│   ├── week-YYYY-MM-DD.md   # one file per week, named by its Monday
│   └── archive/          # finished week files
├── checks/               # verification records for high-risk tasks
├── discussions/          # design talk before a decision
├── scratch/              # throwaway scripts; never in committed folders
└── for-later.md          # decided, but deferred
```

## Task ids and status

- A task without a GitHub issue yet is `LOC-N`. N is the next integer after
  the highest `LOC-` anywhere in `.temp/tasks/` (grep, archive included). Never reuse one.
- `#NUM` only ever means a real GitHub issue. The issue is opened when work
  starts (github-workflow), never ahead of time.
- Status: ⬜ to do · 🔧 in progress · ✅ done (checked in production).
- One task = one list item, 1 to 3 lines: `1. LOC-12 ⬜ <what and why>`.

## New tasks

Add them to `backlog.md` under `## <theme> (YYYY-MM-DD)` with today's date.
If that dated heading already exists, append to it. Never add undated tasks
to an old section.

## Planning a week

Create `week-<Monday's date>.md`:

```
# Week of YYYY-MM-DD
Release: <day>. Check in production: <day>.

1. LOC-12 ⬜ <task> (risk: low|medium|high)
2. ...

## Follow-ups found this week
```

- Propose what fits, and say plainly when a week is overloaded. The user decides.
- Move the chosen tasks out of `backlog.md` into the week file.

## Ending a week

- Unfinished tasks move to the next week's file or back to the backlog.
  Note where they went.
- Move the finished week file into `tasks/archive/`.
- Only the current week and the backlog are read in normal work. That keeps
  the context small.
