# Data Pillar — Maturity Model

**Weight:** 20% of overall CSMM score | **Controls:** 24 | **Levels:** 5

---

## Maturity Levels at a Glance

```mermaid
block-beta
  columns 5

  H1["L1\nExposed\n9 controls"]:1 H2["L2\nGuarded\n6 controls"]:1 H3["L3\nHardened\n6 controls"]:1 H4["L4\nVerified\n2 controls"]:1 H5["L5\nAutonomous\n1 control"]:1

  A1["S3 Account\nPublic Block\n(dat-001)"]:1 A2["S3 Versioning\nEnabled\n(dat-011)"]:1 A3["KMS CMK Key\nRotation\n(dat-016)"]:1 A4["Backup & DR\nArchitecture\n(dat-022)"]:1 A5["Automated Secret\nRotation\n(dat-024)"]:1

  B1["S3 Bucket\nPublic Block\n(dat-002)"]:1 B2["CloudTrail S3\nEncryption\n(dat-010)"]:1 B3["KMS Key Policy\nNo Wildcard\n(dat-017)"]:1 B4["S3 Object Lock\nCompliance\n(dat-023)"]:1 space:1

  C1["S3 Default\nEncryption\n(dat-003)"]:1 C2["RDS Automated\nBackups\n(dat-012)"]:1 C3["No Hardcoded\nSecrets in IaC\n(dat-018)"]:1 space:1 space:1

  D1["S3 Enforce\nSSL/TLS\n(dat-004)"]:1 D2["DynamoDB\nPITR\n(dat-013)"]:1 D3["Data Classification\n& Tagging\n(dat-019)"]:1 space:1 space:1

  E1["RDS No Public\nAccessibility\n(dat-005)"]:1 E2["S3 Server Access\nLogging\n(dat-014)"]:1 E3["Secrets Mgmt\nArchitecture\n(dat-020)"]:1 space:1 space:1

  F1["RDS Encryption\nat Rest\n(dat-006)"]:1 F2["No Cross-Acct S3\nWithout Org Cond\n(dat-015)"]:1 F3["Encryption Key\nMgmt Arch\n(dat-021)"]:1 space:1 space:1

  G1["EBS Default\nEncryption\n(dat-007)"]:1 space:1 space:1 space:1 space:1

  I1["Disable S3\nACLs\n(dat-008)"]:1 space:1 space:1 space:1 space:1

  J1["EFS Encryption\nat Rest\n(dat-009)"]:1 space:1 space:1 space:1 space:1

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
  style F1 fill:#c8e6c9,color:#1b5e20
  style G1 fill:#c8e6c9,color:#1b5e20
  style I1 fill:#c8e6c9,color:#1b5e20
  style J1 fill:#c8e6c9,color:#1b5e20

  style A2 fill:#b2dfdb,color:#004d40
  style B2 fill:#b2dfdb,color:#004d40
  style C2 fill:#b2dfdb,color:#004d40
  style D2 fill:#b2dfdb,color:#004d40
  style E2 fill:#b2dfdb,color:#004d40
  style F2 fill:#b2dfdb,color:#004d40

  style A3 fill:#ffecb3,color:#e65100
  style B3 fill:#ffecb3,color:#e65100
  style C3 fill:#ffecb3,color:#e65100
  style D3 fill:#ffecb3,color:#e65100
  style E3 fill:#ffecb3,color:#e65100
  style F3 fill:#ffecb3,color:#e65100

  style A4 fill:#ffccbc,color:#bf360c
  style B4 fill:#ffccbc,color:#bf360c

  style A5 fill:#e1bee7,color:#4a148c
```

---

## Control Reference

### L1 — Exposed (Foundational Hygiene)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-dat-001 | S3 Account-Level Public Access Block | Critical | 4 |
| csmm-dat-002 | S3 Bucket-Level Public Access Block | Critical | 4 |
| csmm-dat-003 | S3 Default Encryption Enabled | High | 3 |
| csmm-dat-004 | S3 Enforce SSL/TLS via Bucket Policy | High | 3 |
| csmm-dat-005 | RDS No Public Accessibility | Critical | 4 |
| csmm-dat-006 | RDS Encryption at Rest | High | 3 |
| csmm-dat-007 | EBS Default Encryption Enabled | High | 3 |
| csmm-dat-008 | Disable S3 ACLs (BucketOwnerEnforced) | Medium | 2 |
| csmm-dat-009 | EFS File System Encryption at Rest | High | 3 |

### L2 — Guarded (Centralized Guardrails)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-dat-010 | CloudTrail S3 Bucket Encryption | High | 3 |
| csmm-dat-011 | S3 Versioning Enabled | High | 3 |
| csmm-dat-012 | RDS Automated Backups Enabled | High | 3 |
| csmm-dat-013 | DynamoDB Point-in-Time Recovery | High | 3 |
| csmm-dat-014 | S3 Server Access Logging Enabled on Sensitive Buckets | High | 3 |
| csmm-dat-015 | No Cross-Account S3 Bucket Access Without Organization Condition | Critical | 4 |

### L3 — Hardened (Security Architecture)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-dat-016 | KMS CMK Key Rotation Enabled | High | 3 |
| csmm-dat-017 | KMS Key Policies — No Overly Permissive Access | Critical | 4 |
| csmm-dat-018 | No Hardcoded Secrets in IaC | Critical | 4 |
| csmm-dat-019 | Data Classification and Tagging Strategy | High | 3 |
| csmm-dat-020 | Secrets Management Architecture | High | 3 |
| csmm-dat-021 | Encryption Key Management Architecture | High | 3 |

### L4 — Verified (Continuous Scanning)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-dat-022 | Backup and Disaster Recovery Architecture | High | 3 |
| csmm-dat-023 | S3 Object Lock for Compliance Data | Medium | 2 |

### L5 — Autonomous (Self-Healing)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-dat-024 | Automated Secret Rotation and Credential Lifecycle | High | 3 |

---

## Progression Path

```mermaid
flowchart LR
  L1["L1 Exposed\nPublic access blocks\nencryption at rest"] -->|"≥ 80%"| L2["L2 Guarded\nVersioning, backups\nautomated patching"]
  L2 -->|"≥ 80%"| L3["L3 Hardened\nData classification\nkey & secrets arch"]
  L3 -->|"≥ 80%"| L4["L4 Verified\nBackup architecture\nObject Lock WORM"]
  L4 -->|"≥ 80%"| L5["L5 Autonomous\nAutomated rotation\nzero static secrets"]

  style L1 fill:#2e7d32,color:#fff
  style L2 fill:#00695c,color:#fff
  style L3 fill:#f57f17,color:#fff
  style L4 fill:#d84315,color:#fff
  style L5 fill:#6a1b9a,color:#fff
```
