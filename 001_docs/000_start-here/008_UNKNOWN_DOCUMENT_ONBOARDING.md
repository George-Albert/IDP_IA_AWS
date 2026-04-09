# Unknown Document Onboarding

How to add support for a new document type to IDP.

## Overview

Adding a new document type involves:
1. **Define the schema** — what fields the document contains
2. **Set up classification** — how to identify it
3. **Create extractors** — how to pull values
4. **Add validation** — business rules
5. **Validate & test** — ensure it works

---

## Step 1: Define the Schema

Create a JSON Schema describing your document's fields.

```json
{
  "name": "my-document",
  "version": "1.0",
  "schema": {
    "type": "object",
    "properties": {
      "field_1": { "type": "string" },
      "field_2": { "type": "number" },
      "field_3": { "type": "date" }
    },
    "required": ["field_1"]
  }
}
```

**Reference:** [../070_reference/json-schema-migration.md](../070_reference/json-schema-migration.md)

---

## Step 2: Set Up Classification

Define how to identify your document type.

### Option A: Rule-Based

```json
{
  "classification": {
    "method": "rules",
    "rules": [
      {
        "name": "keyword_match",
        "keywords": ["invoice", "bill number"],
        "weight": 0.8
      }
    ]
  }
}
```

### Option B: ML-Based

```json
{
  "classification": {
    "method": "ml",
    "model_id": "classification-model-v1"
  }
}
```

**See:** [../020_patterns/classification.md](../020_patterns/classification.md)

---

## Step 3: Create Extractors

Define extraction patterns for each field.

```json
{
  "extraction": {
    "field_1": {
      "type": "pattern",
      "pattern": "^field 1: (.+)$"
    },
    "field_2": {
      "type": "ai",
      "prompt": "Extract the amount..."
    },
    "field_3": {
      "type": "structured",
      "path": "$.date"
    }
  }
}
```

**See:** [../020_patterns/extraction.md](../020_patterns/extraction.md)

---

## Step 4: Add Validation Rules

Define business rules your documents must satisfy.

```json
{
  "validation": {
    "rules": [
      {
        "name": "field_required",
        "field": "field_1",
        "type": "required"
      },
      {
        "name": "amount_positive",
        "field": "field_2",
        "type": "numeric",
        "constraints": { "min": 0 }
      }
    ]
  }
}
```

**See:** [../020_patterns/rule-validation.md](../020_patterns/rule-validation.md)

---

## Step 5: Implement & Test

### Create Configuration File

```bash
mkdir -p 050_configs/managed_config/my-document
cat > 050_configs/managed_config/my-document/config.json << EOF
{
  "name": "my-document",
  "version": "1.0",
  "schema": { ... },
  "classification": { ... },
  "extraction": { ... },
  "validation": { ... }
}
EOF
```

### Test with CLI

```bash
# Test classification
idp classify 050_configs/managed_config/my-document/config.json my-sample-doc.pdf

# Test extraction
idp extract 050_configs/managed_config/my-document/config.json my-sample-doc.pdf

# Test validation
idp validate 050_configs/managed_config/my-document/config.json --data '{"field_1": "test"}'
```

### Unit Test

```bash
# Add test in 020_shared/idp-packages/idp_common_pkg/tests/unit/
pytest tests/unit/test_my_document.py -v
```

---

## Best Practices

### 1. Start with Examples

Begin with existing document types in `050_configs/managed_config/` and adapt.

**Examples:**
- Invoice: `050_configs/managed_config/invoice/`
- Contract: `050_configs/managed_config/contract/`

### 2. Use Few-Shot Examples

Provide examples for AI-based extraction:

```json
{
  "extraction": {
    "field_1": {
      "type": "ai",
      "examples": [
        { "text": "...", "value": "..." },
        { "text": "...", "value": "..." }
      ]
    }
  }
}
```

**See:** [../040_advanced/few-shot-examples.md](../040_advanced/few-shot-examples.md)

### 3. Validate Early

Test your schema and rules during development:

```bash
make test-unit
```

### 4. Version Your Config

Use semantic versioning for reproducibility:

```json
{ "version": "1.0", "...": "..." }
```

**See:** [../020_patterns/configuration-versions.md](../020_patterns/configuration-versions.md)

### 5. Document Your Config

Add comments explaining extraction logic:

```json
{
  "extraction": {
    "field_1": {
      "comment": "Extract from OCR region [100,200,300,400]",
      "type": "pattern",
      "pattern": "..."
    }
  }
}
```

---

## Common Patterns

### Extracting from Specific Layout Regions

Use bounding box coordinates:

```json
{
  "extraction": {
    "field_1": {
      "type": "region",
      "bounds": { "x0": 100, "y0": 200, "x1": 300, "y1": 400 }
    }
  }
}
```

**See:** [../040_advanced/assessment-bounding-boxes.md](../040_advanced/assessment-bounding-boxes.md)

### Handling Tables

Extract tabular data:

```json
{
  "extraction": {
    "line_items": {
      "type": "table",
      "columns": ["item", "qty", "price"],
      "rows": [...]
    }
  }
}
```

### Multi-Language Support

Support multiple languages:

```json
{
  "extraction": {
    "field_1": {
      "languages": ["en", "es", "fr"],
      "pattern": "..."
    }
  }
}
```

**See:** [../070_reference/languages.md](../070_reference/languages.md)

---

## Integration with Workflows

Once your config is ready, it integrates with:
- **Pattern 1** — Classification-first extraction
- **Pattern 2** — Template-based processing
- **Manual Review** — Human validation before output

**See:** [../020_patterns/pattern-1.md](../020_patterns/pattern-1.md), [../020_patterns/pattern-2.md](../020_patterns/pattern-2.md)

---

## Troubleshooting

**Extraction not working?**
- Check pattern syntax
- Verify region coordinates in sample documents
- Test with `idp extract ... --debug`

**Classification wrong?**
- Add more keywords or training examples
- Adjust confidence thresholds in `config.json`

**Validation too strict?**
- Adjust constraints in validation rules
- Allow optional fields (remove from `required`)

---

## Next Steps

1. **Review examples** — [../020_patterns/pattern-1.md](../020_patterns/pattern-1.md) and [../020_patterns/pattern-2.md](../020_patterns/pattern-2.md)
2. **Set up your config** — Follow steps above
3. **Test thoroughly** — [006_BUILD_AND_VALIDATION.md](006_BUILD_AND_VALIDATION.md)
4. **Deploy** → [010_DEPLOYMENT_AND_INFRA_GUIDE.md](010_DEPLOYMENT_AND_INFRA_GUIDE.md)

---

**API Reference:** [../070_reference/idpcommon-api-reference.md](../070_reference/idpcommon-api-reference.md)  
**Configuration Details:** [../020_patterns/configuration.md](../020_patterns/configuration.md)
