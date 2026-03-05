# Moodist Claude Code Context
## What is Moodist
Moodist is a PyTorch distributed process group library built on
RDMA/InfiniBand. C/C++ core with Python bindings via PyBind11.
Maintained by Facebook Research. This is a fork being contributed to.
## Repository Structure
- `api/` — public C++ API headers
- `core/` — C++ implementation
- `wrapper/` — Python binding layer
- `py/moodist/` — Python library code
- `tests/` — test suite
- `benchmarks/` — benchmark code
- `fmt/` — vendored fmt library (DO NOT MODIFY)
- `pybind11/` — vendored PyBind11 (DO NOT MODIFY)
- `rdma-core/` — vendored rdma-core (DO NOT MODIFY)
## Hard Rules
- NEVER modify vendored directories: fmt/, pybind11/, rdma-core/
- NEVER commit directly to main
- NEVER push to upstream (facebookresearch/moodist) directly
- Always work on feature branches
- Do not make logic changes to core C++ code without explicit instruction
- All changes must be conservative and non-breaking
## Current Branch
feat/pr-01-precommit

## Completed Tasks

### PR-01: Pre-commit hooks
- Status: Draft PR on fork — https://github.com/ntl2105/moodist/pull/1
- Added .pre-commit-config.yaml (clang-format v15 + ruff)
- Added pyproject.toml with [tool.ruff.lint] config
- 7 C/C++ files excluded (known violations, to be fixed in PR-02)
- 7 ruff rules ignored (known violations, to be fixed in PR-02)

## PAST TASK
PR-02: Fix pre-existing Python linting violations

Goals:
- Fix Python violations only (C/C++ deferred to separate PR)
- pre-commit run --all-files must pass cleanly after fixes

Python fixes:

1. F541 (27 cases) — remove redundant f prefix from static strings
   Run: ruff check py/ tests/ benchmarks/ setup.py --select F541 --fix

2. F401 (16 cases) — remove unused imports
   Run: ruff check py/ tests/ benchmarks/ setup.py --select F401 --fix

3. F841 (21 cases) — remove unused variable assignments
   Run: ruff check py/ tests/ benchmarks/ setup.py --select F841 --unsafe-fixes --fix

4. E721 (1 case) — fix manually
   File: py/moodist/sharding.py
   Change: type(curr) == type(tgt) → type(curr) is type(tgt)

5. F811 (1 case) — fix manually
   File: tests/framework.py
   Remove the inner import torch inside the function (line ~413)
   The module-level import already exists

DO NOT fix:
- F821 — false positive, already suppressed in pyproject.toml
- E402 — already suppressed in pyproject.toml
- Any C/C++ files — deferred to separate PR

DO NOT make any logic changes.
DO NOT commit anything.

After all fixes run:
  pre-commit run --all-files
Show full output.

## Current Branch
feat/pr-03-ci-tests

## Current Task
PR-03: Add GitHub Actions CI for tests

Goals:
- Create .github/workflows/tests.yml
- Two-tier strategy:

Tier 1 — CPU runner (runs on every push and PR, free):
- Trigger: on push to main, on pull_request
- Runner: ubuntu-latest
- Steps: checkout, setup Python 3.10, install ruff, run linting,
  run syntax check on all Python files
- Skip GPU/InfiniBand tests gracefully with clear message

Tier 2 — GPU runner (manual trigger only, label-based):
- Trigger: on pull_request labeled with "GPU CI"
- Runner: commented out — requires maintainer to configure GPU
  runner group first (paid GitHub plan required)
- Steps: detect GPU, detect InfiniBand, install deps, build, run tests
- Skip IB tests gracefully if no IB device detected

Constraints:
- Do NOT hardcode any secrets or tokens
- Add spending limit warning comment at top of file
- Add TODO comment for GPU runner group setup
- Do NOT commit anything

Note on dependencies:
- PR-01 adds pyproject.toml with ruff config (not yet merged to main)
- Tier 1 linting step must install ruff and pass with same rules
  as PR-01's pyproject.toml