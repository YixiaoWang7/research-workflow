# Repository Guidelines

<!-- Copy to the project root and replace every <placeholder>. The workspace rules,
~/Documents/code/workflow/rules.md, apply wherever this file says nothing else. Keep it short:
operational detail belongs in docs/operations.md. -->

## Project structure

<Where production code, thin entry points, tests, versioned configuration, documentation and
assets live. What must not be edited, such as vendored or upstream code. Related repositories.>

## Commands

```bash
<create or activate the environment>
<run the tests>
<run the linter>
<the cheapest end-to-end smoke test>
```

## Coding style

<Language version, formatter and linter, naming, and where defaults and configuration live.>

## Testing

<Framework and naming. Tests must not need a GPU, the network or large datasets unless marked so.
What to run before a commit.>

## Jobs, files and tracking

Read `WORKING.md` first. [Operations](docs/operations.md) is the single home of these rules.

- Start long jobs with `<launcher command>`; check them with `<jobs command>` only at a milestone,
  a failure or a request.
- Models live in `runs/`, datasets in `data/`, rebuildable results in `outputs/`. Delete only
  `.work/` folders and what `<cleanup command>` lists.
- Training logs to W&B as `yixiao_wang/<project>`; checkpoints stay local unless
  `<how to turn uploads on>`.
- Code never writes `WORKING.md` or `docs/`; results become records through `<archive command>`.

## Commits and pull requests

<Message style and what a pull request must contain. Never commit credentials, runs, data,
outputs, checkpoints or logs.>
