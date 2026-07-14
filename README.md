# AWS Security Scanner

A cloud security posture assessment project that audits an AWS account against the
**CIS AWS Foundations Benchmark** using [Prowler](https://github.com/prowler-cloud/prowler),
an open-source Cloud Security Posture Management (CSPM) tool.

## Overview

This project demonstrates how cloud environments are assessed for security
misconfigurations. It focuses on two core AWS services — **IAM** (Identity and
Access Management) and **S3** (Simple Storage Service) — and evaluates them against
the CIS AWS Foundations Benchmark v5.0.

The goal is not to rebuild professional tooling from scratch, but to understand how
cloud security auditing works in practice: running a real scan against a real account,
interpreting the findings, and explaining the risks and remediations behind them.

## Approach

1. **Configure a test AWS account** with a deliberate mix of secure and insecure
   settings, to produce meaningful findings.
2. **Authenticate securely** using a dedicated, least-privilege IAM user
   (read-only access, no console login).
3. **Scan against CIS** using Prowler, scoped to the CIS AWS Foundations Benchmark v5.0.
4. **Interpret the findings** — what each misconfiguration is, why it matters, and
   how to remediate it.
5. **Build one check from scratch** to demonstrate understanding of how these
   security checks work under the hood.

## Why This Matters

Cloud misconfigurations are one of the most common causes of real-world data breaches.
Under the AWS **Shared Responsibility Model**, securing configurations like IAM
permissions and S3 access is the customer's responsibility — exactly what this project
audits.

## Tools & Technologies

- **Prowler** — open-source CSPM scanner (CIS, NIST, PCI, and more)
- **AWS** — the cloud environment being assessed
- **Python** — for a custom security check
- **Git & GitHub** — version control and documentation

## Status

🚧 Work in progress — part of an ongoing cloud security learning project.