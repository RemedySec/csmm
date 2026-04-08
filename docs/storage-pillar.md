# Storage Pillar — Maturity Model

**Weight:** 20% of overall CSMM score | **Controls:** 21 | **Levels:** 5

---

## Maturity Levels at a Glance

```mermaid
block-beta
  columns 5

  H1["L1\nExposed\n8 controls"]:1 H2["L2\nGuarded\n5 controls"]:1 H3["L3\nHardened\n5 controls"]:1 H4["L4\nVerified\n2 controls"]:1 H5["L5\nAutonomous\n1 control"]:1

  A1["S3 Account\nPublic Block\n(str-001)"]:1 A2["S3 Versioning\nEnabled\n(str-009)"]:1 A3["KMS CMK Key\nRotation\n(str-019)"]:1 A4["Backup & DR\nArchitecture\n(str-026)"]:1 A5["Automated Secret\nRotation\n(aut-005)"]:1

  B1["S3 Bucket\nPublic Block\n(str-002)"]:1 B2["RDS Automated\nBackups\n(str-010)"]:1 B3["No Hardcoded\nSecrets in IaC\n(str-022)"]:1 B4["S3 Object Lock\nCompliance\n(str-027)"]:1 space:1

  C1["S3 Default\nEncryption\n(str-003)"]:1 C2["RDS Auto Minor\nVersion Upgrade\n(str-011)"]:1 C3["Data Classification\n& Tagging\n(str-023)"]:1 space:1 space:1

  D1["S3 Enforce\nSSL/TLS\n(str-004)"]:1 D2["DynamoDB\nPITR\n(str-012)"]:1 D3["Encryption Key\nMgmt Architecture\n(str-024)"]:1 space:1 space:1

  E1["RDS No Public\nAccessibility\n(str-005)"]:1 E2["CloudTrail S3\nEncryption\n(gov-003)"]:1 E3["Secrets Mgmt\nArchitecture\n(str-025)"]:1 space:1 space:1

  F1["RDS Encryption\nat Rest\n(str-006)"]:1 space:1 space:1 space:1 space:1

  G1["EBS Default\nEncryption\n(str-007)"]:1 space:1 space:1 space:1 space:1

  I1["Disable S3\nACLs\n(str-008)"]:1 space:1 space:1 space:1 space:1

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

  style A2 fill:#b2dfdb,color:#004d40
  style B2 fill:#b2dfdb,color:#004d40
  style C2 fill:#b2dfdb,color:#004d40
  style D2 fill:#b2dfdb,color:#004d40
  style E2 fill:#b2dfdb,color:#004d40

  style A3 fill:#ffecb3,color:#e65100
  style B3 fill:#ffecb3,color:#e65100
  style C3 fill:#ffecb3,color:#e65100
  style D3 fill:#ffecb3,color:#e65100
  style E3 fill:#ffecb3,color:#e65100

  style A4 fill:#ffccbc,color:#bf360c
  style B4 fill:#ffccbc,color:#bf360c

  style A5 fill:#e1bee7,color:#4a148c
```

---

## Control Reference

### L1 — Exposed (Foundational Hygiene)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-str-001 | S3 Account-Level Public Access Block | Critical | 4 |
| csmm-str-002 | S3 Bucket-Level Public Access Block | Critical | 4 |
| csmm-str-003 | S3 Default Encryption Enabled | High | 3 |
| csmm-str-004 | S3 Enforce SSL/TLS via Bucket Policy | High | 3 |
| csmm-str-005 | RDS No Public Accessibility | Critical | 4 |
| csmm-str-006 | RDS Encryption at Rest | High | 3 |
| csmm-str-007 | EBS Default Encryption Enabled | High | 3 |
| csmm-str-008 | Disable S3 ACLs (BucketOwnerEnforced) | High | 3 |

### L2 — Guarded (Centralized Guardrails)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-str-009 | S3 Versioning Enabled | High | 3 |
| csmm-str-010 | RDS Automated Backups Enabled | High | 3 |
| csmm-str-011 | RDS Auto Minor Version Upgrade | Medium | 2 |
| csmm-str-012 | DynamoDB Point-in-Time Recovery | High | 3 |
| csmm-gov-003 | CloudTrail S3 Bucket Encryption | High | 3 |

### L3 — Hardened (Security Architecture)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-str-019 | KMS CMK Key Rotation Enabled | High | 3 |
| csmm-str-022 | No Hardcoded Secrets in IaC | High | 3 |
| csmm-str-023 | Data Classification and Tagging Strategy | High | 3 |
| csmm-str-024 | Encryption Key Management Architecture | High | 3 |
| csmm-str-025 | Secrets Management Architecture | High | 3 |

### L4 — Verified (Continuous Scanning)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-str-026 | Backup and Disaster Recovery Architecture | High | 3 |
| csmm-str-027 | S3 Object Lock for Compliance Data | Medium | 2 |

### L5 — Autonomous (Self-Healing)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-aut-005 | Automated Secret Rotation and Credential Lifecycle | High | 3 |

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
