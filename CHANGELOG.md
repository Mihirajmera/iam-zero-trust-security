# Changelog

## Unreleased

- **Compliance evidence in CI:** Checkov results mapped to NIST 800-53 and CIS AWS v3.0.0, a reviewed exceptions register, and a gate that fails on unexcepted findings. Report and OSCAL output in `compliance/`.
- **Fixed:** the remediation Lambda's policy granted EC2 terminate/stop, bucket ACL and policy writes on `*`, but not the `s3:DeleteBucketPolicy` the code calls, so removing a public bucket policy would have failed. It now grants exactly the two calls the code makes, on S3 buckets only.
- **Fixed:** Lambda runtime python3.9 (deprecated) -> python3.12.
- The break-glass admin policy now also requires MFA, alongside the corporate IP restriction; it is recorded as an accepted risk.
## 1.0.0 (2026-10-17)

First tagged release. Zero Trust IAM baseline in Terraform with AWS Config rules and automated remediation.

- CI on Python 3.11 / current Terraform tooling with every GitHub Action pinned to a commit SHA
- CodeQL, OpenSSF Scorecard and Dependabot
- Security policy with private vulnerability reporting
