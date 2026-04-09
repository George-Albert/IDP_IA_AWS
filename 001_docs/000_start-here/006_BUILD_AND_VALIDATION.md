# Build & Validation

How to build, test, and validate the system.

## Build Targets

All build targets are defined in `Makefile` at the repository root.

```bash
make help  # List all available targets
```

### Common Targets

| Target | Purpose |
|--------|---------|
| `make test-unit` | Run unit tests |
| `make test-cicd` | Run CICD-style tests |
| `make lint` | Run linter (Ruff) |
| `make typecheck` | Run type checker (Pyright) |
| `make format` | Auto-format code |
| `make build` | Build artifacts |
| `make clean` | Clean build artifacts |

---

## Running Tests

### Unit Tests

```bash
# Run all unit tests
make test-unit

# Run specific test file
python -m pytest 020_shared/idp-packages/idp_common_pkg/tests/unit/test_*.py -v

# Run with coverage
make test-unit COVERAGE=1
```

### Integration Tests

```bash
# See 080_tests/ for integration test setup
make test-integration
```

### CICD Tests (GitHub Actions simulation)

```bash
make test-cicd
```

---

## Code Quality

### Type Checking

```bash
make typecheck
```

Pyright is configured in `010_infra/pyrightconfig.json` and scans:
- `020_shared/idp-packages/idp_common_pkg/idp_common`
- `020_shared/idp-packages/idp_cli_pkg/idp_cli`
- `040_modules/lambda`

### Linting

```bash
make lint
```

Ruff is configured in `ruff.toml` with rules for:
- Code style
- Import organization
- Common errors

### Code Formatting

```bash
make format
```

Auto-formats with Black-compatible rules.

---

## Validation Checklist

Before committing code:

- [ ] `make typecheck` passes
- [ ] `make lint` passes
- [ ] `make test-unit` passes
- [ ] `make format` applied
- [ ] No regressions in integration tests

---

## Deployment Validation

### CloudFormation Validation

```bash
# Validate CloudFormation templates
cfn-lint 010_infra/template.yaml
cfn-lint 010_infra/nested/appsync/extracted_resources.yaml
```

### Lambda Package Validation

```bash
# Verify Lambda packages can be imported
python -c "from idp_common import *"
```

### Docker Build Validation (if applicable)

```bash
docker build -f 010_infra/Dockerfile.optimized .
```

---

## Continuous Integration

The following GitHub Actions workflows run automatically on push:

- **developer-tests.yml** — Runs unit tests and type checking on PR
- **deploy-docs.yml** — Deploys documentation on commits to `001_docs/`

See `.github/workflows/` for details.

---

## Phase 1 Validation

A complete validation of the Phase 1 migration is available in:

**[PHASE_1_VALIDATION_REPORT.md](../PHASE_1_VALIDATION_REPORT.md)**

This includes:
- Path migration verification
- File location mapping
- Configuration updates
- Test infrastructure changes

---

## Troubleshooting

**Tests fail with import errors?**
```bash
pip install -e '020_shared/idp-packages/idp_common_pkg[dev]'
```

**Type checking finds issues?**
Check [010_infra/pyrightconfig.json](../../010_infra/pyrightconfig.json) for include paths.

**Linter complains about code style?**
Run `make format` to auto-fix formatting.

**More help?**
See [010_core/troubleshooting.md](../010_core/troubleshooting.md)

---

**Next:** [009_MANUAL_REVIEW_WORKFLOW.md](009_MANUAL_REVIEW_WORKFLOW.md) or [010_DEPLOYMENT_AND_INFRA_GUIDE.md](010_DEPLOYMENT_AND_INFRA_GUIDE.md)
