## IAM Deploy Gap Runbook -- 2026-04-29

## Situation

PR #11 (`forge/devon-iam-cost-explorer-secrets`) was merged to main on 2026-04-29T06:08:30Z but the CDK stack has not been deployed. All three new permission sets return `AccessDeniedException` live.

## Affected WorkItems

| WorkItem | Title | Status |
|---|---|---|
| ef52a6d3 | Grant Devon IAM permissions for secrets hygiene audit | 🔝 Blocked |
| f36a96bc | Add ce:GetCostAndUsage to Devon's IAM role | 🔝 Blocked |

## Permissions In Code (Not Yet Live)

```
SecretsManagerReadOnly: ListSecrets, DescribeSecret, GetSecretValue
EcsReadOnly: ListClusters, ListServices, ListTaskDefinitions, DescribeTaskDefinition
CostExplorerReadOnly: GetCostAndUsage, GetCostForecast, GetDimensionValues, GetUsageForecast
```

## Why Devon Cannot Self-Deploy

Devon's ECS task role (`ResearchAgentStack-TaskDefTaskRole1EDB4A67-K2t7oQ1enBc4`) cannot assume the CDK deploy role (`cdk-hnb659fds-deploy-role-991348730856-us-east-1`). IAM security boundaries prevent self-modification.

## Deploy Command (Human Action Required)

```bash
cd /path/to/agents/cdk
npm install
CDK_DEFAULT_ACCOUNT=991348730856 CDK_DEFAULT_REGION=us-east-1 \
  ./node_modules/.bin/cdk deploy ResearchAgentStack \
  --require-approval never
```

## Verification (After Deploy)

```bash
aws secretsmanager list-secrets --region us-east-1
aws ecs list-task-definitions --region us-east-1
aws ce get-cost-and-usage \
  --time-period Start=2026-04-01,End=2026-04-29 \
  --granularity MONTHLY --metrics BlendedCost --region us-east-1
```

All three should return JSON results (not AccessDeniedException).

## PR Reference

https://github.com/MMRRFinance/agents/pull/11