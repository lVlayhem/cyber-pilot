# Phase 7: ADR Batch Quality Review — Findings

## Summary

| Metric | Value |
|--------|-------|
| ADRs reviewed | 14 / 14 |
| Full checklist (ADR-001–ADR-007) evaluated | 14 / 14 |
| Issues found — CRITICAL | 1 |
| Issues found — HIGH | 4 |
| Issues found — MEDIUM | 5 |
| Issues found — LOW | 3 |
| **Total issues** | **13** |

---

## 1. Per-ADR Summary Table

| ADR | ID | Structure OK? | Decision Clear? | Consequences Complete? | Issues |
|-----|----|:-------------:|:---------------:|:----------------------:|:------:|
| 0001 | `cpt-cypilot-adr-remove-blueprint-system` | ✅ | ✅ | ✅ | 1 |
| 0002 | `cpt-cypilot-adr-python-stdlib-only` | ✅ | ✅ | ✅ | 1 |
| 0003 | `cpt-cypilot-adr-pipx-distribution` | ✅ | ✅ | ✅ | 1 |
| 0004 | `cpt-cypilot-adr-toml-json-formats` | ✅ | ✅ | ✅ | 1 |
| 0005 | `cpt-cypilot-adr-markdown-contract` | ✅ | ✅ | ✅ | 1 |
| 0006 | `cpt-cypilot-adr-gh-cli-integration` | ✅ | ✅ | ✅ | 2 |
| 0007 | `cpt-cypilot-adr-proxy-cli-pattern` | ✅ | ✅ | ✅ | 1 |
| 0008 | `cpt-cypilot-adr-three-directory-layout` | ✅ | ✅ | ✅ | 1 |
| 0009 | `cpt-cypilot-adr-two-workflow-model` | ✅ | ✅ | ✅ | 2 |
| 0010 | `cpt-cypilot-adr-skill-md-entry-point` | ✅ | ✅ | ✅ | 1 |
| 0011 | `cpt-cypilot-adr-structured-id-format` | ✅ | ✅ | ✅ | 1 |
| 0012 | `cpt-cypilot-adr-git-style-conflict-markers` | ✅ | ✅ | ✅ | 2 |
| 0013 | `cpt-cypilot-adr-extract-sdlc-kit` | ⚠️ | ✅ | ✅ | 3 |
| 0014 | `cpt-cypilot-adr-remove-system-from-core-toml` | ✅ | ✅ | ✅ | 1 |

**Legend**: ✅ = pass, ⚠️ = minor structural issue

---

## 2. Issues Detail

### Cross-Cutting Issue

| # | Checklist | Severity | ADRs Affected | Issue | Evidence | Proposal |
|---|-----------|----------|---------------|-------|----------|----------|
| X1 | ADR-001 | LOW | All 14 ADRs | No `cypilot: true` marker in YAML frontmatter | All ADRs have frontmatter with `status`, `date`, `decision-makers` but none include `cypilot: true`. The `**ID**: \`cpt-cypilot-adr-*\`` line in the body serves as equivalent identification, but it is not in the frontmatter itself. | Either add `cypilot: true` to all ADR frontmatter blocks, or document in the ADR template that the `cpt-cypilot-adr-*` ID pattern serves as the equivalent marker. This is systemic — if resolved, apply to all 14. |

### Per-ADR Issues

| # | ADR | Checklist | Severity | Issue | Evidence | Proposal |
|---|-----|-----------|----------|-------|----------|----------|
| A1 | 0006 | ADR-006 | MEDIUM | PRD traceability reference is informal — no `cpt-*` IDs | Line 83: `**PRD**: PR review and PR status capabilities (SDLC kit)` — this is a description, not a traceable ID | Replace with specific IDs. Since PR review/status are SDLC kit capabilities (now in external repo), reference `cpt-cypilot-fr-core-kits` or note that the specific requirement IDs live in the kit's own PRD |
| A2 | 0009 | ADR-006 | MEDIUM | DESIGN traceability reference is informal — no `cpt-*` component ID | Line 91: `**DESIGN**: Architecture Drivers → Generic Workflows` — this is a section reference, not a canonical component or principle ID | Replace with `cpt-cypilot-component-agent-generator` or `cpt-cypilot-principle-two-workflow-model` (or whichever canonical DESIGN ID captures this) |
| A3 | 0012 | ADR-006 | MEDIUM | DESIGN traceability partially informal | Line 93: `**DESIGN**: \`cpt-cypilot-component-kit-manager\`, Resource Diff Engine` — "Resource Diff Engine" is not a canonical `cpt-*` ID | Replace with the canonical ID if one exists (e.g., `cpt-cypilot-component-diff-engine`), or add one to DESIGN |
| A4 | 0013 | ADR-007 | CRITICAL | Filename slug does not match ADR ID slug | Filename: `0013-cpt-cypilot-adr-extract-sdlc-kit-to-github-v1.md` → slug = `extract-sdlc-kit-to-github`. ID (line 9): `cpt-cypilot-adr-extract-sdlc-kit` → slug = `extract-sdlc-kit`. The phase-07 input table also shows expected ID as `cpt-cypilot-adr-extract-sdlc-kit`. | Either rename the file to `0013-cpt-cypilot-adr-extract-sdlc-kit-v1.md` to match the ID, or change the ID to `cpt-cypilot-adr-extract-sdlc-kit-to-github` to match the filename |
| A5 | 0013 | ADR-005 | HIGH | Status is `proposed` but extraction appears to be underway or complete | Frontmatter line 2: `status: proposed`. However, the SDLC kit already exists as a separate repository (`cyberfabric/cyber-pilot-kit-sdlc`) in the workspace, and the kit directory `kits/sdlc/` no longer appears in the Cypilot repo tree | If the decision has been accepted and acted upon, update status to `accepted`. If still pending final confirmation criteria, add a note explaining what remains |
| A6 | 0013 | ADR-001 | MEDIUM | `**ID**` line placement is before TOC, inconsistent with ADRs 0001–0012 | Line 9: `**ID**: \`cpt-cypilot-adr-extract-sdlc-kit\`` appears immediately after the title (line 7), before the `<!-- toc -->` block (line 11). In ADRs 0001–0012, the ID line appears after the closing `<!-- /toc -->` | Move the `**ID**` line after the `<!-- /toc -->` marker, consistent with ADRs 0001–0012. (ADR-0014 has the same pattern — fix both) |
| A7 | 0014 | ADR-001 | MEDIUM | `**ID**` line placement is before TOC, inconsistent with ADRs 0001–0012 | Line 9: `**ID**: \`cpt-cypilot-adr-remove-system-from-core-toml\`` appears before `<!-- toc -->` (line 11). Same structural inconsistency as ADR-0013. | Move the `**ID**` line after `<!-- /toc -->` for consistency |
| A8 | 0014 | ADR-005 | HIGH | Status is `proposed` — verify if decision has been accepted | Frontmatter line 2: `status: proposed`. ADR-0014 was dated 2026-03-10, very recent. If the decision is still under discussion, `proposed` is correct. | Confirm with project maintainer: if the decision is accepted, update status. If still proposed, no action needed |
| A9 | 0001 | ADR-004 | HIGH | Consequences do not explicitly list risks introduced | Consequences section (lines 60–67) lists Good/Bad/Neutral but does not have a dedicated "Risks" subsection. Key risk: bulk structural changes across kit files now require manual editing of every file — no single-source regeneration. This is mentioned as a "Bad" but not framed as a risk. | Add a brief Risks note: "Risk: Kit-wide structural refactors (e.g., heading renames across all artifact rules) have O(n) manual cost. Mitigated by: kit files are few (~20) and changes are infrequent." |
| A10 | 0009 | ADR-004 | HIGH | No negative consequences or risks documented for the chosen option | Consequences (lines 57–62): 3 Good, 1 Bad ("some operations feel like they need their own workflow"), 1 Neutral. No risks identified. The Bad consequence is a UX concern, not a risk. Missing: risk that agent routing errors (misclassifying intent) could produce wrong workflow execution. | Add risk: "Risk: Agent intent misclassification (generate vs analyze) could trigger the wrong workflow. Mitigated by: ambiguous cases prompt the user, and both workflows are read-only in their config-loading phase." |
| A11 | 0006 | ADR-004 | LOW | No negative consequences beyond `gh` pre-installation | Consequences are adequate but do not mention the risk of `gh` CLI API changes breaking subprocess parsing. | Consider adding: "Risk: `gh` CLI output format changes could break parsing. Mitigated by using `--json` flag for structured output." (This is partially addressed in the existing Bad consequence on line 56 but could be more explicit) |
| A12 | 0012 | ADR-004 | LOW | Impact on other components not explicitly noted | Consequences mention kit files being text-only as mitigation for binary limitation, but do not note that this decision depends on ADR-0001 (file-level diff model). The Related section (line 94) notes the dependency but the Consequences section does not. | Consider adding to Consequences: "Neutral, because this approach is enabled by ADR-0001's file-level diff model — conflict markers apply to individual file updates, not blueprint merges" |

---

## 3. Inter-ADR Consistency Analysis

### Dependency Chain Verification

| ADR | Declared Dependencies | Consistent? |
|-----|----------------------|:-----------:|
| 0003 | `cpt-cypilot-adr-python-stdlib-only` (0002) | ✅ |
| 0004 | `cpt-cypilot-adr-python-stdlib-only` (0002) | ✅ |
| 0006 | `cpt-cypilot-adr-python-stdlib-only` (0002) | ✅ |
| 0007 | `cpt-cypilot-adr-pipx-distribution` (0003) | ✅ |
| 0012 | `cpt-cypilot-adr-remove-blueprint-system` (0001) | ✅ |
| 0013 | `cpt-cypilot-adr-remove-blueprint-system` (0001) implicit | ✅ |

### Contradiction Check

No contradictions found between any ADR pairs. Specific checks:

- **ADR-0001 (remove blueprints) ↔ ADR-0012 (conflict markers)**: Consistent — ADR-0012 provides the merge UI for ADR-0001's file-level diff model
- **ADR-0001 (remove blueprints) ↔ ADR-0013 (extract SDLC kit)**: Consistent — ADR-0013 builds on ADR-0001's model of kits as file packages
- **ADR-0002 (stdlib-only) ↔ ADR-0004 (TOML/JSON)**: Consistent — ADR-0004 explicitly notes `tomllib` availability from 3.11+ stdlib
- **ADR-0002 (stdlib-only) ↔ ADR-0006 (gh CLI)**: Consistent — `gh` via subprocess avoids pip dependency
- **ADR-0008 (three-directory) ↔ ADR-0014 (remove system from core.toml)**: Consistent — both simplify `config/` responsibility; ADR-0014 narrows `core.toml` scope
- **ADR-0009 (two workflows) ↔ ADR-0010 (SKILL.md)**: Consistent — SKILL.md documents the two workflow entry points

### Special Attention Items

1. **ADR-0001 (blueprint removal)**: This is the most consequential ADR. Consequences are comprehensive with 3 Good, 2 Bad, 1 Neutral, detailed confirmation criteria, and a component removal table with line counts. The one gap: risks are embedded in "Bad" items rather than called out separately (issue A9).

2. **ADR-0013 (SDLC extraction)**: Properly references external repo `cyberfabric/cyber-pilot-kit-sdlc`. Filename/ID slug mismatch is the most critical finding (A4). Status appears stale (A5).

3. **ADR-0014 (system from core.toml)**: Extensively documents the `artifacts.toml` change with before/after examples, active reader analysis, and migration plan. Very thorough. Status may need updating (A8).

---

## 4. Severity Totals

| Severity | Count |
|----------|-------|
| CRITICAL | 1 (A4: ADR-0013 filename/ID mismatch) |
| HIGH | 4 (A5: ADR-0013 stale status; A8: ADR-0014 status check; A9: ADR-0001 missing risk framing; A10: ADR-0009 missing risks) |
| MEDIUM | 5 (A1: ADR-0006 informal PRD ref; A2: ADR-0009 informal DESIGN ref; A3: ADR-0012 informal DESIGN ref; A6: ADR-0013 ID placement; A7: ADR-0014 ID placement) |
| LOW | 3 (X1: missing `cypilot: true` systemic; A11: ADR-0006 risk gap; A12: ADR-0012 missing impact note) |
| **TOTAL** | **13** |

### Top Findings

1. **ADR-0013 filename/ID slug mismatch** (CRITICAL) — file says `extract-sdlc-kit-to-github` but ID says `extract-sdlc-kit`. This breaks automated ID-to-file resolution.
2. **ADR-0013 and ADR-0014 status may be stale** (HIGH) — both show `proposed` but evidence suggests ADR-0013's decision has been acted upon (SDLC kit exists as separate repo).
3. **Three ADRs have informal traceability references** (MEDIUM) — ADR-0006, ADR-0009, and ADR-0012 use descriptive text instead of canonical `cpt-*` IDs in their Traceability sections.
4. **No `cypilot: true` in any ADR frontmatter** (LOW/systemic) — all 14 ADRs use the `**ID**: \`cpt-*\`` body pattern instead. Template decision needed.
5. **ADR-0001 and ADR-0009 lack explicit risk documentation** (HIGH) — consequences list Good/Bad but do not frame risks as actionable items.
