# Compute Pillar — Maturity Model

**Weight:** 15% of overall CSMM score | **Controls:** 14 | **Levels:** 5

---

## Maturity Levels at a Glance

```mermaid
block-beta
  columns 5

  H1["L1\nExposed\n5 controls"]:1 H2["L2\nGuarded\n3 controls"]:1 H3["L3\nHardened\n3 controls"]:1 H4["L4\nVerified\n2 controls"]:1 H5["L5\nAutonomous\n1 control"]:1

  A1["No Public IPs\non EC2\n(cmp-001)"]:1 A2["IMDSv2 Enforced\n& Blocked at Org\n(cmp-006)"]:1 A3["Lambda No\nWildcard Perms\n(cmp-009)"]:1 A4["SSM-Managed\nNo SSH Keys\n(cmp-012)"]:1 A5["Self-Healing\nInfrastructure\n(cmp-014)"]:1

  B1["Block Public\nAMI Sharing\n(cmp-002)"]:1 B2["No Secrets in\nLambda Env Vars\n(cmp-007)"]:1 B3["Container Security\nArchitecture\n(cmp-010)"]:1 B4["Hardened Image\nPipeline\n(cmp-013)"]:1 space:1

  C1["Block Public\nEBS Snapshots\n(cmp-003)"]:1 C2["ECR Image Scan\non Push\n(cmp-008)"]:1 C3["Patch Mgmt\nProcess\n(cmp-011)"]:1 space:1 space:1

  D1["EKS Private\nAPI Endpoint\n(cmp-004)"]:1 space:1 space:1 space:1 space:1

  E1["Lambda No\nPublic Resource\nPolicy\n(cmp-005)"]:1 space:1 space:1 space:1 space:1

  style H1 fill:#2e7d32,color:#fff
  style H2 fill:#00695c,color:#fff
  style H3 fill:#f57f17,color:#fff
  style H4 fill:#d84315,color:#fff
  style H5 fill:#6a1b9a,color:#fff

  style A1 fill:#c8e6c9,color:#1b5e20
  style B1 fill:#c8e6c9,color:#1b5e20
  style C1 fill:#c8e6c9,color:#1b5e20
  style D1 fill:#c8e6c9,color:#1b5e20
  style E1 fill:#c8e6c9,color:#1b5e20

  style A2 fill:#b2dfdb,color:#004d40
  style B2 fill:#b2dfdb,color:#004d40
  style C2 fill:#b2dfdb,color:#004d40

  style A3 fill:#ffecb3,color:#e65100
  style B3 fill:#ffecb3,color:#e65100
  style C3 fill:#ffecb3,color:#e65100

  style A4 fill:#ffccbc,color:#bf360c
  style B4 fill:#ffccbc,color:#bf360c

  style A5 fill:#e1bee7,color:#4a148c
```

---

## Control Reference

### L1 — Exposed (Foundational Hygiene)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-cmp-001 | No Public IPs on EC2 Unless Explicitly Required | High | 3 |
| csmm-cmp-002 | Block Public AMI Sharing | High | 3 |
| csmm-cmp-003 | Block Public EBS Snapshot Sharing | High | 3 |
| csmm-cmp-004 | EKS Private API Endpoint | High | 3 |
| csmm-cmp-005 | Lambda Function Resource Policies Do Not Allow Public Access | Critical | 4 |

### L2 — Guarded (Centralized Guardrails)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-cmp-006 | IMDSv2 Enforced on All EC2 Instances and Blocked at Org Level | Critical | 4 |
| csmm-cmp-007 | No Secrets or Credentials in Lambda Environment Variables | Critical | 4 |
| csmm-cmp-008 | ECR Image Scan on Push Enabled | High | 3 |

### L3 — Hardened (Security Architecture)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-cmp-009 | Lambda No Wildcard IAM Permissions | High | 3 |
| csmm-cmp-010 | Container Security Architecture | High | 3 |
| csmm-cmp-011 | Patch Management Process | High | 3 |

### L4 — Verified (Continuous Scanning)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-cmp-012 | SSM-Managed Instances (No SSH Keys) | High | 3 |
| csmm-cmp-013 | Hardened Image Pipeline (Golden AMIs) | High | 3 |

### L5 — Autonomous (Self-Healing)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-cmp-014 | Self-Healing Infrastructure | High | 3 |

---

## Progression Path

```mermaid
flowchart LR
  L1["L1 Exposed\nEncrypt, block\npublic exposure"] -->|"≥ 80%"| L2["L2 Guarded\nIMDSv2, launch\ntemplates"]
  L2 -->|"≥ 80%"| L3["L3 Hardened\nContainer security\nisolation, patching"]
  L3 -->|"≥ 80%"| L4["L4 Verified\nGolden AMIs\nSSM-only, shadow IT"]
  L4 -->|"≥ 80%"| L5["L5 Autonomous\nSelf-healing\ninfrastructure"]

  style L1 fill:#2e7d32,color:#fff
  style L2 fill:#00695c,color:#fff
  style L3 fill:#f57f17,color:#fff
  style L4 fill:#d84315,color:#fff
  style L5 fill:#6a1b9a,color:#fff
```
