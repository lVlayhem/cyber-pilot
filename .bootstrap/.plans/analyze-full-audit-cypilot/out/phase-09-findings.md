# Phase 9 Findings — Skill Engine Codebase vs Specs

**Date**: 2025-01-20
**Scope**: `skills/cypilot/scripts/cypilot/` (41 Python files)
**Specs audited**: `cypilot.clispec`, `architecture/DESIGN.md`, ADR-0001, ADR-0002, ADR-0004

---

## 1. Command Traceability

### 1.1 CLISPEC → Code (every CLISPEC command checked for handler)

| # | CLISPEC Command | Code Handler | File | Status |
|---|----------------|-------------|------|--------|
| 1 | `init` | `_cmd_init` | `commands/init.py` | IMPLEMENTED |
| 2 | `validate` | `_cmd_validate` | `commands/validate.py` | IMPLEMENTED |
| 3 | `validate-kits` | `_cmd_validate_kits` | `commands/validate_kits.py` | IMPLEMENTED |
| 4 | `list-ids` | `_cmd_list_ids` | `commands/list_ids.py` | IMPLEMENTED |
| 5 | `list-id-kinds` | `_cmd_list_id_kinds` | `commands/list_id_kinds.py` | IMPLEMENTED |
| 6 | `get-content` | `_cmd_get_content` | `commands/get_content.py` | IMPLEMENTED |
| 7 | `info` | `_cmd_cypilot_info` | `commands/adapter_info.py` | IMPLEMENTED |
| 8 | `resolve-vars` | `_cmd_resolve_vars` | `commands/resolve_vars.py` | IMPLEMENTED |
| 9 | `where-defined` | `_cmd_where_defined` | `commands/where_defined.py` | IMPLEMENTED |
| 10 | `where-used` | `_cmd_where_used` | `commands/where_used.py` | IMPLEMENTED |
| 11 | `generate-agents` | `_cmd_generate_agents` | `commands/agents.py` | IMPLEMENTED |
| 12 | `self-check` | `_cmd_validate_kits` (alias) | `commands/validate_kits.py` → `self_check.py` | IMPLEMENTED (see F-01) |
| 13 | `toc` | `_cmd_toc` | `commands/toc.py` | IMPLEMENTED |
| 14 | `validate-toc` | `_cmd_validate_toc` | `commands/validate_toc.py` | IMPLEMENTED |
| 15 | `update` | `_cmd_update` | `commands/update.py` | IMPLEMENTED |
| 16 | `spec-coverage` | `_cmd_spec_coverage` | `commands/spec_coverage.py` | IMPLEMENTED |
| 17 | `kit install` | `cmd_kit_install` | `commands/kit.py` | IMPLEMENTED |
| 18 | `kit update` | `cmd_kit_update` | `commands/kit.py` | IMPLEMENTED |
| 19 | `kit migrate` | `cmd_kit_migrate` (deprecated stub) | `commands/kit.py` | IMPLEMENTED (deprecated) |
| 20 | `generate-resources` | `_cmd_generate_resources` (deprecated stub) | `cli.py` | IMPLEMENTED (deprecated) |
| 21 | `migrate` | `_cmd_migrate` | `commands/migrate.py` | IMPLEMENTED |
| 22 | `migrate-config` | `_cmd_migrate_config` | `commands/migrate.py` | IMPLEMENTED |

**Result**: 22/22 CLISPEC commands have code handlers. **PASS**

### 1.2 Code → CLISPEC (every code handler checked for spec entry)

| # | Code Command | Handler | CLISPEC Entry | Status |
|---|-------------|---------|--------------|--------|
| 1 | `agents` | `_cmd_agents` → `commands/agents.py:cmd_agents` | **MISSING** | UNDOCUMENTED (see F-02) |
| 2 | `validate-code` | alias → `_cmd_validate` | Not in CLISPEC | Legacy alias (LOW) |
| 3 | `validate-rules` | alias → `_cmd_validate_kits` | Not in CLISPEC | Legacy alias (LOW) |

All other code handlers (19 remaining) have matching CLISPEC entries. **1 undocumented command found.**

### 1.3 DESIGN.md 3.3 → Code (API Contract commands not yet implemented)

| # | DESIGN.md Command | In CLISPEC | In Code | Status |
|---|------------------|-----------|---------|--------|
| 1 | `doctor` | No | No | UNIMPLEMENTED (see F-03) |
| 2 | `config show` | No | No | UNIMPLEMENTED (see F-03) |
| 3 | `config system add/remove` | No | No | UNIMPLEMENTED (see F-03) |
| 4 | `hook install/uninstall` | No | No | UNIMPLEMENTED (see F-03) |
| 5 | `kit move-config <slug>` | No | No | UNIMPLEMENTED (see F-03) |

---

## 2. Component Boundary Verification

Source: `architecture/DESIGN.md` Section 3.2

| # | Component | ID | Code Files | Boundary | Status |
|---|-----------|-----|-----------|----------|--------|
| 1 | Skill Engine | `cpt-cypilot-component-skill-engine` | `cli.py` | Dispatch only, no business logic | COMPLIANT |
| 2 | Validator | `cpt-cypilot-component-validator` | `validate.py`, `validate_kits.py`, `self_check.py`, `constraints.py`, `fixing.py` | Structure checks, cross-refs, constraints | COMPLIANT |
| 3 | Traceability Engine | `cpt-cypilot-component-traceability-engine` | `list_ids.py`, `list_id_kinds.py`, `get_content.py`, `where_defined.py`, `where_used.py`, `document.py`, `parsing.py`, `codebase.py` | ID scan, definition/reference queries | COMPLIANT |
| 4 | Config Manager | `cpt-cypilot-component-config-manager` | `context.py`, `toml_utils.py`, `artifacts_meta.py`, `files.py`, `adapter_info.py`, `resolve_vars.py` | Config CRUD, TOML serialization, migration | COMPLIANT |
| 5 | Kit Manager | `cpt-cypilot-component-kit-manager` | `kit.py`, `diff_engine.py`, `manifest.py` | Install, update, file-level diff | COMPLIANT |
| 6 | Agent Generator | `cpt-cypilot-component-agent-generator` | `agents.py` | Agent entry point generation, SKILL.md composition | COMPLIANT |

**Result**: 6/6 components checked. All COMPLIANT. **PASS**

---

## 3. ADR Compliance

### 3.1 ADR-0001 — Remove Blueprint System

| Check | File | Result | Details |
|-------|------|--------|---------|
| No `process_kit` import | All files | PASS | grep found 0 occurrences |
| No blueprint processing in active paths | `kit.py` | PASS | Docstring explicitly states "no blueprint processing" |
| No `blueprint.py` module | `utils/` | PASS | File does not exist |
| Legacy blueprint refs in comments | `utils/toc.py` lines 6, 312, 330 | WARN | Stale docstring references to "Blueprint artifact generator" (see F-04) |
| Defensive exclusions | `utils/diff_engine.py` line 264-265 | OK | `_KIT_EXCLUDE_FILES` / `_KIT_EXCLUDE_DIRS` exclude legacy blueprint artifacts — correct defensive code |
| Migration code | `commands/migrate.py`, `commands/update.py` | OK | Blueprint references are appropriate in migration context |
| Cleanup code | `commands/update.py` line 597 | OK | `_cleanup_legacy_blueprint_dirs` removes leftover blueprint dirs |

**Result**: ADR-0001 **PASS** (1 LOW documentation debt item)

### 3.2 ADR-0002 — Python stdlib Only

All `import` statements across 41 Python files were audited. Imports found:

`argparse`, `codecs`, `copy`, `dataclasses`, `datetime`, `difflib`, `enum`, `fnmatch`, `glob`, `json`, `os`, `pathlib`, `re`, `shutil`, `subprocess`, `sys`, `tarfile`, `tempfile`, `tomllib`, `typing`, `urllib.error`, `urllib.request`

All are Python stdlib modules. No third-party dependencies.

- `subprocess` is used only in `commands/migrate.py` (for `gh` CLI invocation during PR workflows — external dependency documented in DESIGN.md 3.5).

**Result**: ADR-0002 **PASS**

### 3.3 ADR-0004 — TOML Config Formats

| Check | File | Result | Details |
|-------|------|--------|---------|
| TOML reading/writing | `utils/toml_utils.py` | PASS | Pure `tomllib` (stdlib) for reading, custom serializer for writing |
| Context loading | `utils/context.py` | PASS | Uses `load_artifacts_meta` (TOML-based), no JSON config loading |
| Config operations | `commands/kit.py` | PASS | All config read/write uses `tomllib` and `toml_utils` |
| Backward-compat JSON loading | `utils/files.py` | OK | JSON fallback with deprecation warning — consistent with DESIGN.md Config Manager spec |
| JSON import in `adapter_info.py` | `commands/adapter_info.py` | OK | Used for `_load_json_file` helper (backward-compat) and JSON output formatting |

**Result**: ADR-0004 **PASS**

---

## 4. Dead Code Candidates

### 4.1 Utils Import Coverage

All 17 utility modules in `utils/` are imported by at least one command handler or another utility:

| Module | Imported By |
|--------|------------|
| `artifacts_meta.py` | `self_check.py`, `init.py`, `validate_kits.py`, `validate.py`, `context.py` |
| `codebase.py` | `list_ids.py`, `validate.py`, `coverage.py` |
| `constraints.py` | `validate.py`, `validate_kits.py`, `self_check.py`, `context.py` |
| `context.py` | `validate.py`, `validate_kits.py`, `list_ids.py`, `where_defined.py`, `where_used.py`, `adapter_info.py` |
| `coverage.py` | `spec_coverage.py` |
| `diff_engine.py` | `kit.py` |
| `document.py` | `where_defined.py`, `where_used.py`, `list_ids.py`, `validate.py` |
| `error_codes.py` | `validate.py`, `self_check.py`, `codebase.py` |
| `files.py` | `adapter_info.py`, `resolve_vars.py`, `self_check.py`, `init.py`, `kit.py`, `cli.py` |
| `fixing.py` | `validate.py` |
| `language_config.py` | `coverage.py`, `utils/__init__.py` |
| `manifest.py` | `kit.py` |
| `parsing.py` | `utils/__init__.py` |
| `toc.py` | `validate_toc.py`, `toc.py` (command) |
| `toml_utils.py` | `init.py`, `kit.py` |
| `ui.py` | All command handlers |

**Result**: 0 unused utility modules. **No dead utils found.**

### 4.2 Dead/Deprecated Functions

| Function | File | Status | Details |
|----------|------|--------|---------|
| `_cmd_generate_resources` | `cli.py:80-86` | DEPRECATED | Stub that prints warning and returns 1. Reachable via dispatch. |
| `cmd_kit_migrate` | `kit.py:1623-1633` | DEPRECATED | Stub that prints warning and returns 1. Reachable via `kit migrate` subcommand. |
| `_read_conf_version` | `kit.py:1258-1271` | ACTIVE | Re-exported by `update.py` for tests. Not dead. |
| `_detect_and_migrate_layout` | `kit.py:1278-1445` | ACTIVE | Called by `update.py:172`. Legacy migration helper, still needed. |

**Result**: 2 deprecated stubs found (both intentional, both reachable). **No unreachable dead code found.**

---

## 5. Findings Detail

### F-01 — `self-check` routing mismatch (MEDIUM)

- **Type**: Spec drift
- **Severity**: MEDIUM
- **File**: `cli.py:259`, `self_check.py`
- **Details**: CLISPEC defines `self-check` as a distinct command ("Validate kit examples against their own templates and constraints"). In code, `self-check` is aliased to `validate-kits` (`cli.py:259`). The behavior IS functionally correct because `validate_kits.py` internally imports and calls `self_check.run_self_check_from_meta` (line 107). However, `self_check.py` has no standalone `cmd_self_check` CLI entry point — it cannot be invoked independently of `validate-kits`.
- **Proposal**: Either (a) add a `cmd_self_check` entry point in `self_check.py` and a dedicated `_cmd_self_check` wrapper in `cli.py`, or (b) update CLISPEC to document `self-check` as an alias for `validate-kits`.

### F-02 — `agents` command not in CLISPEC (MEDIUM)

- **Type**: Undocumented command
- **Severity**: MEDIUM
- **File**: `cli.py:279-280`, `commands/agents.py`
- **Details**: The `agents` command is registered in cli.py dispatch, listed in help text as "Show generated agent integration status", and documented in DESIGN.md 3.3 (`agents [--agent A]`). However, no CLISPEC `COMMAND agents` block exists. Only `generate-agents` is in the CLISPEC. These are two distinct commands: `agents` shows status, `generate-agents` creates/updates files.
- **Proposal**: Add a `COMMAND agents` block to `cypilot.clispec` with synopsis, description, options, and exit codes.

### F-03 — 5 DESIGN.md commands not yet implemented (LOW)

- **Type**: Unimplemented spec
- **Severity**: LOW (likely p2 backlog)
- **File**: `architecture/DESIGN.md` Section 3.3
- **Details**: The following commands appear in DESIGN.md API Contracts table but have no code handler and no CLISPEC entry:
  - `doctor` — Environment health check
  - `config show` — Display current core config
  - `config system add/remove` — Manage system definitions
  - `hook install/uninstall` — Manage pre-commit hooks
  - `kit move-config <slug>` — Relocate kit config directory
- **Proposal**: Either implement these commands or mark them explicitly as `p2` / `planned` in DESIGN.md 3.3 to distinguish from implemented commands.

### F-04 — Stale blueprint references in `utils/toc.py` (LOW)

- **Type**: Documentation debt
- **Severity**: LOW
- **File**: `utils/toc.py` lines 6, 312, 330
- **Details**: Docstrings reference "Blueprint artifact generator" and "blueprint-generated content" — these are leftover references from the pre-ADR-0001 blueprint system. The code itself is correct (no blueprint logic), only the documentation is stale.
- **Proposal**: Update docstrings to reference "kit content" instead of "blueprint".

---

## 6. Summary

| Metric | Count |
|--------|-------|
| CLISPEC commands traced | 22/22 |
| Code handlers traced | 22 registered + 3 legacy aliases |
| Undocumented commands | 1 (`agents`) |
| DESIGN.md unimplemented | 5 (likely p2 backlog) |
| Components verified | 6/6 |
| Component violations | 0 |
| ADR-0001 compliance | PASS (1 LOW doc debt) |
| ADR-0002 compliance | PASS |
| ADR-0004 compliance | PASS |
| Dead utils modules | 0 |
| Dead code candidates | 0 (2 intentional deprecated stubs) |
| Total findings | 4 |
| — MEDIUM | 2 (F-01, F-02) |
| — LOW | 2 (F-03, F-04) |
| — HIGH/CRITICAL | 0 |
