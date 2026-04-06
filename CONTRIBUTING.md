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
   controls/<level>/<pillar>/<control-id>.yaml
   ```
   For example: `controls/level-2/identity/iam-mfa-enforcement.yaml`

4. **Ensure your YAML includes all required fields:**
   - `id` — unique control identifier (e.g., `IAM-L2-001`)
   - `name` — short, descriptive control name
   - `level` — maturity level (1–4)
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
id: PILLAR-L#-###                  # e.g., IAM-L2-001
name: ""                           # Short, descriptive name
level: 1                           # Maturity level: 1, 2, 3, or 4
pillar: ""                         # e.g., identity, network, data, logging
priority: critical                 # critical | high | medium | low
severity_weight: 10                # Numeric weight used in maturity scoring

description: >
  Concise description of what the control requires.
  Maximum three sentences. Use the appropriate modal verb
  (must / should / consider) based on priority.

risk: >
  Description of the business impact if this control is absent.
  Focus on operational, financial, or reputational consequences.

verification:
  - step: "Step 1 description"
    tool: "aws-cli / gcloud / az / console / open-source-tool"
    command: ""                    # Optional: exact command to run
  - step: "Step 2 description"
    tool: ""
    command: ""

compliance:
  - framework: "CIS"              # e.g., CIS, NIST CSF, SOC 2, ISO 27001, PCI DSS
    reference: ""                 # e.g., "CIS AWS Foundations 1.5"
  - framework: "NIST CSF"
    reference: ""                 # e.g., "PR.AC-1"

notes: ""                         # Optional: additional context, exceptions, or links
tags: []                          # Optional: e.g., [iam, mfa, authentication]
```

Thank you for helping make CSMM better for the entire cloud security community.
