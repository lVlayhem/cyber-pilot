# Phase 8 Findings — Proxy Codebase vs Specs

**Phase**: 8/11 — Proxy Codebase vs Specs
**Date**: 2026-03-12
**Status**: PASS (with findings)

## 1. Scope

| File | Lines | Purpose |
|------|-------|---------|
| `src/cypilot_proxy/__init__.py` | 11 | Package init, `__version__` |
| `src/cypilot_proxy/__main__.py` | 11 | Entry point (`python -m cypilot_proxy`) |
| `src/cypilot_proxy/cache.py` | 348 | Cache management: GitHub download, local copy, archive extraction |
| `src/cypilot_proxy/cli.py` | 307 | CLI argument parsing, command routing, version check |
| `src/cypilot_proxy/resolve.py` | 230 | Project root detection, skill target resolution |
| **Total** | **907** | |

**Specs reviewed**:
- `architecture/DESIGN.md` — Section 1.2 (Architecture Drivers → Global CLI Installer), Section 3.2 (Component Model → CLI Proxy)
- `architecture/features/core-infra.md` — Flows, algorithms, DoDs for proxy
- `architecture/specs/CLISPEC.md` — CLI format spec (meta-spec, not command definitions)
- `skills/cypilot/cypilot.clispec` — CLI command definitions
- `pyproject.toml` — Package metadata, entry points

## 2. Function/Class Traceability Table

### `__init__.py`

| Symbol | Type | Spec Trace | Status |
|--------|------|-----------|--------|
| `__version__` | constant | `cpt-cypilot-dod-core-infra-global-package` | ✅ Traced |

### `__main__.py`

| Symbol | Type | Spec Trace | Status |
|--------|------|-----------|--------|
| `main()` import + call | entry point | `cpt-cypilot-dod-core-infra-global-package` | ✅ Traced |

### `cache.py`

| Symbol | Type | Spec Trace | Status |
|--------|------|-----------|--------|
| `GITHUB_OWNER` | constant | `inst-cache-helpers` | ✅ Traced |
| `GITHUB_REPO` | constant | `inst-cache-helpers` | ✅ Traced |
| `GITHUB_API_BASE` | constant | `inst-cache-helpers` | ✅ Traced |
| `USER_AGENT` | constant | `inst-cache-helpers` | ✅ Traced |
| `_resolve_api_base(url)` | function | `inst-cache-helpers` (partial) | ⚠️ Fork URL support not in spec |
| `resolve_latest_version(api_base)` | function | `inst-resolve-version` | ✅ Traced |
| `copy_from_local(source_dir, force)` | function | `inst-cache-helpers` (partial) | ⚠️ `--source` flag not in CLISPEC/CDSL |
| `download_and_cache(version, force, url)` | function | `cpt-cypilot-algo-core-infra-cache-skill` | ✅ Traced |
| `_find_common_prefix(members)` | function | `inst-cache-helpers` | ✅ Traced |
| `_extract_stripped(tf, members, prefix, dest)` | function | `inst-cache-helpers` | ✅ Traced |
| `_find_zip_prefix(members)` | function | `inst-cache-helpers` | ✅ Traced |
| `_extract_zip_stripped(zf, members, prefix, dest)` | function | `inst-cache-helpers` | ✅ Traced |

### `cli.py`

| Symbol | Type | Spec Trace | Status |
|--------|------|-----------|--------|
| `_extract_version_param(args)` | function | `inst-cli-proxy-helpers` | ✅ Traced |
| `_extract_named_param(args, name)` | function | `inst-cli-proxy-helpers` | ✅ Traced |
| `main(argv)` | function | `cpt-cypilot-flow-core-infra-cli-invocation` | ✅ Traced |
| `_forward_to_skill(skill_path, args)` | function | `inst-forward-project` / `inst-forward-cache` | ✅ Traced |
| `_background_version_check(project_skill_path)` | function | `inst-bg-version-check` | ✅ Traced (minor drift on "background") |

### `resolve.py`

| Symbol | Type | Spec Trace | Status |
|--------|------|-----------|--------|
| `MARKER_START` | constant | `inst-resolve-helpers` | ✅ Traced |
| `_TOML_FENCE_RE` | constant | `inst-resolve-helpers` | ✅ Traced |
| `_CYPILOT_VAR_RE` | constant | `inst-resolve-helpers` | ✅ Traced |
| `_OLD_NAV_RE` | constant | `inst-resolve-helpers` | ✅ Traced |
| `find_project_root(start_dir)` | function | `inst-walk-parents` | ✅ Traced |
| `_parse_toml_from_markdown(text)` | function | `inst-resolve-helpers` | ✅ Traced |
| `read_cypilot_path(project_root)` | function | `inst-resolve-helpers` | ✅ Traced |
| `find_install_dir(project_root)` | function | `inst-resolve-helpers` | ⚠️ Legacy fallback scan undocumented |
| `get_cache_dir()` | function | `inst-resolve-helpers` | ✅ Traced |
| `get_version_file()` | function | `inst-resolve-helpers` | ✅ Traced |
| `find_project_skill(start_dir)` | function | `inst-walk-parents` / `inst-if-marker` | ✅ Traced |
| `find_cached_skill()` | function | `inst-check-global-cache` | ✅ Traced |
| `resolve_skill(start_dir)` | function | `cpt-cypilot-algo-core-infra-resolve-skill` | ✅ Traced |
| `get_cached_version()` | function | `inst-resolve-helpers` | ✅ Traced |
| `get_project_version(skill_path)` | function | `inst-resolve-helpers` | ✅ Traced |

## 3. Spec → Code Verification (Forward Traceability)

### FR: `cpt-cypilot-fr-core-installer` (Global CLI Installer)

| Requirement | Code Location | Status |
|------------|---------------|--------|
| Check for project-installed skill first, fall back to cache | `resolve.py:resolve_skill()` | ✅ |
| Forward all commands transparently | `cli.py:_forward_to_skill()` | ✅ |
| Cache skill bundle from GitHub releases | `cache.py:download_and_cache()` | ✅ |
| Perform background version checks | `cli.py:_background_version_check()` | ✅ (synchronous, see F-03) |
| Support `--version`, `--help` | `cli.py:69-80` (version), help forwarded | ✅ |

### DESIGN Component: `cpt-cypilot-component-cli-proxy`

| Responsibility | Code Location | Status |
|----------------|---------------|--------|
| Maintain local skill bundle cache | `cache.py` | ✅ |
| Route commands to project or cache skill | `resolve.py` + `cli.py` | ✅ |
| Non-blocking background version checks | `cli.py:276-305` | ✅ (minor wording drift) |
| Display version update notices | `cli.py:297-300` | ✅ |

### DoD: `cpt-cypilot-dod-core-infra-global-package`

| Requirement | Status | Notes |
|-------------|--------|-------|
| Installable via pipx | ✅ | pyproject.toml configured |
| Zero third-party dependencies | ✅ | `dependencies = []` |
| Register `cypilot` and `cpt` as entry points | ⚠️ **F-01** | Only `cpt` registered |
| Work on Linux, Windows, macOS | ✅ | pathlib, no OS-specific calls |

### DoD: `cpt-cypilot-dod-core-infra-cli-routes`

| Requirement | Status | Notes |
|-------------|--------|-------|
| Global `cypilot` and `cpt` CLI entry point | ⚠️ **F-01** | Only `cpt` |
| Resolves skill target | ✅ | |
| Forwards all commands | ✅ | |
| Returns JSON output and exit codes | ✅ | Passes through from skill engine |

### CDSL Flow: `cpt-cypilot-flow-core-infra-cli-invocation`

All 9 flow steps and supporting helpers are implemented with `@cpt-begin`/`@cpt-end` markers in `cli.py`. Each instruction label maps to code. ✅

### CDSL Algorithm: `cpt-cypilot-algo-core-infra-resolve-skill`

All 5 algorithm steps and supporting helpers are implemented in `resolve.py`. ✅

### CDSL Algorithm: `cpt-cypilot-algo-core-infra-cache-skill`

All 8 algorithm steps and supporting helpers are implemented in `cache.py`. ✅

## 4. Architecture Compliance

### Zero Skill Logic in Proxy

The proxy code:
- Does NOT validate artifacts
- Does NOT scan IDs or perform traceability
- Does NOT process kits or interpret kit semantics
- Does NOT modify project files (only cache at `~/.cypilot/cache/`)
- Only routes commands via `subprocess.run()`

**Result**: ✅ PASS — proxy contains zero skill logic

### stdlib-only Constraint

All imports across 5 files:

| Module | File(s) | stdlib? |
|--------|---------|---------|
| `io` | cache.py | ✅ |
| `json` | cache.py, cli.py | ✅ (unused in cli.py — F-08) |
| `re` | resolve.py | ✅ |
| `shutil` | cache.py | ✅ (duplicate import — F-10) |
| `subprocess` | cli.py | ✅ |
| `sys` | cache.py, cli.py, resolve.py | ✅ |
| `tarfile` | cache.py | ✅ |
| `tomllib` | resolve.py | ✅ (Python 3.11+ stdlib) |
| `zipfile` | cache.py | ✅ |
| `pathlib.Path` | cache.py, cli.py, resolve.py | ✅ |
| `typing` | cache.py, cli.py, resolve.py | ✅ |
| `urllib.error` | cache.py | ✅ |
| `urllib.request` | cache.py | ✅ |

Internal imports: `cypilot_proxy.resolve`, `cypilot_proxy.cli` ✅

**pyproject.toml**: `dependencies = []` ✅

**Result**: ✅ PASS — stdlib-only constraint satisfied

### CLI Interface Compliance

Proxy-level flags (handled before forwarding):

| Flag | Commands | In CLISPEC? | Status |
|------|----------|-------------|--------|
| `--version` (no value) | all | Not as proxy flag | ⚠️ F-02 |
| `--version VALUE` | init, update | Not in CLISPEC | ⚠️ F-02 |
| `--force` | init, update | In CLISPEC for init | ✅ partial |
| `--source DIR` | init, update | Not in CLISPEC | ⚠️ F-02 |
| `--url URL` | init, update | Not in CLISPEC | ⚠️ F-02 |
| `--no-cache` | init, update | Not in CLISPEC | ⚠️ F-02 |

Forwarded flags for `update`: `--dry-run`, `--help`, `-h`, `--no-interactive`, `-y`, `--yes` — all match CLISPEC ✅

## 5. Findings

### F-01: Missing `cypilot` Console Entry Point

- **File**: `pyproject.toml`
- **Line(s)**: 17
- **Type**: UNIMPLEMENTED
- **Severity**: HIGH
- **Details**: DoD `cpt-cypilot-dod-core-infra-global-package` requires: "The package MUST register `cypilot` and `cpt` as console entry points." Only `cpt = "cypilot_proxy.cli:main"` is registered. The `cypilot` alias is missing.
- **Proposal**: CODE FIX — add `cypilot = "cypilot_proxy.cli:main"` to `[project.scripts]` in `pyproject.toml`.

### F-02: Proxy-Level CLI Flags Undocumented in CLISPEC

- **File**: `skills/cypilot/cypilot.clispec` (init and update commands)
- **Line(s)**: 1-29, 473-509
- **Type**: UNDOCUMENTED
- **Severity**: MEDIUM
- **Details**: The proxy handles several flags before forwarding: `--version VALUE`, `--source DIR`, `--url URL`, `--no-cache`. These are proxy-level flags not documented in the CLISPEC. The CLISPEC only describes the skill engine's interface. A consumer of `cpt init` or `cpt update` needs to know about these proxy flags.
- **Proposal**: SPEC FIX — add proxy-level flags to the `init` and `update` command definitions in the CLISPEC, or add a separate "Proxy Flags" section.

### F-03: "Background" Version Check Is Synchronous Post-Execution

- **File**: `src/cypilot_proxy/cli.py`
- **Line(s)**: 276-305
- **Type**: SPEC_DRIFT
- **Severity**: LOW
- **Details**: DESIGN 1.2 and core-infra flow step 6 say "non-blocking background version checks." The implementation is a synchronous function called _after_ the skill engine completes. It only compares local file versions (no network I/O), so latency impact is negligible. The spec's intent (don't delay the user's command) is met, but the wording "background" is misleading.
- **Proposal**: SPEC FIX — change wording from "non-blocking background" to "post-execution local" version check, since no network call occurs.

### F-04: `copy_from_local` Not in CDSL Algorithm

- **File**: `src/cypilot_proxy/cache.py`
- **Line(s)**: 94-157
- **Type**: UNDOCUMENTED
- **Severity**: LOW
- **Details**: `copy_from_local()` implements the `--source DIR` flow for local development. The `cpt-cypilot-algo-core-infra-cache-skill` algorithm only describes GitHub download, not local copy. The `--source` flag is also not in the CLISPEC.
- **Proposal**: SPEC FIX — add a local-copy path to the cache algorithm, or document as part of `inst-cache-helpers` supporting functions.

### F-05: Fork URL Support (`_resolve_api_base`) Not in Specs

- **File**: `src/cypilot_proxy/cache.py`
- **Line(s)**: 31-52
- **Type**: UNDOCUMENTED
- **Severity**: LOW
- **Details**: `_resolve_api_base()` supports custom GitHub URLs (forks) via the `--url` proxy flag. This capability is not mentioned in the CDSL algorithm, DESIGN, or CLISPEC.
- **Proposal**: SPEC FIX — add fork/custom-repo support to the cache algorithm spec and CLISPEC.

### F-06: CLISPEC `update` Description References Removed Blueprint System

- **File**: `skills/cypilot/cypilot.clispec`
- **Line(s)**: 475
- **Type**: SPEC_DRIFT
- **Severity**: HIGH
- **Details**: The `update` command description says: "compares blueprint versions (skip same, warn if migration needed), regenerates .gen/ from user blueprints". Per ADR-0001 (`cpt-cypilot-adr-remove-blueprint-system`), blueprints have been removed. The description is stale.
- **Proposal**: SPEC FIX — update description to: "Refreshes .core/ from cache, updates kit files via file-level diff, regenerates .gen/ aggregate files, and ensures config/ scaffold."

### F-07: CLISPEC Defines Removed Commands (`kit migrate`, `generate-resources`)

- **File**: `skills/cypilot/cypilot.clispec`
- **Line(s)**: 617-653 (`kit migrate`), 655-684 (`generate-resources`)
- **Type**: SPEC_DRIFT
- **Severity**: HIGH
- **Details**: The CLISPEC defines `kit migrate` (three-way marker merge) and `generate-resources` (blueprint processing). Both were removed per ADR-0001. `kit migrate` is now a deprecated stub redirecting to `kit update`; `generate-resources` was deleted entirely. Their descriptions reference blueprints.
- **Proposal**: SPEC FIX — remove `generate-resources` from CLISPEC. Update `kit migrate` to show DEPRECATED status with redirect note to `kit update`. Also update RELATED sections in `update` command (lines 507-508) that reference `@CLI.kit-migrate` and `@CLI.generate-resources`.

### F-08: Unused `json` Import in `cli.py`

- **File**: `src/cypilot_proxy/cli.py`
- **Line(s)**: 15
- **Type**: VIOLATION
- **Severity**: LOW
- **Details**: `import json` is present but the `json` module is never used in `cli.py`. Violates `cpt-cypilot-nfr-simplicity` (no unnecessary abstractions).
- **Proposal**: CODE FIX — remove `import json` from `cli.py`.

### F-09: Unused `NoReturn` Import in `cli.py`

- **File**: `src/cypilot_proxy/cli.py`
- **Line(s)**: 19
- **Type**: VIOLATION
- **Severity**: LOW
- **Details**: `from typing import List, NoReturn, Optional` — `NoReturn` is never used in the file.
- **Proposal**: CODE FIX — change to `from typing import List, Optional`.

### F-10: Duplicate `import shutil` in `cache.py`

- **File**: `src/cypilot_proxy/cache.py`
- **Line(s)**: 14 (top-level), 229 (inside function)
- **Type**: VIOLATION
- **Severity**: LOW
- **Details**: `shutil` is imported at the top of the file (line 14) and again inside `download_and_cache()` (line 229). The inner import is redundant.
- **Proposal**: CODE FIX — remove `import shutil` at line 229.

### F-11: `find_install_dir` Legacy Fallback Scan Undocumented

- **File**: `src/cypilot_proxy/resolve.py`
- **Line(s)**: 113-116
- **Type**: UNDOCUMENTED
- **Severity**: LOW
- **Details**: `find_install_dir()` has a fallback that scans for `.cypilot`, `cypilot`, `.cpt` directories when AGENTS.md parsing fails. The spec `cpt-cypilot-algo-core-infra-resolve-skill` only describes reading the TOML variable from AGENTS.md — this legacy fallback is not documented.
- **Proposal**: SPEC FIX — document as backward-compatibility behavior, or remove if legacy support is no longer needed.

### F-12: Init/Update Special Routing Not Documented in CDSL Flow

- **File**: `src/cypilot_proxy/cli.py`
- **Line(s)**: 100-136 (update), 144-162 (init)
- **Type**: SPEC_DRIFT
- **Severity**: LOW
- **Details**: The CDSL flow `cpt-cypilot-flow-core-infra-cli-invocation` describes a uniform flow: check project → check cache → forward. In practice, `init` forces cache-first routing (you can't init into a nonexistent project skill), and `update` has a two-step flow (update cache, then forward to skill). This is correct behavior but not reflected in the spec's linear flow.
- **Proposal**: SPEC FIX — add notes to the CDSL flow clarifying that `init` and `update` have special routing: cache-first for `init`, cache-update-then-forward for `update`.

## 6. Summary

### Counts

| Metric | Count |
|--------|-------|
| Source files analyzed | 5 |
| Total source lines | 907 |
| Functions/constants | 25 |
| Fully traced to spec | 22 (88%) |
| Partially traced (extends beyond spec) | 3 (12%) |
| Untraced (no spec at all) | 0 |
| Total findings | 12 |

### Findings by Severity

| Severity | Count | IDs |
|----------|-------|-----|
| CRITICAL | 0 | — |
| HIGH | 3 | F-01, F-06, F-07 |
| MEDIUM | 1 | F-02 |
| LOW | 8 | F-03, F-04, F-05, F-08, F-09, F-10, F-11, F-12 |

### Findings by Type

| Type | Count | IDs |
|------|-------|-----|
| SPEC_DRIFT | 4 | F-03, F-06, F-07, F-12 |
| UNDOCUMENTED | 4 | F-02, F-04, F-05, F-11 |
| UNIMPLEMENTED | 1 | F-01 |
| VIOLATION | 3 | F-08, F-09, F-10 |

### Architecture Compliance

| Check | Result |
|-------|--------|
| Zero skill logic in proxy | ✅ PASS |
| stdlib-only (no third-party imports) | ✅ PASS |
| Cross-platform (pathlib, no OS-specific) | ✅ PASS |
| @cpt marker coverage | ✅ HIGH — all significant code blocks have `@cpt-begin`/`@cpt-end` markers |

### Overall Assessment

The proxy codebase is well-structured and closely aligned with its specifications. The most significant finding is F-01 (missing `cypilot` entry point), which is a simple fix. F-06 and F-07 are CLISPEC staleness issues caused by ADR-0001 (blueprint removal) not being propagated to the command spec. The remaining findings are low-severity documentation gaps and minor code quality issues.

The proxy correctly implements the stateless proxy pattern: it contains zero skill logic, uses only stdlib imports, and cleanly separates cache management, skill resolution, and command forwarding into distinct modules.
