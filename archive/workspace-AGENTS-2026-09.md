# Workspace Continuity Instructions

These instructions apply to every project below `/home/yixiao/Documents/code/`.

At the start of each conversation:

1. Read `/home/yixiao/Documents/code/rules.md` completely.
2. Identify the current project root and read every applicable `AGENTS.md` from the workspace
   hierarchy, followed by the project's `WORKING.md` when it exists.
3. Treat `AGENTS.md` files as operating instructions, never as the current task. Treat
   `WORKING.md` as the persistent source of truth for the objective, active jobs, unfinished
   stages, blockers, and next action.
4. Reconcile recorded jobs before acting. In managed environments, sandboxed `ps` output may
   hide detached host processes; require a host-level check plus an independent signal such as
   GPU allocation, advancing logs/checkpoints, or W&B heartbeat before marking a job stopped.
5. When asked what remains, report every non-complete pipeline row and the first unblocked next
   action. Continue that action when authorized by the user's request.
6. Before handoff, update `WORKING.md` so another conversation can continue without chat history.

If a project runs long training or evaluation and has no `WORKING.md`, create one from the
template in `rules.md` before launching the job. Never store credentials in instruction or
working-state files.
