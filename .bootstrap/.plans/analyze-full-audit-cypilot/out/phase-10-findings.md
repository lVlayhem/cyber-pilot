# Phase 10 Findings — Specs vs Codebase Bidirectional Gap Analysis

**Plan**: analyze-full-audit-cypilot
**Phase**: 10/11
**Date**: 2026-03-12
**Input files**: `out/phase-01-findings.md`, `out/phase-02-findings.md`, `out/phase-08-findings.md`, `out/phase-09-findings.md`
**Spec files**: `architecture/PRD.md`, `architecture/DESIGN.md`, `architecture/DECOMPOSITION.md`

---

## 1. Summary

| Metric | Result |
|--------|--------|
| PRD FRs traced end-to-end | 21 |
| Full chain (PRD → DESIGN → DECOMP → Code) | 14/21 (67%) |
| Spec gaps (spec without code) | 5 (3 expected p2/p3 backlog) |
| Code gaps (code without spec) | 4 |
| Implementation drift items | 9 (cross-cutting, from phases 8/9 + phase 2) |
| Priority inversions | 0 |
| Extracted feature boundary violations | 0 |
| Overall status | **PASS with observations** |

---

## 2. Full Traceability Matrix — PRD FR → DESIGN → DECOMP → Code

### 2.1 Priority 1 FRs

| # | FR ID | PRD Status | DESIGN | DECOMP Feature | Code Status | Chain |
|---|-------|-----------|--------|----------------|-------------|-------|
| 1 | `cpt-cypilot-fr-core-installer` | [x] done | PASS (Ph1 §2) | F1 Core Infrastructure | Proxy: `resolve.py`, `cli.py`, `cache.py` (Ph8 ✅) | **COMPLETE** — minor: missing `cypilot` entry point (Ph8 F-01) |
| 2 | `cpt-cypilot-fr-core-init` | [x] done | PASS (Ph1 §2) | F1, F10 | `commands/init.py` (Ph9 §1.1 #1) | **COMPLETE** |
| 3 | `cpt-cypilot-fr-core-config` | [x] done | PASS (Ph1 §2) | F1, F10 | `context.py`, `toml_utils.py`, `artifacts_meta.py`, `files.py` (Ph9 §2 #4) | **COMPLETE** |
| 4 | `cpt-cypilot-fr-core-skill-engine` | [x] done | PASS (Ph1 §2) | F1 | `cli.py` dispatch (Ph9 §2 #1) | **COMPLETE** |
| 5 | `cpt-cypilot-fr-core-workflows` | [x] done | PASS (Ph1 §2) | F5, F12 | `.core/workflows/generate.md`, `analyze.md`, `plan.md` | **COMPLETE** |
| 6 | `cpt-cypilot-fr-core-execution-plans` | [ ] open | PASS (Ph1 §2) | F12 | `workflows/plan.md` + `requirements/plan-template.md` + `requirements/plan-decomposition.md` (prompt-level, no CLI by design) | **COMPLETE** (workflow-level) |
| 7 | `cpt-cypilot-fr-core-agents` | [x] done | PASS (Ph1 §2) | F5 | `commands/agents.py` (Ph9 §1.1 #11) | **COMPLETE** — `agents` status command undocumented in CLISPEC (Ph9 F-02) |
| 8 | `cpt-cypilot-fr-core-kits` | [x] done | PASS (Ph1 §2) | F2 | `kit.py`, `diff_engine.py`, `manifest.py` (Ph9 §2 #5) | **COMPLETE** |
| 9 | `cpt-cypilot-fr-core-kit-manifest` | [ ] open | PASS (Ph1 §2) | F2 | `manifest.py` exists (Ph9 §2 #5) | **COMPLETE** (code exists) |
| 10 | `cpt-cypilot-fr-core-resource-diff` | [x] done | PASS (Ph1 §2) | F2 | `diff_engine.py` (Ph9 §2 #5) | **COMPLETE** |
| 11 | `cpt-cypilot-fr-core-layout-migration` | [ ] open | PASS (Ph1 §2) | F7 | `kit.py:_detect_and_migrate_layout` (Ph9 §4.2 ACTIVE) | **COMPLETE** |
| 12 | `cpt-cypilot-fr-core-traceability` | [x] done | PASS (Ph1 §2) | F3, F11 | `list_ids.py`, `where_defined.py`, `where_used.py`, `get_content.py`, `document.py`, `parsing.py`, `codebase.py` (Ph9 §2 #3) | **COMPLETE** |
| 13 | `cpt-cypilot-fr-core-cdsl` | [x] done | PASS (Ph1 §2) | F3, F11 | CDSL parsing in validation + coverage (Ph9 §2 #3) | **COMPLETE** |
| 14 | `cpt-cypilot-fr-core-toc` | [x] done | PASS (Ph1 §2) | **UNALLOCATED** (Ph2 §3) | `commands/toc.py`, `commands/validate_toc.py` (Ph9 §1.1 #13, #14) | **BROKEN** — DECOMP link missing |

### 2.2 Priority 2 FRs

| # | FR ID | PRD Status | DESIGN | DECOMP Feature | Code Status | Chain |
|---|-------|-----------|--------|----------------|-------------|-------|
| 15 | `cpt-cypilot-fr-core-version` | [ ] open | PASS (Ph1 §2) | F7 | `commands/update.py` (Ph9 §1.1 #15) | **COMPLETE** |
| 16 | `cpt-cypilot-fr-core-cli-config` | [ ] open | PASS (Ph1 §2) | F7 | Partial — `config show`, `config system add/remove` NOT implemented (Ph9 F-03) | **PARTIAL** |
| 17 | `cpt-cypilot-fr-core-template-qa` | [ ] open | PASS (Ph1 §2) | F8 | `self_check.py` via `validate-kits` alias (Ph9 F-01) | **COMPLETE** (routing mismatch) |
| 18 | `cpt-cypilot-fr-core-doctor` | [ ] open | PASS (Ph1 §2) | F8 | NOT implemented (Ph9 F-03) | **SPEC GAP** |
| 19 | `cpt-cypilot-fr-core-vscode-plugin` | [ ] open | PASS (Ph1 §2) | F8 | NOT implemented (separate deliverable) | **SPEC GAP** (expected) |

### 2.3 Priority 3 FRs

| # | FR ID | PRD Status | DESIGN | DECOMP Feature | Code Status | Chain |
|---|-------|-----------|--------|----------------|-------------|-------|
| 20 | `cpt-cypilot-fr-core-hooks` | [ ] open | PASS (Ph1 §2) | F8 | NOT implemented (Ph9 F-03) | **SPEC GAP** |
| 21 | `cpt-cypilot-fr-core-completions` | [ ] open | PASS (Ph1 §2) | F8 | NOT implemented | **SPEC GAP** |

### 2.4 Matrix Summary

| Priority | Total | Full Chain | Partial | Spec Gap | Broken Link |
|----------|-------|-----------|---------|----------|-------------|
| p1 | 14 | 13 | 0 | 0 | 1 (`fr-core-toc` DECOMP gap) |
| p2 | 5 | 2 | 1 | 2 | 0 |
| p3 | 2 | 0 | 0 | 2 | 0 |
| **Total** | **21** | **15** | **1** | **4** | **1** |

---

## 3. Spec Gaps — GAP-001: Forward Coverage (Spec → Code)

### 3.1 Specs With No Code Implementation

| # | FR ID | Priority | DECOMP | Gap Type | Impact | Notes |
|---|-------|----------|--------|----------|--------|-------|
| SG-01 | `cpt-cypilot-fr-core-doctor` | p2 | F8 | Unimplemented | Degraded | Environment diagnostics command not implemented. Referenced in DESIGN §3.3 API Contracts. Per Ph9 F-03. |
| SG-02 | `cpt-cypilot-fr-core-hooks` | p3 | F8 | Unimplemented | Cosmetic | `hook install/uninstall` not implemented. Referenced in DESIGN §3.3. Per Ph9 F-03. |
| SG-03 | `cpt-cypilot-fr-core-completions` | p3 | F8 | Unimplemented | Cosmetic | Shell completions not implemented. Not referenced in CLISPEC. |
| SG-04 | `cpt-cypilot-fr-core-vscode-plugin` | p2 | F8 | Unimplemented | Cosmetic | Separate deliverable (VS Code extension). Expected gap. |

### 3.2 Specs With Partial Code Implementation

| # | FR ID | Priority | DECOMP | Gap Type | Impact | Notes |
|---|-------|----------|--------|----------|--------|-------|
| SG-05 | `cpt-cypilot-fr-core-cli-config` | p2 | F7 | Partial | Degraded | `config show`, `config system add/remove` not implemented (Ph9 F-03). `migrate-config` and `--version` ARE implemented. |
| SG-06 | `cpt-cypilot-fr-core-installer` | p1 | F1 | Minor gap | Degraded | Missing `cypilot` console entry point — only `cpt` registered (Ph8 F-01). Core installer functionality works. |

### 3.3 DESIGN API Contracts Without Code

Cross-referencing Ph9 §1.3 with DESIGN §3.3:

| # | DESIGN Command | FR Source | In CLISPEC | In Code | Impact |
|---|---------------|-----------|-----------|---------|--------|
| 1 | `doctor` | `fr-core-doctor` (p2) | No | No | Degraded |
| 2 | `config show` | `fr-core-cli-config` (p2) | No | No | Degraded |
| 3 | `config system add/remove` | `fr-core-cli-config` (p2) | No | No | Degraded |
| 4 | `hook install/uninstall` | `fr-core-hooks` (p3) | No | No | Cosmetic |
| 5 | `kit move-config <slug>` | `fr-core-kits` (p1) | No | No | Degraded |

**Cross-cutting observation**: `kit move-config` is specified in DESIGN §3.3, allocated to F2 Kit Management in DECOMPOSITION (§2.2 Scope), but has no code handler and no CLISPEC entry. This is a **p1 spec gap** that was not flagged as such in Phase 9 because it was grouped under the broader F-03 finding. Unlike the other unimplemented commands (p2/p3), `kit move-config` falls under the p1 `fr-core-kits` requirement.

### 3.4 DECOMPOSITION Traceability Gaps

| # | Issue | Source | Impact |
|---|-------|--------|--------|
| SG-07 | `fr-core-toc` unallocated in DECOMPOSITION | Ph2 §3 | Cosmetic — code exists in F8, just missing from "Requirements Covered" |
| SG-08 | 5 NFRs unallocated by NFR ID in DECOMPOSITION | Ph2 §3 | Cosmetic — addressed via Design Principle references instead |

---

## 4. Code Gaps — GAP-002: Reverse Coverage (Code → Spec)

### 4.1 Undocumented Commands

| # | Code Element | Handler | Spec Coverage | Impact | Source |
|---|-------------|---------|---------------|--------|--------|
| CG-01 | `agents` command | `commands/agents.py:cmd_agents` | In DESIGN §3.3, NOT in CLISPEC | Degraded | Ph9 F-02 |
| CG-02 | `validate-code` alias | → `_cmd_validate` | Not in CLISPEC | Cosmetic | Ph9 §1.2 |
| CG-03 | `validate-rules` alias | → `_cmd_validate_kits` | Not in CLISPEC | Cosmetic | Ph9 §1.2 |

### 4.2 Undocumented Code Capabilities

| # | Code Element | File | Spec Coverage | Impact | Source |
|---|-------------|------|---------------|--------|--------|
| CG-04 | Fork URL support (`_resolve_api_base`) | `cache.py:31-52` | Not in CDSL algo, DESIGN, or CLISPEC | Cosmetic | Ph8 F-05 |
| CG-05 | Local copy (`copy_from_local`) | `cache.py:94-157` | Not in CDSL algo or CLISPEC | Cosmetic | Ph8 F-04 |
| CG-06 | Proxy flags (`--source`, `--url`, `--no-cache`, `--version VALUE`) | `cli.py` | Not in CLISPEC | Degraded | Ph8 F-02 |
| CG-07 | Legacy fallback scan (`find_install_dir`) | `resolve.py:113-116` | Not in resolve algorithm | Cosmetic | Ph8 F-11 |

### 4.3 Code Gap Summary

| Impact | Count | IDs |
|--------|-------|-----|
| Blocking | 0 | — |
| Degraded | 2 | CG-01, CG-06 |
| Cosmetic | 5 | CG-02, CG-03, CG-04, CG-05, CG-07 |

---

## 5. Implementation Drift — GAP-003

### 5.1 Spec Drift Items (Code Contradicts Spec)

| # | Item | Type | Severity | Source | Impact |
|---|------|------|----------|--------|--------|
| DR-01 | CLISPEC `update` description references removed blueprint system | Spec stale | HIGH | Ph8 F-06 | Degraded — spec misleads consumers |
| DR-02 | CLISPEC defines removed commands (`kit migrate`, `generate-resources`) | Spec stale | HIGH | Ph8 F-07 | Degraded — spec advertises non-functional commands |
| DR-03 | Feature 2.2 uses stale ID `cpt-cypilot-feature-blueprint-system` | Spec stale | HIGH | Ph2 §8 | Degraded — breaks DECOMP↔FEATURE traceability |
| DR-04 | 3 FEATURE specs declare stale `blueprint-system` dependency | Spec stale | HIGH | Ph2 §5 | Degraded — misleading dependency chain (F5, F7, F10) |
| DR-05 | Feature 2.12 references non-existent `workflow-engine` component | Spec error | MEDIUM | Ph2 §2 | Cosmetic — should reference `component-agent-generator` |
| DR-06 | "Background" version check is synchronous post-execution | Wording drift | LOW | Ph8 F-03 | Cosmetic — behavior correct, wording misleading |
| DR-07 | Init/Update special routing not in CDSL flow | Spec incomplete | LOW | Ph8 F-12 | Cosmetic — correct behavior, underspecified |
| DR-08 | `self-check` routed as alias to `validate-kits` | Routing mismatch | MEDIUM | Ph9 F-01 | Degraded — CLISPEC implies standalone command |
| DR-09 | Stale blueprint references in `toc.py` docstrings | Doc debt | LOW | Ph9 F-04 | Cosmetic — code correct, comments stale |

### 5.2 Cross-Cutting Drift Pattern: ADR-0001 Propagation Failure

**Pattern**: ADR-0001 (Remove Blueprint System) was accepted and code was correctly updated, but spec artifacts were NOT fully updated. This single ADR accounts for **4 of 9 drift items** (DR-01, DR-02, DR-03, DR-04).

Affected specs:
- `skills/cypilot/cypilot.clispec` — `update` description, `kit migrate` definition, `generate-resources` definition
- `architecture/DECOMPOSITION.md` — Feature 2.2 ID, dependency graph
- `architecture/features/agent-integration.md` — Dependencies section
- `architecture/features/version-config.md` — Dependencies section
- `architecture/features/v2-v3-migration.md` — Dependencies section

**Root cause**: No systematic "ADR propagation checklist" exists to ensure that when an ADR is accepted, all referencing spec artifacts are updated.

### 5.3 Drift Summary

| Severity | Count | IDs |
|----------|-------|-----|
| HIGH | 4 | DR-01, DR-02, DR-03, DR-04 |
| MEDIUM | 2 | DR-05, DR-08 |
| LOW | 3 | DR-06, DR-07, DR-09 |

---

## 6. Priority Alignment — GAP-004

### 6.1 p1 FR Implementation Status

| # | FR ID | PRD Checkbox | Code Exists | Fully Implemented |
|---|-------|-------------|------------|-------------------|
| 1 | `fr-core-installer` | [x] | ✅ | ⚠️ Missing `cypilot` alias (Ph8 F-01) |
| 2 | `fr-core-init` | [x] | ✅ | ✅ |
| 3 | `fr-core-config` | [x] | ✅ | ✅ |
| 4 | `fr-core-skill-engine` | [x] | ✅ | ✅ |
| 5 | `fr-core-workflows` | [x] | ✅ | ✅ |
| 6 | `fr-core-execution-plans` | [ ] | ✅ | ✅ (workflow-level, by design) |
| 7 | `fr-core-agents` | [x] | ✅ | ✅ |
| 8 | `fr-core-kits` | [x] | ✅ | ⚠️ `kit move-config` missing (Ph9 F-03) |
| 9 | `fr-core-kit-manifest` | [ ] | ✅ | ✅ (`manifest.py` present) |
| 10 | `fr-core-resource-diff` | [x] | ✅ | ✅ |
| 11 | `fr-core-layout-migration` | [ ] | ✅ | ✅ |
| 12 | `fr-core-traceability` | [x] | ✅ | ✅ |
| 13 | `fr-core-cdsl` | [x] | ✅ | ✅ |
| 14 | `fr-core-toc` | [x] | ✅ | ✅ |

**p1 result**: 14/14 p1 FRs have code. 12 fully implemented, 2 with minor gaps (`cypilot` alias, `kit move-config`).

### 6.2 p2/p3 FR Implementation Status

| # | FR ID | Priority | Code Exists | Notes |
|---|-------|----------|------------|-------|
| 15 | `fr-core-version` | p2 | ✅ | Fully implemented — needed for update flow |
| 16 | `fr-core-cli-config` | p2 | Partial | 3 commands missing |
| 17 | `fr-core-template-qa` | p2 | ✅ | Via self-check alias |
| 18 | `fr-core-doctor` | p2 | ❌ | Not implemented |
| 19 | `fr-core-vscode-plugin` | p2 | ❌ | Separate deliverable |
| 20 | `fr-core-hooks` | p3 | ❌ | Not implemented |
| 21 | `fr-core-completions` | p3 | ❌ | Not implemented |

### 6.3 Priority Inversions

**None found.** All p1 FRs are implemented before p2/p3 FRs. The p2 items that ARE implemented (`fr-core-version`, `fr-core-template-qa`) were necessary for core workflows (update flow and kit validation respectively). No p2/p3 item was implemented while a p1 item of the same feature area remained unimplemented.

---

## 7. Extracted Feature Boundary — GAP-005

### 7.1 SDLC Code in Core Check

| Check | Result | Source |
|-------|--------|--------|
| No SDLC-specific validation logic in core | ✅ PASS | Ph9 §2 — all 6 components COMPLIANT |
| No SDLC templates/checklists/rules in core | ✅ PASS | Kit content lives in `config/kits/sdlc/` |
| No imports from SDLC kit internals | ✅ PASS | Ph9 §3.1 — grep found 0 `process_kit` occurrences |
| Blueprint cleanup code appropriate | ✅ PASS | Ph9 §3.1 — `update.py` cleanup + `migrate.py` migration context |
| Defensive exclusions correct | ✅ PASS | Ph9 §3.1 — `diff_engine.py` `_KIT_EXCLUDE_*` |
| Extracted features properly marked in DECOMPOSITION | ✅ PASS | Ph2 §7 — F4, F6, F9 all have ADR reference + external repo |

### 7.2 Boundary Assessment

**No violations found.** The SDLC kit extraction (per `cpt-cypilot-adr-extract-sdlc-kit`) is clean. Core codebase contains zero SDLC-specific content. The only SDLC references in core code are:
- Migration code that handles v2→v3 upgrade for projects with SDLC kit installed (appropriate)
- Defensive file exclusions in diff engine for legacy blueprint artifacts (appropriate)

---

## 8. Cross-Cutting Patterns

### 8.1 ADR-0001 Propagation Failure (HIGH)

The most significant cross-cutting finding is the incomplete propagation of ADR-0001 (Remove Blueprint System) across spec artifacts. While code was correctly updated:
- CLISPEC retains blueprint-era command definitions and descriptions (DR-01, DR-02)
- DECOMPOSITION Feature 2.2 retains stale `blueprint-system` ID (DR-03)
- 3 FEATURE specs retain stale `blueprint-system` dependency (DR-04)
- `toc.py` docstrings retain blueprint references (DR-09)

**Recommendation**: Establish an ADR propagation checklist that lists all spec files referencing affected IDs, to be completed before marking an ADR as fully implemented.

### 8.2 CLISPEC as Spec of Record is Incomplete (MEDIUM)

The CLISPEC does not fully document the CLI interface:
- Missing: `agents` command (CG-01), proxy-level flags (CG-06)
- Stale: `kit migrate` and `generate-resources` definitions (DR-02)
- Missing: `kit move-config`, `doctor`, `config show`, `config system add/remove`, `hook install/uninstall`

The CLISPEC is supposed to be the single source of truth for CLI behavior, but diverges from both DESIGN §3.3 (which lists planned commands not in CLISPEC) and actual code (which has commands not in CLISPEC).

**Recommendation**: Audit CLISPEC against both DESIGN §3.3 and actual CLI dispatch table. Add missing commands, remove deleted commands, add proxy-level flags.

### 8.3 PRD Status Checkboxes Lag Behind Implementation (LOW)

Several FRs are marked `[ ]` (open) in PRD despite being fully implemented:
- `fr-core-execution-plans` (p1) — plan workflow exists and is actively used
- `fr-core-kit-manifest` (p1) — `manifest.py` implemented
- `fr-core-layout-migration` (p1) — `_detect_and_migrate_layout` is active
- `fr-core-version` (p2) — update command fully functional

These do not affect functionality but create a false impression of project completion status.

**Recommendation**: Update PRD checkboxes to reflect actual implementation status, or add a status column to DECOMPOSITION that tracks actual vs planned state.

---

## 9. Gap Summary Table

| # | FR / Code Element | Spec Status | Code Status | Gap Type | Impact | Recommendation |
|---|------------------|-------------|-------------|----------|--------|----------------|
| SG-01 | `fr-core-doctor` (p2) | Specified | Not implemented | Spec gap | Degraded | Implement or mark as planned-p2 in DESIGN §3.3 |
| SG-02 | `fr-core-hooks` (p3) | Specified | Not implemented | Spec gap | Cosmetic | Mark as planned-p3 in DESIGN §3.3 |
| SG-03 | `fr-core-completions` (p3) | Specified | Not implemented | Spec gap | Cosmetic | Mark as planned-p3 |
| SG-04 | `fr-core-vscode-plugin` (p2) | Specified | Separate deliverable | Spec gap | Cosmetic | Expected — no action |
| SG-05 | `fr-core-cli-config` (p2) | Specified | Partial (3 cmds missing) | Partial gap | Degraded | Implement or mark as planned-p2 |
| SG-06 | `fr-core-installer` alias (p1) | Specified | Missing `cypilot` alias | Minor gap | Degraded | Add `cypilot` entry point to pyproject.toml |
| SG-07 | `fr-core-toc` DECOMP allocation | Missing in DECOMP | Fully implemented | Traceability gap | Cosmetic | Add to F8 "Requirements Covered" |
| CG-01 | `agents` command | In DESIGN, not CLISPEC | Implemented | Code gap | Degraded | Add CLISPEC `COMMAND agents` block |
| CG-06 | Proxy flags | Not in CLISPEC | Implemented | Code gap | Degraded | Add proxy flags to CLISPEC |
| CG-04/05 | Fork URL + local copy | Not in specs | Implemented | Code gap | Cosmetic | Document in CDSL algo or CLISPEC |
| DR-01/02 | CLISPEC blueprint references | Stale | Code correct | Drift | Degraded | Update CLISPEC per ADR-0001 |
| DR-03/04 | DECOMP + FEATURE blueprint IDs | Stale | Code correct | Drift | Degraded | Rename to `kit-management` everywhere |
| DR-05 | DECOMP F12 `workflow-engine` ref | Error | N/A | Drift | Cosmetic | Fix to `component-agent-generator` |
| DR-08 | `self-check` routing | Mismatch | Alias works | Drift | Degraded | Update CLISPEC or add standalone entry point |
| — | `kit move-config` (p1 scope) | In DESIGN + DECOMP | Not implemented | Spec gap | Degraded | Implement (p1 FR scope) |

---

## 10. Recommendations (Prioritized)

### Immediate (blocking or p1 gaps)

1. **Add `cypilot` console entry point** — single-line fix in `pyproject.toml` (SG-06, Ph8 F-01)
2. **Implement `kit move-config`** — p1 FR scope, specified in DESIGN and DECOMPOSITION but missing code

### High Priority (drift cleanup)

3. **Propagate ADR-0001 to CLISPEC** — remove `generate-resources`, update `kit migrate` to deprecated, fix `update` description (DR-01, DR-02)
4. **Rename `blueprint-system` → `kit-management`** in DECOMPOSITION, dependency graph, and 3 FEATURE specs (DR-03, DR-04)
5. **Add `agents` command to CLISPEC** (CG-01)
6. **Add proxy-level flags to CLISPEC** (CG-06)

### Medium Priority (traceability hygiene)

7. **Add `fr-core-toc` to F8 "Requirements Covered"** in DECOMPOSITION (SG-07)
8. **Fix DECOMP F12 component reference** — `workflow-engine` → `component-agent-generator` (DR-05)
9. **Update `self-check` routing** — either add standalone entry point or update CLISPEC to document alias behavior (DR-08)
10. **Update PRD checkboxes** for implemented FRs still marked `[ ]`

### Low Priority (backlog acknowledgment)

11. **Mark unimplemented p2/p3 commands** explicitly as `planned` in DESIGN §3.3 to distinguish from implemented commands
12. **Document fork URL and local copy** capabilities in cache algorithm spec
13. **Clean up stale blueprint docstrings** in `toc.py`

---

## 11. Acceptance Criteria Self-Verification

- [x] All 4 prior phase findings files read and incorporated — Ph1 (21 FRs, DESIGN coverage), Ph2 (DECOMP chain, Feature 2.2 stale ID), Ph8 (12 proxy findings), Ph9 (4 skill engine findings)
- [x] Traceability matrix produced: PRD FR → DESIGN → DECOMP → Code — §2 covers all 21 FRs across p1/p2/p3, with DESIGN, DECOMP feature, code location, and chain status
- [x] Spec gaps identified with impact categorization — §3: 6 spec gaps (2 degraded, 2 cosmetic, 1 expected, 1 minor) + 5 DESIGN API gaps
- [x] Code gaps identified with impact categorization — §4: 7 code gaps (2 degraded, 5 cosmetic)
- [x] Implementation drift items listed — §5: 9 drift items (4 HIGH, 2 MEDIUM, 3 LOW) with cross-cutting ADR-0001 propagation pattern
- [x] Priority alignment checked — inversions flagged — §6: 0 inversions found; all p1 FRs implemented
- [x] Extracted feature boundary checked — §7: 0 violations; SDLC extraction clean
- [x] Gap summary table produced with recommendations — §9 (15-row summary) + §10 (13 prioritized recommendations)
- [x] Findings saved to `out/phase-10-findings.md` with structured summary
