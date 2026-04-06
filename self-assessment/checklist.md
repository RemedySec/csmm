# CSMM Self-Assessment Checklist

## Instructions

Use this checklist to assess your organization's cloud security maturity. Check each item that your organization has fully implemented. Score each level by counting checked items vs total items. **You must score ≥80% on a level before the next level's score counts toward your overall maturity.**

Work through the levels in order. A partially-implemented control should not be checked — only mark items your organization has fully and consistently applied across all relevant accounts and regions.

---

## Scoring Guide

For each level, calculate your score as:

```
Level Score (%) = (Checked Items / Total Items) × 100
```

Your **effective maturity level** is the highest level at which you score ≥80%, provided all previous levels also score ≥80%.

> **Example:** If you score 90% on L1, 85% on L2, 75% on L3 — your effective maturity is Level 2, because L3 falls below the 80% threshold.

Critical controls (marked ⚠️ Critical) carry extra weight in the weighted scoring worksheet at the bottom of this document. A single unimplemented critical control should be treated as a material gap regardless of overall percentage score.

---

## Level 1 — Exposed (Foundational Hygiene)

> These are the non-negotiable basics. Gaps here represent active, high-severity exposure.

### IAM

- [ ] All console users have MFA enabled (csmm-iam-001) ⚠️ Critical
- [ ] No root account access keys exist (csmm-iam-002) ⚠️ Critical
- [ ] Root account has hardware MFA token (csmm-iam-003) ⚠️ Critical
- [ ] No IAM users created — using SSO/federation (csmm-iam-004)
- [ ] Access keys rotated within 90 days (csmm-iam-005)
- [ ] Unused credentials removed after 45 days (csmm-iam-006)
- [ ] Password policy requires minimum 14 characters (csmm-iam-007)
- [ ] Root usage denied via SCP (csmm-iam-008) ⚠️ Critical

**Level 1 IAM score: ___ / 8**

### Networking

- [ ] No SSH (22) open to 0.0.0.0/0 (csmm-net-001) ⚠️ Critical
- [ ] No RDP (3389) open to 0.0.0.0/0 (csmm-net-002) ⚠️ Critical
- [ ] Default security group restricted (csmm-net-003)
- [ ] No auto-assign public IPs in subnets (csmm-net-004)
- [ ] ALB/NLB HTTPS only (csmm-net-005)
- [ ] CloudFront enforces HTTPS (csmm-net-006)
- [ ] TLS 1.2 minimum everywhere (csmm-net-007)

**Level 1 Networking score: ___ / 7**

### Storage

- [ ] S3 account-level public access block enabled (csmm-str-001) ⚠️ Critical
- [ ] S3 bucket-level public access block enabled (csmm-str-002) ⚠️ Critical
- [ ] S3 default encryption enabled (csmm-str-003)
- [ ] S3 enforces SSL/TLS via bucket policy (csmm-str-004)
- [ ] RDS has no public accessibility (csmm-str-005) ⚠️ Critical
- [ ] RDS encryption at rest enabled (csmm-str-006)
- [ ] EBS default encryption enabled (csmm-str-007)
- [ ] S3 ACLs disabled (BucketOwnerEnforced) (csmm-str-008)

**Level 1 Storage score: ___ / 8**

### Compute

- [ ] EBS encryption at rest enabled (csmm-cmp-001)
- [ ] No public IPs on EC2 unless required (csmm-cmp-002)
- [ ] Public AMI sharing blocked (csmm-cmp-003)
- [ ] Public EBS snapshot sharing blocked (csmm-cmp-004)

**Level 1 Compute score: ___ / 4**

**Level 1 Total: ___ / 27 = ___%**

---

## Level 2 — Guarded (Baseline Guardrails)

> Systematic guardrails that prevent common misconfigurations at scale, typically implemented via SCPs, Config rules, and centralized logging.

### IAM

- [ ] Deny leaving organization via SCP (csmm-iam-009) ⚠️ Critical
- [ ] Deny CloudTrail disable via SCP (csmm-iam-010) ⚠️ Critical
- [ ] Require MFA for sensitive actions via SCP (csmm-iam-011)
- [ ] Restrict regions via SCP (csmm-iam-012)
- [ ] Deny IAM user creation via SCP (csmm-iam-013)
- [ ] Require external ID for cross-account roles (csmm-iam-014)
- [ ] Deny confused deputy via SCP (csmm-iam-015)

**Level 2 IAM score: ___ / 7**

### Networking

- [ ] VPC flow logs enabled (csmm-net-008)
- [ ] NACL denies known bad ports (csmm-net-009)
- [ ] NACL no unrestricted egress (csmm-net-010)
- [ ] NACL no unrestricted ingress (csmm-net-011)
- [ ] S3 gateway endpoint in VPCs (csmm-net-012)
- [ ] DynamoDB gateway endpoint in VPCs (csmm-net-013)
- [ ] ALB drops invalid headers (csmm-net-014)

**Level 2 Networking score: ___ / 7**

### Storage

- [ ] S3 versioning enabled (csmm-str-009)
- [ ] RDS automated backups enabled (csmm-str-010)
- [ ] RDS auto minor version upgrade (csmm-str-011)
- [ ] DynamoDB point-in-time recovery enabled (csmm-str-012)
- [ ] DynamoDB deletion protection enabled (csmm-str-013)
- [ ] ElastiCache encryption at rest (csmm-str-014)
- [ ] ElastiCache encryption in transit (csmm-str-015)
- [ ] EFS encryption at rest (csmm-str-016)
- [ ] Redshift encryption enabled (csmm-str-017)
- [ ] Redshift no public access (csmm-str-018) ⚠️ Critical

**Level 2 Storage score: ___ / 10**

### Compute

- [ ] IMDSv2 enforced on all instances (csmm-cmp-005) ⚠️ Critical
- [ ] IMDSv2 enforced via SCP (csmm-cmp-006)
- [ ] Launch templates used (not launch configs) (csmm-cmp-007)
- [ ] ASG health checks enabled (csmm-cmp-008)
- [ ] Lambda uses supported runtimes only (csmm-cmp-009)
- [ ] ECS no privileged containers (csmm-cmp-010)
- [ ] ECR image scan on push enabled (csmm-cmp-011)
- [ ] ECR immutable image tags (csmm-cmp-012)
- [ ] ASG IMDSv2 in launch templates (csmm-cmp-013)

**Level 2 Compute score: ___ / 9**

### Governance (cross-cutting)

- [ ] CloudTrail enabled in all regions (csmm-gov-001) ⚠️ Critical
- [ ] CloudTrail log file validation enabled (csmm-gov-002)
- [ ] CloudTrail S3 bucket encrypted (csmm-gov-003)
- [ ] GuardDuty enabled (csmm-gov-004) ⚠️ Critical
- [ ] AWS Config enabled (csmm-gov-005)
- [ ] Budget alerts configured (csmm-gov-006)
- [ ] Backup vault lock enabled (csmm-gov-007)

**Level 2 Governance score: ___ / 7**

### Monitoring

- [ ] AWS Security Hub enabled (csmm-mon-001) ⚠️ Critical
- [ ] CloudWatch alarm for unauthorized API calls (csmm-mon-002)
- [ ] CloudWatch alarm for root account usage (csmm-mon-003) ⚠️ Critical
- [ ] CloudWatch alarm for IAM policy changes (csmm-mon-004)
- [ ] CloudWatch alarm for CloudTrail config changes (csmm-mon-005)
- [ ] CloudWatch alarm for S3 bucket policy changes (csmm-mon-006)
- [ ] CloudWatch alarm for security group changes (csmm-mon-007)
- [ ] CloudWatch alarm for VPC changes (csmm-mon-008)
- [ ] CloudWatch alarm for Organizations changes (csmm-mon-009)
- [ ] CloudWatch alarm for console sign-in without MFA (csmm-mon-010)
- [ ] CloudWatch alarm for console authentication failures (csmm-mon-011)
- [ ] CloudWatch alarm for CMK disable/scheduled deletion (csmm-mon-012)

**Level 2 Monitoring score: ___ / 12**

**Level 2 Total: ___ / 52 = ___%**

---

## Level 3 — Hardened (Fine-Grained Controls)

> Least-privilege enforcement, data perimeters, WAF, and CMK-based encryption. Controls at this level require deliberate architectural decisions.

### IAM

- [ ] Permission boundaries on all non-admin roles (csmm-iam-016)
- [ ] Deny IAM admin actions via boundary (csmm-iam-017) ⚠️ Critical
- [ ] Enforce namespace boundaries (csmm-iam-018)
- [ ] ABAC — require project tag on resources (csmm-iam-019)
- [ ] ABAC — enforce tag-based access patterns (csmm-iam-020)
- [ ] Session tag propagation for federated users (csmm-iam-021)
- [ ] Restrict cross-account role assumption scope (csmm-iam-022)
- [ ] Identity perimeter — deny non-org principals (csmm-iam-023)
- [ ] Network perimeter — require VPC endpoints (csmm-iam-024)
- [ ] Resource perimeter — deny external resource access (csmm-iam-025)

**Level 3 IAM score: ___ / 10**

### Networking

- [ ] ALB origin protection (csmm-net-015)
- [ ] VPC endpoint policies restrict access (csmm-net-016)
- [ ] WAF associated with ALBs (csmm-net-017)
- [ ] WAF rate limiting rules (csmm-net-018)
- [ ] API Gateway authorization required (csmm-net-019)
- [ ] CloudFront OAC for S3 origins (csmm-net-020)

**Level 3 Networking score: ___ / 6**

### Storage

- [ ] KMS CMK key rotation enabled (csmm-str-019)
- [ ] KMS key policy scoped (no *) (csmm-str-020)
- [ ] Secrets Manager rotation enabled (csmm-str-021)
- [ ] No hardcoded secrets in IaC (csmm-str-022) ⚠️ Critical
- [ ] S3 access logging enabled (csmm-str-023)
- [ ] RDS audit logging enabled (csmm-str-024)

**Level 3 Storage score: ___ / 6**

### Compute

- [ ] Lambda no wildcard IAM permissions (csmm-cmp-014)
- [ ] Lambda in VPC for internal resources (csmm-cmp-015)
- [ ] EKS private API endpoint (csmm-cmp-016)
- [ ] EKS secrets encryption with CMK (csmm-cmp-017)
- [ ] EKS managed node groups (csmm-cmp-018)
- [ ] No privileged CodeBuild projects (csmm-cmp-019)

**Level 3 Compute score: ___ / 6**

### Governance

- [ ] GuardDuty EKS protection enabled (csmm-gov-008)
- [ ] GuardDuty S3 protection enabled (csmm-gov-009)
- [ ] Config conformance packs deployed (csmm-gov-010)
- [ ] Cost allocation tags enforced (csmm-gov-011)
- [ ] Backup cross-region replication (csmm-gov-012)
- [ ] Tagging policy enforced via SCP (csmm-gov-013)

**Level 3 Governance score: ___ / 6**

### Monitoring

- [ ] CloudWatch alarm for NACL changes (csmm-mon-013)
- [ ] CloudWatch alarm for network gateway changes (csmm-mon-014)
- [ ] CloudWatch alarm for route table changes (csmm-mon-015)
- [ ] CloudWatch alarm for Config configuration changes (csmm-mon-016)

**Level 3 Monitoring score: ___ / 4**

**Level 3 Total: ___ / 38 = ___%**

---

## Level 4 — Verified (Drift Detection & CI/CD Gates)

> Security is codified in pipelines and continuously validated. Drift is detected and surfaced systematically.

### IAM

- [ ] IAM Access Analyzer enabled (csmm-iam-026)
- [ ] Unused IAM roles reviewed quarterly (csmm-iam-027)
- [ ] Service-linked roles preferred over custom (csmm-iam-028)

**Level 4 IAM score: ___ / 3**

### Networking

- [ ] VPC flow logs analyzed for anomalies (csmm-net-021)
- [ ] DNS query logging enabled (csmm-net-022)

**Level 4 Networking score: ___ / 2**

### Storage

- [ ] S3 Object Lock for compliance data (csmm-str-025)
- [ ] CloudTrail log integrity monitoring (csmm-str-026)

**Level 4 Storage score: ___ / 2**

### Compute

- [ ] SSM-managed instances (no SSH keys) (csmm-cmp-020)
- [ ] Container image provenance verified (csmm-cmp-021)

**Level 4 Compute score: ___ / 2**

### Process & Tooling

- [ ] IaC scanned in CI/CD before merge (csmm-ver-001) ⚠️ Critical
- [ ] Terraform drift detection runs weekly (csmm-ver-002)
- [ ] Resource-to-source mapping maintained (csmm-ver-003)
- [ ] Blast radius reviewed before IAM changes (csmm-ver-004)
- [ ] Shadow IT (unmanaged resources) tracked (csmm-ver-005)
- [ ] Companion fix PRs generated for findings (csmm-ver-006)
- [ ] Security score tracked over time (csmm-ver-007)
- [ ] Break-glass access alerts configured (csmm-ver-008)

**Level 4 Process & Tooling score: ___ / 8**

**Level 4 Total: ___ / 17 = ___%**

---

## Level 5 — Autonomous (Self-Healing & Continuous Validation)

> Security is fully automated and self-correcting. Human intervention is reserved for exception handling and policy decisions.

- [ ] Auto-remediation for L1 controls (csmm-aut-001)
- [ ] Policy drift auto-reverted (csmm-aut-002)
- [ ] New resources auto-validated against CSMM (csmm-aut-003)
- [ ] Anomaly-driven GuardDuty → auto-isolate (csmm-aut-004)
- [ ] Access reviews automated (csmm-aut-005)
- [ ] Certificate rotation automated (csmm-aut-006)
- [ ] Secret rotation automated with zero downtime (csmm-aut-007)
- [ ] Continuous compliance evidence collection (csmm-aut-008)
- [ ] Self-healing infrastructure (drift → auto-apply) (csmm-aut-009)
- [ ] Zero-trust continuous verification (csmm-aut-010)

**Level 5 Total: ___ / 10 = ___%**

---

## Scoring Worksheet

### Raw Level Scores

| Level | Checked | Total | Raw % | ≥80%? |
|-------|---------|-------|-------|-------|
| L1 — Exposed | | 27 | | ☐ |
| L2 — Guarded | | 52 | | ☐ |
| L3 — Hardened | | 38 | | ☐ |
| L4 — Verified | | 17 | | ☐ |
| L5 — Autonomous | | 10 | | ☐ |
| **Total** | | **144** | | |

### Severity-Weighted Score

Critical controls (⚠️) carry a weight of **2×**; standard controls carry a weight of **1×**. Use this to surface disproportionate gaps.

```
Weighted Score (%) =
  (Σ checked weights) / (Σ total weights) × 100

Critical controls checked:  ___ × 2 = ___
Standard controls checked:  ___ × 1 = ___
─────────────────────────────────────────
Weighted numerator:               ___

Total critical controls:    ___ × 2 = ___  (28 critical × 2 = 56)
Total standard controls:    ___ × 1 = ___  (116 standard × 1 = 116)
─────────────────────────────────────────
Weighted denominator:             172

Weighted Score = ___ / 172 = ___%
```

> There are **28 critical controls** and **116 standard controls** across all levels, for a weighted denominator of **172**.

### Effective Maturity Level

Your effective maturity level is determined by the progression rule: each level must reach ≥80% before the next counts.

```
Effective Maturity Level: ___

  L1 ≥80%? [Y/N]  →  if N: Effective Level = 0 (Exposed)
  L2 ≥80%? [Y/N]  →  if N: Effective Level = 1
  L3 ≥80%? [Y/N]  →  if N: Effective Level = 2
  L4 ≥80%? [Y/N]  →  if N: Effective Level = 3
  L5 ≥80%? [Y/N]  →  if N: Effective Level = 4
                         if Y: Effective Level = 5
```

### Critical Control Gap Summary

List any unimplemented critical controls here — these should be treated as immediate remediation priorities regardless of overall score:

| Control ID | Description | Level | Status |
|------------|-------------|-------|--------|
| | | | |

---

## What's Next

### Full Specification

The complete CSMM control definitions, rationale, implementation guidance, and SCP/policy examples are maintained in the project repository:

- **Control definitions:** [`/controls/`](../controls/) — individual control files organized by pillar
- **Full specification:** [`/spec/`](../spec/) — the complete CSMM spec with level descriptions, scoring methodology, and design decisions
- **Contributing:** [`CONTRIBUTING.md`](../CONTRIBUTING.md) — how to propose new controls or changes

### Automated Assessment with RemedySec

Manual checklists give you a starting point, but they rely on self-reporting and can drift quickly as your infrastructure changes. [**RemedySec**](https://remedysec.com) provides automated, continuous assessment against the CSMM control set:

- Connects to your AWS accounts via read-only cross-account roles
- Evaluates all CSMM controls automatically across all regions and accounts
- Generates a scored maturity dashboard with control-level evidence
- Produces companion fix PRs (Terraform/CloudFormation) for failing controls
- Tracks your score over time and alerts on regressions

> Automated assessment eliminates the gap between self-reported posture and actual posture — especially important for L4 (drift detection) and L5 (continuous validation) controls that require ongoing monitoring, not point-in-time checks.

---

*CSMM Self-Assessment Checklist — Cloud Security Maturity Model*
*See the [full specification](../spec/) for control rationale and implementation guidance.*
