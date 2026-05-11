# Citum Skills

AI skills for authoring and maintaining Citum citation styles. Install into Claude Code, Codex, Cursor, and other AI agents.

## Installation

```bash
npx skills add citum/skills
```

Install a single skill:

```bash
npx skills add citum/skills --skill style-authoring
```

## Skills

| Skill | Description | Trigger |
|-------|-------------|---------|
| `style-authoring` | Author and validate Citum YAML styles with schema-first workflow | "author a style", "create a style", "write a YAML style" |
| `style-evolve` | Route Citum style work (upgrade, migrate, create) | "upgrade", "migrate", "create", style authoring requests |
| `migrate-research` | Investigate Citum migration fidelity gaps | "research a migration gap", "classify migration failures" |
| `rust-simplify` | Simplify and clean up Citum Rust code | "simplify", "refactor", "clean up Rust" |

## Documentation

Read the Citum project docs in [citum-core](https://github.com/citum-org/citum-core):
- Authoring workflow: `docs/guides/AUTHORING_LOCALES.md`, `docs/specs/AUTHORING_AGENT_SKILL.md`
- Style patterns: `styles/` directory and example YAML files
- Schema reference: `docs/schemas/style.json`
