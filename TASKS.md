Track A — Hygiene
  - PR-01: Pre-commit hooks (clang-format + ruff, permissive)
  - PR-02: Fix violations surfaced by pre-commit

Track B — CI
  - PR-03: CI test runner (GPU + InfiniBand, skip gracefully on free tier)
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
