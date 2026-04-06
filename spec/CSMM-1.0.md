# Cloud Security Maturity Model (CSMM) — Full Framework Specification

**Version:** 1.0.0
**Status:** Draft
**Last Updated:** 2026-04-06

---

## 1. Overview

The Cloud Security Maturity Model (CSMM) is an open, scored, leveled maturity model for AWS cloud security. It provides organizations with a structured, measurable path from a minimally secured cloud environment to a fully autonomous, self-healing security posture.

CSMM is designed to be:

- **Open**: Freely available, community-governed, and vendor-neutral in its recommendations.
- **Scored**: Every assessment produces a quantitative score that reflects both breadth and criticality of implemented controls.
- **Leveled**: Controls are organized into five ascending maturity levels, each with a gating rule to ensure foundational security is in place before advancing.
- **Actionable**: Each control includes risk context, verification steps, and compliance mappings so teams know exactly what to fix and why.

---

## 2. Scope

**Current Scope:** AWS cloud security.

**Planned:** Multi-cloud support (Azure, GCP) is planned for a future major version. The framework structure, scoring model, and governance approach are designed to accommodate additional cloud providers without breaking changes to the core specification.

---

## 3. Core Concepts

### 3.1 Controls

A control is the atomic unit of assessment in CSMM. Each control describes a specific security behavior or configuration that should be present in a cloud environment. Controls are:

- Assigned to exactly one **Level** (L1–L5).
- Assigned to exactly one **Pillar** (IAM, Networking, Storage, Compute, Monitoring).
- Weighted by **severity** to reflect the impact of failing that control.
- Verified through a defined **verification** procedure (console, CLI, policy check, etc.).

### 3.2 Levels

CSMM defines five maturity levels (L1–L5). Levels represent increasing sophistication of security practices:

| Level | Name       | Character                                     |
|-------|------------|-----------------------------------------------|
| L1    | Exposed    | Baseline hygiene; stops the most common attacks |
| L2    | Guarded    | Visibility, preventive controls, encryption at rest and in transit |
| L3    | Hardened   | Least privilege, segmentation, data perimeters |
| L4    | Verified   | Governance, IaC, drift detection, auditability |
| L5    | Autonomous | Automated response, self-healing, zero-trust loops |

**Level Gating:** An organization must achieve ≥80% of the weighted score available within level N before level N+1 controls are counted toward the overall score. This prevents gaming by skipping foundational controls.

### 3.3 Pillars

CSMM organizes controls into five security pillars, each representing a distinct attack surface or protection domain:

| Pillar      | Weight |
|-------------|--------|
| IAM         | 30%    |
| Networking  | 20%    |
| Storage     | 20%    |
| Compute     | 15%    |
| Monitoring  | 15%    |

### 3.4 Scoring

CSMM produces a single numeric overall score (0–100) by aggregating control results through a weighted formula that accounts for severity, level, and pillar. See Section 6 for full details.

---

## 4. Control Schema

Controls are defined in YAML. Each control file (or record within a controls manifest) must conform to the following schema:

```yaml
id: string                  # Unique identifier, e.g. IAM-L1-001
name: string                # Short, human-readable name
level: integer              # 1–5
pillar: string              # IAM | Networking | Storage | Compute | Monitoring
priority: string            # must-have | should-have | nice-to-have
severity_weight: string     # critical | high | medium | low
description: string         # What the control requires and why
risk: string                # What risk is mitigated by this control
verification:               # How to check whether the control is in place
  method: string            # console | cli | iac | automated
  steps:
    - string
compliance:                 # Optional: relevant compliance framework mappings
  - framework: string       # e.g. CIS AWS Foundations, NIST CSF, SOC 2
    reference: string       # e.g. CIS 1.1, NIST PR.AC-1
references:                 # Optional: external documentation links
  - string
```

### Field Definitions

| Field            | Type    | Required | Description |
|------------------|---------|----------|-------------|
| `id`             | string  | Yes      | Unique control identifier. Format: `{PILLAR}-L{N}-{SEQ}` (e.g. `IAM-L1-001`). |
| `name`           | string  | Yes      | A concise label (≤80 characters). |
| `level`          | integer | Yes      | Maturity level this control belongs to (1–5). |
| `pillar`         | string  | Yes      | One of: `IAM`, `Networking`, `Storage`, `Compute`, `Monitoring`. |
| `priority`       | string  | Yes      | `must-have`, `should-have`, or `nice-to-have`. Informational; does not affect scoring. |
| `severity_weight`| string  | Yes      | `critical`, `high`, `medium`, or `low`. Drives the numeric weight used in scoring. |
| `description`    | string  | Yes      | Full description of the expected configuration or behavior. |
| `risk`           | string  | Yes      | The security risk mitigated when this control is in place. |
| `verification`   | object  | Yes      | Method and steps to verify the control. |
| `compliance`     | list    | No       | Mappings to compliance frameworks and their control references. |
| `references`     | list    | No       | URLs to AWS documentation, CIS benchmarks, or other authoritative sources. |

---

## 5. Maturity Levels

### L1 — Exposed (~35 controls)

**Tagline:** "Stop the basics from being completely broken."

L1 covers the security hygiene that every AWS account must have, regardless of workload. Failing L1 controls represents active, exploitable exposure. An organization at L1 or below is vulnerable to trivial attacks including credential compromise, public data exposure, and lateral movement through overly permissive defaults.

Key control themes: MFA enforcement, root account protection, S3 public access blocks, default security group lockdown, basic encryption at rest, GuardDuty enablement.

### L2 — Guarded (~40 controls)

**Tagline:** "Gain visibility and start enforcing preventive guardrails."

L2 introduces organizational controls, visibility mechanisms, and a broader application of encryption. Organizations at L2 have logging in place, basic network segmentation, and preventive guardrails that reduce the blast radius of a compromise.

Key control themes: Service Control Policies (SCPs), IMDSv2 enforcement, VPC Flow Logs, CloudTrail, Network ACLs, TLS enforcement, RDS encryption, automated backups, CloudWatch metric alarms for security events, Security Hub enablement.

### L3 — Hardened (~35 controls)

**Tagline:** "Apply least privilege at scale and enforce data perimeters."

L3 focuses on fine-grained access control, cross-account trust hygiene, and data perimeter enforcement. Organizations at L3 have moved beyond simple deny-all defaults toward intentional, auditable access models.

Key control themes: IAM permission boundaries, ABAC/RBAC patterns, cross-account trust review, data perimeters (resource policies, VPC endpoints), container security baselines.

### L4 — Verified (~20 controls)

**Tagline:** "Prove your security posture through governance and auditability."

L4 ensures that security is enforced through code and process, not just configuration. Organizations at L4 can demonstrate that every resource is traceable to a source of truth and that deviations are detected and flagged.

Key control themes: IaC scanning pipelines, drift detection, resource-to-source mapping, blast radius analysis, shadow IT discovery.

### L5 — Autonomous (~10 controls)

**Tagline:** "Security that responds faster than humans can."

L5 represents the leading edge of cloud security maturity: environments where detected threats and misconfigurations trigger automated remediation without human intervention, and where zero-trust principles are enforced continuously in a closed loop.

Key control themes: Automated remediation pipelines, self-healing infrastructure, anomaly-triggered response, zero-trust enforcement loops.

---

## 6. Pillars

### IAM — 30%

Identity and Access Management is weighted most heavily because the majority of cloud breaches originate from compromised credentials, overly permissive roles, or misconfigured trust relationships. IAM controls span authentication, authorization, delegation, and federation.

### Networking — 20%

Networking controls govern the exposure surface of cloud workloads. This includes VPC design, security groups, network ACLs, ingress/egress filtering, and private connectivity. Poorly configured networking can expose internal services to the public internet or allow unrestricted lateral movement between accounts.

### Storage — 20%

Storage controls protect data at rest and govern access to data stores including S3, RDS, DynamoDB, EBS, and Secrets Manager. Data exposure via misconfigured storage is one of the most common causes of publicly reported cloud breaches.

### Compute — 15%

Compute controls cover workload security including EC2 instance configuration, container security, serverless function permissions, and patch management. Compromised compute resources are frequently used as pivot points for broader attacks.

### Monitoring — 15%

Monitoring controls cover threat detection, security event alerting, and observability. This includes CloudWatch metric filters and alarms for critical security events, Security Hub standards enablement, GuardDuty threat detection, and SNS notification pipelines. Without monitoring, organizations cannot detect when preventive controls fail or when attackers bypass them.

**Note on Governance Controls:** Governance-oriented controls (e.g. CloudTrail, Config, SCPs) are cross-cutting in nature. For scoring purposes, each such control is mapped to the pillar most directly protected by that control (e.g. SCPs → IAM, VPC Flow Logs → Networking, GuardDuty → Monitoring, Security Hub → Monitoring).

---

## 7. Scoring Model

### 7.1 Severity Weights

| Severity  | Numeric Weight |
|-----------|---------------|
| critical  | 4             |
| high      | 3             |
| medium    | 2             |
| low       | 1             |

### 7.2 Level Score

For a given level L within a given pillar P:

```
level_score(L, P) =
  Σ(control_result × severity_weight)
  ────────────────────────────────────  × 100
       Σ(severity_weight)
```

Where:
- `control_result` = 1 if the control is passing, 0 if failing.
- Both sums are over all controls in level L and pillar P.

### 7.3 Level Gating

If `level_score(N, P) < 80` for any pillar P, then controls in level N+1 (and above) within that pillar do not contribute to the overall score. They are assessed and reported for visibility but are excluded from the weighted score computation.

### 7.4 Level Weights

| Level | Weight |
|-------|--------|
| L1    | 0.35   |
| L2    | 0.25   |
| L3    | 0.20   |
| L4    | 0.12   |
| L5    | 0.08   |

### 7.5 Pillar Weights

| Pillar     | Weight |
|------------|--------|
| IAM        | 0.30   |
| Networking | 0.20   |
| Storage    | 0.20   |
| Compute    | 0.15   |
| Monitoring | 0.15   |

### 7.6 Overall Score

```
overall_score =
  Σ_{P} Σ_{L} ( level_score(L, P) × level_weight(L) × pillar_weight(P) )
```

The overall score ranges from 0 to 100. The maximum score of 100 is achieved when all controls pass in all levels and pillars.

---

## 8. Versioning and Governance

### 8.1 Version Scheme

CSMM follows semantic versioning (`MAJOR.MINOR.PATCH`):

- **MAJOR**: Breaking changes to the scoring formula, control schema, level gating rules, or pillar weights.
- **MINOR**: Addition of new controls, new compliance mappings, or non-breaking clarifications to existing controls.
- **PATCH**: Typo fixes, link updates, and editorial corrections that do not affect scoring.

The current version is **1.0.0**.

### 8.2 Governance Model

CSMM is maintained as an open project. Decisions are made through a lightweight RFC (Request for Comment) process:

1. Proposals are submitted as issues or pull requests to the CSMM repository.
2. A comment period of at least 14 days is observed for MAJOR and MINOR changes.
3. Maintainers review and merge approved proposals.
4. All accepted changes are recorded in a `CHANGELOG.md`.

Changes to scoring weights (level weights, pillar weights, severity weights) and to the level gating threshold require a MAJOR version bump and a minimum 30-day comment period.

---

## 9. How to Propose New Controls

1. **Check for duplicates**: Search the existing controls directory to confirm the behavior is not already covered by an existing control.

2. **Open an issue**: File a GitHub issue using the "New Control Proposal" template. Include:
   - Proposed control name and pillar assignment.
   - Justification for the chosen level (L1–L5).
   - Proposed `severity_weight` with rationale.
   - At least one verification method.
   - Any compliance framework references.

3. **Submit a pull request**: Once the issue has been discussed and the proposal direction is agreed upon, submit a PR with:
   - The new control YAML file placed in the correct `controls/{pillar}/` directory.
   - An entry in the controls index if one is maintained.
   - Updates to any affected documentation.

4. **Review period**: New controls are subject to a 14-day community review period before merging. Controls proposed for L1 or L2 require maintainer sign-off given their impact on gating and scoring.

5. **Versioning**: Accepted new controls result in a MINOR version bump to the framework.
