[![CSMM](https://img.shields.io/badge/CSMM-L2_64%25-blue)](https://github.com/remedysec/csmm)

# Cloud Security Maturity Model (CSMM)

The missing scorecard for cloud security. One score, five pillars, five levels.

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

CSMM is an open, scored, leveled maturity model for AWS cloud security. It defines **WHAT** to secure (controls), **WHEN** to secure it (maturity levels), and **HOW** to measure progress (scoring).

Most security tools stop at configuration checks — is MFA on? Is the bucket public? CSMM starts there but goes far beyond. **The real value is in Levels 3–5**, where CSMM scores the architectural decisions and security processes that separate a compliant org from a truly secure one:

- **Data perimeters** — are your resources locked to trusted networks and identities?
- **ABAC/RBAC** — is your access model structured, or a sprawl of ad-hoc inline policies?
- **Cross-account trust** — do your trust policies prevent confused deputy attacks?
- **JIT/break-glass access** — does anyone have standing admin access to production?
- **Auto-remediation** — does your environment self-heal, or wait for a human?

Configuration scanners can't tell you whether your IAM architecture is sound — that requires scoring design decisions. CSMM gives you the criteria to do that.

---

## Beyond Configuration Checks

```
L1–L2  Configuration & Guardrails     ← What scanners cover
       MFA, SCPs, encryption, CloudTrail

L3     Security Architecture           ← Where CSMM diverges
       Data perimeters, ABAC/RBAC, least privilege
       reviews, cross-account trust, permission boundaries

L4     Continuous Verification         ← Process maturity
       Access analysis, JIT access, IaC security gates,
       drift detection

L5     Autonomous Security             ← Self-healing operations
       Auto-remediation, policy drift auto-revert,
       zero standing privilege
```

L1–L2 gets you compliant. **L3–L5 gets you secure.**

---

## The Framework

```
            ╔═════════════════════════╗
            ║     L5  AUTONOMOUS      ║
            ║   Self-healing, zero-   ║
            ║  trust, auto-remediate  ║
         ╔══╩═════════════════════════╩══╗
         ║         L4  VERIFIED          ║
         ║    Drift detection, CI/CD     ║
         ║     gates, IaC governance     ║
      ╔══╩═══════════════════════════════╩══╗
      ║            L3  HARDENED             ║
      ║     Least privilege, ABAC, data     ║
      ║   perimeters, cross-account trust   ║
   ╔══╩═════════════════════════════════════╩══╗
   ║                L2  GUARDED                ║
   ║        SCPs, flow logs, CloudTrail        ║
   ║       encryption, monitoring alarms       ║
╔══╩═══════════════════════════════════════════╩══╗
║                   L1  EXPOSED                   ║
║    MFA, root lockdown, public access blocks     ║
║    default SG restrictions, basic encryption    ║
╚═════════════════════════════════════════════════╝
┃ IAM 30% ┃ NET 20% ┃ STR 20% ┃ CMP 15% ┃ MON 15% ┃
```

Each level builds on the one below it. **Levels are gated** — you must score ≥80% on a level before the next unlocks. You can't skip MFA by being great at ABAC.

### Levels × Pillars

| | IAM (30%) | Networking (20%) | Storage (20%) | Compute (15%) | Monitoring (15%) |
|:---:|:---:|:---:|:---:|:---:|:---:|
| **L5 Autonomous** | Policy drift auto-revert | Anomaly auto-isolate | Automated secret rotation | Self-healing infra | Automated incident response (SOAR) |
| **L4 Verified** | Access Analyzer, JIT access, IaC gates | DNS security, drift detection | Backup architecture, Object Lock | Hardened image pipeline, SSM-only access | Threat hunting, security metrics |
| **L3 Hardened** | ABAC/RBAC, data perimeters, cross-account trust, least privilege reviews | Network segmentation, VPC endpoints, egress filtering, DDoS strategy | Data classification, key mgmt architecture, secrets management | Container security, workload isolation, patch management | Observability architecture, incident response runbooks |
| **L2 Guarded** | SCPs, temp credentials, CloudTrail | Flow logs, NACLs, GuardDuty | Versioning, backups, encryption | IMDSv2, launch templates | Security Hub, 12 CloudWatch alarms |
| **L1 Exposed** | MFA, root lockdown, SSO, key rotation | No open SSH/RDP, HTTPS, TLS 1.2 | S3 public block, RDS private, encryption | EBS encrypt, no public IP/AMI | — |

### Scoring at a Glance

```
Per-Control Pass/Fail  →  × severity weight  →  Level Score (0–100)
                                                       ↓
                                                 ≥ 80%? Gating check
                                                       ↓
                                              Pillar Score (weighted by level)
                                                       ↓
                                                 × pillar weight
                                                       ↓
                                              Overall Score (0–100)
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
- [docs/iam-pillar.md](docs/iam-pillar.md) — IAM pillar deep dive (26 controls)
- [docs/networking-pillar.md](docs/networking-pillar.md) — Networking pillar deep dive (16 controls)
- [docs/storage-pillar.md](docs/storage-pillar.md) — Storage pillar deep dive (21 controls)
- [docs/compute-pillar.md](docs/compute-pillar.md) — Compute pillar deep dive (17 controls)
- [docs/monitoring-pillar.md](docs/monitoring-pillar.md) — Monitoring pillar deep dive (21 controls)
- [self-assessment/](self-assessment/) — Checklist and scoring guide

---

## How CSMM Compares

| | CIS Benchmarks | AWS SMM v2 | NIST CSF | **CSMM** |
|---|---|---|---|---|
| Type | Checklist | Phases | Framework | Maturity model |
| Levels | None | 4 phases | 4 tiers | 5 levels (gated) |
| Scoring | Pass/fail | None | None | 0–100 (maturity) |
| Scores architecture & process | No | Partially | No | **Yes (L3–L5)** |
| Open source | No (free PDF) | No (AWS-owned) | Yes | Yes (Apache 2.0) |
| Progression path | No | Yes | No | Yes (leveled) |
| Community-owned | No | No | No | Yes |

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
