# Folder Mapping – Phase 1 Scaffold

Physical repository structure after Phase 1 reorganization.

## Root-Level Organization

```
refactored-idp-verification/
├── 001_docs/                    ← You are here (documentation)
├── 010_infra/                   ← Infrastructure & deployment
├── 020_shared/idp-packages/     ← Shared Python packages
├── 030_orchestration/           ← Orchestration & workflows
├── 040_modules/                 ← Lambda functions & patterns
├── 050_configs/                 ← Configuration library
├── 060_apps/web-ui/             ← Web UI application
├── 070_samples/                 ← Sample configurations & notebooks
├── 080_tests/                   ← Integration & end-to-end tests
├── 090_scripts/                 ← Utility scripts & maintenance
└── [root config files]          ← Makefile, package.json, etc.
```

---

## 001_docs/ – Documentation (This Folder)

```
001_docs/
├── 000_start-here/              ← 👈 Curated entry layer
│   ├── 001_README.md
│   ├── 002_REPO_READING_ORDER.md
│   ├── 003_TARGET_ARCHITECTURE.md
│   ├── 004_FOLDER_MAPPING.md    (you are here)
│   ├── 005_GETTING_STARTED.md
│   ├── 006_BUILD_AND_VALIDATION.md
│   ├── 007_DOCUMENT_TYPES_AND_CONFIGS.md
│   ├── 008_UNKNOWN_DOCUMENT_ONBOARDING.md
│   ├── 009_MANUAL_REVIEW_WORKFLOW.md
│   ├── 010_DEPLOYMENT_AND_INFRA_GUIDE.md
│   └── 011_PHASE_STATUS.md
├── 010_core/                    ← Core architecture & systems
│   ├── architecture.md
│   ├── deployment.md
│   ├── aws-services-and-roles.md
│   ├── monitoring.md
│   ├── troubleshooting.md
│   └── well-architected.md
├── 020_patterns/                ← Document patterns & workflows
│   ├── pattern-1.md
│   ├── pattern-2.md
│   ├── classification.md
│   ├── extraction.md
│   ├── assessment.md
│   ├── discovery.md
│   ├── human-review.md
│   ├── rule-validation.md
│   ├── configuration.md
│   ├── configuration-versions.md
│   ├── criteria-validation.md
│   └── evaluation.md
├── 030_apps-and-interfaces/     ← User interfaces & SDKs
│   ├── web-ui.md
│   ├── idp-cli.md
│   ├── idp-sdk.md
│   ├── agent-analysis.md
│   ├── agent-companion-chat.md
│   ├── mcp-server.md
│   ├── mcp-connector.md
│   └── custom-MCP-agent.md
├── 040_advanced/                ← Advanced topics & integrations
│   ├── few-shot-examples.md
│   ├── lambda-hook-inference.md
│   ├── post-processing-lambda-hook.md
│   ├── mlflow-integration.md
│   ├── nova-finetuning.md
│   ├── code-intelligence.md
│   ├── idp-configuration-best-practices.md
│   └── assessment-bounding-boxes.md
├── 050_operations/              ← Operational & infrastructure guides
│   ├── capacity-planning.md
│   ├── cost-calculator.md
│   ├── service-tiers.md
│   ├── rbac.md
│   ├── reporting-database.md
│   ├── knowledge-base.md
│   ├── govcloud-deployment.md
│   └── alb-hosting.md
├── 060_setup/                   ← Development environment setup
│   ├── setup-development-env-linux.md
│   ├── setup-development-env-macos.md
│   ├── setup-development-env-WSL.md
│   └── using-notebooks-with-idp-common.md
├── 070_reference/               ← API & technical reference
│   ├── idpcommon-api-reference.md
│   ├── json-schema-migration.md
│   ├── languages.md
│   ├── eu-region-model-support.md
│   ├── error-analyzer.md
│   ├── test-studio.md
│   └── ocr-image-sizing-guide.md
├── 090_history/                 ← Project history & metadata
│   ├── 001_CHANGELOG.md
│   ├── 002_CONTRIBUTING.md
│   ├── 003_LICENSE
│   ├── migration-v04-to-v05.md
│   ├── demo-videos.md
│   └── NOTICE.txt
├── images/                      ← Diagrams & images
├── threat-modeling/             ← Threat modeling documents
├── docs-site/                   ← Astro documentation site
└── PHASE_1_VALIDATION_REPORT.md ← Migration validation results
```

---

## 010_infra/ – Infrastructure & Deployment

```
010_infra/
├── template.yaml                ← Main CloudFormation template
├── nested/
│   ├── appsync/                 ← AppSync resolvers
│   ├── bedrockkb/               ← Bedrock Knowledge Base
│   ├── multi-doc-discovery/     ← Multi-doc discovery
│   └── alb-hosting/             ← ALB hosting
├── Dockerfile.optimized         ← Lambda container image
├── pyrightconfig.json           ← Type checking config
├── iam-roles/
│   └── cloudformation-management/
└── [other CloudFormation resources]
```

---

## 020_shared/idp-packages/ – Shared Python Libraries

```
020_shared/idp-packages/
├── idp_common_pkg/              ← Core IDP abstractions
│   ├── idp_common/              ← Python package
│   ├── tests/
│   ├── CHANGELOG.md
│   └── README.md
├── idp_cli_pkg/                 ← CLI implementation
│   ├── idp_cli/
│   ├── tests/
│   └── README.md
├── idp_sdk/                     ← Public SDK
│   ├── idp_sdk/
│   ├── tests/
│   └── README.md
└── idp_mcp_connector_pkg/       ← MCP connector
    ├── idp_mcp_connector/
    ├── tests/
    └── README.md
```

---

## 040_modules/ – Lambda Functions & Patterns

```
040_modules/
├── lambda/                      ← Individual Lambda functions
│   ├── agent_chat_processor/
│   ├── agent_chat_resolver/
│   ├── classification_function/
│   ├── copy_to_baseline_resolver/
│   ├── delete_tests/
│   ├── discovery_upload/ 
│   ├── extraction_function/
│   └── [60+ more functions]
└── unified-pattern-lambdas/     ← Pattern-based processors
    ├── mlflow_logger_function/
    ├── ocr_function/
    ├── classification_function/
    ├── extraction_function/
    └── [more pattern lambdas]
```

---

## 050_configs/ – Configuration Library

```
050_configs/
├── pricing.yaml                 ← Pricing configurations
├── README.md
├── TEMPLATE_README.md
├── test_config_library.py
├── managed_config/              ← Managed document types
│   ├── invoice/
│   ├── contract/
│   ├── license/
│   └── [more types]
└── unified/                     ← Unified pattern configs
    ├── few_shot_example/
    ├── rvl-cdip/
    └── [more patterns]
```

---

## 060_apps/web-ui/ – Web User Interface

```
060_apps/web-ui/
├── src/
│   ├── components/
│   ├── pages/
│   ├── services/
│   └── App.tsx
├── package.json
├── tsconfig.json
└── [build config]
```

---

## 070_samples/ – Example Configurations & Notebooks

```
070_samples/
├── notebooks/
│   ├── bda/
│   ├── examples/
│   ├── misc/
│   └── usecase-specific-examples/
├── [sample configs]
└── README.md
```

---

## 080_tests/ – Integration & E2E Tests

```
080_tests/
├── integration/
├── e2e/
└── [test configs]
```

---

## 090_scripts/ – Utility Scripts

```
090_scripts/
├── maintenance/
│   ├── sdlc/                    ← SDLC support scripts
│   ├── deployment/              ← Deployment helpers
│   └── [utility scripts]
└── [other scripts]
```

---

## Root-Level Important Files

| File | Purpose |
|------|---------|
| `Makefile` | Build targets and common tasks |
| `package.json` | Node.js dependencies (docs-site) |
| `template.yaml` | SAM template reference |
| `pyrightconfig.json` | Type checking config |
| `ruff.toml` | Linting rules |
| `README.md` | Root documentation (Phase 1 artifact) |
| `PHASE_1_VALIDATION_REPORT.md` | Migration results |
| `VERSION` | Release version |
| `NOTICE` | Legal notices |
| `LICENSE` | License file |

---

## File Location Quick Lookup

**Looking for something? Use these paths:**

| Item | Location |
|------|----------|
| Core SDK code | `020_shared/idp-packages/idp_common_pkg/idp_common/` |
| Lambda handlers | `040_modules/lambda/{function_name}/` |
| CloudFormation | `010_infra/template.yaml` |
| Web UI | `060_apps/web-ui/src/` |
| Config examples | `050_configs/managed_config/` or `050_configs/unified/` |
| CLI code | `020_shared/idp-packages/idp_cli_pkg/idp_cli/` |
| Tests | `020_shared/idp-packages/*/tests/` or `080_tests/` |
| Documentation | `001_docs/` |
| Setup scripts | `090_scripts/maintenance/` |

---

**Next:** [002_REPO_READING_ORDER.md](002_REPO_READING_ORDER.md) — Recommended learning paths  
**Or:** [005_GETTING_STARTED.md](005_GETTING_STARTED.md) — Get set up immediately
