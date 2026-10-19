# Operations Runbook

<!-- Copy to docs/operations.md and replace every <placeholder>; delete sections the project does not
use. This is the single home of the project's operational rules: other documents link here. -->

How long jobs run, where their files go, what may be deleted, and how results become permanent
records. The current objective and next action are in [`WORKING.md`](../WORKING.md).

## Where files go

| Folder | Holds | May it be deleted? |
| --- | --- | --- |
| `runs/` | Trained models, one folder per training run; `runs/selected/` read-only copies | No. Never by hand. |
| `data/` | Datasets and recordings | No. Never by hand. |
| `outputs/` | Rebuildable results: evaluations, videos, previews, analyses | Yes |
| `.work/` in a job folder | Logs, live status, scratch | Yes, once the job finished |

<What a run folder contains, and which files must stay beside a checkpoint for it to load.>

## Starting a job

```bash
<launcher command> <workflow> --name <name> -- <arguments of the workflow's script>
```

| Workflow | Script | Folder |
| --- | --- | --- |
| `<train>` | `<script>` | `runs/<...>/` |

<What the startup check waits for, and the launcher's exit codes.>

## Watching, stopping and resuming jobs

```bash
<jobs command>                # live and recent jobs
<jobs command> show <name>    # status and the end of the log
<jobs command> stop <name>
<resume command> <name>       # continue a stopped multi-stage job
```

<The states a job can be in, and what each means.>

## W&B setup

Entity `yixiao_wang`, project `<project>`. <What is logged, how runs are named, the step axis,
whether checkpoints are uploaded, and how smoke tests stay local.>

## Checkpoints and retention

<Which checkpoints stay on disk (the newest five, the best and the last as best.pt and last.pt),
and how to make a read-only copy that other work can depend on.>

## Recording results

<The archive command, the kinds of record, and how the index is generated.>

## Pinned inputs

<What a pipeline or queue pins (the code's file hashes, configuration, assets) and therefore what
must not change while one runs.>

## Manual fallback

```bash
setsid nohup env PYTHONUNBUFFERED=1 <python> <script> <arguments> \
  > <job folder>/.work/job.log 2>&1 < /dev/null &
```

## Verification and recovery

<What "done" means for each kind of job, and what to do after a failure or a stop.>
