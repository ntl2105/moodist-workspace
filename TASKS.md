Track A — Hygiene
  - [x] PR-01: Pre-commit hooks (clang-format + ruff, permissive) - https://github.com/ntl2105/moodist/pull/1
  - [x] PR-02: Fix Python linting violations — https://github.com/ntl2105/moodist/pull/2

Track B — CI
  - [x] PR-03: GitHub Actions CI (syntax check + GPU test runner) — https://github.com/ntl2105/moodist/pull/3
  - PR-04: CI build verification
  - PR-05: CI cibuildwheel wheel builds as artifacts
  - PR-06: PyPI dev version publishing via Twine

Track C — Dependency Cleanup (audit-first given conservative stance)
  - PR-07-audit: fmt usage audit doc
  - PR-07: Replace fmt/ with std::format
  - PR-08-audit: pybind11 version + patch audit
  - PR-08: pybind11 via CMake ExternalProject
  - PR-09-audit: rdma-core version + patch audit
  - PR-09: rdma-core via CMake ExternalProject
  - PR-10: pyproject.toml

Track D — Benchmarks
  - PR-11: nccl-tests style CLI framework
  - PR-12: YAML workload config harness
  - PR-13: compile_op ablation benchmarks
