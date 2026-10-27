# CSMM Self-Assessment Checklist

## Instructions

Use this checklist to assess your organization's cloud security maturity. Check each item your organization has **fully and consistently** implemented across all relevant accounts and regions. Partial implementation does not count — only check an item when it is complete.

Work through the levels in order. You must score ≥80% at a level (weighted) before the next level's score counts toward your overall maturity.

---

## Scoring Guide

CSMM uses severity-weighted scoring, not simple counts. Each control carries a weight:

| Priority | Severity Weight |
|----------|----------------|
| Critical | 4              |
| High     | 3              |
| Medium   | 2              |
| Low      | 1              |

**Level Score (%) = (Sum of weights of passing controls / Sum of all weights in that level–pillar) × 100**

Your **effective maturity level** is the highest level at which you score ≥80%, provided all lower levels also score ≥80%.

> **Example:** A level–pillar combination has controls whose weights sum to 30. You pass controls worth 24 total weight. Score = 24/30 × 100 = 80% — exactly at the threshold.

Controls marked ⚠️ are Critical (weight 4). A single unimplemented critical control is a material gap regardless of the overall percentage.

### N/A Controls

A control may be marked N/A **only** if the specific AWS service it covers is not deployed in any account in scope. N/A is not available for controls that apply to configuration choices (e.g. you cannot mark "no root access keys" as N/A because you use root). When marking N/A, record written evidence — for example, an AWS Config rule confirming the service has zero resources, or an account-level SCP blocking service creation.

N/A controls are excluded from both the numerator and denominator when calculating the level score. They do not help or hurt your score — they are simply absent from the pool. If the service is later adopted, the control immediately becomes applicable and must be assessed.

---

## Level 1 — Exposed (Foundational Hygiene)

> These are the non-negotiable basics. Gaps here represent active, high-severity exposure.

### IAM

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 4 ⚠️ | All console users have MFA enabled | csmm-iam-001 |
| - [ ] | 4 ⚠️ | No root account access keys exist | csmm-iam-002 |
| - [ ] | 4 ⚠️ | Root account has hardware MFA token | csmm-iam-003 |
| - [ ] | 3 | No IAM users created — using SSO/federation | csmm-iam-004 |
| - [ ] | 3 | Access keys rotated within 90 days | csmm-iam-005 |
| - [ ] | 3 | Unused credentials removed after 45 days | csmm-iam-006 |
| - [ ] | 2 | Password policy requires minimum 14 characters | csmm-iam-007 |
| - [ ] | 4 ⚠️ | Root usage denied via SCP | csmm-iam-008 |
| - [ ] | 2 | No IAM policies attached directly to users | csmm-iam-009 |
| - [ ] | 4 ⚠️ | No IAM role trust policies with wildcard principal and no condition | csmm-iam-010 |
| - [ ] | 4 ⚠️ | No IAM policies with wildcard action and wildcard resource | csmm-iam-011 |

**L1 IAM: passing weight ___ / 33 = ____%**

### Networking

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 4 ⚠️ | No security group allows unrestricted inbound access from internet | csmm-net-001 |
| - [ ] | 3 | Default security group restricted to no traffic | csmm-net-002 |
| - [ ] | 3 | No auto-assign public IPs in subnets | csmm-net-003 |
| - [ ] | 3 | ALB/NLB HTTPS only (no HTTP listeners) | csmm-net-004 |
| - [ ] | 3 | CloudFront enforces HTTPS | csmm-net-005 |
| - [ ] | 2 | TLS 1.2 minimum everywhere | csmm-net-006 |
| - [ ] | 3 | Default VPC not in use or deleted in all regions | csmm-net-007 |

**L1 Networking: passing weight ___ / 21 = ____%**

### Data

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 4 ⚠️ | S3 account-level public access block enabled | csmm-dat-001 |
| - [ ] | 4 ⚠️ | S3 bucket-level public access block enabled on all buckets | csmm-dat-002 |
| - [ ] | 3 | S3 default encryption enabled on all buckets | csmm-dat-003 |
| - [ ] | 3 | S3 enforces SSL/TLS via bucket policy | csmm-dat-004 |
| - [ ] | 4 ⚠️ | RDS instances not publicly accessible | csmm-dat-005 |
| - [ ] | 3 | RDS encryption at rest enabled | csmm-dat-006 |
| - [ ] | 3 | EBS default encryption enabled in all regions | csmm-dat-007 |
| - [ ] | 2 | S3 ACLs disabled (BucketOwnerEnforced) | csmm-dat-008 |
| - [ ] | 3 | EFS file systems encrypted at rest | csmm-dat-009 |

**L1 Data: passing weight ___ / 29 = ____%**

### Compute

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 3 | No EC2 instances with public IPs unless explicitly excepted and tagged | csmm-cmp-001 |
| - [ ] | 3 | No AMIs shared publicly | csmm-cmp-002 |
| - [ ] | 3 | No EBS snapshots shared publicly | csmm-cmp-003 |
| - [ ] | 3 | EKS cluster API endpoint is private | csmm-cmp-004 |
| - [ ] | 4 ⚠️ | Lambda function resource policies do not allow public access | csmm-cmp-005 |

**L1 Compute: passing weight ___ / 16 = ____%**

### Monitoring

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 4 ⚠️ | CloudTrail enabled in all regions | csmm-mon-001 |
| - [ ] | 4 ⚠️ | AWS Config enabled in all regions | csmm-mon-002 |

**L1 Monitoring: passing weight ___ / 8 = ____%**

---

## Level 2 — Guarded (Centralized Guardrails)

> Organizational visibility and preventive controls that reduce blast radius.

### IAM

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 3 | MFA required for sensitive IAM actions via SCP | csmm-iam-012 |
| - [ ] | 3 | Regions restricted via SCP to approved list | csmm-iam-013 |
| - [ ] | 3 | Temporary credentials enforced over long-lived access keys | csmm-iam-014 |
| - [ ] | 4 ⚠️ | CloudTrail disable blocked via SCP | csmm-iam-015 |
| - [ ] | 4 ⚠️ | Leaving AWS Organizations blocked via SCP | csmm-iam-016 |
| - [ ] | 2 | No IAM roles unused in 90+ days | csmm-iam-017 |
| - [ ] | 4 ⚠️ | OIDC federated role trust policies scoped to specific subjects (no wildcard sub) | csmm-iam-018 |

**L2 IAM: passing weight ___ / 23 = ____%**

### Networking

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 4 ⚠️ | GuardDuty enabled in all regions | csmm-net-008 |
| - [ ] | 3 | VPC Flow Logs enabled on all VPCs | csmm-net-009 |
| - [ ] | 3 | NACLs deny known bad ports | csmm-net-010 |
| - [ ] | 2 | NACLs have no unrestricted egress rules | csmm-net-011 |
| - [ ] | 3 | NACLs have no unrestricted ingress rules | csmm-net-012 |
| - [ ] | 2 | VPC peering route tables scoped to least-access CIDRs | csmm-net-013 |
| - [ ] | 3 | Transit Gateway auto-accept shared attachments disabled | csmm-net-014 |

**L2 Networking: passing weight ___ / 20 = ____%**

### Data

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 3 | CloudTrail S3 bucket encrypted | csmm-dat-010 |
| - [ ] | 3 | S3 versioning enabled on all buckets | csmm-dat-011 |
| - [ ] | 3 | RDS automated backups enabled with ≥7-day retention | csmm-dat-012 |
| - [ ] | 3 | DynamoDB Point-in-Time Recovery enabled | csmm-dat-013 |
| - [ ] | 3 | S3 server access logging enabled on sensitive buckets | csmm-dat-014 |
| - [ ] | 4 ⚠️ | No cross-account S3 bucket access without Organization condition | csmm-dat-015 |

**L2 Data: passing weight ___ / 19 = ____%**

### Compute

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 4 ⚠️ | IMDSv2 enforced on all EC2 instances and blocked at org level via SCP | csmm-cmp-006 |
| - [ ] | 4 ⚠️ | No secrets or credentials in Lambda environment variables | csmm-cmp-007 |
| - [ ] | 3 | ECR image scan on push enabled across all repositories | csmm-cmp-008 |

**L2 Compute: passing weight ___ / 11 = ____%**

### Monitoring

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 4 ⚠️ | AWS Security Hub enabled with CIS AWS Foundations standard | csmm-mon-003 |
| - [ ] | 3 | CloudTrail log file validation enabled | csmm-mon-004 |
| - [ ] | 3 | CloudTrail integrated with CloudWatch Logs | csmm-mon-005 |
| - [ ] | 3 | All 11 CIS CloudWatch metric filters and alarms configured | csmm-mon-006 |

**L2 Monitoring: passing weight ___ / 13 = ____%**

---

## Level 3 — Hardened (Security Architecture)

> Deliberate architectural decisions about access structure, trust delegation, and data perimeters.

### IAM

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 3 | Permission boundaries applied to all non-admin roles | csmm-iam-019 |
| - [ ] | 4 ⚠️ | Permission boundaries explicitly deny IAM admin actions | csmm-iam-020 |
| - [ ] | 2 | ABAC/RBAC access patterns enforced | csmm-iam-021 |
| - [ ] | 3 | Periodic least-privilege access review conducted | csmm-iam-022 |
| - [ ] | 3 | Cross-account IAM trust policies scoped — no wildcard principal | csmm-iam-023 |
| - [ ] | 3 | IAM data perimeter controls in place (org conditions on resource policies) | csmm-iam-024 |

**L3 IAM: passing weight ___ / 18 = ____%**

### Networking

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 3 | ALB origin protection — direct ALB access blocked (CloudFront-only) | csmm-net-015 |
| - [ ] | 3 | WAF associated with all public-facing ALBs | csmm-net-016 |
| - [ ] | 3 | Network segmentation strategy documented and enforced | csmm-net-017 |
| - [ ] | 3 | VPC endpoints deployed for S3, DynamoDB, and internal services | csmm-net-018 |
| - [ ] | 3 | Egress filtering architecture in place (proxy or Network Firewall) | csmm-net-019 |
| - [ ] | 2 | DDoS protection strategy documented (Shield Advanced or WAF rate limits) | csmm-net-020 |
| - [ ] | 4 ⚠️ | VPC endpoint policies scoped — no unrestricted principal | csmm-net-021 |

**L3 Networking: passing weight ___ / 21 = ____%**

### Data

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 3 | KMS CMK key rotation enabled | csmm-dat-016 |
| - [ ] | 4 ⚠️ | KMS key policies — no overly permissive access | csmm-dat-017 |
| - [ ] | 4 ⚠️ | No hardcoded secrets or credentials in IaC | csmm-dat-018 |
| - [ ] | 3 | Data classification and tagging strategy applied to data stores | csmm-dat-019 |
| - [ ] | 3 | Secrets management architecture — all secrets in Secrets Manager or SSM | csmm-dat-020 |
| - [ ] | 3 | Encryption key management architecture documented and enforced | csmm-dat-021 |

**L3 Data: passing weight ___ / 20 = ____%**

### Compute

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 3 | Lambda functions have no wildcard IAM permissions | csmm-cmp-009 |
| - [ ] | 3 | Container security architecture enforced (non-root, image scanning, signed images) | csmm-cmp-010 |
| - [ ] | 3 | Patch management process in place with documented SLAs | csmm-cmp-011 |

**L3 Compute: passing weight ___ / 9 = ____%**

### Monitoring

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 2 | Extended CloudWatch alarms for infrastructure changes (VPC, NACL, route tables, Config) | csmm-mon-007 |
| - [ ] | 3 | Centralized security observability architecture (SIEM or cross-account log aggregation) | csmm-mon-008 |
| - [ ] | 3 | Incident response process and runbooks documented and tested | csmm-mon-009 |

**L3 Monitoring: passing weight ___ / 8 = ____%**

---

## Level 4 — Verified (Governance and Auditability)

> Proving security posture through code, continuous scanning, and measured processes.

### IAM

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 3 | IAM Access Analyzer enabled in all regions and the org zone | csmm-iam-025 |
| - [ ] | 3 | JIT/break-glass temporary elevated access process in place | csmm-iam-026 |
| - [ ] | 4 ⚠️ | IaC scanned for security issues in CI/CD before merge | csmm-iam-027 |

**L4 IAM: passing weight ___ / 10 = ____%**

### Networking

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 3 | DNS security and Route53 hardening implemented | csmm-net-022 |

**L4 Networking: passing weight ___ / 3 = ____%**

### Data

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 3 | Backup and disaster recovery architecture documented and tested | csmm-dat-022 |
| - [ ] | 2 | S3 Object Lock configured for compliance data (WORM) | csmm-dat-023 |

**L4 Data: passing weight ___ / 5 = ____%**

### Compute

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 3 | All instances managed via SSM — no direct SSH keys | csmm-cmp-012 |
| - [ ] | 3 | Hardened image pipeline (golden AMIs) in use | csmm-cmp-013 |

**L4 Compute: passing weight ___ / 6 = ____%**

### Monitoring

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 2 | Threat hunting program with MITRE ATT&CK coverage tracking | csmm-mon-010 |
| - [ ] | 2 | Security metrics and maturity tracking dashboard in use | csmm-mon-011 |

**L4 Monitoring: passing weight ___ / 4 = ____%**

---

## Level 5 — Autonomous (Self-Healing)

> Automated response — the environment corrects misconfigurations without human intervention.

### IAM

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 3 | Auto-remediation pipelines for L1 IAM control violations | csmm-iam-028 |
| - [ ] | 3 | IAM policy drift automatically detected and reverted | csmm-iam-029 |

**L5 IAM: passing weight ___ / 6 = ____%**

### Networking

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 3 | GuardDuty findings trigger automated network isolation of compromised instances | csmm-net-023 |

**L5 Networking: passing weight ___ / 3 = ____%**

### Data

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 3 | Automated secret rotation with zero manual steps | csmm-dat-024 |

**L5 Data: passing weight ___ / 3 = ____%**

### Compute

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 3 | Self-healing infrastructure re-provisions from IaC on drift detection | csmm-cmp-014 |

**L5 Compute: passing weight ___ / 3 = ____%**

### Monitoring

| Done | Wt | Control | ID |
|------|----|---------|----|
| - [ ] | 3 | SOAR playbooks auto-isolate compromised resources within defined SLA | csmm-mon-012 |

**L5 Monitoring: passing weight ___ / 3 = ____%**

---

## Scoring Summary

Transfer your level–pillar scores below. A score below 80% gates the next level for that pillar.

| Pillar | Weight | L1 | L2 | L3 | L4 | L5 |
|--------|--------|----|----|----|----|-----|
| IAM | 30% | ___% | ___% | ___% | ___% | ___% |
| Networking | 20% | ___% | ___% | ___% | ___% | ___% |
| Data | 20% | ___% | ___% | ___% | ___% | ___% |
| Compute | 15% | ___% | ___% | ___% | ___% | ___% |
| Monitoring | 15% | ___% | ___% | ___% | ___% | ___% |

**Level weights:** L1 = 35%, L2 = 25%, L3 = 20%, L4 = 12%, L5 = 8%

For the full weighted scoring formula, see [spec/scoring.md](../spec/scoring.md).
