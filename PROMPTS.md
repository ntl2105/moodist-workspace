#PR01 Prompt 
```"I'm working on the moodist repository (a PyTorch distributed process group library using RDMA/InfiniBand, C/C++ core with Python bindings).
Before making any changes, do the following audit first:

Inspect the existing .clang-format file
List all C and C++ files in the repo
Run clang-format in dry-run mode against all C/C++ files and report any violations
List all Python files in the repo
Run ruff in check mode against all Python files and report any violations

Do NOT make any changes yet. Just report what you find. I want to see the full picture before we decide on the pre-commit config."
```

That's expected — Claude Code sessions don't persist across disconnections. But that's fine, we hadn't committed anything yet.

You're back at a clean state. Let's pick up exactly where we left off:

```bash
# Confirm you're on the right branch
cd ~/moodist
git branch
# Should show: * feat/pr-01-precommit
```

Confirm that and then paste this prompt into a fresh Claude Code session:

> "I'm working on the moodist repository (a PyTorch distributed process group library using RDMA/InfiniBand, C/C++ core with Python bindings).
>
> We already ran an audit. Here are the results:
>
> **C/C++ violations (9 total, 3 files):**
> - `core/collective_base.h:34` — trailing semicolon after empty constructor body
> - `core/cputhread.cc:1763-1777` — brace/indentation inside WAIT_DYN macro blocks
> - `core/intrusive_list.h:16` — spacing around `::*` in pointer-to-member template
> - `core/moo_allocator_impl.h:162` — same `::*` spacing issue
>
> **Python violations (68 total):**
> - F541: 27 (f-strings without placeholders)
> - F841: 21 (unused variables)
> - F401: 16 (unused imports)
> - E402: 1, E721: 1, F811: 1, F821: 1
>
> Now create the following:
>
> 1. `.pre-commit-config.yaml` — must pass cleanly on current codebase with zero failures:
>    - clang-format for C/C++, using existing `.clang-format`, excluding these violating files: `core/collective_base.h`, `core/cputhread.cc`, `core/intrusive_list.h`, `core/moo_allocator_impl.h`
>    - ruff for Python on `py/`, `tests/`, `benchmarks/`, `setup.py`, excluding `fmt/` directory, ignoring rules: F541, F841, F401, E402, E721, F811, F821
>
> 2. `pyproject.toml` — containing only the ruff config section, no build system config (setup.py stays as-is)
>
> After creating both files, run `pre-commit run --all-files` and show me the full output. Do not commit anything yet."

Share the output here before we touch git.

### Prompt 1cd ~/moodist
claude
```

It will start an interactive session and automatically read `CLAUDE.md`. Then paste this prompt:

---
```
We are adding pre-commit hooks to the moodist repository.
Read CLAUDE.md first for full context.

Create two files:

1. `.pre-commit-config.yaml`:
   - clang-format hook for C/C++ files in api/, core/, wrapper/
   - Exclude these files that have known violations:
     core/collective_base.h, core/cputhread.cc,
     core/intrusive_list.h, core/moo_allocator_impl.h
   - ruff hook for py/, tests/, benchmarks/, setup.py
   - Ignore rules: F541, F841, F401, E402, E721, F811, F821
   - Exclude fmt/ directory from all hooks

2. `pyproject.toml`:
   - Ruff config section only
   - Same ignore rules and paths as above
   - No build system config (setup.py stays as-is)

After creating both files:
- Run: pre-commit install
- Run: pre-commit run --all-files
- Show me the full output
- Do NOT commit anything

## PR-02: Fix pre-existing linting violations
Date: 2026-03-05
Status: in progress

### Analysis performed before writing prompt

#### F821 — FALSE POSITIVE, no fix needed
- tests/framework.py:39: `"moodist.TcpStore"` is a string annotation
- Already suppressed globally in pyproject.toml ignore list

#### E402 — NO ACTION NEEDED
- Already suppressed globally in pyproject.toml ignore list

#### F841 — 21 cases, all confirmed dead code, safe to auto-fix
- `shape` variables in test files — leftover from copy-paste
- `s`, `m` in test_allocator.py — computed but never asserted
- `final_barrier_count`, `default_pg`, `inner_ranks`, `op` — unused

#### F541 — 27 cases, all redundant f prefix on static strings, safe to auto-fix

#### F401 — 16 cases, all genuinely unused imports, safe to auto-fix

#### E721 — 1 case, type(curr) == type(tgt) → type(curr) is type(tgt)
- Comment says "Same type - must be identical" confirming exact type intent
- Safe to fix manually

#### F811 — 1 case, duplicate import torch inside function
- Module-level import already exists, inner one is redundant
- Safe to fix manually

### C/C++ decision
- Deferred to separate PR — touches Vigard's core files
- Original instructions say "really permissive, don't make Vigard mad"
- Will discuss with supervisor before proceeding

### Scope of PR-02
Python fixes only:
- F541, F401, F841 via auto-fix
- E721, F811 manually
- F821, E402 — skip, already suppressed globally

### Verification performed before opening PR-02

1. Syntax check: `python3 -m py_compile py/moodist/*.py tests/*.py benchmarks/*.py setup.py`
   Result: no output = zero syntax errors ✅

2. Import check: `python3 -c "import py.moodist"`
   Result: ModuleNotFoundError: torch — pre-existing, unrelated to our changes,
   torch not installed on c7i.flex.large (no GPU/CUDA) ✅

3. Ruff check: `ruff check py/ tests/ benchmarks/ setup.py`
   Result: 2 known false positives (F821, E402), suppressed in PR-01's pyproject.toml ✅

Full test suite cannot be run locally — requires GPU cluster + InfiniBand.
Will be validated by CI once PR-03 (GitHub Actions) is in place.