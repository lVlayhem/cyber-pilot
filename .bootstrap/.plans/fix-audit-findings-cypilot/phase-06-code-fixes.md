```toml
[phase]
plan = "fix-audit-findings-cypilot"
number = 6
total = 8
type = "implement"
title = "Quick Code Fixes"
depends_on = []
input_files = ["pyproject.toml", "skills/cypilot/scripts/cypilot/utils/toc.py"]
output_files = ["pyproject.toml", "skills/cypilot/scripts/cypilot/utils/toc.py"]
outputs = ["out/phase-06-done.md"]
inputs = []
```

## Preamble

This is a self-contained phase file. All rules, constraints, and context are included below. Follow the instructions exactly and report results against the acceptance criteria at the end.

## What

Two quick code fixes: (1) add the `cypilot` console entry point alias to `pyproject.toml`, and (2) clean stale blueprint references in `toc.py` docstrings. These fixes address audit findings A-03 and D-09 from `architecture/AUDIT-REPORT.md`.

## Rules

- **Minimal Changes**: Only modify the specific lines described. Do not refactor or restructure.
- **Python stdlib-only**: Per ADR-0002, no third-party dependencies may be added.
- **Entry Points**: The `pyproject.toml` `[project.scripts]` section defines console entry points. Add `cypilot` as an alias pointing to the same module as `cpt`.
- **Docstring Updates**: Replace "blueprint" references with "kit content" or equivalent. Do not change code logic.

## Task

1. **Read** `pyproject.toml` to find the `[project.scripts]` section.

2. **Add `cypilot` entry point**: Add a `cypilot` entry pointing to the same target as `cpt`. For example, if `cpt` points to `cypilot_proxy.cli:main`, add `cypilot = "cypilot_proxy.cli:main"`.

3. **Read** `skills/cypilot/scripts/cypilot/utils/toc.py` to find stale blueprint references in docstrings.

4. **Clean docstrings**: Replace references to "Blueprint artifact generator" and "blueprint-generated content" with "Kit content generator" and "kit-generated content" (or similar accurate descriptions). Only change docstrings/comments — do not change any code logic.

5. **Run tests** to verify nothing is broken: `pytest tests/ -x -q` (if available).

6. Save completion log to `out/phase-06-done.md`.

## Acceptance Criteria

- [ ] `pyproject.toml` has both `cpt` and `cypilot` console entry points
- [ ] `toc.py` docstrings contain no references to "blueprint"
- [ ] No code logic was changed in `toc.py`
- [ ] Tests pass (or note if test runner unavailable)
- [ ] Completion log saved to `out/phase-06-done.md`

## Output Format

When complete, report:

```
PHASE 6/8 COMPLETE
Status: PASS | FAIL
Files modified: {list}
Acceptance criteria: [x] / [ ] each
```

Then generate the prompt for the next phase.
