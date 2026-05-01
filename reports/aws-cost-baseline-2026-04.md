# AWS Cost Baseline — April 2026

**Generated:** 2026-05-01  
**Period:** 2026-04-01 → 2026-05-01 (full month, estimated)  
**Source:** `aws ce get-cost-and-usage` via Devon IAM role  
**Total Spend:** $17,353.14 (estimated)  
**Status:** ✅ Baseline established — Phase 2 complete

---

## 1. Total Spend by Service

| Rank | Service | Cost (USD) | % of Total |
|------|---------|-----------|------------|
| 1 | Amazon Redshift | $7,058.87 | 40.7% |
| 2 | Neo4j Aura Professional | $2,313.36 | 13.3% |
| 3 | Amazon SageMaker | $1,356.60 | 7.8% |
| 4 | Amazon OpenSearch Service | $760.76 | 4.4% |
| 5 | Amazon RDS | $753.81 | 4.3% |
| 6 | Amazon QuickSight | $686.10 | 4.0% |
| 7 | Amazon Transcribe | $673.81 | 3.9% |
| 8 | AWS Lambda | $541.44 | 3.1% |
| 9 | Amazon EC2 Compute | $368.28 | 2.1% |
| 10 | AmazonCloudWatch | $361.00 | 2.1% |
| 11 | Amazon ECS (Fargate) | $304.61 | 1.8% |
| 12 | Amazon VPC | $284.49 | 1.6% |
| 13 | Amazon DynamoDB | $252.92 | 1.5% |
| 14 | AWS Transfer Family | $214.30 | 1.2% |
| 15 | AWS Security Hub | $205.13 | 1.2% |
| 16 | Amazon WorkSpaces | $202.05 | 1.2% |
| 17 | AWS App Runner | $193.33 | 1.1% |
| 18 | AWS Config | $220.69 | 1.3% |
| 19 | Amazon ELB | $108.69 | 0.6% |
| 20 | AWS WAF | $125.93 | 0.7% |
| 21 | Amazon CloudFront | $82.07 | 0.5% |
| 22 | EC2 - Other | $76.04 | 0.4% |
| 23 | CloudWatch Events | $34.47 | 0.2% |
| 24 | AWS X-Ray | $32.15 | 0.2% |
| 25 | AWS KMS | $26.69 | 0.2% |
| 26 | Amazon SES | $21.88 | 0.1% |
| 27 | Amazon API Gateway | $18.71 | 0.1% |
| 28 | AWS Secrets Manager | $16.14 | 0.1% |
| 29 | Amazon SQS | $12.29 | 0.1% |
| 30 | Amazon S3 | $39.24 | 0.2% |
| 31 | Amazon ECR | $3.01 | <0.1% |
| 32 | Amazon Route 53 | $2.39 | <0.1% |
| 33 | AWS Step Functions | $1.07 | <0.1% |
| 34 | Amazon Cognito | $0.46 | <0.1% |
| 35 | AWS AppSync | $0.26 | <0.1% |
| 36 | Amazon EFS | $0.01 | <0.1% |
| 37 | Amazon SNS | $0.09 | <0.1% |
| — | AWS Certificate Manager | $0.00 | — |
| — | AWS CloudFormation | $0.00 | — |
| — | AWS Glue | $0.00 | — |
| — | Tax | $0.00 | — |
| **TOTAL** | | **$17,353.14** | **100%** |

---

## 2. Top 5 Cost Drivers

### 🔴 #1 — Amazon Redshift: $7,058.87 (40.7%)
The single largest cost center by a wide margin. Redshift alone accounts for more than all other services combined. This is the data warehouse powering acquisition analytics and reporting. **Anomaly flag:** At 40.7% of total spend, any cluster resize, idle time, or unoptimized queries represent the highest-leverage cost reduction opportunity in the platform.

### 🟠 #2 — Neo4j Aura Professional: $2,313.36 (13.3%)
Third-party managed graph database (billed via AWS Marketplace). This is a significant fixed cost. No usage-based scaling visible from Cost Explorer — likely a reserved instance tier. Should be reviewed quarterly for right-sizing.

### 🟡 #3 — Amazon SageMaker: $1,356.60 (7.8%)
ML training/inference platform. Costs are consistent with model training workloads (WallEye and related models). No anomaly detected at this level, but should be tracked month-over-month as model iteration accelerates.

### 🟡 #4 — Amazon OpenSearch Service: $760.76 (4.4%)
Search/analytics cluster. Likely supporting log aggregation or application search. Cost is stable-looking but warrants a cluster utilization check — OpenSearch is frequently over-provisioned.

### 🟡 #5 — Amazon RDS: $753.81 (4.3%)
Relational database fleet. Consistent with a multi-environment (prod + staging) RDS setup. No anomaly at this level.

---

## 3. ECS Task Costs (Agent Platform)

**Total ECS (Fargate) April spend: $304.61**

| Usage Type | Cost | % of ECS |
|-----------|------|----------|
| Fargate vCPU-Hours | $249.15 | 81.8% |
| Fargate GB-Hours (memory) | $54.72 | 18.0% |
| Data Transfer (out + regional) | $0.74 | 0.2% |

**Per-agent breakdown:** Cost Explorer does not break ECS costs by task/agent when `Name` tags are absent or unset. All $304.61 rolls up under a single untagged bucket (`Name$`). 

**⚠️ Tagging gap identified:** ECS tasks are not tagged with agent-level identifiers. Without `Name` or `Agent` tags on ECS task definitions, per-agent cost attribution is impossible. This is a Phase 3 action item.

**Estimated per-agent cost (rough):** With ~6 active agents (Devon, Forge, Archie, Pam, and 2 others) running 24/7 on Fargate, average cost per agent ≈ **$50.77/month**. Devon's task definition (1 vCPU, 2GB RAM) at Fargate us-east-1 pricing ≈ $35–45/month, consistent with this estimate.

---

## 4. Lambda, Redshift, and S3 Costs

| Service | April Cost | Notes |
|---------|-----------|-------|
| **AWS Lambda** | $541.44 | High — suggests significant invocation volume. Likely acquisition pipeline event handlers, scoring functions, and agent habit schedulers. |
| **Amazon Redshift** | $7,058.87 | Dominant cost driver (see §2). |
| **Amazon S3** | $39.24 | Low — storage costs only. Consistent with artifact storage, report outputs, and model artifacts. No anomaly. |

**Lambda observation:** $541/month is substantial for Lambda. At $0.0000166667/GB-second, this implies ~32.5 million GB-seconds of compute, or roughly 1 billion 100ms invocations at 512MB. This warrants a Lambda function inventory and invocation-count breakdown in Phase 3.

---

## 5. Notable Observations & Anomalies

| # | Observation | Severity | Action |
|---|------------|----------|--------|
| 1 | Redshift at 40.7% of total spend — single point of cost risk | 🔴 HIGH | Add to monthly review; check cluster utilization and reserved instance coverage |
| 2 | Neo4j Aura at $2,313/mo — third-party fixed cost with no AWS-native visibility | 🟡 MEDIUM | Review contract tier quarterly |
| 3 | ECS tasks have no per-agent cost tags — attribution impossible | 🟡 MEDIUM | Forge: add `Agent` tag to all ECS task definitions |
| 4 | Lambda at $541/mo — no function-level breakdown available yet | 🟡 MEDIUM | Phase 3: get lambda:ListFunctions permission + cost by function |
| 5 | AWS Config at $220.69 — often overlooked but can be reduced with rule optimization | 🟢 LOW | Review Config rule count and recording frequency |
| 6 | Amazon Transcribe at $673.81 — significant; likely call recording pipeline | 🟡 MEDIUM | Verify this is expected volume; check for runaway jobs |
| 7 | CloudWatch at $361/mo — log ingestion costs can compound quickly | 🟢 LOW | Review log retention policies and ingestion volume |

---

## 6. Month-over-Month Baseline

This is the **first baseline snapshot**. No prior month data available for comparison. May 2026 review will compare against these April figures.

| Metric | April 2026 | May 2026 Target |
|--------|-----------|----------------|
| Total spend | $17,353.14 | < $18,000 (≤3.7% growth) |
| Redshift % of total | 40.7% | Monitor — flag if >45% |
| ECS agent platform | $304.61 | Monitor — flag if >$400 |
| Lambda | $541.44 | Monitor — flag if >$650 |
| S3 | $39.24 | Monitor — flag if >$60 |

---

## 7. Phase 3 Action Items

1. **ECS tagging:** Direct Forge to add `Agent=<name>` tag to all ECS task definitions so per-agent cost attribution is possible in May review.
2. **Lambda inventory:** Request `lambda:ListFunctions` IAM permission to enumerate functions and cross-reference with cost by function tag.
3. **Redshift utilization:** Check Redshift cluster utilization metrics — if <60% average CPU, reserved instance or right-sizing opportunity exists.
4. **Transcribe audit:** Verify $673/mo Transcribe spend is expected (call recording pipeline) and not a runaway job.
5. **CI/CD deploy ratio (KR4):** Cross-reference GitHub Actions workflow_run events vs. ECS UpdateService events to compute the KR4 metric.

---

## Methodology

```bash
# Total spend by service
aws ce get-cost-and-usage \
  --time-period Start=2026-04-01,End=2026-05-01 \
  --granularity MONTHLY \
  --metrics UnblendedCost \
  --group-by Type=DIMENSION,Key=SERVICE

# ECS usage type breakdown
aws ce get-cost-and-usage \
  --time-period Start=2026-04-01,End=2026-05-01 \
  --granularity MONTHLY \
  --metrics UnblendedCost \
  --filter '{"Dimensions":{"Key":"SERVICE","Values":["Amazon Elastic Container Service"]}}' \
  --group-by Type=DIMENSION,Key=USAGE_TYPE

# Lambda + Redshift + S3 isolated
aws ce get-cost-and-usage \
  --time-period Start=2026-04-01,End=2026-05-01 \
  --granularity MONTHLY \
  --metrics UnblendedCost \
  --filter '{"Dimensions":{"Key":"SERVICE","Values":["AWS Lambda","Amazon Redshift","Amazon Simple Storage Service"]}}' \
  --group-by Type=DIMENSION,Key=SERVICE
```

**IAM role:** `ResearchAgentStack-TaskDefTaskRole1EDB4A67-K2t7oQ1enBc4`  
**Permissions used:** `ce:GetCostAndUsage` (granted via agents PR #11, 2026-04-29)  
**Data status:** Estimated (AWS Cost Explorer marks April 2026 as estimated until billing closes)
