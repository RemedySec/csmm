# CSMM Security Pillars

**Version:** 1.0.0
**Status:** Draft
**Last Updated:** 2026-04-06

---

## Overview

CSMM organizes all controls into four security pillars. Each pillar represents a distinct attack surface or protection domain within an AWS environment. Pillar weights reflect the relative contribution of each domain to overall security risk, informed by historical cloud breach data and the attack patterns most commonly observed in AWS environments.

| Pillar      | Weight | Rationale Summary |
|-------------|--------|-------------------|
| IAM         | 35%    | Most breaches originate from identity compromise |
| Networking  | 25%    | Exposure surface determines reachability of assets |
| Storage     | 20%    | Data protection is the primary security outcome |
| Compute     | 20%    | Workloads are the primary target of post-compromise activity |

Pillar weights sum to 100%. They are applied in the overall scoring formula alongside level weights. See [scoring.md](scoring.md) for the full formula.

---

## IAM — Identity and Access Management (35%)

### What It Covers

The IAM pillar encompasses all controls related to authentication, authorization, delegation, and federation within and across AWS accounts. This includes:

- IAM users, roles, groups, and policies
- Root account protection
- MFA enforcement
- AWS Organizations Service Control Policies
- IAM Identity Center (SSO) and federation
- Permission boundaries and least-privilege patterns
- Attribute-based and role-based access control
- Cross-account trust relationships
- Secrets and credential management
- IAM Access Analyzer findings

### Why IAM is Weighted Most Heavily (35%)

Identity is the primary attack vector in cloud environments. Unlike traditional infrastructure where network perimeter controls could limit attacker reach, AWS is API-driven: possession of valid credentials is functionally equivalent to physical access to a resource. The consequences of identity compromise cascade across all other pillars — a stolen IAM role with broad permissions can exfiltrate data, modify network configuration, or destroy compute resources regardless of how well those resources are hardened.

Publicly available cloud breach analyses consistently identify credential compromise, misconfigured trust policies, and excessive permissions as the leading initial access and privilege escalation techniques. Investing in IAM controls reduces the probability and blast radius of the most common attack paths.

Additionally, IAM controls provide defense in depth that benefits all other pillars: even if a storage bucket is misconfigured, a well-scoped IAM policy can prevent unauthorized access; even if a compute workload is compromised, a minimal execution role limits lateral movement.

---

## Networking — Network Security (25%)

### What It Covers

The Networking pillar encompasses all controls related to network-level exposure, segmentation, and traffic visibility within AWS environments. This includes:

- VPC design and subnet segmentation
- Security groups and Network ACLs
- Internet gateways and NAT configuration
- VPC peering and Transit Gateway routing
- VPC Flow Logs
- VPC endpoints and PrivateLink
- Load balancer and API Gateway configuration
- TLS enforcement
- WAF and DDoS protection (Shield, CloudFront)
- DNS security

### Why Networking is Weighted 25%

Networking controls determine which assets are reachable and from where. An internet-exposed service that should be internal, an overly permissive security group, or an unencrypted connection are all networking failures that directly expand the attack surface available to an adversary.

Networking controls occupy the second position in weight because, while critical, they represent a necessary but not sufficient layer of defense. A correctly configured network perimeter that sits in front of an account with no MFA and publicly accessible storage provides limited protection — the attacker simply uses the IAM control plane rather than the network. Network controls are most effective when layered on top of strong identity controls.

Networking controls also underpin data protection: without proper network segmentation and VPC endpoint usage, traffic between workloads and data stores may traverse the public internet unnecessarily, increasing the risk of interception.

---

## Storage — Data Protection (20%)

### What It Covers

The Storage pillar encompasses all controls related to protecting data at rest, governing access to data stores, and ensuring recoverability. This includes:

- S3 bucket configuration (public access blocks, encryption, access logging, bucket policies)
- RDS and Aurora encryption, backup, and access control
- DynamoDB encryption and point-in-time recovery
- EBS volume encryption
- EFS encryption
- Secrets Manager and SSM Parameter Store
- KMS key management and rotation
- Backup policies and retention
- Data classification and inventory

### Why Storage is Weighted 20%

Data exposure is the most directly visible and often the most impactful consequence of a cloud security failure. The theft or public exposure of customer data, intellectual property, or regulated information is typically the event that triggers regulatory action, media attention, and customer harm.

Storage controls are weighted at 20% because in a well-architected system, storage should be protected by multiple layers above it (IAM controls restrict who can access data, network controls restrict which paths can reach data stores). A failure that reaches the storage layer represents a multi-layer breakdown. The storage pillar ensures that even when upper layers fail, data at rest is encrypted, access is logged, and recovery is possible.

Storage is also where compliance obligations are most concentrated — GDPR, HIPAA, PCI-DSS, and similar frameworks have specific requirements for data encryption, access control, and audit logging that map directly to this pillar.

---

## Compute — Workload Security (20%)

### What It Covers

The Compute pillar encompasses all controls related to the security of workloads running in AWS. This includes:

- EC2 instance configuration (AMI currency, public IP exposure, patch management)
- Systems Manager usage for instance access and patch compliance
- EC2 instance metadata service configuration (IMDSv2)
- Container security (ECS, EKS, ECR image scanning)
- Serverless function security (Lambda execution roles, environment variable hygiene)
- IaC pipeline security (secrets scanning, dependency scanning, policy-as-code)
- Configuration drift detection
- Shadow IT discovery
- Blast radius analysis
- Automated remediation
- Anomaly detection and response

### Why Compute is Weighted 20%

Compute resources are the primary target of post-compromise activity. Once an attacker has gained initial access — typically through an IAM or networking failure — they use compute resources to execute code, pivot laterally, exfiltrate data, and establish persistence. Securing compute reduces the impact of breaches that penetrate the outer layers.

Compute and Storage are weighted equally at 20% because they represent complementary protections: compute hardening limits what an attacker can do from within the environment, while storage protection limits the value of what they can take from it.

The compute pillar also hosts a disproportionate share of L4 and L5 controls (IaC governance, drift detection, automation) because compute infrastructure is where DevSecOps tooling has the most natural integration points.

---

## Note on Governance and Cross-Cutting Controls

Some controls do not map cleanly to a single pillar because they provide protection across multiple domains. Examples include:

- **CloudTrail** — primarily a governance/logging control, but provides visibility across IAM, networking, storage, and compute events.
- **AWS Config** — monitors resource configuration changes across all service types.
- **SCPs** — restrict actions across all AWS services.
- **Security Hub** — aggregates findings from all other security services.

For scoring purposes, each such control is assigned to the pillar whose security outcomes it most directly protects or whose failures it most directly detects. Assignment decisions are documented in each control's YAML definition via the `pillar` field.

Examples of cross-cutting control assignments:

| Control                              | Assigned Pillar | Rationale |
|--------------------------------------|-----------------|-----------|
| SCPs preventing root usage           | IAM             | Directly restricts identity/access behavior |
| VPC Flow Logs                        | Networking      | Directly monitors network traffic |
| CloudTrail (log file validation)     | Compute         | Primarily a platform governance control |
| S3 server access logging             | Storage         | Directly monitors data store access |
| Security Hub standard enablement    | Compute         | Platform-level aggregation control |
| Config enabled in all regions        | Compute         | Platform-level configuration monitoring |

These assignments are intentional design decisions, not categorization errors. If a control's placement seems counterintuitive, the `description` and `risk` fields in the control YAML explain the rationale.

---

## Pillar Weight Rationale Summary

The 35/25/20/20 split reflects the following principles:

1. **Identity is the cloud control plane.** Compromising identity is functionally equivalent to compromising everything. IAM deserves the highest weight.

2. **Network exposure determines reachability.** Without network controls, internet-exposed services are reachable by anyone. Networking deserves meaningful weight but is secondary to identity because network controls can be bypassed by the IAM control plane.

3. **Data protection is the ultimate outcome.** Storage controls ensure that even when other layers fail, data at rest is encrypted and access is logged. Storage and compute share the remaining weight equally.

4. **Compute hardening limits breach impact.** Secure workloads limit lateral movement and persistence, reducing the damage an attacker can do after gaining initial access.

The weights are reviewed and may be adjusted in future MAJOR versions as cloud breach patterns evolve or as multi-cloud scope is added. Any change to pillar weights constitutes a MAJOR version change under the CSMM versioning policy.
