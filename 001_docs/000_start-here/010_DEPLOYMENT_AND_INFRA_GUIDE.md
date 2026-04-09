# Deployment & Infrastructure Guide

End-to-end deployment to AWS and infrastructure architecture.

## Deployment Overview

IDP deploys to AWS using:
- **CloudFormation** for infrastructure
- **SAM (Serverless Application Model)** for Lambda functions
- **CDK** or **Terraform** for advanced scenarios

**Detailed architecture:** [../010_core/architecture.md](../010_core/architecture.md)  
**Detailed deployment:** [../010_core/deployment.md](../010_core/deployment.md)

---

## Quick Deployment (Sandbox)

### 1. Prerequisites

```bash
# AWS CLI configured
aws configure

# Verify credentials
aws sts get-caller-identity
```

### 2. Deploy Infrastructure

```bash
cd 010_infra
sam deploy --guided
```

During guided deployment, provide:
- Stack name
- AWS region
- S3 bucket for deployment
- Confirmation to create resources

### 3. Configure Document Types

```bash
# Upload your document configs
aws s3 cp 050_configs/ s3://my-bucket/config/ --recursive
```

### 4. Test

```bash
# Get API endpoint from CloudFormation outputs
aws cloudformation describe-stacks \
  --stack-name my-idp-stack \
  --query 'Stacks[0].Outputs'

# Test extraction
curl https://<api-endpoint>/extract \
  -X POST \
  -F "document=@sample.pdf" \
  -F "config=invoice"
```

---

## Infrastructure Components

### Core Services

| Service | Purpose | Location |
|---------|---------|----------|
| **Lambda** | Serverless compute | `040_modules/lambda/` |
| **API Gateway** | REST API | CloudFormation |
| **DynamoDB** | State/results storage | CloudFormation |
| **S3** | Document storage | CloudFormation |
| **SNS/SQS** | Event routing | CloudFormation |
| **AppSync** | GraphQL API | `010_infra/nested/appsync/` |
| **CloudWatch** | Logging & monitoring | CloudFormation |

### Optional Services

- **Bedrock** — AI model inference
- **Textract** — OCR & layout analysis
- **Step Functions** — Complex workflows
- **EventBridge** — Event processing

---

## Configuration

### Environment Variables

Create `.env.production` (or set in CloudFormation parameters):

```bash
# AWS
AWS_REGION=us-east-1
AWS_ACCOUNT_ID=123456789

# IDP
IDP_CONFIG_BUCKET=my-config-bucket
IDP_DOCUMENT_BUCKET=my-documents-bucket
IDP_CONFIDENCE_THRESHOLD=0.85

# AI Models  
BEDROCK_MODEL_ID=anthropic.claude-3-sonnet
BEDROCK_REGION=us-east-1

# Monitoring
LOG_LEVEL=INFO
```

### CloudFormation Parameters

Edit before deployment:

```bash
sam deploy --parameter-overrides \
  ConfigBucket=my-config-bucket \
  DocumentBucket=my-documents-bucket
```

---

## Deployment Variants

### For Production

```bash
sam deploy \
  --stack-name idp-prod \
  --s3-bucket artifacts-prod \
  --capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM \
  --no-fail-on-empty-changeset
```

**See:** [../050_operations/service-tiers.md](../050_operations/service-tiers.md)

### For GovCloud

```bash
sam deploy \
  --stack-name idp-govcloud \
  --region us-gov-west-1 \
  --profile govcloud
```

**See:** [../050_operations/govcloud-deployment.md](../050_operations/govcloud-deployment.md)

### For Multi-Region

Deploy to multiple regions:

```bash
for region in us-east-1 us-west-2 eu-west-1; do
  sam deploy --region $region --stack-name idp-$region
done
```

---

## Operational Tasks

### Monitoring

```bash
# View logs
aws logs tail /aws/lambda/idp-processor --follow

# Get metrics
aws cloudwatch get-metric-statistics \
  --namespace AWS/Lambda \
  --metric-name Duration \
  --start-time 2024-01-01T00:00:00Z \
  --end-time 2024-01-02T00:00:00Z \
  --period 3600
```

**See:** [../010_core/monitoring.md](../010_core/monitoring.md)

### Capacity Planning

```bash
# Check Lambda concurrency
aws lambda get-account-settings

# Monitor DynamoDB usage
aws cloudwatch get-metric-statistics \
  --namespace AWS/DynamoDB \
  --metric-name ConsumedWriteCapacityUnits \
  --dimensions Name=TableName,Value=idp-results
```

**See:** [../050_operations/capacity-planning.md](../050_operations/capacity-planning.md)

### Cost Analysis

```bash
# Estimate costs
python scripts/cost_calculator.py --region us-east-1
```

**See:** [../050_operations/cost-calculator.md](../050_operations/cost-calculator.md)

### RBAC & Access Control

```bash
# Update IAM roles
aws iam update-role-policy \
  --role-name idp-lambda-role \
  --policy-name idp-permissions \
  --policy-document file://policy.json
```

**See:** [../050_operations/rbac.md](../050_operations/rbac.md)

---

## Scaling & Performance

### Auto-Scaling

Lambda automatically scales. Configure through CloudFormation:

```yaml
Resources:
  ProcessorFunction:
    Type: AWS::Lambda::Function
    Properties:
      MemorySize: 3008  # Max available
      Timeout: 900      # 15 minutes
      ReservedConcurrentExecutions: 1000
```

### Batch Processing

Process documents in parallel:

```bash
idp process batch \
  --input s3://bucket/documents/ \
  --output s3://bucket/results/ \
  --parallel-workers 10 \
  --config 050_configs/managed_config/invoice/config.json
```

### Cost Optimization

**See:** [../050_operations/cost-calculator.md](../050_operations/cost-calculator.md)

---

## Troubleshooting

### Deployment Fails

```bash
# Check IAM permissions
aws iam get-user

# Validate CloudFormation template
aws cloudformation validate-template --template-body file://template.yaml

# See detailed errors
sam deploy --debug
```

### Lambda Timeout

Increase timeout in CloudFormation:

```yaml
Timeout: 300  # Seconds
```

### High Costs

1. Check CloudWatch logs for errors
2. Optimize Lambda memory/timeout
3. Review DynamoDB capacity
4. Use CloudFormation cost calculator

**See:** [010_core/troubleshooting.md](../010_core/troubleshooting.md)

---

## Advanced Deployment

### Custom VPC

Deploy Lambda in VPC for database access:

```yaml
VpcConfig:
  SecurityGroupIds:
    - sg-12345
  SubnetIds:
    - subnet-12345
    - subnet-67890
```

### Layer-Based Architecture

Package shared code as Lambda layers:

```bash
# Package layer
pip install -r requirements.txt -t python/
zip -r layer.zip python/
aws lambda publish-layer-version --layer-name idp-common
```

### Container-Based Lambdas

Deploy as Docker images:

```bash
docker build -f 010_infra/Dockerfile.optimized -t idp-lambda .
aws ecr create-repository --repository-name idp-lambda
docker push <account>.dkr.ecr.<region>.amazonaws.com/idp-lambda
```

---

## CI/CD Integration

### GitHub Actions Deployment

See `.github/workflows/` for automated deployment.

### GitLab CI/CD

See `.gitlab-ci.yml` for pipeline configuration.

---

## Backup & Disaster Recovery

### Automated Backups

DynamoDB Point-in-Time Recovery enabled automatically.

### Manual Backup

```bash
aws dynamodb create-backup \
  --table-name idp-results \
  --backup-name idp-backup-$(date +%s)
```

### Recovery Procedure

**See:** [../010_core/deployment.md](../010_core/deployment.md)

---

## Related Documentation

- **Architecture Details** — [../010_core/architecture.md](../010_core/architecture.md)
- **Infrastructure Code** — [../010_core/deployment.md](../010_core/deployment.md)
- **AWS Services** — [../010_core/aws-services-and-roles.md](../010_core/aws-services-and-roles.md)
- **ALB Hosting** — [../050_operations/alb-hosting.md](../050_operations/alb-hosting.md)
- **GovCloud** — [../050_operations/govcloud-deployment.md](../050_operations/govcloud-deployment.md)
- **RBAC** — [../050_operations/rbac.md](../050_operations/rbac.md)
- **Capacity Planning** — [../050_operations/capacity-planning.md](../050_operations/capacity-planning.md)

---

**Troubleshooting?** See [../010_core/troubleshooting.md](../010_core/troubleshooting.md)
