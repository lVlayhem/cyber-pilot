# Phase 4/11 — PRD Quality Review Findings

**Input**: `architecture/PRD.md` (971 lines)
**Date**: 2026-03-12
**Checklist items evaluated**: 20 (14 MUST HAVE + 6 MUST NOT HAVE)

---

## Issues

### Issue Table

| # | Checklist ID | Severity | Issue | Proposal |
|---|-------------|----------|-------|----------|
| 1 | ARCH-PRD-001 | CRITICAL | Implementation decisions embedded in requirements | Extract format/path choices to DESIGN |
| 2 | ARCH-PRD-NO-001 | CRITICAL | Technical implementation details present in FRs | Move implementation specifics to DESIGN |
| 3 | ARCH-PRD-NO-002 | CRITICAL | Architectural decision stated in PRD | Reframe as a product concept or move to DESIGN |
| 4 | TEST-PRD-001 | HIGH | Acceptance criteria cover only 6 of 19+ FRs | Add acceptance criteria for remaining FRs |
| 5 | BIZ-PRD-004 | HIGH | Missing use cases for 3 p2 FRs | Add use cases or explicit coverage rationale |
| 6 | BIZ-PRD-NO-002 | HIGH | VS Code Plugin FR contains spec-level design | Extract detailed UI behaviors to a FEATURE spec |
| 7 | ARCH-PRD-002 | MEDIUM | Monolithic FR covers 9 sub-requirements across multiple concerns | Decompose into separate FRs |
| 8 | BIZ-PRD-006 | MEDIUM | Acronyms not expanded on first use | Add expansions on first occurrence |
| 9 | TEST-PRD-002 | MEDIUM | Vague untestable term in NFR | Replace with measurable criterion |

---

### Issue Details

#### Issue 1 — ARCH-PRD-001: Scope Boundaries (CRITICAL)

**Checklist sub-item**: No implementation decisions embedded in requirements

**Issue**: Several functional requirements embed implementation decisions that belong in DESIGN, not PRD.

**Evidence**:
- `cpt-cypilot-fr-core-execution-plans` (line 261): "Store plans in a git-ignored directory (`{cypilot_path}/.plans/`) with a **TOML manifest** tracking phase status (pending/in_progress/done/failed)" — specifies file format (TOML), directory path, git-ignore behavior, and status enum values
- `cpt-cypilot-fr-core-execution-plans` (line 260): "Enforce a line budget: ≤500 lines target, ≤1000 lines maximum per phase file" — specifies concrete line counts that are implementation-level tuning parameters
- `cpt-cypilot-fr-core-kits` (line 284): "version (GitHub tag) MUST be stored in `core.toml` kit section" — specifies filename
- `cpt-cypilot-fr-core-init` (line 214): "Inject a managed navigation block into the project root `AGENTS.md`" — specifies filename

**Proposal**: Restate requirements in terms of *what* the system must achieve (e.g., "persist plan state in a machine-readable manifest," "enforce a configurable size budget per phase") and move format/path/enum decisions to DESIGN.

---

#### Issue 2 — ARCH-PRD-NO-001: No Technical Implementation Details (CRITICAL)

**Checklist sub-item**: No database schemas, API endpoints, tech stack decisions, code snippets

**Issue**: Multiple FRs contain specific technology and format choices.

**Evidence**:
- TOML format mandated in `cpt-cypilot-fr-core-execution-plans` (line 261) and referenced via `core.toml` in `cpt-cypilot-fr-core-kits` (line 284)
- Specific directory structure `{cypilot_path}/.plans/` prescribed in `cpt-cypilot-fr-core-execution-plans` (line 261)
- VS Code Plugin FR (`cpt-cypilot-fr-core-vscode-plugin`, lines 441–451) specifies 10 detailed UI implementation behaviors including: syntax highlighting rules, autocompletion triggers ("typing `cpt-` MUST trigger autocompletion"), tooltip content structure, sidebar panel layout, and status bar integration — these are implementation specifications, not product requirements
- Specific file naming: `core.toml`, `AGENTS.md`, `plan.toml`, `conf.toml`

**Proposal**: Replace technology-specific references with capability descriptions (e.g., "human-readable configuration format" instead of "TOML"). Move VS Code Plugin implementation details to a dedicated FEATURE spec.

---

#### Issue 3 — ARCH-PRD-NO-002: No Architectural Decisions (CRITICAL)

**Checklist sub-item**: No microservices vs monolith, database choices, cloud providers

**Issue**: An architectural layering decision is stated directly in the PRD.

**Evidence**:
- Section 1.1 (line 56): "The system is a **single-layer generic engine**" — this is an architectural decision about system structure that belongs in DESIGN, not a product requirement

**Proposal**: Reframe as a product concept: "The system provides a generic engine that delegates domain-specific behavior to installable kits" — describing the *what* without prescribing the *how* of layering.

---

#### Issue 4 — TEST-PRD-001: Acceptance Criteria (HIGH)

**Checklist sub-item**: Each functional requirement has verifiable acceptance criteria

**Issue**: Section 9 "Acceptance Criteria" contains only 6 acceptance criteria for 19+ functional requirements. Most FRs have no corresponding acceptance criterion.

**Evidence**:
- Section 9 (lines 932–938) covers: init, validation output, agent generation, diagnostics, config integrity, PR review
- **Missing acceptance criteria for**: `cpt-cypilot-fr-core-skill-engine`, `cpt-cypilot-fr-core-workflows`, `cpt-cypilot-fr-core-execution-plans`, `cpt-cypilot-fr-core-kits`, `cpt-cypilot-fr-core-kit-manifest`, `cpt-cypilot-fr-core-resource-diff`, `cpt-cypilot-fr-core-layout-migration`, `cpt-cypilot-fr-core-traceability`, `cpt-cypilot-fr-core-cdsl`, `cpt-cypilot-fr-core-version`, `cpt-cypilot-fr-core-cli-config`, `cpt-cypilot-fr-core-template-qa`, `cpt-cypilot-fr-core-toc`, `cpt-cypilot-fr-core-hooks`, `cpt-cypilot-fr-core-completions`, `cpt-cypilot-fr-core-vscode-plugin`

**Proposal**: Add at least one verifiable acceptance criterion per FR (or per FR group for closely related FRs). Each criterion should be binary (pass/fail) and testable without subjective judgment.

---

#### Issue 5 — BIZ-PRD-004: Use Case Coverage (HIGH)

**Checklist sub-item**: All primary user journeys represented as use cases

**Issue**: Three p2 functional requirements with distinct user journeys have no corresponding use case.

**Evidence**:
- `cpt-cypilot-fr-core-doctor` (p2, line 410): Environment diagnostics — user runs diagnostics to check system health. Referenced in acceptance criteria (line 936) but no use case documents the journey.
- `cpt-cypilot-fr-core-template-qa` (p2, line 392): Template quality assurance — user validates examples against templates. No use case.
- `cpt-cypilot-fr-core-vscode-plugin` (p2, line 437): VS Code extension — developer uses IDE-native Cypilot features. No use case despite being a major FR with 10 sub-requirements.

**Proposal**: Add UC-014 (Environment Diagnostics), UC-015 (Template QA), and UC-016 (VS Code Plugin Usage) — or explicitly note these as sub-journeys of existing use cases with rationale.

---

#### Issue 6 — BIZ-PRD-NO-002: No Spec-Level Design (HIGH)

**Checklist sub-item**: No detailed user flows, wireframes, algorithm descriptions, state machines

**Issue**: The VS Code Plugin FR contains spec-level UI design that belongs in a FEATURE specification.

**Evidence**:
- `cpt-cypilot-fr-core-vscode-plugin` (lines 441–451) prescribes 10 specific UI behaviors:
  1. ID Syntax Highlighting with "configurable color scheme"
  2. Go to Definition / Find References with specific navigation behavior
  3. Real-Time Validation with "debounce" specification
  4. ID Autocompletion triggered by typing `cpt-` with grouping by kind
  5. Hover Information with 5 specific tooltip fields
  6. Cross-Artifact Link Lens with annotation format example
  7. Traceability Tree View as a sidebar panel with specific hierarchy
  8. Validation Status Bar with click behavior
  9. Quick Fix Actions for 3 specific issue types
  10. Config-Aware with workspace support requirement

This level of detail (e.g., tooltip content fields, debounce behavior, specific annotation format) is design specification, not product requirement.

**Proposal**: In the PRD, state the VS Code Plugin FR as high-level capabilities (e.g., "The plugin MUST support ID navigation, real-time validation, and autocompletion"). Move the 10 detailed behaviors to a FEATURE spec for the VS Code Plugin.

---

#### Issue 7 — ARCH-PRD-002: Modularity Enablement (MEDIUM)

**Checklist sub-item**: No monolithic requirements

**Issue**: `cpt-cypilot-fr-core-kits` contains 9 numbered sub-requirements spanning distinct concerns: installation, GitHub versioning, interactive update/diff, SKILL extensions, system prompt extensions, workflow registrations, config relocation, and init prompting. These cover at least 4 separate capabilities.

**Evidence**:
- Lines 280–296: Single FR with sub-items covering: file structure (1), GitHub installation (2), GitHub versioning (3), update with diff (4), SKILL extensions (5), system prompt extensions (6), workflow registrations (7), config relocation (8), init prompting (9)
- Compare: `cpt-cypilot-fr-core-kit-manifest` (lines 299–313) was correctly extracted as a separate FR for the manifest sub-concern

**Proposal**: Decompose into separate FRs: `cpt-cypilot-fr-core-kit-install` (items 1–3), `cpt-cypilot-fr-core-kit-update` (item 4), `cpt-cypilot-fr-core-kit-extensions` (items 5–7), `cpt-cypilot-fr-core-kit-config` (items 8–9). This improves traceability and testability.

---

#### Issue 8 — BIZ-PRD-006: Terminology & Definitions (MEDIUM)

**Checklist sub-item**: Acronyms expanded on first use

**Issue**: Several acronyms are used without expansion on first occurrence.

**Evidence**:
- "PRD" — title (line 1), never expanded to "Product Requirements Document"
- "SDLC" — first used in line 21 (TOC) and line 60 (body), never expanded to "Software Development Life Cycle"
- "CI/CD" — line 68, never expanded to "Continuous Integration / Continuous Delivery"
- "PR" — line 74, never expanded to "Pull Request"
- "NFR" — line 23 (TOC), never expanded to "Non-Functional Requirement"
- "ADR" — line 60, never expanded to "Architecture Decision Record"
- "FR" — used implicitly in section structure, never expanded to "Functional Requirement"

**Proposal**: Expand each acronym on first use in the document body (e.g., "Product Requirements Document (PRD)"). Alternatively, add all acronyms to the Glossary (Section 1.4) which currently defines only 7 terms.

---

#### Issue 9 — TEST-PRD-002: Testability (MEDIUM)

**Checklist sub-item**: No vague terms ("fast", "easy", "intuitive")

**Issue**: A non-functional requirement uses the vague, untestable term "intuitive."

**Evidence**:
- `cpt-cypilot-nfr-simplicity` (line 483): "Configuration syntax MUST be **intuitive** and readable without documentation" — "intuitive" is subjective and not objectively verifiable

**Proposal**: Replace with a measurable criterion, e.g., "Configuration syntax MUST be parseable by a new user within 5 minutes without documentation" or "Configuration syntax MUST use only standard TOML key-value pairs with no custom DSL."

---

## Passing Items (no issues found)

| Checklist ID | Category | Verdict |
|-------------|----------|---------|
| BIZ-PRD-001 | Vision Clarity | PASS — purpose, users, problems, success criteria, capabilities all present and specific |
| BIZ-PRD-003 | Requirements Completeness | PASS — all capabilities map to FRs, requirements trace to problems, FRs prioritized (p1/p2/p3) |
| BIZ-PRD-005 | Success Metrics | PASS — 5 SMART criteria with baselines and targets in Section 1.3 |
| BIZ-PRD-007 | Assumptions & Open Questions | PASS — 6 assumptions listed; open questions explicitly closed |
| BIZ-PRD-008 | Risks & Non-Goals | PASS — 5 risks with impact/mitigation; 5 non-goals in Section 4.2 |
| SEC-PRD-001/002 | Auth Requirements | PASS — explicit N/A in Section 6.2 line 548 |
| DOC-PRD-001 | Explicit Non-Applicability | PASS — 10 explicit N/A statements in Section 6.2 with reasons |
| BIZ-PRD-NO-001 | No Implementation Tasks | PASS — no sprint plans, task breakdowns, or effort estimates found |
| DATA-PRD-NO-001 | No Data Schema Definitions | PASS — no ERDs, table definitions, or JSON schemas |
| INT-PRD-NO-001 | No API Specifications | PASS — no REST endpoints or request/response schemas; Section 7 stays at interface level |

---

## Non-Applicable Domain Verification

| Domain | PRD Statement | Location | Status |
|--------|--------------|----------|--------|
| Authentication/Authorization | "Not applicable — Cypilot is a local CLI tool, not a multi-user system requiring access control" | Section 6.2, line 548 | ✅ Explicit |
| Availability/Recovery | "Not applicable — Cypilot runs locally as a CLI, not as a service requiring uptime guarantees" | Section 6.2, line 549 | ✅ Explicit |
| Scalability | "Not applicable — Cypilot processes single repositories locally; traditional scaling does not apply" | Section 6.2, line 550 | ✅ Explicit |
| Throughput/Capacity | "Not applicable — Cypilot is a local development tool, not a high-throughput system" | Section 6.2, line 551 | ✅ Explicit |
| Accessibility/i18n | "Not applicable — CLI tool for developers; English-only is acceptable" | Section 6.2, line 552 | ✅ Explicit |
| Regulatory/Legal | "Not applicable — Cypilot is a methodology tool with no user data or regulated industry context" | Section 6.2, line 553 | ✅ Explicit |
| Data Ownership/Lifecycle | "Not applicable — Cypilot does not persist user data; artifacts are owned by the project" | Section 6.2, line 554 | ✅ Explicit |
| Support Requirements | "Not applicable — open-source tool; support is community-driven" | Section 6.2, line 555 | ✅ Explicit |
| Deployment/Monitoring | "Not applicable — installed locally; no server deployment or monitoring required" | Section 6.2, line 556 | ✅ Explicit |
| Safety | "Not applicable — pure information/development tool with no physical interaction or harm potential" | Section 6.2, line 557 | ✅ Explicit |

All 10 non-applicable domains have explicit N/A statements with reasons.

---

## Summary

| Metric | Count |
|--------|-------|
| Total checklist items evaluated | 20 |
| PASS | 11 |
| FAIL | 9 |
| CRITICAL issues | 3 |
| HIGH issues | 3 |
| MEDIUM issues | 3 |
| LOW issues | 0 |
| Non-applicable domains verified | 10/10 |

**Top 3 critical findings**:
1. **Implementation details in requirements** (ARCH-PRD-001 + ARCH-PRD-NO-001): TOML format, specific filenames, directory paths, and VS Code UI specs are embedded in FRs rather than deferred to DESIGN
2. **Architectural decision in PRD** (ARCH-PRD-NO-002): "single-layer generic engine" prescribes system structure
3. **Acceptance criteria gap** (TEST-PRD-001): Only 6 of 19+ FRs have acceptance criteria in Section 9
