# Target Architecture Overview

High-level system design and component relationships for the IDP platform.

## System Architecture

**See detailed documentation:** [../010_core/architecture.md](../010_core/architecture.md)

The IDP platform consists of:

### 1. **Document Input & Discovery**
- Document upload (batch/API)
- Format detection and ingestion
- Layout analysis and OCR

### 2. **Classification & Extraction Layer**
- ML-based document type classification
- Extraction pattern application
- AI model inference

### 3. **Validation & Review**
- Automatic validation against rules
- Quality gates and confidence thresholds
- Manual human-in-the-loop review

### 4. **Processing Patterns**
- **Pattern-1:** Classification-first workflow
- **Pattern-2:** Template-based extraction

### 5. **Interfaces & Integration**
- Web UI for manual review
- CLI for automation
- SDK for programmatic access
- MCP connectors for agent integration

### 6. **Data & Storage**
- Document storage (S3)
- Configuration versioning
- Result tracking and reporting

### 7. **Infrastructure & Operations**
- AWS CloudFormation deployment
- Monitoring and logging
- RBAC and governance

## Quick Component Map

| Component | Location | Purpose |
|-----------|----------|---------|
| **Core SDK** | `020_shared/idp-packages/idp_common_pkg/` | Core IDP abstraction and APIs |
| **CLI** | `020_shared/idp-packages/idp_cli_pkg/` | Command-line interface |
| **Web UI** | `060_apps/web-ui/` | Human review interface |
| **Lambda Functions** | `040_modules/lambda/` | Serverless processing |
| **Patterns** | `040_modules/unified-pattern-lambdas/` | Document workflow patterns |
| **Configuration** | `050_configs/` | Document types and rules |
| **Infrastructure** | `010_infra/` | CloudFormation templates |

## Key Design Principles

1. **Configuration-Driven** — Document types defined via JSON schemas
2. **Pattern-Based** — Reusable processing workflows
3. **Serverless-First** — AWS Lambda for compute
4. **Human-in-the-Loop** — Built-in manual review
5. **Extensible** — Custom patterns, extractors, and validators

## Learning Paths by Role

| Role | Start | Key Docs |
|------|-------|----------|
| **New Developer** | [005_GETTING_STARTED.md](005_GETTING_STARTED.md) | Setup, config, API |
| **DevOps/SRE** | [010_DEPLOYMENT_AND_INFRA_GUIDE.md](010_DEPLOYMENT_AND_INFRA_GUIDE.md) | Deployment, monitoring, RBAC |
| **Config Author** | [008_UNKNOWN_DOCUMENT_ONBOARDING.md](008_UNKNOWN_DOCUMENT_ONBOARDING.md) | Patterns, configs, validation |
| **Data Scientist** | [040_advanced/mlflow-integration.md](../040_advanced/mlflow-integration.md) | Models, fine-tuning, metrics |
| **Solutions Architect** | [010_core/architecture.md](../010_core/architecture.md) | Full design, capacity, operations |

---

**For detailed architecture:** See [../010_core/architecture.md](../010_core/architecture.md)  
**For physical folder layout:** See [004_FOLDER_MAPPING.md](004_FOLDER_MAPPING.md)
