# aws-security-scanner

A lightweight CLI tool that scans an AWS account for common IAM and S3
security misconfigurations and reports findings with severity levels and
remediation guidance.

## Checks

**IAM**
- Root account: MFA disabled, active access keys present
- IAM users with console access and no MFA
- Stale/unused access keys (default threshold: 90 days)
- Customer-managed policies granting `Action:*` on `Resource:*` with no condition
- Users with `AdministratorAccess` attached directly

**S3**
- Publicly accessible buckets (ACL or bucket policy, not fully blocked by Block Public Access)
- Buckets without default server-side encryption
- Buckets without versioning enabled

## Setup

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

Configure AWS credentials via a named profile, environment variables, or an
instance/role — anything `boto3` recognizes.

## Usage

```bash
# Scan everything using the default credential chain
python main.py

# Use a specific named profile
python main.py --profile my-profile

# Only scan S3, write results to JSON
python main.py --services s3 -o findings.json

# Adjust the stale access key threshold
python main.py --stale-days 60
```

Exit code is `1` if any HIGH severity finding is present, `0` otherwise —
useful for CI gating.

## Required permissions

Read-only IAM and S3 permissions are sufficient, e.g.:
`iam:GenerateCredentialReport`, `iam:GetCredentialReport`, `iam:ListPolicies`,
`iam:GetPolicyVersion`, `iam:ListEntitiesForPolicy`, `s3:ListAllMyBuckets`,
`s3:GetBucketAcl`, `s3:GetBucketPolicyStatus`, `s3:GetBucketPublicAccessBlock`,
`s3:GetEncryptionConfiguration`, `s3:GetBucketVersioning`.

## Roadmap

- HTML report output
- EC2/security group checks (open SSH/RDP to 0.0.0.0/0)
- CloudTrail logging coverage check
- Config file support (`.scanner.yml`) for custom thresholds/exclusions
