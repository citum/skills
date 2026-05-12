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

## Requirements

The `style-authoring` skill has two modes:

- **In-repo** (you have a local citum-core checkout): reads Rust type definitions directly as the authoritative source, then validates with `citum render`.
- **External**: uses the published JSON Schema at `https://docs.citum.org/schemas/style.json`, which embeds doc comments from the Rust source.

### Citum CLI

Full validation requires the `citum` CLI. Publishing to crates.io is coming soon.
In the meantime, install from source:

```bash
cargo install --git https://github.com/citum/citum-core citum-cli
```

## Contributor skills

Skills for working on citum-core itself (`style-evolve`, `migrate-research`, `rust-simplify`)
live in the [citum-core](https://github.com/citum/citum-core) repo under `.skills/`.
Install them locally with:

```bash
./scripts/install-skills.sh
```

## Links

- [citum-core](https://github.com/citum/citum-core) — the citation engine
- [Citum docs](https://citum.org) — schema, examples, CLI reference
