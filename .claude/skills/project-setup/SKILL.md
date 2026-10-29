---
name: project-setup
description: Fills in this starter kit's CLAUDE.md for the current project. Run once, only when the user invokes it (/project-setup). Asks what's missing for a new project, reads the repo for an existing one, and writes nothing until the user approves.
disable-model-invocation: true
---

# Set up the starter kit for this project

## 1. New or existing?

Check for manifests (`package.json`, `pyproject.toml`, `go.mod`, `Cargo.toml`…),
CI files, an existing `CLAUDE.md` and git history.

## 2a. New project: ask, at most these

1. What are you building, and for whom?
2. Which stack (language, framework, database)?
3. Where will it run or deploy?
4. Which commands install, run, test and build it? ("not decided yet" is fine)
5. What's risky: user data, payments, auth, anything that deletes data?
6. Working mode: review-first (you write, Claude reviews) or Claude implements?

## 2b. Existing project: read first, then ask only what's missing

- Commands: from the manifest scripts and CI.
- Layout: the top-level folders and what's in them.
- Conventions: lint config, and patterns that repeat in the code.
- If a `CLAUDE.md` already exists, keep its project-specific rules and merge the
  kit's sections around them. Don't drop the user's rules.
- Usually still ask: risky areas, how deploys happen, and the working mode.

## 3. Fill in, show, wait

- Replace the `<…>` placeholders in `CLAUDE.md`. Leave everything else as it is.
- Aim for under 100 lines. Add a rule only if Claude couldn't infer it from the repo.
- Show the result, and write it only after the user says OK.
- Don't install anything, create other files, or run git commands.
- `.temp/` is created later, by the task-planning skill, on first use.

## 4. Check the basics, then say so

- `gh auth status` (if it fails, tell the user to run `gh auth login` themselves).
- `.gitignore` contains `.temp/`.
