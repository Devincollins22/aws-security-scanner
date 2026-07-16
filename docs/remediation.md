# Remediation & Verification

Four misconfigurations were remediated and verified via the AWS CLI. Each fix was
confirmed with the same command that originally detected the issue.

## 1. S3 — Block Public Access (bucket-level)

**Resource:** `devin-insecure-bucket-lab-2026`
**Risk:** Public buckets are the leading cause of cloud data breaches.

| Before | After |
|---|---|
| `BlockPublicAcls: false` | `BlockPublicAcls: true` |
| `IgnorePublicAcls: false` | `IgnorePublicAcls: true` |
| `BlockPublicPolicy: false` | `BlockPublicPolicy: true` |
| `RestrictPublicBuckets: false` | `RestrictPublicBuckets: true` |

Verified: `aws s3api get-public-access-block --bucket devin-insecure-bucket-lab-2026`

## 2. S3 — Versioning

**Resource:** `devin-insecure-bucket-lab-2026`
**Risk:** Without versioning, deleted or overwritten objects are unrecoverable —
no protection against accidental deletion or ransomware.

**Before:** no versioning configuration returned
**After:** `"Status": "Enabled"`

Verified: `aws s3api get-bucket-versioning --bucket devin-insecure-bucket-lab-2026`

## 3. IAM — MFA on console user

**Resource:** `test-user-insecure`
**Risk:** Console access protected by password alone; a stolen credential is
sufficient for account access.

**Before:** "has Console Password enabled but MFA disabled"
**After:** "has a virtual MFA instead of a hardware MFA device"

The remaining finding reflects virtual rather than hardware MFA — a "good but not
best" state. The critical gap (no second factor) is closed.

## 4. S3 — Account-level Block Public Access

**Scope:** Account 164599052013 (all buckets, including future ones)
**Rationale:** Defense in depth. The bucket-level fix addresses one resource;
the account-level guardrail addresses the entire class of problem and applies
by default to any bucket created later.

All four settings verified `true` via
`aws s3control get-public-access-block --account-id 164599052013`

## Accepted Risk — Not Remediated

| Finding | Reason |
|---|---|
| `devin-admin` has AdministratorAccess attached | Remediation affects primary administrative access; requires a change window and tested rollback |
| Hardware MFA (root, `devin-admin`, `dev-user-1`, `test-user-insecure`) | Requires a physical FIDO2 device not available in this environment |
| `scanner-readonly` uses long-lived credentials | Expected for a CLI-based assessment identity; mitigated by read-only scope and disabled console access. Production would use an assumed role with temporary credentials |
| Root user recently accessed | Legitimate account recovery during this engagement; self-clears with disuse |

## Note

The read-only assessment identity (`scanner-readonly`) could verify each fix but
could not perform it — AWS rejects write operations for that principal. Separation
between the identity that assesses and the identity that changes is functioning
as designed.