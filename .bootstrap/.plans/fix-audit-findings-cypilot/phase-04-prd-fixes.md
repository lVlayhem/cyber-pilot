```toml
[phase]
plan = "fix-audit-findings-cypilot"
number = 4
total = 8
type = "generate"
title = "PRD Quality Fixes"
depends_on = []
input_files = ["architecture/PRD.md"]
output_files = ["architecture/PRD.md"]
outputs = ["out/phase-04-done.md"]
inputs = []
```

## Preamble

This is a self-contained phase file. All rules, constraints, and context are included below. Follow the instructions exactly and report results against the acceptance criteria at the end.

## What

Fix PRD quality issues: extract implementation details from requirements, reframe architectural statements as product concepts, update status checkboxes, expand acronyms, and add acceptance criteria for FRs that lack them. These fixes address audit findings Q-01, Q-02, Q-03, Q-05, Q-07, D-14 from `architecture/AUDIT-REPORT.md`.

## Rules

- **PRD Purpose**: The PRD defines WHAT the system must do, not HOW. Requirements should state capabilities, not formats, paths, or technology choices.
- **Preserve Intent**: When extracting implementation details, preserve the original requirement intent. Replace specific details with capability descriptions.
- **Acceptance Criteria**: Each AC must be binary (pass/fail) and testable without subjective judgment.
- **Minimal Changes**: Do not rewrite the entire PRD. Make targeted edits to address specific findings.
- **Checkbox Updates**: Mark FRs as `[x]` only if code exists and the feature is operational. FRs still marked `[ ]` in PRD but fully implemented: `fr-core-execution-plans`, `fr-core-kit-manifest`, `fr-core-layout-migration`, `fr-core-version`.

## Task

1. **Read** `architecture/PRD.md`.

2. **Extract implementation details from FRs** (Q-01, Q-02):
   - `fr-core-execution-plans` (line ~261): Replace "Store plans in a git-ignored directory (`{cypilot_path}/.plans/`) with a **TOML manifest** tracking phase status (pending/in_progress/done/failed)" with "Persist plan state in a machine-readable manifest with status tracking per phase."
   - `fr-core-execution-plans` (line ~260): Replace "Enforce a line budget: ≤500 lines target, ≤1000 lines maximum per phase file" with "Enforce a configurable size budget per phase file to ensure each phase fits within a single AI agent context."
   - `fr-core-kits` (line ~284): Replace "version (GitHub tag) MUST be stored in `core.toml` kit section" with "version (GitHub tag) MUST be stored in project configuration."
   - `fr-core-init` (line ~214): Replace "Inject a managed navigation block into the project root `AGENTS.md`" with "Inject a managed navigation block into the project's agent entry point file."

3. **Reframe architectural statement** (Q-03): In Section 1.1 (line ~56), change "The system is a **single-layer generic engine**" to "The system provides a generic engine that delegates all domain-specific behavior to installable kits."

4. **Replace vague term** (Q-05): In `nfr-simplicity` (line ~483), replace "Configuration syntax MUST be **intuitive** and readable without documentation" with "Configuration syntax MUST use only standard TOML key-value pairs and be parseable by a new user within 5 minutes without documentation."

5. **Update PRD checkboxes** (D-14): Change `[ ]` to `[x]` for these implemented FRs:
   - `cpt-cypilot-fr-core-execution-plans`
   - `cpt-cypilot-fr-core-kit-manifest`
   - `cpt-cypilot-fr-core-layout-migration`
   - `cpt-cypilot-fr-core-version`

6. **Expand acronyms on first use** (Q-07): On first occurrence in the document body, expand:
   - PRD → Product Requirements Document (PRD)
   - SDLC → Software Development Life Cycle (SDLC)
   - CI/CD → Continuous Integration / Continuous Delivery (CI/CD)
   - PR → Pull Request (PR)
   - NFR → Non-Functional Requirement (NFR)
   - ADR → Architecture Decision Record (ADR)
   - FR → Functional Requirement (FR)

7. **Add acceptance criteria** for FRs that lack them (at least 5 new ACs in Section 9):
   - `fr-core-skill-engine`: "Running any CLI command on a valid project produces deterministic output given identical input."
   - `fr-core-kits`: "Installing a kit from a GitHub repository creates the kit config directory with all expected files."
   - `fr-core-traceability`: "Running `list-ids` on a project returns all `cpt-*` IDs defined in artifacts."
   - `fr-core-resource-diff`: "Updating a kit with user-modified files presents an interactive diff for each changed file."
   - `fr-core-toc`: "Running `toc` on a Markdown file creates or updates the table of contents block."

8. Save completion log to `out/phase-04-done.md`.

## Acceptance Criteria

- [ ] No TOML format, directory path, or specific filename references remain in FR requirement text (implementation details extracted)
- [ ] "single-layer generic engine" reframed as product concept
- [ ] "intuitive" replaced with measurable criterion in `nfr-simplicity`
- [ ] 4 implemented FRs updated from `[ ]` to `[x]`
- [ ] At least 5 acronyms expanded on first use
- [ ] At least 5 new acceptance criteria added to Section 9
- [ ] No unintended changes to other sections
- [ ] Completion log saved to `out/phase-04-done.md`

## Output Format

When complete, report:

```
PHASE 4/8 COMPLETE
Status: PASS | FAIL
Files modified: {list}
Acceptance criteria: [x] / [ ] each
```

Then generate the prompt for the next phase.
