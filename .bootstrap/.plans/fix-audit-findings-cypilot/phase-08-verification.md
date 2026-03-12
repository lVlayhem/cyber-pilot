```toml
[phase]
plan = "fix-audit-findings-cypilot"
number = 8
total = 8
type = "analyze"
title = "Verification Pass"
depends_on = [1, 2, 3, 4, 5, 6, 7]
input_files = ["architecture/AUDIT-REPORT.md"]
output_files = []
outputs = ["out/phase-08-done.md"]
inputs = ["out/phase-01-done.md", "out/phase-02-done.md", "out/phase-03-done.md", "out/phase-04-done.md", "out/phase-05-done.md", "out/phase-06-done.md", "out/phase-07-done.md"]
```

## Preamble

This is a self-contained phase file. All rules, constraints, and context are included below. Follow the instructions exactly and report results against the acceptance criteria at the end.

## What

Verify that all 27 audit recommendations have been addressed by phases 1-7. Read each phase's completion log, cross-reference against the audit report recommendations, and produce a final verification summary.

## Prior Context

All 7 prior phases should be complete. Each phase saved a completion log to `out/phase-{N}-done.md`.

## Task

1. **Read all 7 completion logs** from `out/phase-01-done.md` through `out/phase-07-done.md`.

2. **Read** `architecture/AUDIT-REPORT.md` §7 (Prioritized Recommendations) to get the full list of 27 recommendations.

3. **Cross-reference**: For each recommendation, verify it was addressed by a phase. Mark as:
   - ✅ Fixed — the phase log confirms the change was made
   - ⚠️ Partial — the change was attempted but not fully completed
   - ❌ Not addressed — no phase covered this recommendation

4. **Spot-check key fixes** by reading the actual files:
   - Verify zero occurrences of `blueprint-system` in DECOMPOSITION.md: `grep -r "blueprint-system" architecture/DECOMPOSITION.md`
   - Verify ADR-0013 file was renamed: check `architecture/ADR/` listing
   - Verify `cypilot` entry point exists in `pyproject.toml`
   - Verify `agents` command exists in CLISPEC

5. **Run tests**: `pytest tests/ -x -q` to verify no regressions.

6. **Produce verification summary** in `out/phase-08-done.md`:
   - Table: Rec # | Description | Phase | Status (✅/⚠️/❌)
   - Test results
   - Any remaining items to address
   - Overall PASS/FAIL

7. **Update plan.toml**: Set plan lifecycle to `archive` if all recommendations are addressed.

## Acceptance Criteria

- [ ] All 7 phase completion logs read
- [ ] All 27 recommendations cross-referenced with phase outcomes
- [ ] At least 4 spot-checks performed on actual files
- [ ] Tests run and results reported
- [ ] Verification summary saved to `out/phase-08-done.md`
- [ ] Plan lifecycle updated if appropriate

## Output Format

When complete, report:

```
PHASE 8/8 COMPLETE
Status: PASS | FAIL
Files modified: plan.toml (if lifecycle updated)
Acceptance criteria: [x] / [ ] each
Recommendations: N/27 fixed, N partial, N not addressed
Test results: PASS/FAIL
```

This is the **last phase**. Output:

```
ALL PHASES COMPLETE (8/8)
Plan: .bootstrap/.plans/fix-audit-findings-cypilot/plan.toml
Lifecycle: archive
```

Then ask: `Continue in this chat? [y/n]`
