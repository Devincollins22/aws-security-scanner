# AWS Security Scanner

A hands-on cloud security posture assessment of AWS **IAM** and **S3**, benchmarked
against the **CIS AWS Foundations Benchmark v5.0** using
[Prowler](https://github.com/prowler-cloud/prowler), an open-source Cloud Security
Posture Management (CSPM) tool.

## Overview

This project demonstrates the full lifecycle of a cloud security assessment. Rather than
simply running a scanner, it follows the workflow a cloud security engineer uses in
practice: understand how resources *should* be secured, build an environment to test
against, assess it, remediate the findings, and prove the fixes worked.

The scope is deliberately focused on two core AWS services — **IAM** (Identity and Access
Management) and **S3** (Simple Storage Service) — the two most common sources of
real-world cloud misconfigurations.

## Methodology

1. **Learn the architecture** — how IAM and S3 should be securely designed
   (least privilege, groups vs. direct policy attachment, Block Public Access,
   encryption, versioning).
2. **Build a lab environment** — a deliberate mix of secure and insecure IAM and S3
   configurations to produce meaningful findings.
3. **Assess** — scan the environment with Prowler, scoped to IAM and S3, against
   CIS AWS Foundations Benchmark v5.0.
4. **Interpret & remediate** — analyze each finding (what it is, why it matters, how to
   fix it) and apply the fixes in AWS.
5. **Prove it** — re-scan and verify each fix from the CLI to produce before/after
   evidence.
6. **Document** — record remediations, and the reasoning behind findings accepted
   rather than fixed.

## Documentation

- [Baseline Findings](docs/baseline-findings.md) — initial assessment: 11 findings across IAM and S3
- [Remediation & Verification](docs/remediation.md) — fixes applied, before/after evidence, accepted risk

## Security Practices Demonstrated

- **Least privilege** — assessment performed via a dedicated read-only IAM user
  (`scanner-readonly`), with console access disabled. The assessment identity can verify
  configuration but cannot modify the environment it audits.
- **Credential protection** — AWS credentials and scan outputs are excluded from version
  control to prevent leaking sensitive account information.
- **Shared Responsibility Model** — every control assessed falls within the customer's
  responsibility for securing IAM and S3.
- **Risk-based triage** — findings were prioritized by severity and evaluated in context
  rather than treated as uniformly actionable.

## Tools & Technologies

- **Prowler 5.33.1** — open-source CSPM scanner (CIS, NIST, PCI, and more)
- **AWS** — the cloud environment being assessed (IAM, S3)
- **AWS CLI** — configuration verification and before/after evidence
- **Python** — Prowler's runtime, managed in an isolated virtual environment
- **Git & GitHub** — version control and documentation

## Status

**Assessment complete.** Lab environment built, baseline established (11 findings),
findings triaged, four misconfigurations remediated and verified via the AWS CLI, and
remaining findings documented as accepted risk with supporting rationale.

## Future Work

- Define the lab environment as code using **Terraform** for reproducibility.
- Implement a custom check in Python to demonstrate check mechanics internally.
- Extend coverage to additional CIS controls and AWS services (EC2, Security Groups).
- Explore continuous assessment via AWS Security Hub and AWS Config.
