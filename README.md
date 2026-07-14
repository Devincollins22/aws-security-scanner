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
   (least privilege, roles vs. users, Block Public Access, encryption, versioning).
2. **Build a lab environment** — a deliberate mix of secure and insecure IAM and S3
   configurations to produce meaningful findings.
3. **Assess** — scan the environment with Prowler, scoped to IAM and S3, against
   CIS AWS Foundations Benchmark v5.0.
4. **Interpret & remediate** — analyze each finding (what it is, why it matters, how to
   fix it) and apply the fixes in AWS.
5. **Prove it** — re-scan to produce before/after evidence that the misconfigurations
   were resolved.
6. **Deliver** — produce a professional remediation report documenting the findings and
   fixes.

## Security Practices Demonstrated

- **Least privilege** — assessment performed via a dedicated read-only IAM user
  (`scanner-readonly`), with console access disabled.
- **Credential protection** — AWS credentials and scan outputs are excluded from version
  control to prevent leaking sensitive account information.
- **Shared Responsibility Model** — every control assessed falls within the customer's
  responsibility for securing IAM and S3.

## Tools & Technologies

- **Prowler 5.33.1** — open-source CSPM scanner (CIS, NIST, PCI, and more)
- **AWS** — the cloud environment being assessed (IAM, S3)
- **Python** — for a custom security check demonstrating how a check works internally
- **Git & GitHub** — version control and documentation

## Status

🚧 Work in progress — an active cloud security learning project.

## Future Work

- Define the lab environment as code using **Terraform** for reproducibility.
- Extend coverage to additional CIS controls and AWS services.