# Phase 1 Findings — PRD ↔ DESIGN Cross-Reference Validation

**Plan**: analyze-full-audit-cypilot
**Phase**: 1/11
**Date**: 2026-03-12
**Input files**: `architecture/PRD.md`, `architecture/DESIGN.md`

---

## 1. Summary

| Metric | Result |
|--------|--------|
| PRD FR IDs (actual) | 21 |
| PRD FR IDs addressed in DESIGN | 21/21 (100%) |
| PRD NFR IDs | 9 |
| PRD NFR IDs addressed in DESIGN | 9/9 (100%) |
| PRD Use Cases | 13 |
| Use Cases with explicit DESIGN sequences | 5/13 (38%) |
| Use Cases implicitly covered | 8/13 (62%) |
| Actor IDs in PRD | 4 |
| Actor mismatches | 0 (2 minor naming inconsistencies) |
| DESIGN scope beyond PRD | 2 minor items |
| Interface ID inconsistencies | 2 |
| Overall status | **PASS with observations** |

### Phase Input Table Errata

The phase file's input table claims 19 FRs. The actual PRD contains **21 FRs**. Discrepancies:

- **Phantom FR**: `cpt-cypilot-fr-core-auto-config` is listed in the phase input table but does **not exist** in `architecture/PRD.md` (grep confirms zero matches). A file `requirements/auto-config.md` exists in the workspace but is not a PRD FR section.
- **Missing from phase input**: `cpt-cypilot-fr-core-hooks` (p3), `cpt-cypilot-fr-core-completions` (p3), `cpt-cypilot-fr-core-vscode-plugin` (p2) — all present in PRD §5.1 but omitted from the phase input table.

This analysis covers all **21 actual PRD FRs**.

---

## 2. Forward Traceability — PRD FR → DESIGN

### FR Cross-Reference Table

| # | FR ID | PRD Section | Priority | DESIGN Status | DESIGN Location | Evidence |
|---|-------|-------------|----------|---------------|-----------------|----------|
| 1 | `cpt-cypilot-fr-core-installer` | §5.1 Global CLI Installer | p1 | **PASS** | §1.2 line 48–50 | Design Response: "Thin proxy shell with local cache at `~/.cypilot/cache/`. On every invocation the proxy ensures a cached skill bundle exists, routes commands to the project-installed or cached skill, and performs non-blocking background version checks." |
| 2 | `cpt-cypilot-fr-core-init` | §5.1 Project Initialization | p1 | **PASS** | §1.2 line 54–56 | Design Response: "Interactive bootstrapper that copies the skill from cache into the project, creates the `config/` and `.gen/` directory structure, and generates agent entry points." Also covered by sequence diagram `cpt-cypilot-seq-init` (§3.6 line 800–857). |
| 3 | `cpt-cypilot-fr-core-config` | §5.1 Config Directory | p1 | **PASS** | §1.2 line 60–62 | Design Response describes `core.toml`, `artifacts.toml`, `config/kits/<slug>/`, `.gen/` structure, system identity in artifacts.toml. References ADR `cpt-cypilot-adr-remove-system-from-core-toml`. |
| 4 | `cpt-cypilot-fr-core-skill-engine` | §5.1 Deterministic Skill Engine | p1 | **PASS** | §1.2 line 66–68 | Design Response: "Python command router that dispatches to command handlers. All commands output JSON. Exit codes follow the convention: 0=PASS, 1=filesystem error, 2=FAIL." Component `cpt-cypilot-component-skill-engine` (§3.2 line 518–543). |
| 5 | `cpt-cypilot-fr-core-workflows` | §5.1 Generic Workflows | p1 | **PASS** | §1.2 line 72–74 | Design Response: "Two universal Markdown workflows (generate and analyze) with a common execution protocol. Workflows are loaded and interpreted by AI agents, not by the tool itself." |
| 6 | `cpt-cypilot-fr-core-execution-plans` | §5.1 Execution Plans | p1 | **PASS** | §1.2 line 78–80 | Design Response: "A third universal Markdown workflow (`plan.md`) that extends the generate/analyze system with phase-based task decomposition." Describes decomposition strategies, line budget, `.plans/` storage, TOML manifest. |
| 7 | `cpt-cypilot-fr-core-agents` | §5.1 Multi-Agent Integration | p1 | **PASS** | §1.2 line 84–86 | Design Response: "Agent Generator component produces entry points in each agent's native format: `.windsurf/workflows/`, `.cursor/rules/`, `.claude/commands/`, `.github/prompts/`." Component `cpt-cypilot-component-agent-generator` (§3.2 line 668–691). |
| 8 | `cpt-cypilot-fr-core-kits` | §5.1 Extensible Kit System | p1 | **PASS** | §1.2 line 90–92 | Design Response describes Kit Manager lifecycle: installation from GitHub, file-level diff updates, `core.toml` registration, version tracking. Component `cpt-cypilot-component-kit-manager` (§3.2 line 637–664). |
| 9 | `cpt-cypilot-fr-core-kit-manifest` | §5.1 Declarative Kit Manifest | p1 | **PASS** | §1.2 line 96–137 | Extensive Design Response: manifest.toml format, resource identifiers, user_modifiable flag, template variable resolution, update behavior, legacy install migration. Includes manifest TOML example. |
| 10 | `cpt-cypilot-fr-core-resource-diff` | §5.1 Resource Editing & Diff | p1 | **PASS** | §1.2 line 193–195 | Design Response: "Resource Diff Engine handles interactive conflict resolution for kit file updates." Describes 5 modes: accept, decline, accept-all, decline-all, modify. Component `cpt-cypilot-component-resource-diff-engine` (referenced in §3.2 mermaid diagram). |
| 11 | `cpt-cypilot-fr-core-layout-migration` | §5.1 Directory Layout Migration | p1 | **PASS** | §1.2 line 199–201 | Design Response: "Layout Migrator is a component in the Kit Manager" — describes detection, migration steps, rollback. Also §4 Migration Strategy (line 1050–1062). |
| 12 | `cpt-cypilot-fr-core-traceability` | §5.1 ID and Traceability System | p1 | **PASS** | §1.2 line 141–143 | Design Response: "Traceability Engine scans artifacts for `cpt-{system}-{kind}-{slug}` IDs, resolves cross-references, detects code tags (`@cpt-*`), and provides query commands." Component `cpt-cypilot-component-traceability-engine` (§3.2 line 580–604). |
| 13 | `cpt-cypilot-fr-core-cdsl` | §5.1 Cypilot DSL | p1 | **PASS** | §1.2 line 147–149 | Design Response: "CDSL is a plain English specification language embedded in Markdown. The tool parses CDSL instruction markers for implementation tracking." |
| 14 | `cpt-cypilot-fr-core-version` | §5.1 Version Detection & Updates | p2 | **PASS** | §1.2 line 157–159 | Design Response describes update command: skill copy, layout detection, config migration, bundled kit reference migration, agent regeneration. Sequence diagram `cpt-cypilot-seq-update` (§3.6 line 966–993). |
| 15 | `cpt-cypilot-fr-core-cli-config` | §5.1 CLI Configuration Interface | p2 | **PASS** | §1.2 line 163–165 | Design Response: "Core CLI commands manage `core.toml` and `artifacts.toml`. All config changes validated against schemas." |
| 16 | `cpt-cypilot-fr-core-template-qa` | §5.1 Template Quality Assurance | p2 | **PASS** | §1.2 line 210 (grouped) | Listed under "Utility Commands" block. Design Response: "`self-check` validates examples against templates." |
| 17 | `cpt-cypilot-fr-core-toc` | §5.1 Table of Contents Management | p1 | **PASS** | §1.2 line 209 (grouped) | Listed under "Utility Commands" block. Also in API Contracts table (§3.3 line 727–728): `toc` and `validate-toc` commands. |
| 18 | `cpt-cypilot-fr-core-doctor` | §5.1 Environment Diagnostics | p2 | **PASS** | §1.2 line 211 (grouped) | Listed under "Utility Commands" block. Design Response: "`doctor` checks environment health (Python version, git, gh, agents, config integrity)." Also in API Contracts (§3.3 line 719). |
| 19 | `cpt-cypilot-fr-core-hooks` | §5.1 Pre-Commit Hook Integration | p3 | **PASS** | §1.2 line 212 (grouped) | Listed under "Utility Commands" block. Design Response: "`hook install/uninstall` manages git pre-commit hooks for lightweight validation." API Contract (§3.3 line 724). |
| 20 | `cpt-cypilot-fr-core-completions` | §5.1 Shell Completions | p3 | **PASS** | §1.2 line 213 (grouped) | Listed under "Utility Commands" block. Design Response: "`completions install` generates shell completion scripts for bash/zsh/fish." |
| 21 | `cpt-cypilot-fr-core-vscode-plugin` | §5.1 VS Code Plugin | p2 | **PASS** | §1.2 line 169–171 | Design Response: "VS Code extension delegates all validation to the installed Cypilot skill (`cpt validate`). The plugin reads config from the project's install directory, provides ID syntax highlighting, go-to-definition, real-time validation, autocompletion, hover info, CodeLens, traceability tree view, and quick fixes." |

**Result**: All 21 PRD FR IDs have a Design Response in DESIGN §1.2. **0 missing**.

---

## 3. Forward Traceability — PRD NFR → DESIGN

| # | NFR ID | PRD Section | Priority | DESIGN Status | DESIGN Location | Evidence |
|---|--------|-------------|----------|---------------|-----------------|----------|
| 1 | `cpt-cypilot-nfr-dry` | §6.1 DRY Configuration | p1 | **PASS** | §1.2 NFR table line 231; §2.1 Principle `cpt-cypilot-principle-dry` line 326–328 | NFR Allocation: "Config is the single source of truth; kit constraints reference config, never duplicate it." Principle: "Every piece of knowledge MUST have exactly one authoritative source." |
| 2 | `cpt-cypilot-nfr-simplicity` | §6.1 Simplicity | p1 | **PASS** | §1.2 NFR table line 232; §2.1 Principle `cpt-cypilot-principle-occams-razor` line 332–334 | NFR Allocation: "Python stdlib-only. Each component has a single responsibility." Principle: "The simplest solution that satisfies the requirements is the correct one." |
| 3 | `cpt-cypilot-nfr-ci-automation-first` | §6.1 CI & Automation First | p1 | **PASS** | §1.2 NFR table line 233; §2.1 Principle `cpt-cypilot-principle-ci-automation-first` line 338–340 | NFR Allocation: "All validation and scanning commands are deterministic pure functions. Exit codes follow convention." |
| 4 | `cpt-cypilot-nfr-zero-harm` | §6.1 Zero Harm | p1 | **PASS** | §1.2 NFR table line 234; §2.1 Principle `cpt-cypilot-principle-zero-harm` line 342–346 | NFR Allocation: "Validator is advisory — never modifies files. Workflows are opt-in." Principle: "Adopting Cypilot MUST NOT impose costs on the development workflow." |
| 5 | `cpt-cypilot-nfr-upgradability` | §6.1 No Manual Maintenance | p1 | **PASS** | §1.2 NFR table line 235; §2.1 Principle `cpt-cypilot-principle-no-manual-maintenance` line 350–352 | NFR Allocation: "Kit updates use file-level diff with interactive prompts. Config migration creates backup." |
| 6 | `cpt-cypilot-nfr-validation-performance` | §6.1 Validation Performance | p2 | **PASS** | §1.2 NFR table line 227 | NFR Allocation: "Single-pass scanning, no external calls, in-memory processing, no LLM dependency." Validator component (§3.2 line 565): "Single-pass scanning for performance (≤ 3s per artifact)." |
| 7 | `cpt-cypilot-nfr-security-integrity` | §6.1 Security and Integrity | p1 | **PASS** | §1.2 NFR table line 228 | NFR Allocation: "Validator reads files as text only — no eval/exec. Config Manager rejects files containing known secret patterns." |
| 8 | `cpt-cypilot-nfr-reliability-recoverability` | §6.1 Reliability & Recoverability | p2 | **PASS** | §1.2 NFR table line 229 | NFR Allocation: "Config migration creates backup before applying changes. All error messages include file path, line number, and remediation steps." |
| 9 | `cpt-cypilot-nfr-adoption-usability` | §6.1 Adoption and Usability | p2 | **PASS** | §1.2 NFR table line 230 | NFR Allocation: "Init uses sensible defaults. CLI provides `--help` with usage examples for every command." |

**Result**: All 9 PRD NFR IDs are addressed in DESIGN §1.2 NFR Allocation table, with 5 also having corresponding Design Principles in §2.1. **0 missing**.

---

## 4. Reverse Traceability — DESIGN → PRD (Scope Beyond PRD)

### Items Checked

| # | DESIGN Element | PRD Basis | Verdict | Notes |
|---|---------------|-----------|---------|-------|
| 1 | **Spec Coverage** command (§1.2 line 217–221) | Partially under `cpt-cypilot-fr-core-traceability` | **OBSERVATION** | DESIGN adds `spec-coverage` as a distinct command with coverage percentage and granularity score metrics. PRD's traceability FR mentions "code tags linking implementation to design" but does not explicitly define coverage measurement as a capability. This is a **minor scope extension** — reasonable but not explicitly PRD-mandated. |
| 2 | **Deprecated Blueprint FR** `cpt-cypilot-fr-core-blueprint` (§1.2 line 179–183) | Not in current PRD | **PASS** | Explicitly marked DEPRECATED per ADR. Historical record, not new scope. |
| 3 | **Interface ID divergence** (§3.3 line 701, 747) | PRD §7 | **OBSERVATION** | DESIGN uses `cpt-cypilot-interface-cli-json` where PRD uses `cpt-cypilot-interface-cli`. DESIGN uses `cpt-cypilot-interface-github-gh-cli` where PRD uses `cpt-cypilot-contract-github`. Different IDs for the same interfaces — see §7 below. |
| 4 | **12 Design Principles** (§2.1 line 284–352) | Refinement of PRD NFRs | **PASS** | Within DESIGN's remit. Principles refine PRD NFRs into architectural guidance. |
| 5 | **5 Constraints** (§2.2 line 356–384) | Refinement of PRD NFRs + §3 | **PASS** | `python-stdlib`, `markdown-contract`, `git-project-heuristics`, `cross-platform`, `no-weakening` — all derive from PRD requirements. |
| 6 | **14 ADRs** (§1.2 line 239–254) | Architecture decisions | **PASS** | Expected in DESIGN. Two ADRs (`extract-sdlc-kit`, `remove-system-from-core-toml`) are DESIGN-level decisions not referenced in PRD, but PRD §5.2 acknowledges SDLC extraction. |
| 7 | **JSON → TOML Migration** (§4 line 1064–1088) | Under `cpt-cypilot-fr-core-config` + `cpt-cypilot-fr-core-version` | **PASS** | Implementation of config migration capability already in PRD. |
| 8 | **`cpt-cypilot-tech-python-stdlib`** (§1.3 line 268) | PRD §3.1 environment constraints | **PASS** | Technology tracking ID, not an FR. |

**Result**: No unauthorized scope expansion found. **2 observations** (spec-coverage minor extension, interface ID divergence).

---

## 5. Actor Consistency

### PRD Actor Definitions (§2)

| Actor ID | Name | Type | PRD Section |
|----------|------|------|-------------|
| `cpt-cypilot-actor-user` | User | Human | §2.1 line 115–117 |
| `cpt-cypilot-actor-ai-agent` | AI Agent | System | §2.2 line 123–125 |
| `cpt-cypilot-actor-ci-pipeline` | CI/CD Pipeline | System | §2.2 line 129–131 |
| `cpt-cypilot-actor-cypilot-cli` | Cypilot CLI | System | §2.2 line 135–137 |

### DESIGN Actor References

The DESIGN references the same 4 actor IDs in sequence diagram descriptions (§3.6):
- `cpt-cypilot-actor-user` — in seq-init (line 804), seq-validate (line 878), seq-generate-workflow (line 910), seq-pr-review (line 938), seq-update (line 970), seq-traceability-query (line 1001)
- `cpt-cypilot-actor-cypilot-cli` — in seq-init (line 804), seq-update (line 970)
- `cpt-cypilot-actor-ai-agent` — in seq-generate-workflow (line 910), seq-pr-review (line 938), seq-traceability-query (line 1001)
- `cpt-cypilot-actor-ci-pipeline` — in seq-validate (line 878)

**Result**: All 4 PRD actor IDs are used consistently in DESIGN. **0 mismatches**.

### Minor Naming Inconsistencies

| Context | PRD Text | DESIGN Text | Severity |
|---------|----------|-------------|----------|
| Agent list | "Claude" (§2.2 line 125) | "Claude Code" (§1.3 line 260) | LOW — product name drift |
| Agent list | "Copilot, OpenAI" (§2.2 line 125) | "GitHub Copilot, OpenAI Codex" (§1.3 line 260) | LOW — DESIGN uses full product names |

These are cosmetic differences in how agent products are named, not actor ID mismatches.

---

## 6. Use Case Coverage

### Explicit Sequence Diagrams in DESIGN §3.6

| UC | PRD ID | DESIGN Sequence | Status |
|----|--------|-----------------|--------|
| UC-002 | `cpt-cypilot-usecase-init` | `cpt-cypilot-seq-init` (line 800–857) | **COVERED** |
| UC-004 | `cpt-cypilot-usecase-create-artifact` | `cpt-cypilot-seq-generate-workflow` (line 906–930) | **COVERED** |
| UC-005 | `cpt-cypilot-usecase-validate` | `cpt-cypilot-seq-validate` (line 874–902) | **COVERED** |
| UC-007 | `cpt-cypilot-usecase-pr-review` | `cpt-cypilot-seq-pr-review` (line 934–962) | **COVERED** |
| UC-011 | `cpt-cypilot-usecase-update` | `cpt-cypilot-seq-update` (line 966–993) | **COVERED** |

### Implicitly Covered (no dedicated sequence diagram)

| UC | PRD ID | DESIGN Coverage | Gap Assessment |
|----|--------|-----------------|----------------|
| UC-001 | `cpt-cypilot-usecase-install` | CLI Proxy component (§3.2 line 495–514); §1.1 describes proxy, cache, pipx | **LOW GAP** — straightforward install; component description sufficient |
| UC-003 | `cpt-cypilot-usecase-enable` | Agent entry points (§1.3 line 260); SKILL.md entry point (§3.2 line 529) | **LOW GAP** — agent-initiated action, not tool logic |
| UC-006 | `cpt-cypilot-usecase-implement` | CDSL FR design response (§1.2 line 147–149); Traceability Engine (§3.2 line 580–604) | **MEDIUM GAP** — no sequence showing code generation + traceability tag insertion flow |
| UC-008 | `cpt-cypilot-usecase-pr-status` | Similar to PR Review; SDLC kit extraction noted | **LOW GAP** — analogous to UC-007; SDLC kit responsibility |
| UC-009 | `cpt-cypilot-usecase-configure` | CLI Config FR design response (§1.2 line 163–165); Config Manager (§3.2 line 608–633) | **MEDIUM GAP** — no sequence showing config CLI interaction flow |
| UC-010 | `cpt-cypilot-usecase-kit-manage` | Kit Manager component (§3.2 line 637–664); init sequence includes kit install path | **LOW GAP** — init sequence covers kit install; standalone kit install not diagrammed but component is detailed |
| UC-012 | `cpt-cypilot-usecase-migrate` | SDLC kit extraction noted; init sequence covers project setup | **MEDIUM GAP** — brownfield migration is SDLC kit capability; no core sequence |
| UC-013 | `cpt-cypilot-usecase-execution-plan` | Execution Plans FR design response (§1.2 line 78–80) | **MEDIUM GAP** — detailed FR design response but no sequence diagram showing the plan → phase → execute loop |

**Result**: 5/13 explicitly covered. 8/13 implicitly covered (4 LOW gap, 4 MEDIUM gap). No use case is entirely unaddressed.

---

## 7. Interface ID Inconsistencies

| Interface | PRD ID | DESIGN ID | Issue |
|-----------|--------|-----------|-------|
| CLI Interface | `cpt-cypilot-interface-cli` (PRD §7.1 line 567) | `cpt-cypilot-interface-cli-json` (DESIGN §3.3 line 701) | Different IDs for the same interface. DESIGN appends `-json` to indicate output format. |
| GitHub Integration | `cpt-cypilot-contract-github` (PRD §7.2 line 581) | `cpt-cypilot-interface-github-gh-cli` (DESIGN §3.3 line 747) | Different ID prefix (`contract` vs `interface`) and different slug. DESIGN specifies `gh` CLI implementation detail. |

**Recommendation**: Align interface IDs between PRD and DESIGN. Either the PRD should adopt the more specific DESIGN IDs or the DESIGN should reference the PRD IDs and add specificity in the description.

---

## 8. Key Findings Summary

### Critical Issues
None.

### High-Priority Observations

1. **Phase input table inaccuracy** — The phase file lists `cpt-cypilot-fr-core-auto-config` as a PRD FR, but this ID does not exist in `architecture/PRD.md`. The actual PRD has 21 FRs, not 19.
2. **Interface ID divergence** — PRD and DESIGN use different IDs for 2 shared interfaces (CLI, GitHub). This breaks bidirectional traceability for these specific interfaces.

### Medium-Priority Observations

3. **4 use cases lack sequence diagrams** — UC-006 (Implement Feature), UC-009 (Configure), UC-012 (Migrate), UC-013 (Execution Plan) have FR-level design responses but no interaction sequences showing the end-to-end flow.
4. **Spec Coverage scope extension** — DESIGN adds `spec-coverage` as a command under traceability that isn't explicitly called out in the PRD's traceability FR.

### Low-Priority Observations

5. **Agent naming drift** — "Claude" vs "Claude Code", "Copilot" vs "GitHub Copilot", "OpenAI" vs "OpenAI Codex" between PRD §2.2 and DESIGN §1.3.
6. **Grouped utility FRs** — 5 FRs (toc, template-qa, doctor, hooks, completions) share a single Design Response paragraph in DESIGN §1.2. Individual design detail is minimal for each.

---

## 9. Acceptance Criteria Self-Verification

- [x] All 21 actual PRD FR IDs checked against DESIGN — each PRESENT with evidence (the phase file's claimed 19 is incorrect; 21 is the true count)
- [x] All 9 PRD NFR IDs checked against DESIGN — each ADDRESSED with evidence
- [x] DESIGN checked for scope beyond PRD — 2 observations flagged (spec-coverage, interface ID divergence)
- [x] Actor names compared between PRD and DESIGN — 0 mismatches; 2 minor naming inconsistencies flagged
- [x] Use case coverage assessed — 5/13 explicit, 8/13 implicit; 4 MEDIUM gaps noted
- [x] Every finding includes section reference — all evidence cites DESIGN line numbers and section references
- [x] Summary table of all cross-reference checks produced — §2 (FR table), §3 (NFR table), §6 (UC table)
- [x] Findings saved to `out/phase-01-findings.md`
