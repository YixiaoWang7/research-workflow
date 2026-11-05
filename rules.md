# Workflow Rules

The default way of working in every project under `~/Documents/code/`, for people and for Claude
or Codex sessions alike. A project adapts these defaults in its own `AGENTS.md` and
`docs/operations.md`; where those differ, the project wins, and the user's explicit request wins
over both. Start a project from [`templates/`](templates/). RoboDecision
([operations](../looplearning/RoboDecision/docs/operations.md)) implements all of this and is the
reference.

## 1. Reading order

1. This file.
2. The project's `AGENTS.md` files, from the workspace down: how to work in that project.
3. The project's `WORKING.md`, if it has one: what is current.
4. The user's request: what to do now. Instruction files describe how to work; they are never the
   task by themselves.

## 2. Project documents

| File | Holds | Written by |
| --- | --- | --- |
| `README.md` | Purpose, installation, quick start | People |
| `AGENTS.md` | Repository map, commands, style, tests, and the project's choices about jobs, files and tracking, briefly, linking to `docs/operations.md` | People |
| `WORKING.md` | What is current: objective, decisions in force, open issues, running jobs, one next action (§6) | People, by hand |
| `docs/README.md` | A map of every active document | People |
| `docs/operations.md` | The single home of operational rules: file layout, starting, checking, stopping and resuming jobs, tracking, cleanup, records | People |
| `docs/<task>.md` | A stable guide per task or pipeline: commands, settings and why, quality gates, expected outputs | People |
| `docs/experiments/` | Permanent records of results worth keeping, and their generated index (§7) | The archive command |
| `docs/plans/` | Approved plans, one dated file each (§8) | Sessions, right after approval |
| `docs/archive/` | Superseded documents, kept for history | People |

- **One home per rule.** Each rule lives in one file; other files link to it instead of repeating
  it.
- **Code never writes these documents,** except the archive command, which writes only
  `docs/experiments/`.
- **Guides stay stable:** no progress notes, process IDs or dated results. Those belong in
  `WORKING.md` while current and in a record once permanent.
- File names under `docs/` are lowercase kebab-case.

## 3. Files: what to keep and what may go

| Folder | Holds | May it be deleted? |
| --- | --- | --- |
| `runs/` | Trained models, one folder per training run; `runs/selected/` holds read-only copies that other work depends on | No. Never by hand. |
| `data/` | Datasets and recordings | No. Never by hand. |
| `results/` | Sealed results: read-only copies of finished experiments, the tables, videos and evidence a paper cites | No. Never by code or hand. |
| `outputs/` | Work in progress: one folder per experiment (`<name>-<id>/`), and `scratch/` for everything else | Only through the cleanup command: scratch a week after it ended, an experiment once sealed or discarded |
| `.work/` in any job folder | Logs, live status, scratch, local tracking files | Yes, once the job finished |

- **Models and datasets live only under `runs/` or `data/`,** never only in `outputs/`, `.work/`,
  `/tmp` or a cache such as `~/.cache`.
- **Only training writes into a model's folder.** Evaluation, playback, videos and exports read
  the checkpoint and the files beside it, and write to their own folder under `outputs/`. Work
  that depends on a checkpoint uses a read-only copy in `runs/selected/`.
- **Everything disposable goes into `.work/`.**
- **Cite it? Register it.** Work that may be cited belongs to a named experiment, registered
  before its jobs start; its jobs live together in `outputs/<name>-<id>/`, whatever days they ran
  on. Anything else is scratch. An experiment ends sealed into `results/<name>-<id>/` (tables,
  videos, raw evidence, a manifest and `SHA256SUMS`, read-only) or discarded; its record in
  `docs/experiments/` carries the same id.
- **Every job folder explains itself:** a generated `README.md` says what it is, its state and what
  may be deleted, and a record (`job.json` or `command.json`) says how it was made.
- **Checkpoints:** keep the newest five, the best and the last; name the best and the last
  `best.pt` and `last.pt`.
- **Delete through a cleanup command** that lists before it deletes, never follows symlinks,
  never touches `runs/`, `data/` or `results/`, never lists what the project's documents name, and
  removes an experiment only once its sealed result verifies. Never `rm -rf` generated folders by
  hand.
- Never commit `runs/`, `data/`, `outputs/`, `results/`, checkpoints, logs or tracking caches.

## 4. Long jobs

- **Start them through the project's launcher.** It:
  - runs the job detached, in its own session, so it outlives terminals and conversations;
  - records how the job started: command, code identity, uncommitted files, non-secret environment;
  - returns only once the job works: its first progress line, and its tracking URL;
  - judges success from the job's result files, not its exit code;
  - keeps the live state in `.work/status.json`.
- **One jobs command** lists, shows, stops and resumes jobs. A multi-stage job (a pipeline,
  benchmark or queue) resumes from its first unfinished stage and keeps the finished ones.
- **Pin code by content.** A comparison or pipeline checks before each stage that the code (its
  file hashes) is unchanged, so it never mixes versions; a commit that leaves the code unchanged,
  such as one that edits documentation, does not matter. Make code changes in a separate worktree
  while one runs.
- **No hand-made supervisors** in `outputs/` or `/tmp`. Until a project has a launcher, detach with
  `setsid nohup <python> <script> <arguments> > <job folder>/.work/job.log 2>&1 < /dev/null &`,
  record the folder, log and process ID in `WORKING.md`, and build the launcher before the project
  runs many long jobs.
- **Before a long chain,** run the exact command small first, use absolute interpreter paths, and
  keep a GPU memory reserve.
- **Checking jobs.** Sessions make one startup check, then check only at a milestone, a failure or
  a user request: never poll in a loop, and at most one unchanged-state check per turn. A job is
  stopped only when two independent signals say so, such as its status plus no process on the
  host, no GPU use or no log growth; a sandboxed `ps` proves nothing.

## 5. Experiment tracking

- **Training logs online to W&B,** entity `yixiao_wang`, one W&B project per project, set in
  versioned configuration rather than left to W&B's default. The step axis is the training update.
- **Defaults:** metrics, evaluation scores and evaluation clips go to W&B; checkpoints stay local
  unless the project turns uploads on; standalone evaluations, previews and smoke tests stay local.
- Run `wandb login --verify` before a long job (the launcher should). After startup the log shows
  `https://wandb.ai/yixiao_wang/<project>/runs/<id>`. Confirm a run's final state through the W&B
  API or website, never from a local exit code.
- Credentials live only in W&B's own store (`~/.netrc`): never in code, configuration, documents,
  logs or chat.

## 6. `WORKING.md`

A short status page, not a log ([template](templates/WORKING.md)):

- **Objective:** what we are trying to achieve now, and how we will know it is done.
- **Decisions in force:** dated, with the reason; move each into a guide once it is stable.
- **Open issues:** problems that affect the work, and what would resolve them.
- **Jobs:** a pointer to the jobs command, plus one line per running job: folder, tracking URL,
  ETA, and what must not be touched meanwhile.
- **Next action:** exactly one.

Keep it under 150 lines and edit it by hand; code never writes it. When a job ends, archive its
result if it is worth keeping and delete its line: history lives in Git and in the records. Update
the page when a job starts or ends, when a decision is made, and at every handoff.

## 7. Results and records

- A result worth keeping (a reference, a production run, a diagnostic that explains something)
  becomes a record, `docs/experiments/<task>-<YYYY-MM-DD>-<label>.md`, through the project's
  archive command or by hand from the [record template](templates/experiment-record.md).
- A record holds what was run and why, the commands, the code identity and settings, the metrics
  with their sample counts, the W&B links, and artifact paths with SHA-256. It must stay useful
  after the local files are gone.
- Records are immutable: correct a mistake with a dated note, and give a later run its own record.
- Previews, smoke tests and failed attempts get no record; their folders say what they were.
- The index of records is generated, never edited by hand. Guides link to the current baselines.

## 8. Plans and decisions

- Save every approved plan at once to `docs/plans/<YYYY-MM-DD>-<topic>.md`
  ([template](templates/plan.md)): a plan that exists only in a conversation gets lost.
- When the work is done, add "Implemented in `<commit>`" under the plan's title instead of
  rewriting it.

## 9. Sessions

- **At the start,** read in the order of §1. Compare `WORKING.md`'s jobs with the jobs command and,
  for a job that should be running, one host-level signal.
- **During the work,** continue the next action when the request covers it. Ask when the next step
  needs a destructive action, new external access, or a choice the documents do not settle.
- **Other sessions,** Claude or Codex, may work in the same checkout at the same time. Check
  `git status` and the running jobs again before changing code, committing or starting GPU work.
- **At a handoff** (the user says stop, end or hand off, or the work is done), check the jobs and
  update `WORKING.md`: jobs, open issues and the next action, so a new session can continue
  without this conversation.

## 10. Safety

- Ask before deleting, overwriting, pushing, publishing or anything else hard to undo; an approval
  covers that action only.
- Commit or push only when asked. Never commit credentials or generated files.
- Look at what a deletion would remove before doing it, and prefer the cleanup command.
- Report what was verified and how. Never claim a result, an upload or a stopped job without
  evidence.

## 11. A new project or machine

- **New project:** copy [`templates/`](templates/), fill them with the project's real commands,
  paths and settings, configure W&B for training, add the generated folders to `.gitignore`, run
  the tests, and report what was created and the first next action. Start no expensive job,
  deletion, commit or push without the user.
- **New machine:** recreate the environment, run the cheapest smoke test, verify W&B and storage,
  and record the machine in `WORKING.md`. Parallelism (environment counts, batch sizes, workers) is
  machine-specific: measure before raising it.

## 12. Done means

Tests and lint pass; every stage finished and passed its quality gate; results exist locally and,
for training, in W&B; evaluation used held-out seeds; failures are labeled; `WORKING.md` is
current; and a production or reference result has its record.
