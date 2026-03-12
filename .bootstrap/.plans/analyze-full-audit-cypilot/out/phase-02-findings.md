# Phase 2 Findings — DESIGN ↔ DECOMPOSITION ↔ FEATURE Traceability Chain

**Plan**: analyze-full-audit-cypilot
**Phase**: 2/11
**Date**: 2026-03-12
**Input files**: `architecture/DESIGN.md` (§3.2), `architecture/DECOMPOSITION.md`, 9 FEATURE spec files, `out/phase-01-findings.md`

---

## 1. Summary

| Metric | Result |
|--------|--------|
| DESIGN component IDs (§3.2) | 7 |
| Components covered by DECOMPOSITION | 7/7 (100%) |
| PRD FR IDs (from Phase 1) | 21 |
| FRs allocated in DECOMPOSITION | 20/21 (95%) |
| PRD NFR IDs (from Phase 1) | 9 |
| NFRs allocated in DECOMPOSITION | 4/9 (44%) |
| Non-extracted FEATURE spec files | 9 |
| FEATURE files existing | 9/9 (100%) |
| FEATURE specs checked for backlinks | 9/9 |
| Dependency graph acyclic | Yes |
| Dependency references valid | Yes |
| Extracted features properly marked | 3/3 |
| Naming anomalies | 1 (Feature 2.2 stale ID) |
| Dependency mismatches (DECOMPOSITION vs FEATURE) | 3 |
| Overall status | **PASS with observations** |

### Phase Input Table Errata

The phase file's Prior Context states "The PRD defines 19 FRs". Phase 1 established the actual count is **21 FRs**. This analysis uses the Phase 1 verified count of 21 FRs and 9 NFRs.

---

## 2. DESIGN → DECOMPOSITION Component Coverage

### Component Coverage Table

| # | DESIGN Component ID | DESIGN §3.2 Line | DECOMPOSITION Feature(s) | Status |
|---|---------------------|-------------------|--------------------------|--------|
| 1 | `cpt-cypilot-component-cli-proxy` | 495 | F1 (Core Infrastructure) — explicit | **COVERED** |
| 2 | `cpt-cypilot-component-skill-engine` | 518 | F1 (Core Infrastructure) — explicit; F10 (V2→V3) — explicit; F7 (Version Config) — implicit reuse | **COVERED** |
| 3 | `cpt-cypilot-component-validator` | 547 | F3 (Traceability & Validation) — explicit; F11 (Spec Coverage) — explicit; F8 (Dev Experience) — implicit reuse | **COVERED** |
| 4 | `cpt-cypilot-component-traceability-engine` | 580 | F3 (Traceability & Validation) — explicit; F11 (Spec Coverage) — explicit; F8 (Dev Experience) — implicit reuse | **COVERED** |
| 5 | `cpt-cypilot-component-config-manager` | 608 | F1 (Core Infrastructure) — explicit; F10 (V2→V3) — explicit; F7 (Version Config) — implicit reuse | **COVERED** |
| 6 | `cpt-cypilot-component-kit-manager` | 637 | F2 (Kit Management) — explicit; F10 (V2→V3) — explicit; F7 (Version Config) — implicit reuse | **COVERED** |
| 7 | `cpt-cypilot-component-agent-generator` | 668 | F5 (Agent Integration) — explicit | **COVERED** |

**Result**: All 7 DESIGN component IDs are covered by at least one DECOMPOSITION entry. **0 uncovered**.

### Observation: Resource Diff Engine

The DESIGN §3.2 mermaid diagram includes an 8th component `DE["Resource Diff Engine"]` in the "Core Skill Engine" subgraph. However, this component has **no formal component section** with an `**ID**:` definition — unlike the other 7 components which each have dedicated subsections. The functionality is covered by Feature 2.2 (Kit Management) via `cpt-cypilot-fr-core-resource-diff`, but the component itself lacks a formal ID in DESIGN.

### Observation: Feature 2.12 References Non-Existent Component

Feature 2.12 (Execution Plans) states "Components reused from Feature 5 (`workflow-engine` via generate/analyze patterns)". There is **no** DESIGN component called `workflow-engine`. The closest match is `cpt-cypilot-component-agent-generator` (Feature 5's actual component). This is an incorrect component reference in the DECOMPOSITION.

---

## 3. FR/NFR Allocation in DECOMPOSITION

### FR Allocation Table

| # | FR ID | DECOMPOSITION Feature(s) | Status |
|---|-------|--------------------------|--------|
| 1 | `cpt-cypilot-fr-core-installer` | F1 (Core Infrastructure) | **ALLOCATED** |
| 2 | `cpt-cypilot-fr-core-init` | F1 (Core Infrastructure), F10 (V2→V3 Migration) | **ALLOCATED** |
| 3 | `cpt-cypilot-fr-core-config` | F1 (Core Infrastructure), F10 (V2→V3 Migration) | **ALLOCATED** |
| 4 | `cpt-cypilot-fr-core-skill-engine` | F1 (Core Infrastructure) | **ALLOCATED** |
| 5 | `cpt-cypilot-fr-core-workflows` | F5 (Agent Integration), F12 (Execution Plans) | **ALLOCATED** |
| 6 | `cpt-cypilot-fr-core-execution-plans` | F12 (Execution Plans) | **ALLOCATED** |
| 7 | `cpt-cypilot-fr-core-agents` | F5 (Agent Integration) | **ALLOCATED** |
| 8 | `cpt-cypilot-fr-core-kits` | F2 (Kit Management) | **ALLOCATED** |
| 9 | `cpt-cypilot-fr-core-kit-manifest` | F2 (Kit Management) | **ALLOCATED** |
| 10 | `cpt-cypilot-fr-core-resource-diff` | F2 (Kit Management) | **ALLOCATED** |
| 11 | `cpt-cypilot-fr-core-layout-migration` | F7 (Version & Config) | **ALLOCATED** |
| 12 | `cpt-cypilot-fr-core-traceability` | F3 (Traceability & Validation), F11 (Spec Coverage) | **ALLOCATED** |
| 13 | `cpt-cypilot-fr-core-cdsl` | F3 (Traceability & Validation), F11 (Spec Coverage) | **ALLOCATED** |
| 14 | `cpt-cypilot-fr-core-version` | F7 (Version & Config) | **ALLOCATED** |
| 15 | `cpt-cypilot-fr-core-cli-config` | F7 (Version & Config) | **ALLOCATED** |
| 16 | `cpt-cypilot-fr-core-template-qa` | F8 (Developer Experience) | **ALLOCATED** |
| 17 | `cpt-cypilot-fr-core-toc` | — | **UNALLOCATED** |
| 18 | `cpt-cypilot-fr-core-doctor` | F8 (Developer Experience) | **ALLOCATED** |
| 19 | `cpt-cypilot-fr-core-hooks` | F8 (Developer Experience) | **ALLOCATED** |
| 20 | `cpt-cypilot-fr-core-completions` | F8 (Developer Experience) | **ALLOCATED** |
| 21 | `cpt-cypilot-fr-core-vscode-plugin` | F8 (Developer Experience) | **ALLOCATED** |

**Result**: 20/21 FRs allocated. **1 unallocated**: `cpt-cypilot-fr-core-toc`.

**Note on `fr-core-toc`**: TOC generation IS implemented in Feature 8 (Developer Experience) — the `developer-experience.md` FEATURE spec includes a "TOC Generation" actor flow (`cpt-cypilot-flow-developer-experience-toc`) and a "TOC Command" implementation module. However, `cpt-cypilot-fr-core-toc` is **not listed** in Feature 8's "Requirements Covered" section in `DECOMPOSITION.md`.

### NFR Allocation Table

| # | NFR ID | DECOMPOSITION Feature(s) | Status | Notes |
|---|--------|--------------------------|--------|-------|
| 1 | `cpt-cypilot-nfr-dry` | — | **UNALLOCATED** | Covered via Design Principle `cpt-cypilot-principle-dry` in F2 |
| 2 | `cpt-cypilot-nfr-simplicity` | — | **UNALLOCATED** | Covered via Design Principle `cpt-cypilot-principle-occams-razor` in F1 |
| 3 | `cpt-cypilot-nfr-ci-automation-first` | — | **UNALLOCATED** | Covered via Design Principle `cpt-cypilot-principle-ci-automation-first` in F3 |
| 4 | `cpt-cypilot-nfr-zero-harm` | — | **UNALLOCATED** | Covered via Design Principle `cpt-cypilot-principle-zero-harm` in F1, F8 |
| 5 | `cpt-cypilot-nfr-upgradability` | — | **UNALLOCATED** | Covered via Design Principle `cpt-cypilot-principle-no-manual-maintenance` in F2, F5, F7, F10 |
| 6 | `cpt-cypilot-nfr-validation-performance` | F3 (Traceability & Validation) | **ALLOCATED** | |
| 7 | `cpt-cypilot-nfr-security-integrity` | F3 (Traceability & Validation) | **ALLOCATED** | |
| 8 | `cpt-cypilot-nfr-reliability-recoverability` | F1 (Core Infrastructure), F7 (Version & Config) | **ALLOCATED** | |
| 9 | `cpt-cypilot-nfr-adoption-usability` | F1 (Core Infrastructure) | **ALLOCATED** | |

**Result**: 4/9 NFRs allocated by NFR ID. **5 unallocated** by their NFR IDs.

**Mitigating factor**: All 5 unallocated NFRs are addressed through corresponding Design Principles listed in "Design Principles Covered" sections. The DECOMPOSITION uses Design Principle IDs (e.g., `cpt-cypilot-principle-dry`) rather than NFR IDs (e.g., `cpt-cypilot-nfr-dry`) for cross-architectural NFRs. This is a traceability notation gap rather than a substantive coverage gap — the NFR intent is addressed but the direct PRD-to-DECOMPOSITION link by NFR ID is missing.

---

## 4. DECOMPOSITION → FEATURE File Existence

| # | Feature | DECOMPOSITION Link | File Path | Status |
|---|---------|-------------------|-----------|--------|
| 2.1 | Core Infrastructure | `features/core-infra.md` | `architecture/features/core-infra.md` | **EXISTS** (529 lines) |
| 2.2 | Kit Management | `features/kit-management.md` | `architecture/features/kit-management.md` | **EXISTS** (539 lines) |
| 2.3 | Traceability & Validation | `features/traceability-validation.md` | `architecture/features/traceability-validation.md` | **EXISTS** (551 lines) |
| 2.4 | SDLC Kit (EXTRACTED) | N/A | N/A | **N/A** — extracted |
| 2.5 | Agent Integration | `features/agent-integration.md` | `architecture/features/agent-integration.md` | **EXISTS** (197 lines) |
| 2.6 | PR Workflows (EXTRACTED) | N/A | N/A | **N/A** — extracted |
| 2.7 | Version & Config | `features/version-config.md` | `architecture/features/version-config.md` | **EXISTS** (216 lines) |
| 2.8 | Developer Experience | `features/developer-experience.md` | `architecture/features/developer-experience.md` | **EXISTS** (286 lines) |
| 2.9 | Advanced SDLC (EXTRACTED) | N/A | N/A | **N/A** — extracted |
| 2.10 | V2 → V3 Migration | `features/v2-v3-migration.md` | `architecture/features/v2-v3-migration.md` | **EXISTS** (771 lines) |
| 2.11 | Spec Coverage | `features/spec-coverage.md` | `architecture/features/spec-coverage.md` | **EXISTS** (246 lines) |
| 2.12 | Execution Plans | `features/execution-plans.md` | `architecture/features/execution-plans.md` | **EXISTS** (330 lines) |

**Result**: All 9 non-extracted FEATURE spec files exist. **0 broken links**.

---

## 5. FEATURE → DESIGN Backlinks

All 9 non-extracted FEATURE specs were checked for backlinks to DESIGN components and PRD requirements in their References section (§1.4).

| # | Feature | PRD FR Backlinks | DESIGN Component Backlinks | Dependencies Match DECOMPOSITION? | Status |
|---|---------|-----------------|---------------------------|-----------------------------------|--------|
| F1 | Core Infrastructure | Generic link only (FRs cited in body §2, not §1.4) | Generic link only (no component IDs in §1.4) | ✅ None / None | **WEAK** |
| F2 | Kit Management | `fr-core-kits`, `fr-core-kit-manifest`, `fr-core-resource-diff` | `component-kit-manager`, `component-config-manager`, `component-validator` | N/A (no explicit Dependencies field) | **PASS** |
| F3 | Traceability & Validation | `fr-core-traceability`, `fr-core-cdsl` | `component-validator`, `component-traceability-engine` | ✅ `core-infra` | **PASS** |
| F5 | Agent Integration | `fr-core-agents`, `fr-core-workflows` | `component-agent-generator` | ❌ FEATURE says `blueprint-system`; DECOMP says `core-infra` | **MISMATCH** |
| F7 | Version & Config | `fr-core-version`, `fr-core-layout-migration`, `fr-core-cli-config` | `component-config-manager`, `seq-update` | ⚠️ FEATURE adds `blueprint-system` not in DECOMP | **MISMATCH** |
| F8 | Developer Experience | `fr-core-template-qa`, `fr-core-doctor`, `fr-core-hooks`, `fr-core-completions` | `component-validator` | ✅ `traceability-validation` | **PASS** |
| F10 | V2 → V3 Migration | Generic link only (FRs cited in DoD sections) | Generic link only (components cited in DoD sections) | ⚠️ FEATURE adds `blueprint-system` not in DECOMP | **WEAK + MISMATCH** |
| F11 | Spec Coverage | `fr-core-traceability`, `fr-core-cdsl` | `component-traceability-engine`, `component-validator` | ✅ `traceability-validation` | **PASS** |
| F12 | Execution Plans | `fr-core-workflows`, `fr-core-execution-plans` | `component-agent-generator` | ✅ `agent-integration` | **PASS** |

**Result**: 9/9 checked. 5 PASS, 2 WEAK (generic links without specific IDs), 3 dependency MISMATCH.

### Backlink Detail: Weak References

1. **core-infra.md** — References section uses generic `[PRD.md](../PRD.md)` and `[DESIGN.md](../DESIGN.md)` links without listing specific FR or component IDs. FR IDs (`fr-core-installer`, `fr-core-init`, `fr-core-config`) appear in the Purpose section body text instead.

2. **v2-v3-migration.md** — References section uses generic PRD and DESIGN links. Specific component IDs (`component-config-manager`, `component-kit-manager`, `component-skill-engine`) and FR IDs (`fr-core-init`, `fr-core-config`) appear in the Definitions of Done "Covers" sections but not in the formal References section.

### Backlink Detail: Dependency Mismatches

| Feature | DECOMPOSITION "Depends On" | FEATURE spec "Dependencies" | Discrepancy |
|---------|---------------------------|---------------------------|-------------|
| F5 Agent Integration | `core-infra` | `blueprint-system` | Completely different dependency |
| F7 Version & Config | `core-infra` | `core-infra`, `blueprint-system` | FEATURE adds `blueprint-system` |
| F10 V2 → V3 Migration | `core-infra`, `traceability-validation` | `core-infra`, `blueprint-system`, `traceability-validation` | FEATURE adds `blueprint-system` |

**Assessment**: The pattern is consistent — 3 FEATURE specs declare a dependency on `cpt-cypilot-feature-blueprint-system` (Kit Management) that the DECOMPOSITION does not reflect. This suggests either:
- (a) The DECOMPOSITION "Depends On" fields are incomplete and should include `blueprint-system`, or
- (b) The FEATURE specs have stale dependency declarations

Given that F5 (Agent Integration) references `cpt-cypilot-feature-blueprint-system` in its Dependencies but the DECOMPOSITION says it depends on `core-infra`, and the Agent Integration overview says it "bridges Cypilot's unified skill system to diverse AI coding assistants" (no kit management dependency apparent), **option (b) appears more likely** — these are likely stale references from when blueprint processing was part of the architecture.

### Backlink Detail: Missing FR in Developer Experience

Feature 8 (Developer Experience) DECOMPOSITION lists `cpt-cypilot-fr-core-vscode-plugin` in "Requirements Covered", but the FEATURE spec's §1.4 References does not include `fr-core-vscode-plugin` in its PRD backlinks. The VS Code plugin is mentioned in the DECOMPOSITION scope description and the feature overview, but the formal PRD reference in the FEATURE spec omits it.

---

## 6. Dependency Graph Consistency

### Acyclicity Check

**Adjacency list** (from DECOMPOSITION "Depends On"):
```
core-infra → (none)
blueprint-system → core-infra
traceability-validation → core-infra
agent-integration → core-infra
version-config → core-infra
developer-experience → traceability-validation
v2-v3-migration → core-infra, traceability-validation
spec-coverage → traceability-validation
execution-plans → agent-integration
```

**Topological order**: `core-infra` → {`blueprint-system`, `traceability-validation`, `agent-integration`, `version-config`} → {`developer-experience`, `spec-coverage`, `v2-v3-migration`, `execution-plans`}

**Result**: Graph is **acyclic**. Valid DAG.

### Valid Reference Check

All feature IDs referenced in "Depends On" fields exist in the DECOMPOSITION:
- `cpt-cypilot-feature-core-infra` — ✅ exists (F1)
- `cpt-cypilot-feature-traceability-validation` — ✅ exists (F3)
- `cpt-cypilot-feature-agent-integration` — ✅ exists (F5)

**Result**: All dependency references are **valid**. **0 dangling references**.

### Layer Boundary Consistency

The dependency graph respects DESIGN layer boundaries:
- **Foundation layer** (F1) has no dependencies
- **Core capability layer** (F2, F3, F5, F7) depends only on F1
- **Higher-order features** (F8, F10, F11, F12) depend on core capabilities

**Result**: **Consistent** with DESIGN architectural layers.

### Diagram vs Entry Mismatch

The DECOMPOSITION §3 dependency diagram matches the "Depends On" fields in the entries. No discrepancies between the textual graph and the per-entry declarations.

---

## 7. Extracted Features Verification

| # | Feature | Extraction Marker | ADR Reference | External Repo | Status |
|---|---------|-------------------|---------------|---------------|--------|
| 2.4 | SDLC Kit & Artifact Pipeline | "EXTRACTED per `cpt-cypilot-adr-extract-sdlc-kit`" | ✅ | `cyberfabric/cyber-pilot-kit-sdlc` | **PROPERLY MARKED** |
| 2.6 | PR Workflows | "EXTRACTED per `cpt-cypilot-adr-extract-sdlc-kit`" | ✅ | `cyberfabric/cyber-pilot-kit-sdlc` | **PROPERLY MARKED** |
| 2.9 | Advanced SDLC Workflows | "EXTRACTED per `cpt-cypilot-adr-extract-sdlc-kit`" | ✅ | `cyberfabric/cyber-pilot-kit-sdlc` | **PROPERLY MARKED** |

All three extracted features:
- Have blockquote markers starting with "> **EXTRACTED per..."
- Reference the ADR `cpt-cypilot-adr-extract-sdlc-kit`
- Name the external repository `cyberfabric/cyber-pilot-kit-sdlc`
- Are listed in the dependency graph as "(EXTRACTED to cyberfabric/cyber-pilot-kit-sdlc)"
- The Overview §1 mentions: "SDLC-specific features (F4, F6, F9) have been **extracted**"

**Result**: All 3 extracted features are **explicitly and correctly marked**. No accidentally missing features.

---

## 8. Naming Anomaly — Feature 2.2

### Issue

Feature 2.2 has a **stale ID from the removed blueprint system**:

| Attribute | Value |
|-----------|-------|
| DECOMPOSITION heading | "2.2 Kit Management" |
| DECOMPOSITION ID | `cpt-cypilot-feature-blueprint-system` |
| Feature spec filename | `kit-management.md` |
| Feature spec `featstatus` ID | `cpt-cypilot-featstatus-kit-management` |
| Feature spec `feature` ID | `cpt-cypilot-featstatus-kit-management` (duplicated — should be `feature-*`) |
| Relevant ADR | `cpt-cypilot-adr-remove-blueprint-system` (ADR-0001, accepted) |
| Feature purpose | "Manage kit lifecycle — installation, file-level diff updates..." |

### Analysis

1. **Stale DECOMPOSITION ID**: The ID `cpt-cypilot-feature-blueprint-system` reflects the pre-ADR-0001 architecture where the feature was about blueprint processing. ADR-0001 explicitly removed the blueprint system, renaming the feature to "Kit Management". The ID was never updated.

2. **Broken traceability in FEATURE spec**: The FEATURE spec `kit-management.md` uses `cpt-cypilot-featstatus-kit-management` in **both** ID positions (lines 46 and 50). Every other FEATURE spec follows the pattern: `featstatus-*` at top, `feature-*` in Feature Context section. Kit Management is the only FEATURE spec that does NOT define a `cpt-cypilot-feature-*` ID — neither `feature-blueprint-system` nor `feature-kit-management`.

3. **Dependency chain impact**: Three other FEATURE specs reference `cpt-cypilot-feature-blueprint-system` in their Dependencies sections (F5, F7, F10). This propagates the stale name across the traceability chain. Reading these dependencies suggests a dependency on a removed system rather than the current kit management feature.

### Severity: **HIGH**

The stale ID breaks bidirectional traceability between DECOMPOSITION and FEATURE spec for Feature 2.2 and creates confusing dependency references across the architecture.

### Recommendation

Rename `cpt-cypilot-feature-blueprint-system` → `cpt-cypilot-feature-kit-management` in:
- `DECOMPOSITION.md` entry 2.2 ID
- `DECOMPOSITION.md` dependency graph
- `kit-management.md` Feature Context section (replace duplicate `featstatus` with `feature-kit-management`)
- All FEATURE spec dependency references (F5, F7, F10)

---

## 9. Key Findings Summary

### Critical Issues

None.

### High-Priority Issues

1. **Feature 2.2 stale ID** (`cpt-cypilot-feature-blueprint-system`) — Breaks traceability between DECOMPOSITION and FEATURE spec. Propagates through 3 dependency declarations. Feature spec missing `feature-*` ID entirely. See §8.

2. **5 NFRs unallocated by NFR ID** — `nfr-dry`, `nfr-simplicity`, `nfr-ci-automation-first`, `nfr-zero-harm`, `nfr-upgradability` are not listed in any DECOMPOSITION "Requirements Covered" section by their NFR IDs. They are addressed through Design Principle references, but the direct PRD NFR → DECOMPOSITION traceability link is missing. See §3.

3. **3 dependency mismatches** between DECOMPOSITION and FEATURE specs — Features F5, F7, and F10 declare `cpt-cypilot-feature-blueprint-system` as a dependency in their FEATURE specs, but DECOMPOSITION does not list this dependency. Likely stale from the old blueprint architecture. See §5.

### Medium-Priority Issues

4. **1 FR unallocated** — `cpt-cypilot-fr-core-toc` is not listed in any DECOMPOSITION "Requirements Covered" section, despite TOC generation being fully implemented in Feature 8 (Developer Experience). See §3.

5. **Feature 2.12 references non-existent component** — DECOMPOSITION entry for Execution Plans references "workflow-engine" which does not exist as a DESIGN component. Should reference `component-agent-generator`. See §2.

6. **2 FEATURE specs have weak backlinks** — `core-infra.md` and `v2-v3-migration.md` use generic PRD/DESIGN links in their References sections without listing specific FR or component IDs. The IDs exist in body text/DoD sections but not in the formal References. See §5.

### Low-Priority Observations

7. **Resource Diff Engine lacks formal component ID** — DESIGN §3.2 mermaid diagram shows Resource Diff Engine as a component but no formal `**ID**:` section exists for it, unlike the other 7 components. See §2.

8. **Developer Experience missing `fr-core-vscode-plugin` backlink** — DECOMPOSITION allocates `fr-core-vscode-plugin` to Feature 8, but the FEATURE spec's References section does not include it in PRD backlinks. See §5.

---

## 10. Acceptance Criteria Self-Verification

- [x] All DESIGN component IDs checked against DECOMPOSITION — 7/7 COVERED (§2)
- [x] All PRD FR/NFR IDs checked against DECOMPOSITION "Requirements Covered" — 20/21 FR ALLOCATED, 4/9 NFR ALLOCATED; 1 FR + 5 NFR UNALLOCATED with mitigation notes (§3)
- [x] All 9 non-extracted FEATURE spec file links verified — 9/9 EXISTS (§4)
- [x] At least 5 FEATURE spec files checked for DESIGN/PRD backlinks — 9/9 checked (§5)
- [x] Dependency graph verified as acyclic with valid references (§6)
- [x] Extracted features (2.4, 2.6, 2.9) verified as explicitly excluded with ADR reference and external repo (§7)
- [x] Feature 2.2 naming anomaly assessed — stale ID from removed blueprint system, HIGH severity (§8)
- [x] Summary table produced with coverage status per DESIGN component (§2) and FR/NFR (§3)
- [x] Findings saved to `out/phase-02-findings.md` with structured summary
