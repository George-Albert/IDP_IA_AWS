# 050_configs Structure

## Purpose
Configuration files for document verification workflows, including document type schemas, metadata capture forms, and processor configurations.

## Directory Structure

### `/document-types`
Document-specific configurations and schemas.

#### `/document-types/known/_template`
Template files for creating known document type configurations:
- **draft_schema.template.json**: JSON Schema for field validation. Copy and customize for each known document type.
- **draft_form.template.json**: Manual review form template for known types.

##### Adding a Known Document Type
1. Create a new directory: `known/your-document-type/`
2. Copy templates and customize:
   - `schema.json` - Finalized extraction schema
   - `form.json` - Manual review form definition
   - `metadata.yaml` (optional) - Type metadata
   - `examples.md` (optional) - Reference examples

#### `/document-types/unknown`
Processing pipeline for documents of unknown/unclassified types:
- **onboarding_form.template.json**: Capture form for new document types entering the system
- **classification_rules.yaml** (to be created): Rules for initial classification attempts
- **pipeline_config.yaml** (to be created): Workflow orchestration for unknown type onboarding

### `/workflows`
Workflow definitions for document processing stages.

#### Workflow Processors
Planned/implemented processors (stub files in `040_modules/`):

1. **schema-builder** - (040_modules/schema_builder/)
   - Auto-generates schemas from document samples
   - Validates completeness and field coverage
   
2. **manual-review-processor** - (040_modules/manual_review_processor/)
   - Handles human-in-the-loop review
   - Integrates with step functions for approval workflows
   
3. **metadata-capture** - (040_modules/metadata_capture/)
   - Extracts and validates document metadata
   - Applies consistent metadata standards

4. **new-type-onboarding** - (040_modules/new_type_onboarding/)
   - Orchestrates initial classification of unknown types
   - Manages feedback loops back to schema-builder and manual review

## Configuration Format Guidelines

All schemas should:
- Use JSON Schema Draft 7 or OpenAPI 3.0 format
- Include descriptions for all fields
- Define validation constraints (required fields, min/max, patterns)
- Reference templates for consistency
- Be versioned in git with meaningful commit messages

## Example Workflow

### Known Document Type (e.g., "pay-statement")
```
Document arrives
  ↓
Classification model identifies as "pay-statement"
  ↓
Load schema from 050_configs/document-types/known/pay-statement/schema.json
  ↓
Extract and validate fields
  ↓
Confidence check:
  - High confidence → Auto-approve
  - Low confidence → Send to manual review (use draft_form.template.json)
```

### Unknown Document Type
```
Document arrives
  ↓
Classification model returns UNKNOWN or low confidence across all types
  ↓
Route to onboarding workflow
  ↓
Capture metadata using onboarding_form.template.json
  ↓
Analyst reviews → decides if:
  - New document type to onboard → schema-builder workflow
  - Misclassified → retry classification
  - Invalid → reject
  ↓
If onboarding: Schema builder generates draft schema
  ↓
Human review/approval → activate new type
```

## Related Directories

- **030_orchestration/**: Step Functions state machines that orchestrate these workflows
- **040_modules/**: Lambda functions and processors referenced in configs
- **050_configs/config-library/**: Legacy configuration patterns (being migrated to this structure)
