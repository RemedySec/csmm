# Monitoring Pillar — Maturity Model

**Weight:** 15% of overall CSMM score | **Controls:** 21 | **Levels:** 4 (no L1)

---

## Maturity Levels at a Glance

```mermaid
block-beta
  columns 4

  H2["L2\nGuarded\n12 controls"]:1 H3["L3\nHardened\n6 controls"]:1 H4["L4\nVerified\n2 controls"]:1 H5["L5\nAutonomous\n1 control"]:1

  A2["Security Hub\nEnabled\n(mon-001)"]:1 A3["NACL Changes\nAlarm\n(mon-013)"]:1 A4["Threat Hunting\nProgram\n(mon-019)"]:1 A5["Automated IR\nSOAR\n(aut-006)"]:1

  B2["Unauthorized\nAPI Alarm\n(mon-002)"]:1 B3["Gateway Changes\nAlarm\n(mon-014)"]:1 B4["Security Metrics\n& Tracking\n(mon-020)"]:1 space:1

  C2["Root Usage\nAlarm\n(mon-003)"]:1 C3["Route Table\nChanges Alarm\n(mon-015)"]:1 space:1 space:1

  D2["IAM Policy\nChanges Alarm\n(mon-004)"]:1 D3["Config Changes\nAlarm\n(mon-016)"]:1 space:1 space:1

  E2["CloudTrail\nChanges Alarm\n(mon-005)"]:1 E3["Observability\nArchitecture\n(mon-017)"]:1 space:1 space:1

  F2["S3 Policy\nChanges Alarm\n(mon-006)"]:1 F3["Incident Response\nRunbooks\n(mon-018)"]:1 space:1 space:1

  G2["Security Group\nChanges Alarm\n(mon-007)"]:1 space:1 space:1 space:1

  I2["VPC Changes\nAlarm\n(mon-008)"]:1 space:1 space:1 space:1

  J2["Org Changes\nAlarm\n(mon-009)"]:1 space:1 space:1 space:1

  K2["Console No-MFA\nAlarm\n(mon-010)"]:1 space:1 space:1 space:1

  L2["Auth Failures\nAlarm\n(mon-011)"]:1 space:1 space:1 space:1

  M2["CMK Deletion\nAlarm\n(mon-012)"]:1 space:1 space:1 space:1

  style H2 fill:#00695c,color:#fff
  style H3 fill:#f57f17,color:#fff
  style H4 fill:#d84315,color:#fff
  style H5 fill:#6a1b9a,color:#fff

  style A2 fill:#b2dfdb,color:#004d40
  style B2 fill:#b2dfdb,color:#004d40
  style C2 fill:#b2dfdb,color:#004d40
  style D2 fill:#b2dfdb,color:#004d40
  style E2 fill:#b2dfdb,color:#004d40
  style F2 fill:#b2dfdb,color:#004d40
  style G2 fill:#b2dfdb,color:#004d40
  style I2 fill:#b2dfdb,color:#004d40
  style J2 fill:#b2dfdb,color:#004d40
  style K2 fill:#b2dfdb,color:#004d40
  style L2 fill:#b2dfdb,color:#004d40
  style M2 fill:#b2dfdb,color:#004d40

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

## The Config-to-Architecture Shift

Monitoring is where the CSMM philosophy is most visible. L2 is 12 CloudWatch alarms — pure config checks that any scanner covers. **The real maturity starts at L3:**

| Level | What it checks | Example |
|-------|---------------|---------|
| L2 | Is the alarm configured? | "CloudWatch alarm for root usage exists" |
| L3 | Is your observability **architecture** sound? | "All logs flow to a centralized SIEM with cross-account correlation" |
| L4 | Do you have **processes** for proactive security? | "Monthly threat hunts with MITRE ATT&CK coverage tracking" |
| L5 | Does your environment **respond autonomously**? | "SOAR playbooks auto-isolate compromised instances in < 5 min" |

---

## Control Reference

### L2 — Guarded (CloudWatch Alarms)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-mon-001 | AWS Security Hub Enabled | Critical | 4 |
| csmm-mon-002 | CloudWatch Alarm for Unauthorized API Calls | High | 3 |
| csmm-mon-003 | CloudWatch Alarm for Root Account Usage | Critical | 4 |
| csmm-mon-004 | CloudWatch Alarm for IAM Policy Changes | High | 3 |
| csmm-mon-005 | CloudWatch Alarm for CloudTrail Config Changes | High | 3 |
| csmm-mon-006 | CloudWatch Alarm for S3 Bucket Policy Changes | High | 3 |
| csmm-mon-007 | CloudWatch Alarm for Security Group Changes | High | 3 |
| csmm-mon-008 | CloudWatch Alarm for VPC Changes | Medium | 2 |
| csmm-mon-009 | CloudWatch Alarm for Organizations Changes | High | 3 |
| csmm-mon-010 | CloudWatch Alarm for Console Sign-In Without MFA | High | 3 |
| csmm-mon-011 | CloudWatch Alarm for Console Authentication Failures | Medium | 2 |
| csmm-mon-012 | CloudWatch Alarm for CMK Disable/Deletion | High | 3 |

### L3 — Hardened (Observability Architecture)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-mon-013 | CloudWatch Alarm for NACL Changes | Medium | 2 |
| csmm-mon-014 | CloudWatch Alarm for Network Gateway Changes | Medium | 2 |
| csmm-mon-015 | CloudWatch Alarm for Route Table Changes | Medium | 2 |
| csmm-mon-016 | CloudWatch Alarm for Config Configuration Changes | Medium | 2 |
| csmm-mon-017 | Centralized Security Observability Architecture | High | 3 |
| csmm-mon-018 | Incident Response Process and Runbooks | High | 3 |

### L4 — Verified (Proactive Security)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-mon-019 | Threat Hunting Program | Medium | 2 |
| csmm-mon-020 | Security Metrics and Maturity Tracking | Medium | 2 |

### L5 — Autonomous (Self-Healing)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-aut-006 | Automated Incident Response (SOAR) | High | 3 |

---

## Progression Path

```mermaid
flowchart LR
  L2["L2 Guarded\n12 CloudWatch alarms\nSecurity Hub"] -->|"≥ 80%"| L3["L3 Hardened\nObservability arch\nIR runbooks"]
  L3 -->|"≥ 80%"| L4["L4 Verified\nThreat hunting\nsecurity metrics"]
  L4 -->|"≥ 80%"| L5["L5 Autonomous\nSOAR playbooks\nauto-containment"]

  style L2 fill:#00695c,color:#fff
  style L3 fill:#f57f17,color:#fff
  style L4 fill:#d84315,color:#fff
  style L5 fill:#6a1b9a,color:#fff
```
