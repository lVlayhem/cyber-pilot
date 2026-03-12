```toml
[phase]
plan = "fix-audit-findings-cypilot"
number = 5
total = 8
type = "generate"
title = "DESIGN Updates"
depends_on = [2]
input_files = ["architecture/DESIGN.md"]
output_files = ["architecture/DESIGN.md"]
outputs = ["out/phase-05-done.md"]
inputs = []
```

## Preamble

This is a self-contained phase file. All rules, constraints, and context are included below. Follow the instructions exactly and report results against the acceptance criteria at the end.

## What

Update DESIGN.md to add missing ADR traceability entries, capture under-documented ADR details, mark unimplemented commands as planned, and add a module/package mapping. These fixes address audit findings T-06, A-01, A-02, A-05, Q-08, Q-09 from `architecture/AUDIT-REPORT.md`.

## Prior Context

Phase 2 (ADR Housekeeping) must complete first — it updates ADR-0013 and ADR-0014 status to `accepted`. This phase adds those ADRs to DESIGN Section 5.

## Rules

- **DESIGN Purpose**: The DESIGN document describes the system architecture — components, principles, constraints, interactions. It does not contain implementation code or task breakdowns.
- **Preserve Structure**: Do not reorganize existing sections. Add new content in the appropriate location within existing sections.
- **Mermaid Diagrams**: Use Mermaid syntax for any new diagrams. Keep diagrams simple and focused.
- **Minimal Changes**: Make targeted edits. Do not rewrite existing content.

## Task

1. **Read** `architecture/DESIGN.md`.

2. **Add ADRs 0013 and 0014 to Section 5** (Traceability): Section 5 currently lists 12 ADRs. Add entries for:
   - `cpt-cypilot-adr-extract-sdlc-kit` (0013) — accepted
   - `cpt-cypilot-adr-remove-system-from-core-toml` (0014) — accepted

3. **Add dual-mode CLI output to DESIGN** (A-01): In Section 2.1 (Principles), update `cpt-cypilot-principle-machine-readable` to capture that the default output is human-readable (to stderr with ANSI colors/icons) and `--json` is an opt-in flag producing structured JSON to stdout. This is per ADR-0004.

4. **Add conflict marker format to DESIGN** (A-02): In the Resource Diff Engine section (§3.2), add a brief note that the `[m]odify` mode uses git-style conflict markers (`<<<<<<< installed (yours)` / `=======` / `>>>>>>> upstream (source)`) and re-validates that no unresolved markers remain after editing. This is per ADR-0012.

5. **Mark unimplemented commands as planned** (A-05): In Section 3.3 (API Contracts), for these commands that appear in DESIGN but are not yet implemented, add a `(planned)` marker:
   - `doctor`
   - `config show`
   - `config system add`
   - `config system remove`
   - `hook install`
   - `hook uninstall`
   - `kit move-config`

6. **Add module/package mapping** (Q-09): Add a brief subsection (e.g., after §3.2 or in §1.3) showing how architecture components map to Python modules:

   | Component | Modules |
   |-----------|---------|
   | CLI Proxy | `src/cypilot_proxy/cli.py`, `resolve.py`, `cache.py` |
   | Skill Engine | `cli.py` (dispatch) |
   | Validator | `commands/validate.py`, `commands/validate_kits.py` |
   | Traceability Engine | `commands/list_ids.py`, `where_defined.py`, `where_used.py`, `get_content.py`, `document.py`, `parsing.py`, `codebase.py` |
   | Config Manager | `context.py`, `toml_utils.py`, `artifacts_meta.py`, `files.py` |
   | Kit Manager | `kit.py`, `diff_engine.py`, `manifest.py` |
   | Agent Generator | `commands/agents.py` |

7. Save completion log to `out/phase-05-done.md`.

## Acceptance Criteria

- [ ] ADRs 0013 and 0014 appear in DESIGN Section 5 traceability list
- [ ] `principle-machine-readable` updated to describe dual-mode (human-readable default + `--json`)
- [ ] Resource Diff Engine section mentions git-style conflict markers and unresolved-marker detection
- [ ] Unimplemented commands in §3.3 marked as `(planned)`
- [ ] Module/package mapping added showing component → Python module correspondence
- [ ] No unintended changes to existing content
- [ ] Completion log saved to `out/phase-05-done.md`

## Output Format

When complete, report:

```
PHASE 5/8 COMPLETE
Status: PASS | FAIL
Files modified: {list}
Acceptance criteria: [x] / [ ] each
```

Then generate the prompt for the next phase.
