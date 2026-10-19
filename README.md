# Workflow

The shared way of working for every project under `~/Documents/code/`:

- [`rules.md`](rules.md): the default rules, for people and for Claude or Codex sessions.
- [`templates/`](templates/): the files a project copies to adopt them: `AGENTS.md`,
  `WORKING.md`, `docs/operations.md`, an experiment record and a plan.
- [`archive/`](archive/): the workspace instruction files as they were before 2026-10-19.

## How sessions load it

`~/Documents/code/AGENTS.md` tells every session to read `rules.md` first, then the project's
`AGENTS.md` files and its `WORKING.md`. A project's own files override these defaults where they say
so. `~/Documents/code/rules.md` is a link to this `rules.md`, so older references keep working.

## Adopting it in a project

Give a new session this prompt in the project's folder:

> Read ~/Documents/code/workflow/rules.md. Set up this repository to follow it: copy the templates
> from ~/Documents/code/workflow/templates/, fill them with this project's real commands, paths and
> settings, configure online W&B for training, add the generated folders to .gitignore, and run the
> tests. Report what you created and the first next action. Start no long job, deletion, commit or
> push without asking.

A project that runs many long jobs also needs a launcher, a jobs command and a cleanup command
(rules §3 and §4). RoboDecision's `src/robodecision/jobs/` package is the reference
implementation; turning it into a shared package is planned for when a second project needs it.

## Changing the rules

Edit through Git, and say why in the commit message. Keep `rules.md` to principles and defaults:
anything specific to one project belongs in that project's `AGENTS.md` or `docs/operations.md`.
