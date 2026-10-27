# Networking Pillar — Maturity Model

**Weight:** 20% of overall CSMM score | **Controls:** 23 | **Levels:** 5

---

## Maturity Levels at a Glance

```mermaid
block-beta
  columns 5

  H1["L1\nExposed\n7 controls"]:1 H2["L2\nGuarded\n7 controls"]:1 H3["L3\nHardened\n7 controls"]:1 H4["L4\nVerified\n1 control"]:1 H5["L5\nAutonomous\n1 control"]:1

  A1["No Unrestricted\nInbound SG\n(net-001)"]:1 A2["GuardDuty\nEnabled\n(net-008)"]:1 A3["ALB Origin\nProtection\n(net-015)"]:1 A4["DNS Security\nRoute53\n(net-022)"]:1 A5["GuardDuty\nAuto-Isolate\n(net-023)"]:1

  B1["Restrict Default\nSecurity Group\n(net-002)"]:1 B2["Enable VPC\nFlow Logs\n(net-009)"]:1 B3["WAF on All\nALBs\n(net-016)"]:1 space:1 space:1

  C1["No Auto-Assign\nPublic IPs\n(net-003)"]:1 C2["NACL Deny\nBad Ports\n(net-010)"]:1 C3["Network\nSegmentation\n(net-017)"]:1 space:1 space:1

  D1["ALB/NLB\nHTTPS Only\n(net-004)"]:1 D2["NACL No\nUnrestricted Egress\n(net-011)"]:1 D3["VPC Endpoint\nStrategy\n(net-018)"]:1 space:1 space:1

  E1["CloudFront\nEnforce HTTPS\n(net-005)"]:1 E2["NACL No\nUnrestricted Ingress\n(net-012)"]:1 E3["Egress Filtering\nArchitecture\n(net-019)"]:1 space:1 space:1

  F1["TLS 1.2\nMinimum\n(net-006)"]:1 F2["VPC Peering\nLeast-Access CIDRs\n(net-013)"]:1 F3["DDoS Protection\nStrategy\n(net-020)"]:1 space:1 space:1

  G1["Default VPC\nNot in Use\n(net-007)"]:1 G2["TGW No\nAuto-Accept\n(net-014)"]:1 G3["Endpoint Policies\nNo Wildcard\n(net-021)"]:1 space:1 space:1

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
  style G3 fill:#ffecb3,color:#e65100

  style A4 fill:#ffccbc,color:#bf360c

  style A5 fill:#e1bee7,color:#4a148c
```

---

## Control Reference

### L1 — Exposed (Foundational Hygiene)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-net-001 | No Security Group Allows Unrestricted Inbound Access from Internet | Critical | 4 |
| csmm-net-002 | Restrict Default Security Group to No Traffic | High | 3 |
| csmm-net-003 | No Auto-Assign Public IPs in Subnets | High | 3 |
| csmm-net-004 | ALB/NLB HTTPS Only | High | 3 |
| csmm-net-005 | CloudFront Enforce HTTPS | High | 3 |
| csmm-net-006 | TLS 1.2 Minimum Everywhere | Medium | 2 |
| csmm-net-007 | Default VPC Not in Use or Deleted in All Regions | High | 3 |

### L2 — Guarded (Centralized Guardrails)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-net-008 | GuardDuty Enabled | Critical | 4 |
| csmm-net-009 | Enable VPC Flow Logs | High | 3 |
| csmm-net-010 | NACL Deny Known Bad Ports | High | 3 |
| csmm-net-011 | NACL No Unrestricted Egress | Medium | 2 |
| csmm-net-012 | NACL No Unrestricted Ingress | High | 3 |
| csmm-net-013 | VPC Peering Route Tables Scoped to Least-Access CIDRs | Medium | 2 |
| csmm-net-014 | Transit Gateway Auto-Accept Shared Attachments Disabled | High | 3 |

### L3 — Hardened (Security Architecture)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-net-015 | ALB Origin Protection | High | 3 |
| csmm-net-016 | WAF Associated with ALBs | High | 3 |
| csmm-net-017 | Network Segmentation Strategy | High | 3 |
| csmm-net-018 | VPC Endpoint Strategy for Private Connectivity | High | 3 |
| csmm-net-019 | Egress Filtering Architecture | High | 3 |
| csmm-net-020 | DDoS Protection Strategy | Medium | 2 |
| csmm-net-021 | VPC Endpoint Policies Scoped — No Unrestricted Principal | Critical | 4 |

### L4 — Verified (Continuous Scanning)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-net-022 | DNS Security and Route53 Hardening | High | 3 |

### L5 — Autonomous (Self-Healing)

| ID | Control | Priority | Severity |
|----|---------|----------|----------|
| csmm-net-023 | Anomaly-Driven GuardDuty Auto-Isolate | High | 3 |

---

## Progression Path

```mermaid
flowchart LR
  L1["L1 Exposed\nBlock open ports\nHTTPS everywhere"] -->|"≥ 80%"| L2["L2 Guarded\nFlow logs, NACLs\nGuardDuty"]
  L2 -->|"≥ 80%"| L3["L3 Hardened\nSegmentation, egress\nVPC endpoints, DDoS"]
  L3 -->|"≥ 80%"| L4["L4 Verified\nDrift detection\nDNS security"]
  L4 -->|"≥ 80%"| L5["L5 Autonomous\nAuto-isolate\nanomalies"]

  style L1 fill:#2e7d32,color:#fff
  style L2 fill:#00695c,color:#fff
  style L3 fill:#f57f17,color:#fff
  style L4 fill:#d84315,color:#fff
  style L5 fill:#6a1b9a,color:#fff
```
