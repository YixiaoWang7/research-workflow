# Repository Guidelines

## Project Structure & Module Organization

This workspace contains two independently versioned Python projects. `RoboDecision/` owns IsaacLab environments, task registries, training, evaluation, recording, and dataset tooling; implementation belongs in `RoboDecision/src/robodecision/`, with thin CLI wrappers in `RoboDecision/scripts/` and tests in `RoboDecision/tests/`. `robopolicy/` owns flow-matching algorithms, datasets, models, and trainers under `robopolicy/src/policies/`; runnable experiments live in `robopolicy/scripts/`, JSON configurations in `src/policies/config/`, and unit tests in `robopolicy/tests/`. Treat `runs/`, `logs/`, `outputs/`, checkpoints, and processed HDF5 shards as generated artifacts unless a fixture is intentionally being updated.

## Build, Test, and Development Commands

Run commands from the relevant project directory.

- `cd robopolicy && uv sync --python 3.10 --all-extras`: create `.venv` and install training and development dependencies.
- `cd robopolicy && uv run pytest -q tests`: run the policy unit suite without collecting operational `scripts/policies/test_*.py` entrypoints.
- `cd robopolicy && uv run python scripts/policies/test_robot_flow_trainer_v2_state.py --config <config.json>`: launch config-driven state-policy training.
- `cd RoboDecision && python -m pytest -q`: run the configured unit suite (`tests/`, with `src/` on `PYTHONPATH`).
- `cd RoboDecision && ruff check src scripts tests`: apply the repository's Ruff rules.
- `cd RoboDecision && ./IsaacLab/isaaclab.sh -p scripts/train_rsl_rl.py --help`: verify an IsaacLab-backed entrypoint before a full training run.

## Coding Style & Naming Conventions

Use four-space indentation, PEP 8 naming, `snake_case` for modules/functions, and `PascalCase` for classes. Prefer explicit type hints and Google-style docstrings. In tensor-heavy `robopolicy` code, document input/output shapes and annotate significant reshapes; infer devices instead of hardcoding `.cuda()`. RoboDecision targets Python 3.11, uses a 110-character Ruff line length, and requires imports sorted by Ruff. Keep reusable logic in `src/`, not CLI scripts.

## Testing Guidelines

Pytest is used in both projects. Name tests `test_<behavior>.py` and test functions `test_<expected_result>`. Add focused regression tests for bug fixes and keep unit tests independent of GPUs, Isaac Sim, network access, and large datasets where possible. No fixed coverage threshold is configured; cover new branches and failure paths.

## Commit & Pull Request Guidelines

Commit inside the affected child repository. Existing history favors short, imperative summaries such as `add history to transformer` or `Refine catching object visibility`; include a scope when ambiguity is likely. Pull requests should identify the affected project, explain behavioral/configuration changes, link relevant issues, and report exact test commands. Include screenshots or short videos for rendering or simulation changes. Do not commit credentials, local paths, generated runs, checkpoints, or bulk datasets.
