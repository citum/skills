---
name: style-evolve
description: "AI skill entrypoint for Citum style work. Activate on: 'upgrade', 'migrate', 'create', any style authoring request, or any request to fix/improve/convert a Citum or CSL citation style. Route to the shared workflow docs."
---

# Style Evolve

Use this skill for any Citum style request that should be handled through the shared
style workflow.

Read first (in a citum-core checkout, or fetch from GitHub):
- `docs/policies/STYLE_WORKFLOW_DECISION_RULES.md`
- `docs/guides/STYLE_WORKFLOW_EXECUTION.md`
- `docs/guides/CODEX_SKILLS.md`

Fetch from https://github.com/citum-org/citum-core/blob/main/:
- `docs/policies/STYLE_WORKFLOW_DECISION_RULES.md`
- `docs/guides/STYLE_WORKFLOW_EXECUTION.md`

## Public Modes

- `upgrade`: improve an existing Citum style.
- `migrate`: convert CSL 1.0 source into Citum style YAML.
- `create`: author a new Citum style from source evidence.

## Routing

For complex requests, escalate or coordinate with:
- Schema/architecture decisions → Consult `docs/specs/` files in citum-core
- Migration fidelity gaps → Use `$migrate-research` skill
- Bounded Rust fixes → Use `$rust-simplify` skill
- Style QA validation → Use published schema and CLI validation

## Operating Rules

- Keep fidelity as the hard gate.
- Treat code quality as secondary optimization only.
- Before editing a style, classify it by semantic class and implementation form.
- Profile-family work may require a `create` pass for a hidden family root followed by `upgrade` reduction of the public handles.
- Journal/profile reductions must choose parents from guide-backed authority.
- Keep waves bounded to one family or one clearly related cohort.
- Keep this skill focused on routing and high-level workflow direction.
