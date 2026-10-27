# Contributing to CSMM

Thank you for your interest in contributing to the Cloud Security Maturity Model (CSMM). This project is community-driven, and contributions of all kinds — new controls, improvements to existing ones, and changes to the specification itself — are welcome and appreciated.

---

## Table of Contents

- [Proposing a New Control](#proposing-a-new-control)
- [Control Quality Criteria](#control-quality-criteria)
- [Suggesting Changes to Existing Controls](#suggesting-changes-to-existing-controls)
- [Proposing Changes to the Spec](#proposing-changes-to-the-spec)
- [Code of Conduct](#code-of-conduct)
- [PR Template and YAML Schema](#pr-template-and-yaml-schema)

---

## Proposing a New Control

Follow these steps to submit a new control for inclusion in CSMM:

1. **Fork the repository** and create a feature branch from `main`.

2. **Create a YAML file** for your control following the [control schema](#pr-template-and-yaml-schema) defined below.

3. **Place the file in the correct directory** based on the control's level and pillar:
   ```
   controls/<level-dir>/<pillar>/<control-id>.yaml
   ```
   Level directories use the names: `L1-exposed`, `L2-guarded`, `L3-hardened`, `L4-verified`, `L5-autonomous`. Pillars are: `iam`, `networking`, `data`, `compute`, `monitoring`. For example: `controls/L2-guarded/iam/csmm-iam-018.yaml`

4. **Assign a control ID** using the convention `csmm-<pillar-prefix>-<NNN>` where:
   - Pillar prefixes: `iam`, `net`, `dat`, `cmp`, `mon`
   - `NNN` is a zero-padded three-digit number (e.g. `001`, `023`)
   - IDs are **globally unique** across all levels within a pillar — do not reuse numbers across levels
   - Before assigning an ID, find the highest existing ID for the pillar: `ls controls/*/<pillar>/ | sort` — your new control gets the next number (e.g. `ls controls/*/networking/ | sort`)
   - File name must match the `id` field exactly: `csmm-iam-018.yaml` contains `id: "csmm-iam-018"`
   - **Do not submit a PR with a placeholder ID** (e.g. `csmm-iam-999`) — maintainers will assign the correct ID during review if you are unsure

5. **Ensure your YAML includes all required fields:**
   - `id` — unique control identifier following the convention above (e.g., `csmm-iam-018`)
   - `name` — short, descriptive control name
   - `level` — maturity level (1–5)
   - `pillar` — the security pillar this control belongs to
   - `priority` — `critical`, `high`, `medium`, or `low`
   - `severity_weight` — numeric weight for scoring purposes
   - `description` — what the control requires (3 sentences or fewer)
   - `risk` — business impact if the control is not implemented
   - `verification` — step-by-step instructions to verify the control is in place
   - `compliance` — one or more compliance framework mappings (e.g., CIS, NIST, SOC 2)

5. **Open a Pull Request** against `main` and apply the `new-control` label. Reference any related issues in the PR description.

---

## Control Quality Criteria

All submitted controls must meet the following quality bar before they will be merged:

- **Verifiable without proprietary tooling.** Verification steps must be executable using publicly available tools, cloud-provider native APIs/consoles, or open-source tooling. Controls that require a specific paid product will not be accepted.

- **At least one compliance mapping.** Every control must map to at least one recognized compliance framework or standard (e.g., CIS Benchmarks, NIST CSF, SOC 2, ISO 27001, PCI DSS).

- **Description under 3 sentences.** The `description` field must be concise. If more context is needed, use the `notes` field.

- **Risk explains business impact.** The `risk` field must articulate the real-world business or operational consequence of not implementing the control — not just a restatement of the technical risk.

- **Use the appropriate modal verb by priority:**
  - `critical` controls: use **"must"**
  - `high` controls: use **"should"**
  - `medium` and `low` controls: use **"consider"**

---

## Suggesting Changes to Existing Controls

To propose a modification to an existing control (e.g., updated verification steps, revised compliance mappings, corrected severity weight):

1. Open an issue describing the proposed change and the reason for it.
2. Fork the repo, make your changes to the relevant YAML file, and open a PR referencing the issue.
3. Apply the `control-update` label to the PR.
4. Changes that affect a control's level, pillar, or priority are considered significant and will receive additional review.

---

## Proposing Changes to the Spec

Changes to the CSMM specification itself — including the control schema, maturity level definitions, pillar taxonomy, or scoring model — require an RFC (Request for Comments) process:

1. Open a GitHub Issue with the title prefix `[RFC]:` describing the proposed change and the motivation behind it.
2. The community will discuss the proposal in the issue thread for a minimum of 14 days.
3. If consensus is reached, a maintainer will approve the RFC and a PR implementing the change can be opened and linked to the issue.
4. Spec changes that are backward-incompatible require a major version bump and a migration note in the changelog.

---

## Code of Conduct

This project follows the [Contributor Covenant Code of Conduct](CODE_OF_CONDUCT.md). By participating, you agree to uphold a welcoming and respectful environment for everyone. Please report unacceptable behavior to the maintainers.

---

## PR Template and YAML Schema

When opening a PR for a new control, include a filled-out version of the schema below in your PR description:

```yaml
id: "csmm-<pillar>-<###>"          # e.g., csmm-iam-013
name: ""                           # Short, descriptive name
level: 1                           # Maturity level: 1, 2, 3, 4, or 5
pillar: ""                         # iam | networking | data | compute | monitoring
priority: "critical"               # critical | high | medium | low
severity_weight: 4                 # 4=critical, 3=high, 2=medium, 1=low

description: >
  Concise description of what the control requires. Use specific
  thresholds, tag names, or CLI commands rather than vague language
  like "appropriate" or "periodically". Every control must produce
  a clear pass/fail signal.

risk: >
  Description of the business impact if this control is absent.
  Focus on operational, financial, or reputational consequences —
  not just a restatement of the technical risk.

verification:
  - "Run: aws <service> <command> --query '...' — result must be empty/zero/match-pattern"
  - "Verify <specific resource> has <specific tag/property> set to <specific value>"
  - "Test: <specific command or scenario> — expected output: <specific value>"

compliance:
  - framework: "CIS AWS Foundations"
    control: "1.14"
    version: "3.0"
  - framework: "SOC2"
    control: "CC6.1"
  - framework: "NIST 800-53"
    control: "AC-2(3)"

references:
  - "https://docs.aws.amazon.com/..."
  - "https://docs.aws.amazon.com/..."
```

**Verification step guidelines:** Each step must be a string that produces a clear pass/fail. Prefer concrete CLI commands (`aws ...`), specific tag/property checks, or testable scenarios over phrases like "confirm", "review", or "ensure". See [docs/iam-pillar.md](docs/iam-pillar.md) and existing controls in `controls/L3-hardened/` for examples.

Thank you for helping make CSMM better for the entire cloud security community.
