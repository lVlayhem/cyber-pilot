```toml
[phase]
plan = "fix-audit-findings-cypilot"
number = 2
total = 8
type = "generate"
title = "ADR Housekeeping"
depends_on = []
input_files = ["architecture/ADR/0001-cpt-cypilot-adr-remove-blueprint-system-v1.md", "architecture/ADR/0009-cpt-cypilot-adr-two-workflow-model-v1.md", "architecture/ADR/0013-cpt-cypilot-adr-extract-sdlc-kit-to-github-v1.md", "architecture/ADR/0014-cpt-cypilot-adr-remove-system-from-core-toml-v1.md", "architecture/ADR/0006-cpt-cypilot-adr-gh-cli-integration-v1.md", "architecture/ADR/0012-cpt-cypilot-adr-git-style-conflict-markers-v1.md"]
output_files = ["architecture/ADR/0013-cpt-cypilot-adr-extract-sdlc-kit-v1.md"]
outputs = ["out/phase-02-done.md"]
inputs = []
```

## Preamble

This is a self-contained phase file. All rules, constraints, and context are included below. Follow the instructions exactly and report results against the acceptance criteria at the end.

## What

Fix ADR structural issues, stale statuses, missing risk documentation, and informal traceability references. These fixes address audit findings Q-14, Q-15, Q-16, Q-17, D-12, D-13 from `architecture/AUDIT-REPORT.md`.

## Rules

- **ADR Structure**: All ADRs follow the same template. The `**ID**:` line MUST appear after the `<!-- /toc -->` marker, consistent with ADRs 0001–0012.
- **Status Values**: Valid statuses are `proposed`, `accepted`, `deprecated`, `rejected`. Update frontmatter YAML `status:` field.
- **Traceability IDs**: The Traceability section of each ADR should use canonical `cpt-*` IDs (e.g., `cpt-cypilot-component-kit-manager`) rather than informal descriptions.
- **Risk Format**: Add risks as a subsection within the existing Consequences section, using the pattern: "Risk: {description}. Mitigated by: {mitigation}."
- **File Rename**: When renaming an ADR file, use `git mv` to preserve history.

## Task

1. **Rename ADR-0013 file** to match its ID slug:
   - Current: `0013-cpt-cypilot-adr-extract-sdlc-kit-to-github-v1.md`
   - New: `0013-cpt-cypilot-adr-extract-sdlc-kit-v1.md`
   - Use `git mv` command.

2. **Update ADR-0013 status** from `proposed` to `accepted` in YAML frontmatter. The SDLC kit has been extracted to a separate repository — the decision is fully acted upon.

3. **Update ADR-0014 status** from `proposed` to `accepted` in YAML frontmatter. The DESIGN fully integrates this decision with no hedging language.

4. **Fix ID placement in ADR-0013 and ADR-0014**: Move the `**ID**: \`cpt-cypilot-adr-*\`` line from before `<!-- toc -->` to after `<!-- /toc -->`, consistent with ADRs 0001–0012.

5. **Add risk documentation to ADR-0001** (Consequences section): Add "Risk: Kit-wide structural refactors have O(n) manual cost — no single-source regeneration. Mitigated by: kit files are few (~20) and structural changes are infrequent."

6. **Add risk documentation to ADR-0009** (Consequences section): Add "Risk: Agent intent misclassification (generate vs analyze) could trigger the wrong workflow. Mitigated by: ambiguous cases prompt the user, and both workflows are read-only in their config-loading phase."

7. **Fix informal traceability references**:
   - ADR-0006: Replace `**PRD**: PR review and PR status capabilities (SDLC kit)` with `**PRD**: \`cpt-cypilot-fr-core-kits\` (PR capabilities are SDLC kit-specific, now in external repo)`
   - ADR-0009: Replace `**DESIGN**: Architecture Drivers → Generic Workflows` with `**DESIGN**: \`cpt-cypilot-component-agent-generator\`, \`cpt-cypilot-principle-two-workflow-model\``
   - ADR-0012: Replace informal `Resource Diff Engine` with `\`cpt-cypilot-component-diff-engine\`` (or the actual canonical ID if different — check DESIGN §3.2)

8. Save completion log to `out/phase-02-done.md`.

## Acceptance Criteria

- [ ] ADR-0013 file renamed to `0013-cpt-cypilot-adr-extract-sdlc-kit-v1.md` (via git mv)
- [ ] ADR-0013 frontmatter status = `accepted`
- [ ] ADR-0014 frontmatter status = `accepted`
- [ ] ADR-0013 and ADR-0014 `**ID**` lines are after `<!-- /toc -->`
- [ ] ADR-0001 Consequences section includes risk documentation
- [ ] ADR-0009 Consequences section includes risk documentation
- [ ] ADR-0006, ADR-0009, ADR-0012 traceability sections use canonical `cpt-*` IDs
- [ ] Completion log saved to `out/phase-02-done.md`

## Output Format

When complete, report:

```
PHASE 2/8 COMPLETE
Status: PASS | FAIL
Files modified: {list}
Acceptance criteria: [x] / [ ] each
```

Then generate the prompt for the next phase.
