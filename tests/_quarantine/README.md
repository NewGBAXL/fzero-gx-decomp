# Quarantined tests

These 59 test files (plus `conftest.py` and the `_fx/` fixtures they share)
were moved out of `tests/` on 2026-09-04 because none of them can currently
run: they import `tools/<name>.py` scripts (mostly `natc_*.py`, plus
`sda21_fix.py`, `similar.py`, `find_xrefs.py`, `find_slice_unit.py`,
`carve_guard.py`, `emit_m2c_asm.py`, `tool_health_sweep.py`, and others) that
do not exist anywhere in this repository.

Verified before moving: `pytest tests/ --continue-on-collection-errors` gave
**81 failed, 1 passed, 2 skipped, 86 errors** out of 60 files. The 1 pass was
`test_decomp_report.py`, which is the only script under test that's actually
committed — it stayed in `tests/`.

Best guess at the cause: this public repo is a mirror that receives
converted source via "public mirror" commits from a private automation
pipeline (see `notes/STRUCTURE-AUDIT.md`), but the pipeline's own tooling
and this test suite were committed here without the tooling itself ever
landing in `tools/`.

Nothing here was deleted. If the missing `tools/*.py` scripts get committed
(or the private pipeline is retired and these are rewritten against
whatever replaces it), pull the relevant files back into `tests/` and drop
them from `.gitignore`/CI-exclusion as needed. Until then, `pytest tests/`
should only ever discover `test_decomp_report.py` — don't add this
directory back to test discovery without checking that the specific
`tools/*.py` a test needs actually exists first.

See `notes/STRUCTURE-AUDIT.md` for the full audit this came out of.
