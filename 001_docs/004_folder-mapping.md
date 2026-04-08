# Folder Mapping - Phase 1 Refactoring

This document tracks the migration from the original flat structure to a business-oriented, top-level numbered structure.

**Date:** April 8, 2026  
**Scope:** Phase 1 - Complete restructuring with clean migration trail  
**Strategy:** Move when target is clear, update imports, preserve git history

## Top-Level Structure Overview

```
001_docs/              → All documentation, references, and guidance
010_infra/             → Infrastructure as Code (CloudFormation, SAM, IAM)
020_shared/            → Shared libraries and common functionality
030_orchestration/     → Workflow orchestration (Step Functions definitions)
040_modules/           → Processing modules (Lambda functions, processors)
050_configs/           → Configuration files and document schemas
060_apps/              → Applications (UI, web services)
070_samples/           → Sample documents, test data, notebooks
080_tests/             → Test suites (currently empty, to be populated)
090_scripts/           → Utility and maintenance scripts
```

## Detailed Mapping

### 001_docs/ → Documentation

| Old Location | New Location | Notes |
|-------------|-------------|-------|
| docs/* | 001_docs/ | All markdown documentation files |
| README.md | 001_docs/000_README.md | Renamed with sequence number |
| CHANGELOG.md | 001_docs/001_CHANGELOG.md | Renamed with sequence number |
| CONTRIBUTING.md | 001_docs/002_CONTRIBUTING.md | Renamed with sequence number |
| LICENSE | 001_docs/003_LICENSE.txt | Renamed with sequence number |
| CLAUDE.md | 001_docs/CLAUDE-instructions.md | Renamed for clarity |
| VERSION | 001_docs/VERSION.txt | Version tracking file |
| NOTICE | 001_docs/NOTICE.txt | Legal notice |
| docs-site/ | 001_docs/docs-site/ | Astro documentation site |
| threat-modeling/ | 001_docs/threat-modeling/ | Security threat models (DrawIO) |
| images/ | 001_docs/images/ | Reference images and diagrams |

### 010_infra/ → Infrastructure

| Old Location | New Location | Notes |
|-------------|-------------|-------|
| template.yaml | 010_infra/template.yaml | Main CloudFormation template |
| nested/ | 010_infra/nested/ | Nested CloudFormation templates |
| iam-roles/ | 010_infra/iam-roles/ | IAM role definitions |
| patterns/unified/template.yaml | 010_infra/pattern-unified-template.yaml | Pattern-specific stack template |
| patterns/unified/buildspec.yml | 010_infra/pattern-unified-buildspec.yml | Main CodeBuild specification |
| patterns/unified/buildspec-bda.yml | 010_infra/pattern-unified-buildspec-bda.yml | BDA-specific build spec |
| patterns/unified/buildspec-pipeline.yml | 010_infra/pattern-unified-buildspec-pipeline.yml | Pipeline-specific build spec |
| Dockerfile.optimized | 010_infra/Dockerfile.optimized | Docker image definition |
| publish.py | 010_infra/publish.py | Publication/deployment script |
| pyrightconfig.json | 010_infra/pyrightconfig.json | Type checking configuration |
| ruff.toml | 010_infra/ruff.toml | Ruff linter configuration |

### 020_shared/ → Shared Packages

| Old Location | New Location | Notes |
|-------------|-------------|-------|
| lib/idp_common_pkg | 020_shared/idp-packages/idp_common_pkg | Core IDP utilities and base classes |
| lib/idp_cli_pkg | 020_shared/idp-packages/idp_cli_pkg | CLI tool |
| lib/idp_sdk | 020_shared/idp-packages/idp_sdk | Python SDK for IDP |
| lib/idp_mcp_connector_pkg | 020_shared/idp-packages/idp_mcp_connector_pkg | MCP (Model Context Protocol) connector |

**Note:** All `lib/` package structures preserved within `020_shared/idp-packages/`

### 030_orchestration/ → Workflow Orchestration

| Old Location | New Location | Notes |
|-------------|-------------|-------|
| patterns/unified/statemachine/ | 030_orchestration/unified-statemachine/ | Step Functions state machine definitions |

**Future:** Additional orchestration patterns and workflow definitions

### 040_modules/ → Processing Modules

| Old Location | New Location | Notes |
|-------------|-------------|-------|
| src/lambda/* | 040_modules/lambda/* | All production Lambda functions |
| patterns/unified/src/* | 040_modules/unified-pattern-lambdas/* | Pattern-specific Lambda implementations |
| (New) | 040_modules/schema_builder/ | Schema builder processor (Phase 1 stub) |
| (New) | 040_modules/manual_review_processor/ | Manual review handler (Phase 1 stub) |
| (New) | 040_modules/metadata_capture/ | Metadata extraction processor (Phase 1 stub) |
| (New) | 040_modules/new_type_onboarding/ | Unknown type onboarding orchestrator (Phase 1 stub) |

### 050_configs/ → Configuration Files

| Old Location | New Location | Notes |
|-------------|-------------|-------|
| config_library/ | 050_configs/config-library/ | Existing configuration patterns (legacy) |
| (New) | 050_configs/document-types/known/_template/ | Known document type schema templates |
| (New) | 050_configs/document-types/unknown/ | Unknown document type onboarding forms |
| (New) | 050_configs/workflows/ | Workflow process definitions |

### 060_apps/ → Applications

| Old Location | New Location | Notes |
|-------------|-------------|-------|
| src/ui/ | 060_apps/web-ui/ | React/TypeScript web UI application |

### 070_samples/ → Samples and References

| Old Location | New Location | Notes |
|-------------|-------------|-------|
| samples/* | 070_samples/ | Sample documents (PDFs, etc.) |
| notebooks/ | 070_samples/notebooks/ | Jupyter notebooks for exploration and examples |

### 080_tests/ → Test Suites

| Old Location | New Location | Notes |
|-------------|-------------|-------|
| (Planned) | 080_tests/ | Integration tests, E2E tests, test fixtures (Phase 2+) |

**Note:** Individual package tests remain in their respective modules (e.g., 020_shared/idp-packages/*/tests/)

### 090_scripts/ → Maintenance Scripts

| Old Location | New Location | Notes |
|-------------|-------------|-------|
| scripts/dsr/ | 090_scripts/maintenance/dsr/ | Dependency security review tools |
| scripts/sdlc/ | 090_scripts/maintenance/sdlc/ | SDLC automation (validation, type checking, builds) |
| scripts/setup/ | 090_scripts/maintenance/setup/ | Initial setup and configuration scripts |
| scripts/*.sh | 090_scripts/maintenance/*.sh | Individual utility scripts |
| scripts/*.py | 090_scripts/maintenance/*.py | Individual utility scripts |

## Impact on Build Process

### Makefile Updates
All `Makefile` targets have been updated to reference new paths:
- Package installation points to `020_shared/idp-packages/*`
- Lambda test paths point to `040_modules/lambda/*`
- UI build paths point to `060_apps/web-ui/`
- Script paths point to `090_scripts/maintenance/*`
- Config validation paths point to `050_configs/*` and `010_infra/*`
- Docs build paths point to `001_docs/docs-site/`

### Python Import Changes Required

**In 020_shared/idp-packages/*** - Internal package imports need review
- Check for `from lib.` imports and update to local relative imports
- Verify no imports from `src/` or `scripts/`

**In 040_modules/lambda/*** - Lambda function imports
- Update imports from `lib/` to `../../020_shared/idp-packages/`
- May need systematic refactoring for deployment package structure

**In 060_apps/web-ui*** - UI imports (TypeScript/JavaScript)
- No major changes expected (relative paths already used)

**In 090_scripts/maintenance/*** - Utility script imports
- Update imports from `lib/` to relative paths or installed packages
- Verify paths to referenced modules/data files

**In 070_samples/notebooks*** - Jupyter notebooks
- Update import statements and file references
- Update relative paths to config files and sample data

### Configuration File References

Files that may reference old paths:
- `010_infra/*.yaml` - CloudFormation templates (path references in comments, scripts)
- `030_orchestration/*.json` - Step Functions (Lambda ARNs, S3 paths)
- `050_configs/config-library/*.yaml` - Configuration loader queries
- `060_apps/web-ui/.env*` - Environment configuration
- `Makefile` - Already updated ✓

## Git Migration Trail

This refactoring preserves the full git history through `git mv` commands. To review the migration:

```bash
# See all move operations
git log --oneline --follow -- <new-file-path>

# Review the complete refactoring commit
git show <refactor-commit-hash>

# Compare structure before/after
git show <refactor-commit-hash>^:docs/architecture.md | head -50
git show <refactor-commit-hash>:001_docs/architecture.md | head -50
```

## Downstream Consumers

If other projects/repos depend on this codebase:

1. **Direct Package Imports**
   - Update from `from genaiic_idp.lib import ...` to `from genaiic_idp.shared import ...`
   - Or use installed packages from `020_shared/idp-packages/`

2. **File References**
   - Update configuration file paths from `docs/configs/` to `050_configs/`
   - Update template references from `patterns/` to `010_infra/` or `030_orchestration/`

3. **API Changes**
   - No public API changes - all changes are organizational

## Testing Checklist

- [ ] Makefile targets run successfully (setup, lint, test)
- [ ] Package installation from `020_shared/` works
- [ ] Lambda functions deploy correctly with new paths
- [ ] UI builds and runs from `060_apps/web-ui/`
- [ ] Docs site builds from `001_docs/docs-site/`
- [ ] CloudFormation templates validate with new file references
- [ ] Scripts in `090_scripts/` execute with updated paths
- [ ] Notebooks in `070_samples/` run without path errors

## Future Phases

**Phase 2:**
- Implement full test suite organization in `080_tests/`
- Expand workflow processor stubs in `040_modules/`
- Add config validation and schema enforcement
- Implement unknown document type onboarding

**Phase 3:**
- Consolidate legacy configs from `050_configs/config-library/` into new structure
- Migrate all example/reference configs to new template system
- Remove config-library if fully migrated

## Notes

- The redirect from `patterns/` directory logic to new locations is automatic through this mapping
- Internal structure of packages (e.g., `020_shared/idp-packages/idp_common_pkg/idp_common/`) remains unchanged
- Third-level deep structures are preserved to minimize internal churn
- Focus was on business logic organization at top 2 levels
