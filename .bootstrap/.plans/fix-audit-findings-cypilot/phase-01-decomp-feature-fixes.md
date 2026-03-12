```toml
[phase]
plan = "fix-audit-findings-cypilot"
number = 1
total = 8
type = "generate"
title = "DECOMPOSITION + FEATURE Spec Fixes"
depends_on = []
input_files = ["architecture/DECOMPOSITION.md", "architecture/features/kit-management.md", "architecture/features/agent-integration.md", "architecture/features/version-config.md", "architecture/features/v2-v3-migration.md", "architecture/features/developer-experience.md"]
output_files = ["architecture/DECOMPOSITION.md", "architecture/features/kit-management.md", "architecture/features/agent-integration.md", "architecture/features/version-config.md", "architecture/features/v2-v3-migration.md", "architecture/features/developer-experience.md"]
outputs = ["out/phase-01-done.md"]
inputs = []
```

## Preamble

This is a self-contained phase file. All rules, constraints, and context are included below. Follow the instructions exactly and report results against the acceptance criteria at the end.

## What

Fix stale IDs, missing sections, checkbox inconsistencies, and status indicators across DECOMPOSITION.md and 5 FEATURE spec files. These fixes address audit findings D-03, D-04, D-10, Q-10, Q-11, Q-12, Q-13, T-01 from `architecture/AUDIT-REPORT.md`.

## Rules

- **ID Rename**: Replace `cpt-cypilot-feature-blueprint-system` with `cpt-cypilot-feature-kit-management` everywhere. This is a global rename — search all 6 files.
- **Preserve Structure**: Do not change any content beyond what is specified. Do not add or remove sections. Do not rewrite prose.
- **Checkbox Consistency**: If a parent flow step references an algorithm that is fully checked `[x]`, the parent step should also be `[x]`. If the algorithm is partially checked, the parent stays `[ ]`.
- **Component IDs**: Use canonical `cpt-cypilot-component-*` format (e.g., `cpt-cypilot-component-config-manager`, not informal `config-manager`).

## Task

1. **Global rename** `cpt-cypilot-feature-blueprint-system` → `cpt-cypilot-feature-kit-management` in all 6 input files. Search for both the full ID and any partial references to `blueprint-system` in dependency lists.

2. **DECOMPOSITION.md fixes**:
   - §2.8 (Developer Experience): Add `cpt-cypilot-fr-core-toc` to the "Requirements Covered" list (it is implemented in this feature but was omitted).
   - §2.12 (Execution Plans): Replace informal `workflow-engine` component reference with `cpt-cypilot-component-agent-generator`.
   - §2.7 and §2.8: Replace informal component names (e.g., `config-manager`, `skill-engine`, `validator`, `traceability-engine`, `kit-manager`) with canonical `cpt-cypilot-component-*` IDs.
   - Update status indicators: Features with checked `[x]` checkboxes but ⏳ headers should have headers updated to ✅. Specifically check §2.1, §2.10, §2.11.

3. **v2-v3-migration.md**: Add a `## 6. Implementation Modules` section before the current §6 (Acceptance Criteria). List the implementation modules (e.g., `commands/migrate.py`, `commands/update.py`). Renumber subsequent sections.

4. **kit-management.md**: Reconcile flow/algo checkbox mismatch — if `cpt-cypilot-algo-kit-manifest-install` is `[x]`, then the parent flow step 7 (manifest-driven install) in `cpt-cypilot-flow-kit-install-cli` should also be `[x]`.

5. **agent-integration.md**: Add `[x]` checkbox markers to all CDSL steps that currently lack them (~15-20 steps). These steps are implemented — they just lack the tracking prefix.

6. **developer-experience.md**: Add `[x]` or `[ ]` checkbox markers to the Self-Check algorithm steps that lack them.

7. Save a brief completion log to `out/phase-01-done.md` listing all files modified and changes made.

## Acceptance Criteria

- [ ] Zero occurrences of `blueprint-system` remain in any of the 6 files
- [ ] `cpt-cypilot-fr-core-toc` appears in DECOMPOSITION §2.8 Requirements Covered
- [ ] `workflow-engine` replaced with canonical component ID in DECOMPOSITION §2.12
- [ ] Informal component names replaced with `cpt-cypilot-component-*` IDs in §2.7, §2.8
- [ ] DECOMPOSITION status indicators (§2.1, §2.10, §2.11) updated where checkbox is `[x]` but header shows ⏳
- [ ] v2-v3-migration.md has an "Implementation Modules" section
- [ ] kit-management.md flow/algo checkbox consistency resolved
- [ ] agent-integration.md CDSL steps all have checkbox markers
- [ ] developer-experience.md Self-Check steps all have checkbox markers
- [ ] Completion log saved to `out/phase-01-done.md`

## Output Format

When complete, report:

```
PHASE 1/8 COMPLETE
Status: PASS | FAIL
Files modified: {list}
Acceptance criteria: [x] / [ ] each
```

Then generate the prompt for the next phase.
