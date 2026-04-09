# Manual Review Workflow

Human-in-the-loop (HITL) process for validating extracted data.

## Overview

The manual review workflow enables humans to:
- Verify extraction accuracy
- Correct mistakes
- Provide feedback for model improvement
- Make final approval decisions

---

## Workflow Stages

### 1. Automatic Processing

1. Document received → Classification
2. Extraction → Validation
3. Confidence scoring
4. Route to review if needed

### 2. Submission to Review

Documents are submitted for manual review when:
- Confidence score below threshold
- Validation rule violations
- Ambiguous extraction results
- Explicit user request

### 3. Human Review

Reviewer uses Web UI to:
- View document and extracted fields
- Edit incorrect values
- Add notes or context
- Approve or reject extraction

### 4. Feedback & Storage

- Approved results → output
- Rejected results → reprocessing
- Corrections → model feedback

---

## Web UI Review Interface

**Location:** `060_apps/web-ui/`

### Review Dashboard

Shows:
- Pending review items
- Document preview
- Extracted fields with confidence scores
- Validation errors

### Edit Mode

Allows:
- Field value correction
- Adding missing data
- Removing false positives
- Adding reviewer notes

### Approval

- **Approve** — Accept extraction, proceed to output
- **Reject** — Return for reprocessing
- **Flag** — Mark for expert review

---

## Configuration

### Review Thresholds

Set in document configuration:

```json
{
  "review": {
    "confidence_threshold": 0.85,
    "require_review_for": ["critical_fields"],
    "review_timeout_hours": 24
  }
}
```

**See:** [../020_patterns/configuration.md](../020_patterns/configuration.md)

### Routing Rules

Define where documents go:

```json
{
  "routing": {
    "rules": [
      {
        "condition": "confidence < 0.80",
        "route": "human_review"
      },
      {
        "condition": "validation_errors > 0",
        "route": "human_review"
      },
      {
        "condition": "default",
        "route": "approve"
      }
    ]
  }
}
```

---

## Batch Processing

### Review a Batch

```bash
# Get pending items
idp review list --status pending

# Process batch
idp review batch 050_configs/managed_config/invoice/config.json \
  --input documents/ \
  --output results/
```

### Bulk Approval

```bash
# Approve all with high confidence
idp review approve --confidence-above 0.95 --batch-id BATCH-001
```

---

## Metrics & Feedback

### Quality Metrics

Track review outcomes:

```bash
idp review metrics --config 050_configs/managed_config/invoice/config.json
```

Shows:
- Approval rate
- Average corrections per document
- Most common errors
- Review time

### Feedback for Model Training

Reviewed data feeds back into:
- Model fine-tuning
- Extraction pattern improvement
- Validation rule refinement

**See:** [../040_advanced/mlflow-integration.md](../040_advanced/mlflow-integration.md)

---

## Access Control

### User Roles

| Role | Permissions |
|------|-------------|
| **Reviewer** | View documents, edit fields, approve/reject |
| **Approver** | Final approval, bulk operations |
| **Admin** | Configure workflows, access analytics |

**See:** [../050_operations/rbac.md](../050_operations/rbac.md)

### Document Access

- Reviewers see assigned documents only
- Audit logging for all actions
- Sensitive field masking (if configured)

---

## Best Practices

### 1. Set Appropriate Thresholds

- Too high → excessive manual review
- Too low → missing errors
- Start at 80-85% confidence

### 2. Batch by Document Type

Review invoices separately from contracts.

### 3. Provide Clear Instructions

Document field extraction rules in config:

```json
{
  "field_1": {
    "comment": "Extract the invoice date from top-right corner",
    "type": "date_pattern",
    "pattern": "Date: (\\d{1,2}/\\d{1,2}/\\d{4})"
  }
}
```

### 4. Monitor Review Quality

Track metrics to improve over time:

```bash
idp review quality-report --period last_7_days
```

### 5. Iterate on Rules

Use reviewer feedback to refine:
- Extraction patterns
- Validation rules
- Confidence thresholds

---

## API Integration

### List Pending Reviews

```python
from idp_common import ReviewClient

client = ReviewClient()
pending = client.list_pending(config_name="invoice")
```

### Process Review

```python
client.process_review(
    document_id="DOC-123",
    approved=True,
    corrections={"field_1": "corrected value"},
    notes="Verified against original"
)
```

**See:** [../070_reference/idpcommon-api-reference.md](../070_reference/idpcommon-api-reference.md)

---

## Troubleshooting

**Too many items in review?**
- Increase confidence threshold
- Tighten validation rules
- Improve extraction patterns

**Reviewers spending too long?**
- Add more context/instructions in config
- Pre-select likely correct values
- Batch similar documents together

**System not capturing corrections?**
- Check logging in Web UI
- Verify collector Lambda is running
- See [010_core/troubleshooting.md](../010_core/troubleshooting.md)

---

## Related Documentation

- **Web UI Details** — [../030_apps-and-interfaces/web-ui.md](../030_apps-and-interfaces/web-ui.md)
- **RBAC & Governance** — [../050_operations/rbac.md](../050_operations/rbac.md)
- **Assessment & Metrics** — [../020_patterns/assessment.md](../020_patterns/assessment.md)
- **Configuration** — [../020_patterns/configuration.md](../020_patterns/configuration.md)

---

**Next:** [010_DEPLOYMENT_AND_INFRA_GUIDE.md](010_DEPLOYMENT_AND_INFRA_GUIDE.md) to deploy the system  
**Or:** [../030_apps-and-interfaces/web-ui.md](../030_apps-and-interfaces/web-ui.md) for UI details
