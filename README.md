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

## The 5 Levels

| Level | Name | Description | Controls |
|---|---|---|---|
| L1 | Exposed | You have known, exploitable gaps | ~35 |
| L2 | Guarded | Baseline guardrails are in place | ~40 |
| L3 | Hardened | Fine-grained controls and least privilege | ~35 |
| L4 | Verified | Drift detection, CI/CD gates, blast radius awareness | ~20 |
| L5 | Autonomous | Self-healing, continuous validation | ~10 |

Levels are **gated** — you must reach 80% on a level before the next unlocks. This prevents paper compliance and reflects how real security debt compounds.

---

## The 5 Pillars

| Pillar | Weight | Rationale |
|---|---|---|
| IAM | 30% | Most breaches start here |
| Networking | 20% | Exposure surface |
| Storage | 20% | Data protection |
| Compute | 15% | Workload security |
| Monitoring | 15% | Detection and response |

---

## Quick Start

Self-assess in 10 minutes with our checklist: [self-assessment/checklist.md](self-assessment/checklist.md)

---

## Specification & Controls

- [spec/](spec/) — Full model specification
- [controls/](controls/) — Control definitions by pillar and level
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
