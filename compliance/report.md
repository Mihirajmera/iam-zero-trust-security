# IaC compliance evidence: iam-zero-trust-security

Checkov 3.3.22 scanned the Terraform on 2026-10-21. Scan output SHA-256: `9e83e65605c1b5e9…`.

| Passed | Failed, open | Failed, exception | Unique checks |
|---:|---:|---:|---:|
| 142 | **0** | 33 | 51 |

**Gate:** CI fails if any failed check lacks a reviewed, unexpired exception.

## Exceptions

Reviewed 2026-10-21 by Mihir Ajmera. Each one is a decision with a reason and an expiry, not a suppression.

| Check | Resource | Decision | Reason | Expires | NIST 800-53 |
|---|---|---|---|---|---|
| CKV2_AWS_14 Ensure that IAM groups includes at least one IAM user | `aws_iam_group_membership.admin_members` | not-applicable | False positive. Every group has members through aws_iam_group_membership built with a for expression, which Checkov's graph check doesn't resolve (see iam-groups.tf). | 2027-04-21 | AC-2 |
| CKV2_AWS_14 Ensure that IAM groups includes at least one IAM user | `aws_iam_group_membership.developer_members` | not-applicable | False positive. Every group has members through aws_iam_group_membership built with a for expression, which Checkov's graph check doesn't resolve (see iam-groups.tf). | 2027-04-21 | AC-2 |
| CKV2_AWS_14 Ensure that IAM groups includes at least one IAM user | `aws_iam_group_membership.readonly_members` | not-applicable | False positive. Every group has members through aws_iam_group_membership built with a for expression, which Checkov's graph check doesn't resolve (see iam-groups.tf). | 2027-04-21 | AC-2 |
| CKV2_AWS_21 Ensure that all IAM users are members of at least one IAM group. | `aws_iam_group_membership.admin_members` | not-applicable | False positive. Every user is a member of exactly one group through aws_iam_group_membership (see iam-groups.tf). | 2027-04-21 | AC-2 |
| CKV2_AWS_21 Ensure that all IAM users are members of at least one IAM group. | `aws_iam_group_membership.developer_members` | not-applicable | False positive. Every user is a member of exactly one group through aws_iam_group_membership (see iam-groups.tf). | 2027-04-21 | AC-2 |
| CKV2_AWS_21 Ensure that all IAM users are members of at least one IAM group. | `aws_iam_group_membership.readonly_members` | not-applicable | False positive. Every user is a member of exactly one group through aws_iam_group_membership (see iam-groups.tf). | 2027-04-21 | AC-2 |
| CKV2_AWS_40 Ensure AWS IAM policy does not allow full IAM privileges | `aws_iam_policy_document.admin_policy` | risk-accepted | Same break-glass policy as CKV_AWS_1 (MFA and source-IP conditions). | 2027-04-21 | AC-6 |
| CKV2_AWS_61 Ensure that an S3 bucket has a lifecycle configuration | `aws_s3_bucket.config_bucket` | deferred | Add a lifecycle rule once the evidence retention period is agreed. | 2027-01-31 | SI-12 |
| CKV2_AWS_62 Ensure S3 buckets should have event notifications enabled | `aws_s3_bucket.config_bucket` | not-applicable | No downstream consumer needs bucket event notifications; compliance changes already flow through EventBridge. | 2027-04-21 | SI-4 |
| CKV_AWS_1 Ensure IAM policies that allow full "*-*" administrative privileges are not created | `aws_iam_policy_document.admin_policy` | risk-accepted | Break-glass administrator policy. Usable only with MFA and only from the corporate IP range, attached only to the admins group. | 2027-04-21 | AC-6 |
| CKV_AWS_115 Ensure that AWS Lambda function is configured for function-level concurrent execution limit | `aws_lambda_function.s3_remediation` | not-applicable | Functions are triggered by low-volume AWS Config events; account concurrency limits are sufficient. | 2027-04-21 | SC-5 |
| CKV_AWS_115 Ensure that AWS Lambda function is configured for function-level concurrent execution limit | `aws_lambda_function.sns_notifier` | not-applicable | Functions are triggered by low-volume AWS Config events; account concurrency limits are sufficient. | 2027-04-21 | SC-5 |
| CKV_AWS_116 Ensure that AWS Lambda function is configured for a Dead Letter Queue(DLQ) | `aws_lambda_function.s3_remediation` | deferred | Add a dead-letter queue so failed remediations are retained for review. | 2027-01-31 | SI-11 |
| CKV_AWS_116 Ensure that AWS Lambda function is configured for a Dead Letter Queue(DLQ) | `aws_lambda_function.sns_notifier` | deferred | Add a dead-letter queue so failed remediations are retained for review. | 2027-01-31 | SI-11 |
| CKV_AWS_117 Ensure that AWS Lambda function is configured inside a VPC | `aws_lambda_function.s3_remediation` | not-applicable | Functions only call public AWS APIs (S3, SNS); placing them in a VPC would add NAT cost without reducing exposure. | 2027-04-21 | SC-7 |
| CKV_AWS_117 Ensure that AWS Lambda function is configured inside a VPC | `aws_lambda_function.sns_notifier` | not-applicable | Functions only call public AWS APIs (S3, SNS); placing them in a VPC would add NAT cost without reducing exposure. | 2027-04-21 | SC-7 |
| CKV_AWS_144 Ensure that S3 bucket has cross-region replication enabled | `aws_s3_bucket.config_bucket` | not-applicable | Configuration snapshots are reproducible from AWS Config; cross-region replication isn't needed for this data. | 2027-04-21 | CP-9, CP-6 |
| CKV_AWS_145 Ensure that S3 buckets are encrypted with KMS by default | `aws_s3_bucket.config_bucket` | risk-accepted | Bucket uses SSE-S3 encryption. A customer-managed KMS key adds key-policy management with little benefit for configuration snapshots. | 2027-04-21 | SC-28, SC-12 |
| CKV_AWS_173 Check encryption settings for Lambda environmental variable | `aws_lambda_function.s3_remediation` | not-applicable | Environment variables hold only a bucket name and topic ARN, no secrets; default Lambda encryption at rest applies. | 2027-04-21 | SC-28 |
| CKV_AWS_173 Check encryption settings for Lambda environmental variable | `aws_lambda_function.sns_notifier` | not-applicable | Environment variables hold only a bucket name and topic ARN, no secrets; default Lambda encryption at rest applies. | 2027-04-21 | SC-28 |
| CKV_AWS_18 Ensure the S3 bucket has access logging enabled | `aws_s3_bucket.config_bucket` | deferred | Server access logging for the AWS Config bucket needs a separate log bucket; CloudTrail already records data access. | 2027-01-31 | AU-2, AU-12 |
| CKV_AWS_26 Ensure all data stored in the SNS topic is encrypted | `aws_sns_topic.compliance_alerts` | deferred | Enable SSE with the AWS-managed SNS key; alerts carry resource IDs, not data. | 2027-01-31 | SC-28 |
| CKV_AWS_272 Ensure AWS Lambda function is configured to validate code-signing | `aws_lambda_function.s3_remediation` | deferred | Code signing needs an AWS Signer profile; planned with the move to packaged deployment artifacts. | 2027-01-31 | SI-7, CM-14 |
| CKV_AWS_272 Ensure AWS Lambda function is configured to validate code-signing | `aws_lambda_function.sns_notifier` | deferred | Code signing needs an AWS Signer profile; planned with the move to packaged deployment artifacts. | 2027-01-31 | SI-7, CM-14 |
| CKV_AWS_273 Ensure access is controlled through SSO and not AWS IAM defined users | `aws_iam_user.admin_users["admin1"]` | not-applicable | This repository demonstrates IAM user, group and MFA controls, so it defines IAM users on purpose. A production rollout would federate through IAM Identity Center. | 2027-04-21 | AC-2, IA-2 |
| CKV_AWS_273 Ensure access is controlled through SSO and not AWS IAM defined users | `aws_iam_user.admin_users["admin2"]` | not-applicable | This repository demonstrates IAM user, group and MFA controls, so it defines IAM users on purpose. A production rollout would federate through IAM Identity Center. | 2027-04-21 | AC-2, IA-2 |
| CKV_AWS_273 Ensure access is controlled through SSO and not AWS IAM defined users | `aws_iam_user.developer_users["dev1"]` | not-applicable | This repository demonstrates IAM user, group and MFA controls, so it defines IAM users on purpose. A production rollout would federate through IAM Identity Center. | 2027-04-21 | AC-2, IA-2 |
| CKV_AWS_273 Ensure access is controlled through SSO and not AWS IAM defined users | `aws_iam_user.developer_users["dev2"]` | not-applicable | This repository demonstrates IAM user, group and MFA controls, so it defines IAM users on purpose. A production rollout would federate through IAM Identity Center. | 2027-04-21 | AC-2, IA-2 |
| CKV_AWS_273 Ensure access is controlled through SSO and not AWS IAM defined users | `aws_iam_user.readonly_users["readonly1"]` | not-applicable | This repository demonstrates IAM user, group and MFA controls, so it defines IAM users on purpose. A production rollout would federate through IAM Identity Center. | 2027-04-21 | AC-2, IA-2 |
| CKV_AWS_49 Ensure no IAM policies documents allow "*" as a statement's actions | `aws_iam_policy_document.admin_policy` | risk-accepted | Same break-glass policy as CKV_AWS_1 (MFA and source-IP conditions). | 2027-04-21 | AC-6 |
| CKV_AWS_50 X-Ray tracing is enabled for Lambda | `aws_lambda_function.s3_remediation` | not-applicable | Small event-driven functions; CloudWatch Logs are enough and X-Ray adds cost without diagnostic value here. | 2027-04-21 | AU-12, SI-4 |
| CKV_AWS_50 X-Ray tracing is enabled for Lambda | `aws_lambda_function.sns_notifier` | not-applicable | Small event-driven functions; CloudWatch Logs are enough and X-Ray adds cost without diagnostic value here. | 2027-04-21 | AU-12, SI-4 |
| CKV_AWS_61 Ensure AWS IAM policy does not allow assume role permission across all services | `aws_iam_role.cross_account_role` | risk-accepted | Cross-account role trusts one named external account and requires an external ID and the corporate source IP. The account ID is a placeholder to replace on deployment. | 2027-04-21 | AC-6, AC-3 |

## Passed checks by NIST 800-53 control

| Control | Passed checks |
|---|---:|
| AC-2 | 12 |
| AC-21 | 2 |
| AC-3 | 34 |
| AC-4 | 2 |
| AC-6 | 114 |
| CM-2 | 1 |
| CM-8 | 1 |
| CP-9 | 1 |
| IA-5 | 10 |
| IA-8 | 10 |
| SA-22 | 2 |
| SC-28 | 1 |
| SC-7 | 14 |
| SI-12 | 1 |
| SI-2 | 2 |

Mappings to NIST 800-53 and CIS are my own judgment ([`iac_controls.yaml`](iac_controls.yaml)); checks without a mapping are counted as *unmapped*.
