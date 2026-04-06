# CSMM Maturity Levels — Detailed Definitions

**Version:** 1.0.0
**Status:** Draft
**Last Updated:** 2026-04-06

---

## Overview

CSMM defines five maturity levels (L1–L5). Each level builds on the previous one: controls at higher levels assume the foundational controls below them are in place. The levels progress from eliminating active exposure through achieving a fully autonomous, self-correcting security posture.

**Gating Rule (applies to all levels):** An organization must achieve a weighted level score of ≥80% on level N before level N+1 controls count toward the overall score. This is evaluated per pillar. See [scoring.md](scoring.md) for the full formula.

| Level | Name       | Approx. Controls | Key Themes |
|-------|------------|------------------|------------|
| L1    | Exposed    | ~35              | MFA, root protection, public access blocks, default SG, basic encryption, GuardDuty |
| L2    | Guarded    | ~40              | SCPs, IMDSv2, VPC Flow Logs, CloudTrail, NACLs, TLS, RDS encryption, backups, CloudWatch alarms, Security Hub |
| L3    | Hardened   | ~35              | Permission boundaries, ABAC/RBAC, cross-account trust, data perimeters, container security |
| L4    | Verified   | ~20              | IaC scanning, drift detection, resource-to-source mapping, blast radius, shadow IT |
| L5    | Autonomous | ~10              | Auto-remediation, self-heal, anomaly response, zero-trust loops |

---

## L1 — Exposed

**Tagline:** "Stop the basics from being completely broken."

**Approximate Control Count:** ~35

### Description

L1 controls represent the absolute baseline of AWS security hygiene. An environment that fails L1 controls is actively exploitable through well-documented, widely-automated attack techniques. L1 is not about sophistication — it is about ensuring that the most common and consequential misconfigurations are eliminated before anything else is assessed.

An organization at L1 has not yet established meaningful security posture. Root accounts may be used interactively, MFA may not be enforced, S3 buckets may be publicly accessible, and default security groups may allow unrestricted ingress. These are not theoretical risks; they are among the most frequently observed causes of real-world AWS breaches.

L1 controls tend to be binary (enabled/disabled or configured/not configured), verifiable in minutes, and remediable with low operational overhead. There is no valid business justification for failing L1 controls in a production account.

### Example Controls

| ID           | Name                                                         | Severity  |
|--------------|--------------------------------------------------------------|-----------|
| IAM-L1-001   | MFA enabled for all IAM users with console access            | critical  |
| IAM-L1-002   | Root account has MFA enabled                                 | critical  |
| IAM-L1-003   | Root account has no active access keys                       | critical  |
| IAM-L1-004   | Root account is not used for day-to-day tasks                | high      |
| IAM-L1-005   | No IAM users with AdministratorAccess except break-glass     | high      |
| IAM-L1-006   | IAM password policy meets minimum requirements               | medium    |
| IAM-L1-007   | Access keys rotated within 90 days                           | high      |
| IAM-L1-008   | Unused IAM credentials deactivated within 90 days            | high      |
| MON-L1-001   | GuardDuty enabled in all regions                             | high      |
| NET-L1-001   | Default security group in every VPC blocks all traffic       | high      |
| NET-L1-002   | No security group allows unrestricted SSH (0.0.0.0/0 port 22) | critical |
| NET-L1-003   | No security group allows unrestricted RDP (0.0.0.0/0 port 3389) | critical |
| NET-L1-004   | No security group allows unrestricted access on all ports    | critical  |
| STR-L1-001   | S3 account-level public access block enabled                 | critical  |
| STR-L1-002   | No S3 buckets with public ACLs enabled                       | critical  |
| STR-L1-003   | No S3 buckets with public bucket policy                      | critical  |
| STR-L1-004   | EBS volumes encrypted at rest                                | high      |
| STR-L1-005   | S3 buckets have default encryption enabled                   | high      |
| STR-L1-006   | No S3 bucket policies with wildcard principal without condition | high    |
| COM-L1-001   | EC2 instances do not use deprecated AMIs older than one year | medium    |
| COM-L1-002   | No EC2 instances with public IPs in non-DMZ subnets          | high      |
| COM-L1-003   | CloudWatch billing alerts configured                         | low       |

### Gating Rule

To unlock L2 scoring, an organization must achieve ≥80% of the available weighted L1 score within each pillar. If the L1 score in any pillar is below 80%, L2 controls within that pillar do not contribute to the overall score.

---

## L2 — Guarded

**Tagline:** "Gain visibility and start enforcing preventive guardrails."

**Approximate Control Count:** ~40

### Description

L2 builds the visibility and preventive control layer that transforms an account from one where attacks are easy into one where attacks are logged, partially blocked by policy, and limited in blast radius.

An organization at L2 has comprehensive audit logging across services and accounts, network traffic visibility via VPC Flow Logs, and organizational guardrails in place through Service Control Policies. Encryption is enforced more broadly, including in transit and for managed database services. Backup policies ensure recoverability. Instance metadata access is restricted to v2 to eliminate SSRF-based credential theft from the EC2 metadata endpoint.

L2 controls often require cross-account or organizational coordination (SCPs require AWS Organizations, CloudTrail org-level trails require a delegated administrator account) and may require modest operational investment to deploy and maintain. The return on that investment is significant: an L2 environment provides the telemetry needed to detect attacks and the preventive controls needed to reduce their impact.

### Example Controls

| ID           | Name                                                          | Severity  |
|--------------|---------------------------------------------------------------|-----------|
| IAM-L2-001   | SCPs deny root account usage in all member accounts           | critical  |
| IAM-L2-002   | SCPs enforce approved AWS region restrictions                 | high      |
| IAM-L2-003   | SCPs prevent disabling of CloudTrail                          | high      |
| IAM-L2-004   | SCPs prevent disabling of GuardDuty                           | high      |
| IAM-L2-005   | IAM roles used for EC2 instance access (no long-term keys)    | high      |
| IAM-L2-006   | IAM roles used for Lambda execution (no long-term keys)       | high      |
| NET-L2-001   | VPC Flow Logs enabled for all VPCs in all regions             | high      |
| NET-L2-002   | Network ACLs configured on subnets containing sensitive workloads | medium |
| NET-L2-003   | No VPC peering connections with overly broad route tables     | high      |
| NET-L2-004   | TLS 1.2 or higher enforced on all load balancers              | high      |
| NET-L2-005   | CloudFront distributions enforce HTTPS-only viewer policy     | medium    |
| STR-L2-001   | RDS instances encrypted at rest                               | high      |
| STR-L2-002   | RDS automated backups enabled with retention of at least 7 days | medium  |
| STR-L2-003   | S3 server access logging enabled on sensitive buckets         | medium    |
| STR-L2-004   | Secrets Manager or SSM Parameter Store used for all credentials | critical |
| STR-L2-005   | No plaintext secrets in EC2 userdata or environment variables | critical  |
| COM-L2-001   | IMDSv2 enforced on all EC2 instances                          | high      |
| COM-L2-002   | AWS Systems Manager used for instance access (SSH keys not required) | medium |
| COM-L2-003   | AWS Config enabled in all regions                             | high      |
| COM-L2-004   | CloudTrail enabled across all regions with log file validation | high     |
| COM-L2-005   | CloudTrail logs delivered to a centralized, restricted S3 bucket | medium  |
| MON-L2-001   | CloudWatch alarm configured for root account usage            | critical  |
| MON-L2-002   | CloudWatch alarm for unauthorized API calls                   | high      |
| MON-L2-003   | CloudWatch alarm for IAM policy changes                       | high      |
| MON-L2-004   | CloudWatch alarm for CloudTrail configuration changes         | high      |
| MON-L2-005   | Security Hub enabled with CIS AWS Foundations Benchmark standard | high   |
| MON-L2-006   | SNS alerting configured for high-severity GuardDuty findings  | high      |
| MON-L2-007   | CloudWatch Logs centralized across all accounts and regions   | medium    |

### Gating Rule

To unlock L3 scoring, an organization must achieve ≥80% of the available weighted L2 score within each pillar.

---

## L3 — Hardened

**Tagline:** "Apply least privilege at scale and enforce data perimeters."

**Approximate Control Count:** ~35

### Description

L3 represents a qualitative shift in security sophistication. Where L1 and L2 address obvious misconfigurations and logging gaps, L3 requires deliberate, organization-wide design decisions about how access is structured, how trust is delegated, and where data perimeters are drawn.

An organization at L3 has implemented permission boundaries to prevent privilege escalation through developer-provisioned roles, adopted ABAC or RBAC patterns for scalable least-privilege, audited all cross-account trust relationships to ensure they are scoped to specific principals, deployed VPC endpoints to eliminate internet-exposed data paths for internal services, and established a container security baseline that includes image scanning and non-root execution.

L3 controls are harder to verify because they often require policy analysis rather than a simple enabled/disabled check. They may require IAM Access Analyzer, custom Config rules, or policy simulation tools. Implementing L3 controls typically requires collaboration between platform, security, and application teams.

### Example Controls

| ID           | Name                                                              | Severity  |
|--------------|-------------------------------------------------------------------|-----------|
| IAM-L3-001   | Permission boundaries applied to all developer-created IAM roles  | high      |
| IAM-L3-002   | IAM Access Analyzer enabled in all regions and the org zone       | high      |
| IAM-L3-003   | No IAM roles with wildcard resource in Allow statements in production | critical |
| IAM-L3-004   | ABAC tag conditions used to scope resource access in sensitive accounts | medium |
| IAM-L3-005   | All cross-account role trust relationships audited and documented | high      |
| IAM-L3-006   | No cross-account trust policies with wildcard principal           | critical  |
| IAM-L3-007   | IAM Identity Center (SSO) enforced for all human console access   | high      |
| IAM-L3-008   | Separation of duties enforced between deployment and approval roles | high     |
| NET-L3-001   | VPC endpoints deployed for S3 and DynamoDB in all VPCs            | high      |
| NET-L3-002   | VPC endpoints deployed for all internal AWS services in use       | medium    |
| NET-L3-003   | No internet gateway routes present in private or data subnet route tables | high |
| NET-L3-004   | WAF deployed on all public-facing load balancers and API Gateway stages | high |
| NET-L3-005   | Origin access enforced for CloudFront (ALB/S3 direct access blocked) | medium |
| STR-L3-001   | S3 bucket policies enforce aws:PrincipalOrgID or aws:SourceVpc condition | high |
| STR-L3-002   | Customer-managed KMS keys used for sensitive data stores          | high      |
| STR-L3-003   | KMS key policies reviewed for cross-account access scope          | high      |
| STR-L3-004   | VPC endpoint policies restrict S3 access to organization principals | high    |
| COM-L3-001   | Container images scanned for vulnerabilities before deployment    | high      |
| COM-L3-002   | ECS tasks and EKS pods do not run as root                         | high      |
| COM-L3-003   | Lambda execution roles scoped to specific resource ARNs           | high      |
| COM-L3-004   | No hardcoded credentials in Lambda environment variables or container images | critical |
| COM-L3-005   | EC2 instances enrolled in Systems Manager Inventory               | medium    |
| COM-L3-006   | Patch Manager or equivalent used for OS patch compliance          | high      |
| MON-L3-001   | CloudWatch alarms for network configuration changes (VPC, SG, NACL) | high   |
| MON-L3-002   | CloudWatch alarm for S3 bucket policy changes                     | high      |
| MON-L3-003   | GuardDuty S3 Protection and EKS Protection plans enabled          | medium    |
| MON-L3-004   | Security Hub findings routed to ticketing or SIEM system          | medium    |

### Gating Rule

To unlock L4 scoring, an organization must achieve ≥80% of the available weighted L3 score within each pillar.

---

## L4 — Verified

**Tagline:** "Prove your security posture through governance and auditability."

**Approximate Control Count:** ~20

### Description

L4 shifts the security assurance model from "we configured it correctly" to "we can prove we configured it correctly, and we know immediately when it changes." An organization at L4 manages infrastructure as code, runs security checks in CI/CD pipelines, detects and alerts on configuration drift, and can trace every deployed resource back to a source of truth.

L4 also introduces awareness of the organization's full AWS resource footprint, including resources provisioned outside of sanctioned IaC pipelines (shadow IT). Blast radius analysis informs incident response planning by identifying which credentials or workloads have the broadest access.

L4 controls require investment in platform engineering and tooling. They are not achievable through console configuration alone. Adoption of tools such as Terraform, CloudFormation, or CDK along with policy-as-code frameworks, Config conformance packs, and Security Hub is typically necessary.

### Example Controls

| ID           | Name                                                              | Severity  |
|--------------|-------------------------------------------------------------------|-----------|
| IAM-L4-001   | All IAM roles and policies managed exclusively through IaC        | high      |
| IAM-L4-002   | IaC pipelines include IAM policy linting via IAM Access Analyzer  | high      |
| IAM-L4-003   | Privilege escalation paths identified and documented via automated tooling | high |
| IAM-L4-004   | Cross-account access paths documented and reviewed quarterly      | medium    |
| NET-L4-001   | Network topology changes require IaC pull request approval        | medium    |
| NET-L4-002   | Security group rule changes detected and alerted via Config rules | high      |
| STR-L4-001   | S3 bucket configuration drift detected and alerted automatically  | high      |
| STR-L4-002   | All data stores inventoried and classified by data sensitivity    | medium    |
| COM-L4-001   | All compute resources traceable to an IaC source repository       | high      |
| COM-L4-002   | Drift detection in place for EC2, ECS, Lambda, and RDS configurations | high   |
| COM-L4-003   | Shadow IT discovery process runs at least weekly across all accounts | medium  |
| COM-L4-004   | Blast radius analysis documented for all critical production workloads | medium |
| COM-L4-005   | Secrets scanning enforced in all CI/CD pipelines                  | high      |
| COM-L4-006   | Dependency vulnerability scanning enforced for application and IaC packages | medium |
| COM-L4-007   | Resource tagging policy enforced (owner, environment, source repo) | low      |
| COM-L4-008   | No untagged or unowned resources exist in production accounts     | medium    |

### Gating Rule

To unlock L5 scoring, an organization must achieve ≥80% of the available weighted L4 score within each pillar.

---

## L5 — Autonomous

**Tagline:** "Security that responds faster than humans can."

**Approximate Control Count:** ~10

### Description

L5 represents the leading edge of cloud security maturity. An organization at L5 has not only detected and resolved the misconfigurations addressed in L1–L4, but has built automation to prevent them from persisting. Detected threats and policy violations trigger automated remediation pipelines that return the environment to a known-good state without requiring human intervention for well-understood scenarios.

L5 also encompasses continuous zero-trust enforcement: access decisions are made dynamically based on identity, context, and device posture, and are continuously re-evaluated rather than granted persistently.

L5 controls are aspirational for most organizations. Achieving and maintaining them requires mature platform engineering, significant investment in tooling and runbook testing, and organizational processes that support automated changes to production environments. The risk of incorrect automated remediation must be carefully managed through staged rollout, kill switches, and audit trails for all automated actions.

### Example Controls

| ID           | Name                                                                  | Severity  |
|--------------|-----------------------------------------------------------------------|-----------|
| IAM-L5-001   | Automated remediation triggers on detected IAM policy violations      | high      |
| IAM-L5-002   | Zero-trust access decisions enforced dynamically via a policy engine  | high      |
| IAM-L5-003   | Just-in-time privileged access with automatic expiry enforced         | high      |
| NET-L5-001   | Unauthorized security group rules auto-reverted within 5 minutes      | high      |
| NET-L5-002   | Anomalous VPC traffic patterns trigger automated network isolation    | high      |
| STR-L5-001   | Public S3 bucket access auto-blocked within 5 minutes of detection    | critical  |
| COM-L5-001   | Compromised EC2 instances auto-isolated and snapshotted on GuardDuty finding | high |
| COM-L5-002   | Anomaly detection integrated with automated incident response playbooks | high    |
| COM-L5-003   | Self-healing infrastructure re-provisions from IaC on drift detection | medium    |
| COM-L5-004   | Certificate and key rotation fully automated with zero manual steps   | medium    |

### Gating Rule

L5 is the final level. There is no gating rule beyond L5. Organizations achieving ≥80% at L5 within all pillars achieve the maximum CSMM maturity designation.

---

## Gating Rules — Summary

| Level | Prerequisite           | Threshold |
|-------|------------------------|-----------|
| L2    | L1 score (per pillar)  | ≥80%      |
| L3    | L2 score (per pillar)  | ≥80%      |
| L4    | L3 score (per pillar)  | ≥80%      |
| L5    | L4 score (per pillar)  | ≥80%      |

If a level is gated out (its prerequisite is below 80%), that level's score is treated as zero in the overall calculation. All levels above it are also zeroed, since the gating chain is broken.

### Why 80% and not 100%?

The 80% threshold acknowledges operational reality. Some controls may be legitimately deferred due to legacy systems, contractual constraints, or planned migration timelines. A requirement for 100% would make the model binary at each level and would discourage adoption by organizations making genuine progress but unable to achieve complete coverage immediately.

80% is high enough to require substantial completion — an organization cannot skip a majority of L1 controls and still unlock L2 — but leaves room for documented, time-bounded exceptions.

### Why Gating Prevents Gaming

Without gating, an organization could score well on L4 and L5 controls while neglecting the foundational L1 and L2 controls that address the most systemic risks. Consider a pathological case: an organization with 100% on L3–L5 but only 40% on L1. Without gating, high upper-level scores could produce a respectable-looking overall score. With gating, the L1 failure gates out L2, which gates out L3–L5. The resulting score reflects the actual risk posture honestly.

Gating ensures the score represents the weakest link in the chain, not an average that obscures critical gaps.
