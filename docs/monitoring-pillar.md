# Monitoring Pillar — Maturity Model

**Weight:** 15% of overall CSMM score | **Controls:** 12 | **Levels:** 5

---

## Maturity Levels at a Glance

```mermaid
block-beta
  columns 5

  H1["L1\nExposed\n2 controls"]:1 H2["L2\nGuarded\n4 controls"]:1 H3["L3\nHardened\n3 controls"]:1 H4["L4\nVerified\n2 controls"]:1 H5["L5\nAutonomous\n1 control"]:1

  A1["CloudTrail\nAll Regions\n(mon-001)"]:1 A2["Security Hub\nEnabled\n(mon-003)"]:1 A3["Extended CW\nAlarms — Infra\n(mon-007)"]:1 A4["Threat Hunting\nProgram\n(mon-010)"]:1 A5["Automated IR\nSOAR\n(mon-012)"]:1

  B1["Config Enabled\nAll Regions\n(mon-002)"]:1 B2["CloudTrail Log\nValidation\n(mon-004)"]:1 B3["Centralized\nObservability Arch\n(mon-008)"]:1 B4["Security Metrics\n& Tracking\n(mon-011)"]:1 space:1

  space:1 C2["CloudTrail +\nCloudWatch Logs\n(mon-005)"]:1 C3["Incident Response\nRunbooks\n(mon-009)"]:1 space:1 space:1

  space:1 D2["CIS CloudWatch\nMetric Alarms\n(mon-006)"]:1 space:1 space:1 space:1

  style H1 fill:#2e7d32,color:#fff
  style H2 fill:#00695c,color:#fff
  style H3 fill:#f57f17,color:#fff
  style H4 fill:#d84315,color:#fff
  style H5 fill:#6a1b9a,color:#fff

  style A1 fill:#c8e6c9,color:#1b5e20
  style B1 fill:#c8e6c9,color:#1b5e20

  style A2 fill:#b2dfdb,color:#004d40
  style B2 fill:#b2dfdb,color:#004d40
  style C2 fill:#b2dfdb,color:#004d40
  style D2 fill:#b2dfdb,color:#004d40

  style A3 fill:#ffecb3,color:#e65100
  style B3 fill:#ffecb3,color:#e65100
  style C3 fill:#ffecb3,color:#e65100

  style A4 fill:#ffccbc,color:#bf360c
  style B4 fill:#ffccbc,color:#bf360c

  style A5 fill:#e1bee7,color:#4a148c
```

---

## The Config-to-Architecture Shift

Monitoring is where the CSMM philosophy is most visible. L2 is CloudWatch alarm configuration — pure config checks that any scanner covers. **The real maturity starts at L3:**

| Level | What it checks | Example |
|-------|---------------|---------|
| L1 | Is logging turned on? | "CloudTrail enabled in all regions" |
| L2 | Are the right alarms configured? | "CIS CloudWatch alarms for all 11 required events" |
| L3 | Is your observability **architecture** sound? | "All logs flow to a centralized SIEM with cross-account correlation" |
| L4 | Do you have **processes** for proactive security? | "Monthly threat hunts with MITRE ATT&CK coverage tracking" |
| L5 | Does your environment **respond autonomously**? | "SOAR playbooks auto-isolate compromised instances in < 5 min" |

---

## Control Reference

### L1 — Exposed (Logging Foundations)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-mon-001 | CloudTrail Enabled in All Regions | Critical | 4 |
| csmm-mon-002 | AWS Config Enabled in All Regions | Critical | 4 |

### L2 — Guarded (CloudWatch Alarms)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-mon-003 | AWS Security Hub Enabled | Critical | 4 |
| csmm-mon-004 | CloudTrail Log File Validation | High | 3 |
| csmm-mon-005 | CloudTrail Integrated with CloudWatch Logs | High | 3 |
| csmm-mon-006 | CIS CloudWatch Metric Alarms Configured | High | 3 |

### L3 — Hardened (Observability Architecture)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-mon-007 | Extended CloudWatch Alarms for Infrastructure Changes | Medium | 2 |
| csmm-mon-008 | Centralized Security Observability Architecture | High | 3 |
| csmm-mon-009 | Incident Response Process and Runbooks | High | 3 |

### L4 — Verified (Proactive Security)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-mon-010 | Threat Hunting Program | Medium | 2 |
| csmm-mon-011 | Security Metrics and Maturity Tracking | Medium | 2 |

### L5 — Autonomous (Self-Healing)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-mon-012 | Automated Incident Response (SOAR) | High | 3 |

---

## Progression Path

```mermaid
flowchart LR
  L1["L1 Exposed\nCloudTrail & Config\nenabled everywhere"] -->|"≥ 80%"| L2["L2 Guarded\nSecurity Hub, alarms\nlog validation"]
  L2 -->|"≥ 80%"| L3["L3 Hardened\nObservability arch\nIR runbooks"]
  L3 -->|"≥ 80%"| L4["L4 Verified\nThreat hunting\nsecurity metrics"]
  L4 -->|"≥ 80%"| L5["L5 Autonomous\nSOAR playbooks\nauto-containment"]

  style L1 fill:#2e7d32,color:#fff
  style L2 fill:#00695c,color:#fff
  style L3 fill:#f57f17,color:#fff
  style L4 fill:#d84315,color:#fff
  style L5 fill:#6a1b9a,color:#fff
```
