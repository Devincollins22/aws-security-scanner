# Baseline Assessment — IAM & S3

**Scan:** `prowler aws --service iam s3 --status FAIL --severity critical high`
**Date:** 2026-07-14 | **Account:** 164599052013 | **Identity:** `scanner-readonly` (read-only)
**Result:** 11 findings — 3 Critical, 8 High

## Critical

| # | Finding | Resource |
|---|---|---|
| 1 | `AdministratorAccess` policy allows `*:*` | AWS-managed policy |
| 2 | AdministratorAccess attached directly to user | `devin-admin` |
| 3 | Root uses virtual MFA, not hardware | root account |

## High

| # | Finding | Resource |
|---|---|---|
| 4 | Long-lived credentials used beyond IAM/STS | `scanner-readonly` |
| 5 | Virtual MFA instead of hardware | `dev-user-1` |
| 6 | Virtual MFA instead of hardware | `devin-admin` |
| 7 | **Console access with MFA disabled** | `test-user-insecure` |
| 8 | Password policy: no reuse prevention | account |
| 9 | Password policy: no expiration | account |
| 10 | **Block Public Access disabled** | `devin-insecure-bucket-lab-2026` |
| 11 | Account-level Block Public Access not configured | account |

## Validation

The deliberately insecure resources were both detected (#7, #10). The securely
configured resources — `devin-secure-bucket-lab-2026` and the `Developers`
group pattern — produced no findings, confirming the controls work as intended.

## Note on Finding #4

The assessment identity itself was flagged for long-lived credentials. This is
technically correct but expected for a CLI-based scanner; risk is mitigated by
the identity being read-only with console access disabled. A production
deployment would use temporary credentials via an assumed role.