```toml
[phase]
plan = "fix-audit-findings-cypilot"
number = 7
total = 8
type = "implement"
title = "Implement kit move-config Command"
depends_on = [3]
input_files = ["architecture/DESIGN.md", "architecture/DECOMPOSITION.md", "architecture/features/kit-management.md", "skills/cypilot/scripts/cypilot/kit.py", "skills/cypilot/scripts/cypilot/cli.py"]
output_files = []
outputs = ["out/phase-07-done.md"]
inputs = []
```

## Preamble

This is a self-contained phase file. All rules, constraints, and context are included below. Follow the instructions exactly and report results against the acceptance criteria at the end.

## Prior Context

Phase 3 (CLISPEC Remediation) should be complete. The CLISPEC now reflects the current CLI surface. This phase adds a new command.

## What

Implement the `kit move-config` command, which relocates a kit's config directory to a new path. This is a p1 FR scope item (`cpt-cypilot-fr-core-kits` sub-requirement 8: "Kit config relocation") that is specified in DESIGN §3.3 and DECOMPOSITION §2.2 but has no code handler. This fix addresses audit finding A-04 from `architecture/AUDIT-REPORT.md`.

## Rules

- **Python 3.11+ stdlib-only**: No third-party dependencies (ADR-0002).
- **Existing patterns**: Follow the same command implementation pattern as other commands in `commands/`. Each command is a function that receives parsed arguments and a context object.
- **Kit config location**: Kit config directories are stored in `core.toml` under the kit's section. The command must update this path after moving files.
- **Preserve user edits**: The move must preserve all file contents. Use `shutil.copytree` + verify + remove, not `shutil.move`, for safety.
- **Error handling**: Fail gracefully if the source doesn't exist, the target already exists, or the move fails mid-operation.
- **Testing**: Write at least 2 unit tests: (1) successful move, (2) target-already-exists error.

## Task

1. **Read** the following to understand the implementation context:
   - `architecture/DESIGN.md` §3.3 for the `kit move-config` command specification
   - `architecture/features/kit-management.md` for the CDSL flow/algorithm if any
   - `skills/cypilot/scripts/cypilot/cli.py` for command registration pattern
   - `skills/cypilot/scripts/cypilot/kit.py` for existing kit operations

2. **Implement** `move_kit_config(slug, new_path, cypilot_dir)` function in `kit.py`:
   - Validate: slug exists in `core.toml`, source directory exists, target does not exist
   - Copy source to target using `shutil.copytree`
   - Verify copy integrity (file count matches)
   - Update `core.toml` kit path to new location
   - Remove source directory
   - Return success/failure result

3. **Add CLI handler** `_cmd_kit_move_config` following the existing command pattern. Register it in `cli.py` dispatch.

4. **Add CLISPEC entry** for `kit move-config <slug> <path>` in `skills/cypilot/cypilot.clispec`.

5. **Write tests** in `tests/test_kit_move_config.py`:
   - Test successful move (mock filesystem)
   - Test error when target exists
   - Test error when slug not found

6. Save completion log to `out/phase-07-done.md`.

## Acceptance Criteria

- [ ] `move_kit_config` function exists in `kit.py`
- [ ] CLI handler registered and dispatches correctly
- [ ] CLISPEC entry added for `kit move-config`
- [ ] Command validates inputs (slug exists, source exists, target doesn't exist)
- [ ] Command updates `core.toml` after successful move
- [ ] At least 2 tests written and passing
- [ ] No third-party dependencies added
- [ ] Completion log saved to `out/phase-07-done.md`

## Output Format

When complete, report:

```
PHASE 7/8 COMPLETE
Status: PASS | FAIL
Files modified: {list}
Files created: {list}
Acceptance criteria: [x] / [ ] each
```

Then generate the prompt for the next phase.
