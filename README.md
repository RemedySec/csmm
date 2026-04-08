[![CSMM](https://img.shields.io/badge/CSMM-L2_64%25-blue)](https://github.com/remedysec/csmm)

# Cloud Security Maturity Model (CSMM)

DORA metrics for cloud security. One score, five pillars, five levels.

```
L1 ████████████████████░ 82%
L2 █████████████░░░░░░░ 64%
L3 ████████░░░░░░░░░░░░ 41%
L4 ░░░░░░░░░░░░░░░░░░░░ (locked — reach 80% on L3 first)
L5 ░░░░░░░░░░░░░░░░░░░░ (locked — reach 80% on L4 first)
Overall: Level 2 Guarded — 58/100
```

---

## What is CSMM?

CSMM is an open, scored, leveled maturity model for AWS cloud security. It defines **WHAT** to secure (controls), **WHEN** to secure it (maturity levels), and **HOW** to measure progress (scoring). Think "DORA metrics, but for cloud security."

---

## The Framework

```
                        ╔══════════════════════════╗
                        ║     L5  AUTONOMOUS       ║
                        ║   Self-healing, zero-     ║
                        ║   trust, auto-remediate   ║
                     ╔══╩══════════════════════════╩══╗
                     ║       L4  VERIFIED              ║
                     ║   Drift detection, CI/CD gates, ║
                     ║   blast radius, IaC governance   ║
                  ╔══╩══════════════════════════════════╩══╗
                  ║         L3  HARDENED                    ║
                  ║   Least privilege, ABAC, data           ║
                  ║   perimeters, cross-account trust       ║
               ╔══╩════════════════════════════════════════╩══╗
               ║           L2  GUARDED                        ║
               ║   SCPs, flow logs, CloudTrail, encryption,   ║
               ║   monitoring alarms, baseline guardrails     ║
            ╔══╩══════════════════════════════════════════════╩══╗
            ║             L1  EXPOSED                            ║
            ║   MFA, root lockdown, public access blocks,       ║
            ║   default SG restrictions, basic encryption       ║
            ╚════════════════════════════════════════════════════╝
            ┃ IAM 30%  ┃ NET 20%  ┃ STR 20%  ┃ CMP 15% ┃ MON 15% ┃
```

Each level builds on the one below it. **Levels are gated** — you must score ≥80% on a level before the next unlocks. You can't skip MFA by being great at ABAC.

### Levels × Pillars

```mermaid
block-beta
  columns 6

  space:1 H1["🔐 IAM\n30%"]:1 H2["🌐 Networking\n20%"]:1 H3["💾 Storage\n20%"]:1 H4["⚙️ Compute\n15%"]:1 H5["📡 Monitoring\n15%"]:1

  L5["L5\nAutonomous"]:1 A5["Auto-revoke\npolicy drift"]:1 B5["Auto-isolate\nanomalies"]:1 C5["Auto-rotate\nsecrets"]:1 D5["Self-healing\ninfra"]:1 E5["Continuous\nverification"]:1

  L4["L4\nVerified"]:1 A4["Access Analyzer\nunused roles"]:1 B4["Drift detection\nweekly"]:1 C4["Object Lock\nintegrity"]:1 D4["SSM managed\nno SSH keys"]:1 E4["Score tracking\nover time"]:1

  L3["L3\nHardened"]:1 A3["Permission\nboundaries\nABAC"]:1 B3["WAF, origin\nprotection\nVPC endpoints"]:1 C3["CMK rotation\nscoped policies\nsecret rotation"]:1 D3["No wildcard\nLambda perms\nEKS private"]:1 E3["NACL/gateway\nroute table\nalerts"]:1

  L2["L2\nGuarded"]:1 A2["SCPs: deny\nroot, region\nrestrict"]:1 B2["Flow logs\nNACLs\ngateway EPs"]:1 C2["Versioning\nbackups\nencryption"]:1 D2["IMDSv2\nECR scan\nno privileged"]:1 E2["Security Hub\nroot alarm\nAPI alerts"]:1

  L1["L1\nExposed"]:1 A1["MFA all users\nno root keys\nSSO only"]:1 B1["No open SSH\nno open RDP\nHTTPS only"]:1 C1["S3 public\nblock, RDS\nno public"]:1 D1["EBS encrypt\nno public IP\nno public AMI"]:1 E1["(no L1\nmonitoring\ncontrols)"]:1

  style L5 fill:#1a1a2e,color:#e94560
  style L4 fill:#16213e,color:#0f3460
  style L3 fill:#0f3460,color:#e94560
  style L2 fill:#533483,color:#e94560
  style L1 fill:#e94560,color:#1a1a2e

  style A5 fill:#2d2d44,color:#fff
  style B5 fill:#2d2d44,color:#fff
  style C5 fill:#2d2d44,color:#fff
  style D5 fill:#2d2d44,color:#fff
  style E5 fill:#2d2d44,color:#fff

  style A4 fill:#1e3a5f,color:#fff
  style B4 fill:#1e3a5f,color:#fff
  style C4 fill:#1e3a5f,color:#fff
  style D4 fill:#1e3a5f,color:#fff
  style E4 fill:#1e3a5f,color:#fff

  style A3 fill:#1b4f72,color:#fff
  style B3 fill:#1b4f72,color:#fff
  style C3 fill:#1b4f72,color:#fff
  style D3 fill:#1b4f72,color:#fff
  style E3 fill:#1b4f72,color:#fff

  style A2 fill:#6c3483,color:#fff
  style B2 fill:#6c3483,color:#fff
  style C2 fill:#6c3483,color:#fff
  style D2 fill:#6c3483,color:#fff
  style E2 fill:#6c3483,color:#fff

  style A1 fill:#e94560,color:#1a1a2e
  style B1 fill:#e94560,color:#1a1a2e
  style C1 fill:#e94560,color:#1a1a2e
  style D1 fill:#e94560,color:#1a1a2e
  style E1 fill:#e94560,color:#1a1a2e
```

### Scoring at a Glance

```mermaid
flowchart LR
  A["Per-Control\nPass/Fail"] -->|"× severity\nweight"| B["Level Score\n0–100"]
  B -->|"≥ 80%?\nGating check"| C["Pillar Score\nweighted by level"]
  C -->|"× pillar\nweight"| D["Overall Score\n0–100"]

  style A fill:#e94560,color:#fff,stroke:#e94560
  style B fill:#533483,color:#fff,stroke:#533483
  style C fill:#0f3460,color:#fff,stroke:#0f3460
  style D fill:#1a1a2e,color:#e94560,stroke:#e94560
```

| Severity | Weight | | Level | Weight | | Pillar | Weight |
|----------|--------|-|-------|--------|-|--------|--------|
| Critical | 4× | | L1 | 0.35 | | IAM | 30% |
| High | 3× | | L2 | 0.25 | | Networking | 20% |
| Medium | 2× | | L3 | 0.20 | | Storage | 20% |
| Low | 1× | | L4 | 0.12 | | Compute | 15% |
| | | | L5 | 0.08 | | Monitoring | 15% |

---

## Quick Start

Self-assess in 10 minutes with our checklist: [self-assessment/checklist.md](self-assessment/checklist.md)

---

## Specification & Controls

- [spec/](spec/) — Full model specification
- [controls/](controls/) — Control definitions by pillar and level
- [docs/iam-pillar.md](docs/iam-pillar.md) — IAM pillar deep dive (26 controls, visual maturity map)
- [self-assessment/](self-assessment/) — Checklist and scoring guide

---

## How CSMM Compares

| | CIS Benchmarks | AWS SMM v2 | NIST CSF | Prowler | **CSMM** |
|---|---|---|---|---|---|
| Type | Checklist | Phases | Framework | Scanner | Maturity model |
| Levels | None | 4 phases | 4 tiers | None | 5 levels (gated) |
| Scoring | Pass/fail | None | None | 0–100 (compliance) | 0–100 (maturity) |
| Open source | No (free PDF) | No (AWS-owned) | Yes | Partial | Yes (Apache 2.0) |
| Progression path | No | Yes | No | No | Yes (leveled) |
| Community-owned | No | No | No | Partial | Yes |

---

## Tooling

[RemedySec](https://remedysec.com) is the reference implementation — it ingests your AWS environment and returns a live CSMM score with per-control remediation guidance.

CSMM is tooling-agnostic. The spec is open and the scoring model is documented so any scanner can emit a compliant score.

---

## Contributing

Contributions are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on proposing new controls, adjusting weights, or improving the spec.

---

## License

Apache 2.0. See [LICENSE](LICENSE).
