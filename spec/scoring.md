# CSMM Scoring Methodology

**Version:** 1.0.0
**Status:** Draft
**Last Updated:** 2026-04-06

---

## Overview

CSMM produces a single numeric overall score from 0 to 100. The score is a weighted aggregate of control assessment results, where weighting reflects:

1. **Severity** of the individual control (how bad is it to fail this control?).
2. **Level** of the control (are foundational controls weighted more than advanced ones?).
3. **Pillar** of the control (are identity controls weighted more than workload controls?).

The scoring model is designed to be honest: an environment with critical gaps at foundational levels cannot achieve a high score by excelling only at advanced controls. This is enforced through level gating.

---

## Step 1 — Severity Weights

Each control carries a `severity_weight` field that assigns a numeric weight used in aggregation.

| Severity Label | Numeric Weight |
|----------------|---------------|
| critical       | 4             |
| high           | 3             |
| medium         | 2             |
| low            | 1             |

A critical control failing has four times the negative score impact of a low control failing.

---

## Step 2 — Control Result

Each control is assessed as either passing or failing:

- `control_result = 1` if the control is fully satisfied.
- `control_result = 0` if the control is not satisfied or cannot be verified.

Partial credit is not awarded. A control either passes or fails.

---

## Step 3 — Level Score

For each combination of level L and pillar P, a level score is calculated as the weighted pass rate across all controls in that level and pillar:

```
level_score(L, P) =
  Σ ( control_result_i × severity_weight_i )
  ─────────────────────────────────────────  × 100
       Σ ( severity_weight_i )
```

Where both sums are over all controls `i` belonging to level L and pillar P.

The level score is a number from 0 to 100. A score of 100 means all controls in that level and pillar are passing; a score of 0 means none are passing.

---

## Step 4 — Level Gating

Before contributing level scores to the overall calculation, gating is applied per pillar.

**Gating rule:** If `level_score(N, P) < 80`, then for pillar P, level N+1 and all higher levels are excluded from the overall score calculation. Their scores are computed and reported for visibility but their contribution to the overall score is set to zero.

| If this fails the 80% threshold | Then these are gated out (per pillar) |
|----------------------------------|---------------------------------------|
| L1                               | L2, L3, L4, L5                        |
| L2                               | L3, L4, L5                            |
| L3                               | L4, L5                                |
| L4                               | L5                                    |

Gating is evaluated independently per pillar. An organization may have all five levels contributing to its IAM score while having L3–L5 gated out of its Networking score.

---

## Step 5 — Level Weights

Each level carries a weight that reflects its relative importance to the overall score. Lower levels carry more weight because they address higher-impact, more systemic risks.

| Level | Level Weight |
|-------|-------------|
| L1    | 0.35        |
| L2    | 0.25        |
| L3    | 0.20        |
| L4    | 0.12        |
| L5    | 0.08        |

Level weights sum to 1.00.

---

## Step 6 — Pillar Weights

Each pillar carries a weight that reflects its relative importance to overall cloud security risk.

| Pillar     | Pillar Weight |
|------------|--------------|
| IAM        | 0.35         |
| Networking | 0.25         |
| Storage    | 0.20         |
| Compute    | 0.20         |

Pillar weights sum to 1.00.

---

## Step 7 — Overall Score

The overall CSMM score is the sum across all levels and pillars of the gated level score multiplied by the level weight and the pillar weight:

```
overall_score =
  Σ_{P ∈ Pillars} Σ_{L ∈ Levels} (
    gated_level_score(L, P) × level_weight(L) × pillar_weight(P)
  )
```

Where `gated_level_score(L, P)` is:
- `level_score(L, P)` if the level is not gated out for pillar P, or
- `0` if the level is gated out for pillar P.

The maximum possible overall score is 100 and is achieved when all controls pass in all levels and all pillars (and therefore no gating is triggered).

---

## Maximum Score Verification

To confirm the formula is correctly bounded at 100:

```
max overall_score
  = Σ_{P} Σ_{L} ( 100 × level_weight(L) × pillar_weight(P) )
  = 100 × Σ_{P} pillar_weight(P) × Σ_{L} level_weight(L)
  = 100 × 1.00 × 1.00
  = 100
```

---

## Worked Example

The following example illustrates how a real assessment score is computed for a single pillar (IAM), then shows how the IAM component combines with the other pillars into an overall score.

### Scenario

An organization has been assessed against the CSMM IAM controls. The following results are recorded.

#### IAM — L1 Controls (level_weight = 0.35)

| Control ID   | Severity  | Weight | Result | Weighted Result |
|--------------|-----------|--------|--------|-----------------|
| IAM-L1-001   | critical  | 4      | PASS   | 4               |
| IAM-L1-002   | critical  | 4      | PASS   | 4               |
| IAM-L1-003   | critical  | 4      | PASS   | 4               |
| IAM-L1-004   | high      | 3      | PASS   | 3               |
| IAM-L1-005   | high      | 3      | FAIL   | 0               |
| IAM-L1-006   | medium    | 2      | PASS   | 2               |
| IAM-L1-007   | high      | 3      | FAIL   | 0               |
| IAM-L1-008   | high      | 3      | PASS   | 3               |
| IAM-L1-009   | high      | 3      | PASS   | 3               |
| **Totals**   |           | **29** |        | **23**          |

```
IAM L1 score = 23 / 29 × 100 = 79.3
```

The IAM L1 score is **79.3%**, which is below the 80% threshold. This means IAM L2 and above are **gated out** for this pillar. Their scores will be computed for reporting but will not contribute to the overall score.

#### IAM — L2 Controls (gated out — shown for reporting only)

| Control ID   | Severity  | Weight | Result | Weighted Result |
|--------------|-----------|--------|--------|-----------------|
| IAM-L2-001   | critical  | 4      | PASS   | 4               |
| IAM-L2-002   | high      | 3      | PASS   | 3               |
| IAM-L2-003   | high      | 3      | PASS   | 3               |
| IAM-L2-004   | high      | 3      | FAIL   | 0               |
| IAM-L2-005   | high      | 3      | PASS   | 3               |
| IAM-L2-006   | high      | 3      | PASS   | 3               |
| **Totals**   |           | **19** |        | **16**          |

```
IAM L2 score (gated — not counted) = 16 / 19 × 100 = 84.2
```

Even though the L2 score itself would pass the gating threshold (84.2% ≥ 80%), it is still gated out because L1 did not pass. The gating chain is broken at L1, so L2, L3, L4, and L5 all contribute zero to the IAM pillar's score in the overall formula.

#### IAM Pillar Contribution to Overall Score

Because gating stops at L1, only the L1 score contributes:

```
IAM contribution =
    (IAM L1 score × L1 weight × IAM pillar weight)
  + (0 × L2 weight × IAM pillar weight)     ← gated
  + (0 × L3 weight × IAM pillar weight)     ← gated
  + (0 × L4 weight × IAM pillar weight)     ← gated
  + (0 × L5 weight × IAM pillar weight)     ← gated

= (79.3 × 0.35 × 0.35)
= 79.3 × 0.1225
= 9.71
```

### Full Assessment — All Pillars

The following table summarizes a complete assessment across all four pillars. IAM L1 is the same as the detailed example above. The other pillar values are illustrative.

| Pillar     | Level | Level Score | Gated? | Level Weight | Pillar Weight | Contribution |
|------------|-------|-------------|--------|--------------|---------------|--------------|
| IAM        | L1    | 79.3        | No     | 0.35         | 0.35          | 9.71         |
| IAM        | L2    | 84.2        | Yes    | 0.25         | 0.35          | 0.00         |
| IAM        | L3    | —           | Yes    | 0.20         | 0.35          | 0.00         |
| IAM        | L4    | —           | Yes    | 0.12         | 0.35          | 0.00         |
| IAM        | L5    | —           | Yes    | 0.08         | 0.35          | 0.00         |
| Networking | L1    | 91.0        | No     | 0.35         | 0.25          | 7.96         |
| Networking | L2    | 85.0        | No     | 0.25         | 0.25          | 5.31         |
| Networking | L3    | 72.0        | No     | 0.20         | 0.25          | 3.60         |
| Networking | L4    | 65.0        | Yes    | 0.12         | 0.25          | 0.00         |
| Networking | L5    | —           | Yes    | 0.08         | 0.25          | 0.00         |
| Storage    | L1    | 95.0        | No     | 0.35         | 0.20          | 6.65         |
| Storage    | L2    | 88.0        | No     | 0.25         | 0.20          | 4.40         |
| Storage    | L3    | 80.0        | No     | 0.20         | 0.20          | 3.20         |
| Storage    | L4    | 75.0        | Yes    | 0.12         | 0.20          | 0.00         |
| Storage    | L5    | —           | Yes    | 0.08         | 0.20          | 0.00         |
| Compute    | L1    | 88.0        | No     | 0.35         | 0.20          | 6.16         |
| Compute    | L2    | 83.0        | No     | 0.25         | 0.20          | 4.15         |
| Compute    | L3    | 78.0        | No     | 0.20         | 0.20          | 3.12         |
| Compute    | L4    | 60.0        | Yes    | 0.12         | 0.20          | 0.00         |
| Compute    | L5    | —           | Yes    | 0.08         | 0.20          | 0.00         |

**Notes on the Networking and Storage gating in this example:**

- Networking L3 score (72.0%) is below 80%, so Networking L4 and L5 are gated out.
- Storage L3 score (80.0%) exactly meets the threshold, so Storage L4 is not gated out due to L3 — however, Storage L4's own score (75.0%) is below 80%, which gates out Storage L5.
- Compute L3 score (78.0%) is below 80%, so Compute L4 and L5 are gated out.

### Overall Score Calculation

```
overall_score = sum of all non-gated contributions

= 9.71   (IAM L1)
+ 0.00   (IAM L2–L5, gated)
+ 7.96   (Networking L1)
+ 5.31   (Networking L2)
+ 3.60   (Networking L3)
+ 0.00   (Networking L4–L5, gated)
+ 6.65   (Storage L1)
+ 4.40   (Storage L2)
+ 3.20   (Storage L3)
+ 0.00   (Storage L4–L5, gated)
+ 6.16   (Compute L1)
+ 4.15   (Compute L2)
+ 3.12   (Compute L3)
+ 0.00   (Compute L4–L5, gated)

= 54.26
```

**Overall CSMM Score: 54.3 / 100**

### Interpretation of Example Score

This score of 54.3 reflects an organization that:

- Has strong L1 performance in Networking (91%), Storage (95%), and Compute (88%) but a critical L1 failure in IAM (79.3%) that gates out all IAM upper levels — pulling the IAM pillar contribution down significantly.
- Has reasonable L2 and L3 coverage in Networking, Storage, and Compute but not yet achieved the L3 threshold needed to unlock L4 in those pillars.
- Has not yet implemented meaningful L4 or L5 controls.

The gating impact is substantial: without gating, this organization would have received partial credit for IAM L2–L5 and for the L4 controls attempted in Storage and Compute. Gating removes this credit, producing a score that honestly reflects the weakest link in each chain.

**Priority action for this organization:** Remediate the two failing IAM L1 controls (IAM-L1-005 access key rotation, IAM-L1-007 unused credential cleanup) to raise IAM L1 above 80%. This single action would unlock IAM L2–L5 scoring and could materially improve the overall score.

---

## Score Interpretation Guide

| Score Range | Interpretation |
|-------------|----------------|
| 0–30        | Critical gaps exist at foundational levels. Immediate remediation required. |
| 31–55       | Foundational controls are partially in place but significant L1/L2 gaps remain. |
| 56–70       | L1 and L2 broadly addressed. Work underway on L3 hardening. |
| 71–83       | Strong foundational posture. Focus on L3 data perimeters and least privilege. |
| 84–92       | L3 hardening in place. Advancing toward L4 governance and auditability. |
| 93–100      | Comprehensive coverage including governance and automation. Leading-edge posture. |

These ranges are guidelines. A score of 70 with all L1 controls passing represents a meaningfully different risk posture from a score of 70 achieved with partial L1 coverage and high L4 scores (which gating is designed to prevent).

---

## Frequently Asked Questions

**Q: Can an organization receive partial credit for a control?**

No. Controls are binary: pass or fail. This keeps assessments objective and comparable across organizations and over time.

**Q: What if a control is not applicable to the organization's environment?**

Controls that are genuinely not applicable (e.g. RDS encryption controls for an organization that does not use RDS) may be marked as N/A. N/A controls are excluded from both the numerator and denominator of the level score calculation. An N/A control does not help or hurt the score — it is simply absent from the pool.

**Q: How often should scores be recalculated?**

CSMM scores should be recalculated whenever significant infrastructure changes are made. At a minimum, organizations should perform a full assessment quarterly. Automated assessment tools can provide continuous score tracking.

**Q: Can two organizations with the same score have different risk profiles?**

Yes. The overall score is a summary. Two organizations with a score of 60 may have achieved it through very different control combinations. Scores should always be read alongside the per-pillar and per-level breakdowns for a complete picture of the security posture.

**Q: What happens to the score when new controls are added in a MINOR version?**

Adding new controls increases the denominator of the relevant level scores, which may reduce existing level scores and overall scores even if no new failures are introduced. This is intentional: new controls represent newly recognized risks. Organizations should review the CHANGELOG when upgrading to a new MINOR version to understand which controls were added and what remediation may be required.
