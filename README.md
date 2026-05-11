# citum/skills

AI skills for Citum style authors. Install via the [skills CLI](https://skills.sh):

```bash
npx skills add citum/skills
```

Works with Claude Code, Cursor, Codex, Cline, and other `skills`-compatible agents.

## Skills

| Skill | Description | Trigger phrases |
|-------|-------------|-----------------|
| `style-authoring` | Author and validate Citum YAML citation styles | "create a style", "author a style", "write a citation style", "edit this style" |
| `style-evolve` | Route style work: upgrade, migrate, or create | "upgrade this style", "migrate this CSL", "fix this style" |
| `migrate-research` | Research and repair migration fidelity gaps | "investigate this migration gap", "fix migrate fidelity" |
| `rust-simplify` | Bounded Rust quality pass | "simplify this Rust", "clean up this crate" |

## Requirements

The `style-authoring` skill has two modes:

- **In-repo** (you have a local citum-core checkout): reads Rust type definitions directly as the authoritative source, then validates with `citum render`.
- **External**: uses the published JSON Schema at `docs/schemas/style.json`, which embeds doc comments from the Rust source.

### Citum CLI

Full validation requires the `citum` CLI. Publishing to crates.io is coming soon.
In the meantime, install from source:

```bash
cargo install --git https://github.com/citum-org/citum-core citum-cli
```

## Links

- [citum-core](https://github.com/citum-org/citum-core) — the citation engine
- [Citum docs](https://citum.org) — schema, examples, CLI reference
