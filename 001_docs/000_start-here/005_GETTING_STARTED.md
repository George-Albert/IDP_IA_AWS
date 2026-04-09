# Getting Started

Quick start guide to set up your environment and run IDP locally.

## Prerequisites

- **Python 3.9+**
- **Node.js 18+** (for web UI development)
- **AWS Account** (for deployment)
- **Git**

> **Platform-specific setup?** See [../060_setup/](../060_setup/) for Linux, macOS, or WSL-specific instructions.

## Quick Setup (5 minutes)

### 1. Clone the Repository

```bash
git clone <repository-url>
cd refactored-idp-verification
```

### 2. Initialize Your Environment

```bash
# Copy environment template
cp .env.example .env

# Edit .env with your AWS credentials and settings
# (see 010_DEPLOYMENT_AND_INFRA_GUIDE.md for details)
```

### 3. Install Python Dependencies

```bash
# Use your preferred package manager (uv, pip, poetry, etc.)
pip install -e '020_shared/idp-packages/idp_common_pkg[dev]'
pip install -e '020_shared/idp-packages/idp_cli_pkg[dev]'
```

### 4. Verify Installation

```bash
# Check CLI works
idp --version

# Run unit tests
make test-unit
```

### 5. Start Development

**Web UI development:**
```bash
cd 060_apps/web-ui
npm install
npm run dev
```

**Lambda/backend development:**
```bash
# See Makefile targets
make help
```

---

## Core Commands

| Command | Purpose |
|---------|---------|
| `make help` | List all build targets |
| `make test-unit` | Run unit tests |
| `make typecheck` | Run Pyright type checker |
| `make lint` | Run Ruff linter |
| `idp --help` | CLI help |

---

## Directory Structure Overview

| Folder | Purpose |
|--------|---------|
| `001_docs/` | Documentation (this folder) |
| `010_infra/` | CloudFormation templates & deployment |
| `020_shared/idp-packages/` | Shared Python packages |
| `040_modules/lambda/` | Lambda function handlers |
| `050_configs/` | Document type configurations |
| `060_apps/web-ui/` | Web UI application |

**See [004_FOLDER_MAPPING.md](004_FOLDER_MAPPING.md) for complete folder reference.**

---

## Next Steps

1. **Want to understand the system?** → [002_REPO_READING_ORDER.md](002_REPO_READING_ORDER.md)
2. **Set up for your OS?** → [../060_setup/](../060_setup/)
3. **Ready to configure documents?** → [007_DOCUMENT_TYPES_AND_CONFIGS.md](007_DOCUMENT_TYPES_AND_CONFIGS.md)
4. **Deploy to AWS?** → [010_DEPLOYMENT_AND_INFRA_GUIDE.md](010_DEPLOYMENT_AND_INFRA_GUIDE.md)

---

## Troubleshooting

**Import errors with packages?**
```bash
# Reinstall in editable mode
pip install -e '020_shared/idp-packages/idp_common_pkg[dev]'
```

**Tests failing?**
See [006_BUILD_AND_VALIDATION.md](006_BUILD_AND_VALIDATION.md)

**More help?**
[010_core/troubleshooting.md](../010_core/troubleshooting.md)

---

**For detailed instructions by OS:** See [060_setup/setup-development-env-linux.md](../060_setup/setup-development-env-linux.md)
