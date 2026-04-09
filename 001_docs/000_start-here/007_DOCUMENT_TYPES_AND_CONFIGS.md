# Document Types & Configurations

Core abstraction: how documents are defined, classified, and processed.

## Core Concepts

An **IDP document type** is defined by:
1. **Schema** — what fields it contains (JSON Schema)
2. **Classification** — how to identify it (ML model + rules)
3. **Extraction** — how to pull out values (patterns + AI)
4. **Validation** — business rules it must satisfy
5. **Review** — how humans verify results

All configuration is **JSON Schema–based** and **version-controlled**.

---

## Configuration File Structure

Document types are defined in:
- `050_configs/managed_config/` — Pre-built types (invoice, contract, license, etc.)
- `050_configs/unified/` — Unified pattern configurations

### Example: Invoice Configuration

```json
{
  "name": "Invoice",
  "version": "1.0",
  "schema": {
    "type": "object",
    "properties": {
      "invoice_number": { "type": "string" },
      "date": { "type": "string", "format": "date" },
      "total_amount": { "type": "number" },
      "line_items": {
        "type": "array",
        "items": { "type": "object" }
      }
    },
    "required": ["invoice_number", "date", "total_amount"]
  },
  "classification": { /* how to identify */ },
  "extraction": { /* how to extract fields */ },
  "validation": { /* business rules */ }
}
```

**See:** [../020_patterns/configuration.md](../020_patterns/configuration.md)

---

## Processing Workflows

### Pattern 1: Classification-First

1. Receive document
2. Classify type
3. Extract from known pattern
4. Validate
5. Review if necessary

**See:** [../020_patterns/pattern-1.md](../020_patterns/pattern-1.md)

### Pattern 2: Template-Based

1. Match template
2. Extract fields
3. Validate
4. Review

**See:** [../020_patterns/pattern-2.md](../020_patterns/pattern-2.md)

---

## Configuration Components

| Component | Purpose | Reference |
|-----------|---------|-----------|
| **Schema** | Field definitions | [../070_reference/json-schema-migration.md](../070_reference/json-schema-migration.md) |
| **Classification** | Document type identification | [../020_patterns/classification.md](../020_patterns/classification.md) |
| **Extraction** | Value extraction patterns | [../020_patterns/extraction.md](../020_patterns/extraction.md) |
| **Validation** | Business rule enforcement | [../020_patterns/rule-validation.md](../020_patterns/rule-validation.md) |
| **Assessment** | Quality metrics | [../020_patterns/assessment.md](../020_patterns/assessment.md) |

---

## Versioning

Configurations are versioned for reproducibility.

```bash
# See version control
idp-cli config list-versions
```

**See:** [../020_patterns/configuration-versions.md](../020_patterns/configuration-versions.md)

---

## Validation & Criteria

Every extracted document is validated against:
- **Schema constraints** — field types, required fields
- **Business rules** — domain-specific validation
- **Quality criteria** — confidence thresholds

**See:** [../020_patterns/criteria-validation.md](../020_patterns/criteria-validation.md)

---

## Common Tasks

### Add a New Document Type

[008_UNKNOWN_DOCUMENT_ONBOARDING.md](008_UNKNOWN_DOCUMENT_ONBOARDING.md)

### Update an Extraction Pattern

[../020_patterns/extraction.md](../020_patterns/extraction.md)

### Define Validation Rules

[../020_patterns/rule-validation.md](../020_patterns/rule-validation.md)

### Add Few-Shot Examples

[../040_advanced/few-shot-examples.md](../040_advanced/few-shot-examples.md)

---

## Key Files

| File | Purpose |
|------|---------|
| `050_configs/managed_config/{type}/schema.json` | Field definitions |
| `050_configs/managed_config/{type}/classifier.json` | Classification rules |
| `050_configs/managed_config/{type}/extractors.json` | Extraction patterns |
| `050_configs/managed_config/{type}/validators.json` | Validation rules |

---

## Learning Path

1. **Understand configs** — This document
2. **See a pattern** — [../020_patterns/pattern-1.md](../020_patterns/pattern-1.md) or [../020_patterns/pattern-2.md](../020_patterns/pattern-2.md)
3. **Add your own** → [008_UNKNOWN_DOCUMENT_ONBOARDING.md](008_UNKNOWN_DOCUMENT_ONBOARDING.md)
4. **Validate results** → [006_BUILD_AND_VALIDATION.md](006_BUILD_AND_VALIDATION.md)

---

**See also:** [../070_reference/idpcommon-api-reference.md](../070_reference/idpcommon-api-reference.md)
