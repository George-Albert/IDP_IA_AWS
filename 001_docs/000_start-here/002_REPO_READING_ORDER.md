# Recommended Reading Order

Choose your learning path based on your goals.

---

## 🎯 Path 1: Complete System Understanding (Full Onboarding)

**Duration:** 2-3 hours  
**For:** New team members, architects, comprehensive system knowledge

1. **Foundation**
   - [Target Architecture](003_TARGET_ARCHITECTURE.md)
   - [Folder Mapping](004_FOLDER_MAPPING.md) — understand the physical organization

2. **Core Concepts**
   - [010_core/architecture.md](../010_core/architecture.md) — detailed system design
   - [007_DOCUMENT_TYPES_AND_CONFIGS.md](007_DOCUMENT_TYPES_AND_CONFIGS.md) — core abstraction
   - [010_core/aws-services-and-roles.md](../010_core/aws-services-and-roles.md) — AWS infrastructure

3. **Document Processing**
   - [020_patterns/classification.md](../020_patterns/classification.md)
   - [020_patterns/extraction.md](../020_patterns/extraction.md)
   - [020_patterns/assessment.md](../020_patterns/assessment.md)
   - [020_patterns/discovery.md](../020_patterns/discovery.md)
   - [020_patterns/human-review.md](../020_patterns/human-review.md)

4. **Interfaces & Integration**
   - [030_apps-and-interfaces/idp-cli.md](../030_apps-and-interfaces/idp-cli.md) — interact with IDP
   - [030_apps-and-interfaces/web-ui.md](../030_apps-and-interfaces/web-ui.md) — user interface
   - [030_apps-and-interfaces/idp-sdk.md](../030_apps-and-interfaces/idp-sdk.md) — programmatic integration

5. **Deployment & Operation**
   - [010_DEPLOYMENT_AND_INFRA_GUIDE.md](010_DEPLOYMENT_AND_INFRA_GUIDE.md)
   - [010_core/deployment.md](../010_core/deployment.md)
   - [050_operations/capacity-planning.md](../050_operations/capacity-planning.md)
   - [050_operations/rbac.md](../050_operations/rbac.md)

6. **Advanced Topics**
   - [040_advanced/idp-configuration-best-practices.md](../040_advanced/idp-configuration-best-practices.md)
   - [040_advanced/mlflow-integration.md](../040_advanced/mlflow-integration.md)
   - [040_advanced/lambda-hook-inference.md](../040_advanced/lambda-hook-inference.md)

---

## 🚀 Path 2: Document Verification Scaffold Only (Dev-Focused)

**Duration:** 45 minutes  
**For:** Developers implementing the document verification system without deploying full IDP

1. **Quick Context**
   - [Target Architecture](003_TARGET_ARCHITECTURE.md)
   - [Folder Mapping](004_FOLDER_MAPPING.md)

2. **Scaffold Essentials**
   - [004_FOLDER_MAPPING.md](004_FOLDER_MAPPING.md) — find everything
   - [007_DOCUMENT_TYPES_AND_CONFIGS.md](007_DOCUMENT_TYPES_AND_CONFIGS.md) — core data model
   - [009_MANUAL_REVIEW_WORKFLOW.md](009_MANUAL_REVIEW_WORKFLOW.md) — HITL flow

3. **Get Running**
   - [005_GETTING_STARTED.md](005_GETTING_STARTED.md)
   - [060_setup/setup-development-env-linux.md](../060_setup/setup-development-env-linux.md) (or macOS/WSL)
   - [006_BUILD_AND_VALIDATION.md](006_BUILD_AND_VALIDATION.md)

4. **Deep Dive on Configs**
   - [020_patterns/configuration.md](../020_patterns/configuration.md)
   - [020_patterns/configuration-versions.md](../020_patterns/configuration-versions.md)
   - [020_patterns/rule-validation.md](../020_patterns/rule-validation.md)

5. **Reference**
   - [070_reference/idpcommon-api-reference.md](../070_reference/idpcommon-api-reference.md)
   - [070_reference/json-schema-migration.md](../070_reference/json-schema-migration.md)

6. **If You Get Stuck**
   - [010_core/troubleshooting.md](../010_core/troubleshooting.md)

---

## 📋 Path 3: Unknown Document Type Onboarding (Config Author)

**Duration:** 1 hour  
**For:** Configuration authors adding support for new document types

1. **Understand the System**
   - [007_DOCUMENT_TYPES_AND_CONFIGS.md](007_DOCUMENT_TYPES_AND_CONFIGS.md)
   - [008_UNKNOWN_DOCUMENT_ONBOARDING.md](008_UNKNOWN_DOCUMENT_ONBOARDING.md)

2. **Learn by Example**
   - [020_patterns/pattern-1.md](../020_patterns/pattern-1.md)
   - [020_patterns/pattern-2.md](../020_patterns/pattern-2.md)
   - [020_patterns/classification.md](../020_patterns/classification.md)

3. **Configure Your Document Type**
   - [020_patterns/configuration.md](../020_patterns/configuration.md)
   - [020_patterns/rule-validation.md](../020_patterns/rule-validation.md)
   - [020_patterns/criteria-validation.md](../020_patterns/criteria-validation.md)

4. **Validate & Test**
   - [006_BUILD_AND_VALIDATION.md](006_BUILD_AND_VALIDATION.md)
   - [070_reference/idpcommon-api-reference.md](../070_reference/idpcommon-api-reference.md)

5. **Reference Material**
   - [040_advanced/few-shot-examples.md](../040_advanced/few-shot-examples.md)
   - [070_reference/test-studio.md](../070_reference/test-studio.md)

---

## 🔬 Path 4: Advanced Integration & Optimization (Expert)

**Duration:** 2+ hours  
**For:** Architects designing custom integrations and optimizations

**Prerequisites:** Complete Path 1

1. **Architecture Deep Dives**
   - [010_core/well-architected.md](../010_core/well-architected.md)
   - [010_core/monitoring.md](../010_core/monitoring.md)
   - [050_operations/capacity-planning.md](../050_operations/capacity-planning.md)

2. **Advanced Patterns**
   - [040_advanced/mlflow-integration.md](../040_advanced/mlflow-integration.md)
   - [040_advanced/nova-finetuning.md](../040_advanced/nova-finetuning.md)
   - [040_advanced/lambda-hook-inference.md](../040_advanced/lambda-hook-inference.md)
   - [040_advanced/post-processing-lambda-hook.md](../040_advanced/post-processing-lambda-hook.md)

3. **Governance & Operations**
   - [050_operations/rbac.md](../050_operations/rbac.md)
   - [050_operations/service-tiers.md](../050_operations/service-tiers.md)
   - [050_operations/govcloud-deployment.md](../050_operations/govcloud-deployment.md)
   - [050_operations/alb-hosting.md](../050_operations/alb-hosting.md)

4. **AI/ML Customization**
   - [020_patterns/classification.md](../020_patterns/classification.md) (advanced section)
   - [040_advanced/code-intelligence.md](../040_advanced/code-intelligence.md)
   - [070_reference/languages.md](../070_reference/languages.md)

5. **Systems Integration**
   - [030_apps-and-interfaces/mcp-connector.md](../030_apps-and-interfaces/mcp-connector.md)
   - [030_apps-and-interfaces/custom-MCP-agent.md](../030_apps-and-interfaces/custom-MCP-agent.md)
   - [030_apps-and-interfaces/agent-analysis.md](../030_apps-and-interfaces/agent-analysis.md)

---

## Quick Reference Index

| Goal | Documents |
|------|-----------|
| **Set up environment** | [005_GETTING_STARTED.md](005_GETTING_STARTED.md), [060_setup/](../060_setup/) |
| **Understand config format** | [007_DOCUMENT_TYPES_AND_CONFIGS.md](007_DOCUMENT_TYPES_AND_CONFIGS.md), [020_patterns/configuration.md](../020_patterns/configuration.md) |
| **Add a new document type** | [008_UNKNOWN_DOCUMENT_ONBOARDING.md](008_UNKNOWN_DOCUMENT_ONBOARDING.md) |
| **Deploy to AWS** | [010_DEPLOYMENT_AND_INFRA_GUIDE.md](010_DEPLOYMENT_AND_INFRA_GUIDE.md) |
| **Learn the codebase** | [004_FOLDER_MAPPING.md](004_FOLDER_MAPPING.md), [030_apps-and-interfaces/idp-sdk.md](../030_apps-and-interfaces/idp-sdk.md) |
| **Troubleshoot issues** | [010_core/troubleshooting.md](../010_core/troubleshooting.md) |
| **Optimize performance** | [050_operations/capacity-planning.md](../050_operations/capacity-planning.md), [040_advanced/](../040_advanced/) |
| **Understand API** | [070_reference/idpcommon-api-reference.md](../070_reference/idpcommon-api-reference.md) |

---

**Pro Tip:** Bookmark this page and return as your role evolves. Different paths serve different needs.
