# IAM Pillar — Maturity Model

**Weight:** 30% of overall CSMM score | **Controls:** 26 | **Levels:** 5

---

## Maturity Levels at a Glance

```mermaid
block-beta
  columns 5

  H1["L1\nExposed\n8 controls"]:1 H2["L2\nGuarded\n7 controls"]:1 H3["L3\nHardened\n6 controls"]:1 H4["L4\nVerified\n3 controls"]:1 H5["L5\nAutonomous\n2 controls"]:1

  A1["MFA for All\nConsole Users\n(iam-001)"]:1 A2["Deny Leaving\nOrg via SCP\n(iam-009)"]:1 A3["Permission\nBoundaries\n(iam-016)"]:1 A4["IAM Access\nAnalyzer\n(iam-026)"]:1 A5["Auto-Remediate\nL1 Controls\n(aut-001)"]:1

  B1["No Root\nAccess Keys\n(iam-002)"]:1 B2["Deny CloudTrail\nDisable via SCP\n(iam-010)"]:1 B3["Deny IAM Admin\nvia Boundary\n(iam-017)"]:1 B4["Temporary\nElevated Access\n(iam-027)"]:1 B5["Policy Drift\nAuto-Reverted\n(aut-002)"]:1

  C1["Root MFA\nHardware Token\n(iam-003)"]:1 C2["MFA for\nSensitive Actions\n(iam-011)"]:1 C3["Least Privilege\nAccess Review\n(iam-018)"]:1 C4["IaC Scanned\nin CI/CD\n(ver-001)"]:1 space:1

  D1["No IAM Users\nUse SSO\n(iam-004)"]:1 D2["Restrict Regions\nvia SCP\n(iam-012)"]:1 D3["Enforce ABAC\nRBAC Patterns\n(iam-019)"]:1 space:1 space:1

  E1["Rotate Keys\n90 Days\n(iam-005)"]:1 E2["Enforce Temp\nCredentials\n(iam-013)"]:1 E3["Cross-Account\nTrust Controls\n(iam-020)"]:1 space:1 space:1

  F1["Remove Unused\nCreds 45 Days\n(iam-006)"]:1 F2["CloudTrail\nAll Regions\n(gov-001)"]:1 F3["IAM Data\nPerimeter\n(iam-021)"]:1 space:1 space:1

  G1["Password Policy\n14 Chars\n(iam-007)"]:1 G2["CloudTrail Log\nValidation\n(gov-002)"]:1 space:1 space:1 space:1

  I1["Deny Root\nvia SCP\n(iam-008)"]:1 space:1 space:1 space:1 space:1

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
  style F2 fill:#b2dfdb,color:#004d40
  style G2 fill:#b2dfdb,color:#004d40

  style A3 fill:#ffecb3,color:#e65100
  style B3 fill:#ffecb3,color:#e65100
  style C3 fill:#ffecb3,color:#e65100
  style D3 fill:#ffecb3,color:#e65100
  style E3 fill:#ffecb3,color:#e65100
  style F3 fill:#ffecb3,color:#e65100

  style A4 fill:#ffccbc,color:#bf360c
  style B4 fill:#ffccbc,color:#bf360c
  style C4 fill:#ffccbc,color:#bf360c

  style A5 fill:#e1bee7,color:#4a148c
  style B5 fill:#e1bee7,color:#4a148c
```

---

## Control Reference

### L1 — Exposed (Foundational Hygiene)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-iam-001 | Enforce MFA for All Console Users | Critical | 4 |
| csmm-iam-002 | No Root Account Access Keys | Critical | 4 |
| csmm-iam-003 | Root Account MFA with Hardware Token | Critical | 4 |
| csmm-iam-004 | No IAM Users — Use SSO/Federation | High | 3 |
| csmm-iam-005 | Rotate Access Keys Within 90 Days | High | 3 |
| csmm-iam-006 | Remove Unused Credentials After 45 Days | High | 3 |
| csmm-iam-007 | Password Policy Minimum 14 Characters | Medium | 2 |
| csmm-iam-008 | Deny Root Usage via SCP | Critical | 4 |

### L2 — Guarded (Centralized Guardrails)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-iam-009 | Deny Leaving Organization via SCP | Critical | 4 |
| csmm-iam-010 | Deny CloudTrail Disable via SCP | Critical | 4 |
| csmm-iam-011 | Require MFA for Sensitive Actions via SCP | High | 3 |
| csmm-iam-012 | Restrict Regions via SCP | High | 3 |
| csmm-iam-013 | Enforce Temporary Credentials | High | 3 |
| csmm-gov-001 | CloudTrail Enabled in All Regions | Critical | 4 |
| csmm-gov-002 | CloudTrail Log File Validation | High | 3 |

### L3 — Hardened (Privilege Boundaries)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-iam-016 | Permission Boundaries on All Non-Admin Roles | High | 3 |
| csmm-iam-017 | Deny IAM Admin Actions via Boundary | High | 3 |
| csmm-iam-018 | Periodic Least Privilege Access Review | High | 3 |
| csmm-iam-019 | Enforce ABAC/RBAC Access Patterns | Medium | 2 |
| csmm-iam-020 | Cross-Account IAM Trust Controls | High | 3 |
| csmm-iam-021 | IAM Data Perimeter Controls | High | 3 |

### L4 — Verified (Continuous Scanning)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-iam-026 | IAM Access Analyzer Enabled | High | 3 |
| csmm-iam-027 | Temporary Elevated Access (JIT/Break-Glass) | High | 3 |
| csmm-ver-001 | IaC Scanned in CI/CD Before Merge | High | 3 |

### L5 — Autonomous (Self-Healing)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-aut-001 | Auto-Remediation for L1 Controls | High | 3 |
| csmm-aut-002 | Policy Drift Auto-Reverted | High | 3 |

---

## Progression Path

```mermaid
flowchart LR
  L1["L1 Exposed\nBasic identity\nhygiene"] -->|"≥ 80%"| L2["L2 Guarded\nOrg-wide\nguardrails"]
  L2 -->|"≥ 80%"| L3["L3 Hardened\nLeast privilege\n& boundaries"]
  L3 -->|"≥ 80%"| L4["L4 Verified\nContinuous\nanalysis"]
  L4 -->|"≥ 80%"| L5["L5 Autonomous\nSelf-healing\nIAM"]

  style L1 fill:#2e7d32,color:#fff
  style L2 fill:#00695c,color:#fff
  style L3 fill:#f57f17,color:#fff
  style L4 fill:#d84315,color:#fff
  style L5 fill:#6a1b9a,color:#fff
```

**Gating rule:** You must score ≥ 80% on a level before the next level's controls count toward your score.
