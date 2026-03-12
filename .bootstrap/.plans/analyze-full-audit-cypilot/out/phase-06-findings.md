# Phase 6: DECOMPOSITION + FEATURE Quality Review — Findings

## Summary

| Metric | Value |
|--------|-------|
| DECOMPOSITION entries reviewed | 12 / 12 |
| FEATURE spec files reviewed | 9 / 9 |
| CDSL quality assessed (files) | 9 / 9 (representative samples from all) |
| Acceptance criteria evaluated (files) | 9 / 9 |
| Issues found — CRITICAL | 2 |
| Issues found — HIGH | 8 |
| Issues found — MEDIUM | 7 |
| Issues found — LOW | 3 |
| **Total issues** | **20** |

---

## 1. DECOMPOSITION Issues

| # | Checklist Item | Severity | Artifact | Issue | Evidence | Proposal |
|---|---------------|----------|----------|-------|----------|----------|
| D1 | DECOMP-001 | HIGH | `DECOMPOSITION.md` §2.2 | Feature ID for Kit Management is stale — still uses the old blueprint-system ID | `**ID**: \`cpt-cypilot-feature-blueprint-system\`` (line 109) — ADR-0001 removed the blueprint system and renamed the feature to Kit Management | Rename to `cpt-cypilot-feature-kit-management` throughout DECOMPOSITION.md, DESIGN.md, and all FEATURE specs that reference it |
| D2 | DECOMP-001 | MEDIUM | `DECOMPOSITION.md` §2.7 | Design Components section says "Components reused from Feature 1 and Feature 2" without listing explicit component IDs | §2.7 line 345: `Components reused from Feature 1 (\`config-manager\`, \`skill-engine\`) and Feature 2 (\`kit-manager\`)` — backticked names are informal, not the canonical `cpt-cypilot-component-*` IDs | Use canonical IDs: `cpt-cypilot-component-config-manager`, `cpt-cypilot-component-skill-engine`, `cpt-cypilot-component-kit-manager` |
| D3 | DECOMP-001 | MEDIUM | `DECOMPOSITION.md` §2.8 | Design Components section says "Components reused from Feature 3" with informal names instead of canonical IDs | §2.8 line 403: `Components reused from Feature 3 (\`validator\`, \`traceability-engine\`)` | Use `cpt-cypilot-component-validator`, `cpt-cypilot-component-traceability-engine` |
| D4 | DECOMP-001 | MEDIUM | `DECOMPOSITION.md` §2.12 | Design Components section references `workflow-engine` which is not a defined component ID | §2.12 line 590: `Components reused from Feature 5 (\`workflow-engine\` via generate/analyze patterns)` — no `cpt-cypilot-component-workflow-engine` exists in DESIGN | Reference the actual component: `cpt-cypilot-component-agent-generator` |
| D5 | DECOMP-002 | HIGH | `DECOMPOSITION.md` §2.5 | Dependency references stale blueprint ID | §2.5 line 57 in `agent-integration.md`: `Dependencies: \`cpt-cypilot-feature-blueprint-system\`` — should be `cpt-cypilot-feature-kit-management` or `cpt-cypilot-feature-core-infra` as listed in DECOMPOSITION | Fix dependency reference to match DECOMPOSITION entry (which says `cpt-cypilot-feature-core-infra`) |
| D6 | DECOMP-003 | HIGH | `DECOMPOSITION.md` §2.10 | V2→V3 Migration depends on `cpt-cypilot-feature-traceability-validation` per entry, but the dependency graph (§3) shows it depends on both `core-infra` AND `traceability-validation` | §2.10 line 433: `Depends On: \`cpt-cypilot-feature-core-infra\`, \`cpt-cypilot-feature-traceability-validation\`` — this is correct. But Feature spec §1.4 line 84 also lists `cpt-cypilot-feature-blueprint-system` as a dependency — stale ID | Remove `cpt-cypilot-feature-blueprint-system` from v2-v3-migration.md §1.4 References or replace with `cpt-cypilot-feature-kit-management` |
| D7 | DECOMP-004 | MEDIUM | `DECOMPOSITION.md` §2.1 | Status shows ⏳ (in-progress) but ALL acceptance criteria in feature spec are checked `[x]` | §2.1 header: `⏳ HIGH` but `core-infra.md` §7 has all criteria checked | Update DECOMPOSITION status to ✅ or reconcile: either the feature is complete or some criteria should be unchecked |
| D8 | DECOMP-004 | MEDIUM | `DECOMPOSITION.md` §2.10 | Status shows ⏳ but feature checkbox is `[x]` (checked) | §2.10 line 429: `[x] \`p1\` - **ID**: \`cpt-cypilot-feature-v2-v3-migration\`` yet header says ⏳ | Update header status to ✅ to match the checked checkbox |
| D9 | DECOMP-004 | MEDIUM | `DECOMPOSITION.md` §2.11 | Status shows ⏳ but feature checkbox is `[x]` (checked) | §2.11 line 495: `[x] \`p1\` - **ID**: \`cpt-cypilot-feature-spec-coverage\`` yet header says ⏳ | Update header status to ✅ to match the checked checkbox |

---

## 2. FEATURE Spec Issues

### 2.1 core-infra.md

| # | Checklist Item | Severity | Issue | Evidence | Proposal |
|---|---------------|----------|-------|----------|----------|
| F1 | FEAT-001 | LOW | Heading numbering inconsistency in Feature Context | §1 subsections use `### 1. Overview`, `### 2. Purpose` etc. (numbered 1–4) but this conflicts with the parent `## 1. Feature Context` — looks like "1.1, 1.2" but rendered as "1, 2, 3, 4" | Adopt `### 1.1 Overview` pattern used consistently by `v2-v3-migration.md` and `execution-plans.md` |
| F2 | FEAT-004 | LOW | Implementation tracking checkbox on `cpt-cypilot-algo-core-infra-display-info` is `[ ]` (unchecked) but 11/12 child steps are `[x]` | Line 285: `[ ] \`p1\` - **ID**: \`cpt-cypilot-algo-core-infra-display-info\`` with step `inst-info-collect-resources` (line 304) also unchecked | Either check the parent (if nearly done) or note the one unchecked step as the blocker |

### 2.2 kit-management.md

| # | Checklist Item | Severity | Issue | Evidence | Proposal |
|---|---------------|----------|-------|----------|----------|
| F3 | FEAT-001 | HIGH | Feature ID at context level is stale: still uses blueprint-system ID | Line 50: `cpt-cypilot-featstatus-kit-management` (correct) but line 50 in §1 context: `cpt-cypilot-feature-blueprint-system` is wrong — should be `cpt-cypilot-feature-kit-management` | Rename the context-level ID reference; this was missed during ADR-0001 rename |
| F4 | FEAT-002 | CRITICAL | `cpt-cypilot-flow-kit-install-cli` step 7 checkbox is `[ ]` (unchecked) for manifest-driven install, but step 8 (legacy install) is `[x]` and the parent flow ID is also `[ ]` — the manifest install path is not yet marked implemented, yet several downstream algorithms (`cpt-cypilot-algo-kit-manifest-install`, etc.) are all `[x]` | Line 92: `[ ] - \`p1\` - **IF** kit source contains \`manifest.toml\`: delegate to manifest-driven installation` — but `cpt-cypilot-algo-kit-manifest-install` (line 392) is `[x]` | Reconcile: if manifest install is implemented, check the flow step; if not, uncheck the algorithm |

### 2.3 traceability-validation.md

| # | Checklist Item | Severity | Issue | Evidence | Proposal |
|---|---------------|----------|-------|----------|----------|
| F5 | FEAT-002 | HIGH | Validate Artifacts flow step 6 (resolve kit resources for manifest-driven kits) is `[ ]` unchecked while parent flow is also `[ ]` | Line 94: `[ ] - \`p1\` - **FOR EACH** kit: resolve resource paths — for manifest-driven kits...` | Track as unimplemented; no inconsistency per se, but it indicates a gap in manifest-driven kit support within validation |

### 2.4 agent-integration.md

| # | Checklist Item | Severity | Issue | Evidence | Proposal |
|---|---------------|----------|-------|----------|----------|
| F6 | FEAT-002 | HIGH | Multiple CDSL steps lack checkbox markers entirely | Lines 79, 80, 81, 83, 84, 85, 86, 100, 101, 102, 111, 112, 121, 122, 133, 134, 135, 142, 143: steps have no `[x]` or `[ ]` prefix — e.g., `4. - \`p1\` - Discover all workflow files...` | Add checkbox markers to all CDSL steps for implementation tracking consistency |
| F7 | FEAT-001 | HIGH | Feature References section lists stale dependency | Line 57: `Dependencies: \`cpt-cypilot-feature-blueprint-system\`` — this ID no longer exists per ADR-0001; DECOMPOSITION says dependency is `cpt-cypilot-feature-core-infra` | Replace with `cpt-cypilot-feature-core-infra` |

### 2.5 version-config.md

| # | Checklist Item | Severity | Issue | Evidence | Proposal |
|---|---------------|----------|-------|----------|----------|
| F8 | FEAT-001 | HIGH | Feature References section lists stale dependency | Line 55: `Dependencies: \`cpt-cypilot-feature-core-infra\`, \`cpt-cypilot-feature-blueprint-system\`` | Replace `cpt-cypilot-feature-blueprint-system` with `cpt-cypilot-feature-kit-management` |

### 2.6 developer-experience.md

| # | Checklist Item | Severity | Issue | Evidence | Proposal |
|---|---------------|----------|-------|----------|----------|
| F9 | FEAT-002 | MEDIUM | Self-Check algo steps 1, 3–5 lack checkbox markers | Lines 202–206: e.g., `1. - \`p1\` - Load constraints.toml for each kit` — no `[x]`/`[ ]` | Add checkboxes for implementation tracking |

### 2.7 spec-coverage.md

No issues found. Excellent structural completeness, CDSL quality, and acceptance criteria.

### 2.8 v2-v3-migration.md

| # | Checklist Item | Severity | Issue | Evidence | Proposal |
|---|---------------|----------|-------|----------|----------|
| F10 | FEAT-001 | LOW | Heading numbering uses `### 1.1 Overview` style (correct) — no issue. Included for completeness: Feature Context reference lists stale dependency | Line 84: `Dependencies: \`cpt-cypilot-feature-core-infra\`, \`cpt-cypilot-feature-blueprint-system\`, \`cpt-cypilot-feature-traceability-validation\`` | Replace `cpt-cypilot-feature-blueprint-system` with `cpt-cypilot-feature-kit-management` |
| F11 | FEAT-001 | CRITICAL | Section structure deviates from other features: uses `## 6. Acceptance Criteria` + `## 7. Additional Context` instead of the standard `## 6. Implementation Modules` + `## 7. Acceptance Criteria` | No "Implementation Modules" section exists — the v2-v3-migration spec does not list which code modules implement the algorithms | Add `## 6. Implementation Modules` table (e.g., `skills/.../commands/migrate.py`) and renumber subsequent sections |

### 2.9 execution-plans.md

| # | Checklist Item | Severity | Issue | Evidence | Proposal |
|---|---------------|----------|-------|----------|----------|
| F12 | FEAT-004 | HIGH | ALL CDSL steps across all flows and algorithms are `[ ]` (unchecked) — nothing is implemented | Every step in §2 and §3 is unchecked | This is expected for a new feature, but the DECOMPOSITION status (⏳ HIGH) should be noted — it's fully unimplemented. Acceptance criteria at §6 are also all `[ ]` |

---

## 3. Scope Overlap Analysis

| Feature Pair | Potential Overlap | Assessment |
|--------------|-------------------|------------|
| F1 (Core Infra) ↔ F7 (Version & Config) | Both touch `core.toml` and config management | **Clean partition**: F1 creates config during init; F7 migrates/updates config during `cpt update`. F7 explicitly says "Initial project setup (Feature 1)" is out of scope. |
| F1 (Core Infra) ↔ F10 (V2→V3 Migration) | Both create `core.toml`, `artifacts.toml`, inject AGENTS.md | **Clean partition**: F10 handles the one-time v2→v3 conversion; F1 handles fresh init. Migration reuses F1 algorithms but the scope boundary is clear. |
| F2 (Kit Management) ↔ F7 (Version & Config) | Both touch kit updates | **Clean partition**: F2 owns kit file-level diff/update; F7 owns the `cpt update` pipeline that triggers F2 for kit updates. F7 delegates to F2 explicitly. |
| F3 (Traceability) ↔ F8 (Dev Experience) | Both involve validation | **Clean partition**: F3 owns the validation engine; F8 owns developer tools that delegate to F3 (`self-check`, `doctor`, hooks). F8 explicitly notes "Components reused from Feature 3". |
| F3 (Traceability) ↔ F11 (Spec Coverage) | Both scan `@cpt-*` markers in code | **Clean partition**: F3 validates marker correctness (orphans, mismatches); F11 measures marker coverage (line ratio, granularity). F11 reuses F3's `codebase.py` scanning infrastructure. |
| F5 (Agent Integration) ↔ F12 (Execution Plans) | Both involve workflows | **Clean partition**: F5 generates agent entry points and discovers workflows; F12 adds the plan workflow as content consumed by F5. F12 depends on F5. |

**Conclusion**: No scope overlaps detected. Responsibilities are cleanly partitioned with explicit out-of-scope references.

---

## 4. Cross-Cutting Issues

| # | Severity | Issue | Files Affected | Proposal |
|---|----------|-------|----------------|----------|
| X1 | HIGH | Stale `cpt-cypilot-feature-blueprint-system` ID referenced across 5 files | `DECOMPOSITION.md` §2.2, `kit-management.md` §1, `agent-integration.md` §1.4, `version-config.md` §1.4, `v2-v3-migration.md` §1.4 | Global rename to `cpt-cypilot-feature-kit-management` |
| X2 | MEDIUM | Inconsistent Feature Context subsection numbering | `core-infra.md`, `kit-management.md`, `traceability-validation.md`, `agent-integration.md`, `version-config.md`, `developer-experience.md`, `spec-coverage.md` use `### 1. Overview` while `v2-v3-migration.md` and `execution-plans.md` use `### 1.1 Overview` | Standardize to one convention (recommend `### 1.1` for hierarchical clarity) |
| X3 | MEDIUM | Missing checkboxes on CDSL steps in `agent-integration.md` and `developer-experience.md` | ~20 steps without `[x]`/`[ ]` prefix | Add checkbox markers for implementation tracking completeness |

---

## 5. Severity Totals

| Severity | Count |
|----------|-------|
| CRITICAL | 2 (F4: kit-management flow/algo checkbox mismatch; F11: v2-v3 missing Implementation Modules section) |
| HIGH | 8 (D1, D5, D6, F3, F5, F6, F7, F8 — mostly stale blueprint-system ID + missing checkboxes) |
| MEDIUM | 7 (D2, D3, D4, D7, D8, D9, F9 — informal component refs, status inconsistencies) |
| LOW | 3 (F1, F2, F10 — heading style, minor tracking) |
| **TOTAL** | **20** |

### Top Findings

1. **Stale `cpt-cypilot-feature-blueprint-system` ID** — referenced in 5+ locations across DECOMPOSITION and FEATURE specs despite ADR-0001 removing the blueprint system. This is the single highest-impact fix.
2. **DECOMPOSITION status indicators lag behind reality** — 3 features show ⏳ in DECOMPOSITION headers but have checked `[x]` feature-level checkboxes.
3. **agent-integration.md has ~15 CDSL steps without checkbox markers** — implementation tracking is incomplete.
4. **v2-v3-migration.md is missing the standard "Implementation Modules" section** — deviates from the structural template used by all other feature specs.
5. **Kit Management flow/algo checkbox inconsistency** — manifest install flow step unchecked but underlying algorithm fully checked.
