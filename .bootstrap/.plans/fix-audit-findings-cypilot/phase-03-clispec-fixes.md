```toml
[phase]
plan = "fix-audit-findings-cypilot"
number = 3
total = 8
type = "generate"
title = "CLISPEC Remediation"
depends_on = []
input_files = ["skills/cypilot/cypilot.clispec", "architecture/DESIGN.md"]
output_files = ["skills/cypilot/cypilot.clispec"]
outputs = ["out/phase-03-done.md"]
inputs = []
```

## Preamble

This is a self-contained phase file. All rules, constraints, and context are included below. Follow the instructions exactly and report results against the acceptance criteria at the end.

## What

Update the CLISPEC to remove stale blueprint-era commands, fix the `update` description, and add missing commands and proxy flags. These fixes address audit findings D-01, D-02, D-05, D-06 from `architecture/AUDIT-REPORT.md`.

## Rules

- **CLISPEC Format**: The file `skills/cypilot/cypilot.clispec` uses a specific format. Read it first to understand the syntax before making changes.
- **Do not invent**: Only add commands that are actually implemented in code. Check `architecture/DESIGN.md` §3.3 for the authoritative command list.
- **Deprecation**: Deprecated commands should be marked with a `DEPRECATED` notice and a redirect to the replacement command, not deleted entirely.
- **Proxy flags**: These are flags handled by the proxy layer (`src/cypilot_proxy/cli.py`), not by the skill engine.

## Task

1. **Read** `skills/cypilot/cypilot.clispec` to understand the format.

2. **Fix `update` command description**: Remove any reference to blueprints. The update command now: updates the core skill from cache, performs file-level kit updates with interactive diff, regenerates `.gen/` aggregates, and runs layout migration if needed.

3. **Remove or deprecate `generate-resources`**: This command was removed per ADR-0001. Either delete the entry entirely or mark as `DEPRECATED — removed per ADR-0001. Kit files are now direct file packages.`

4. **Deprecate `kit migrate`**: Mark as `DEPRECATED — use \`kit update\` instead (per ADR-0001).`

5. **Add `agents` command**: This command regenerates agent integration files. It is implemented in `commands/agents.py` as `generate-agents` (skill-level name) but exposed to users as `agents`. Check DESIGN §3.3 for the correct description. Synopsis: `cpt agents [--agent AGENT]`.

6. **Add proxy-level flags** documentation: The proxy (`cpt` command) supports these flags before the subcommand:
   - `--source <dir>` — Use a local directory as the skill source instead of downloading from GitHub
   - `--url <url>` — Use a custom GitHub repository URL for downloads
   - `--no-cache` — Skip cache and force fresh download
   - `--version <version>` — Use a specific version tag

7. Save completion log to `out/phase-03-done.md`.

## Acceptance Criteria

- [ ] `update` command description contains no blueprint references
- [ ] `generate-resources` command removed or marked DEPRECATED
- [ ] `kit migrate` command marked DEPRECATED with redirect to `kit update`
- [ ] `agents` command added with correct synopsis and description
- [ ] Proxy-level flags documented (`--source`, `--url`, `--no-cache`, `--version`)
- [ ] No other commands or descriptions were accidentally modified
- [ ] Completion log saved to `out/phase-03-done.md`

## Output Format

When complete, report:

```
PHASE 3/8 COMPLETE
Status: PASS | FAIL
Files modified: {list}
Acceptance criteria: [x] / [ ] each
```

Then generate the prompt for the next phase.
