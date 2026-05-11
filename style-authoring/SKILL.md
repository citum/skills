---
name: style-authoring
description: "Author and validate Citum citation styles using schema-first workflow. Activate on style authoring requests: 'create a style', 'author a style', 'write a citation style'."
---

# Style Authoring

Use this skill to author, revise, or validate Citum citation styles with machine-backed validation.

## Purpose

Help draft Citum YAML styles that are valid against the published JSON Schema and pass
Citum CLI rendering checks. Never present a draft as final unless both schema and CLI
validation pass, or the user explicitly accepts an unverified draft.

## Context Detection

**Check which path applies before starting:**

- **In-repo** (preferred): `crates/citum-schema-style/src/` is present. Read Rust types
  directly — they are the authoritative source. JSON Schema is secondary confirmation.
- **External**: No local citum-core checkout. Use the published JSON Schema (which embeds
  doc comments from the Rust source) as the primary field-validity anchor.

## Source of Truth

| Source | In-repo | External |
|--------|---------|----------|
| Field definitions | `crates/citum-schema-style/src/` (Rust types) | `docs/schemas/style.json` (has embedded doc comments) |
| Behavioral patterns | `styles/*.yaml` examples | `styles/*.yaml` on GitHub |
| Rendering behavior | `citum render` (run locally) | Describe steps; label output unverified |

Schema URL: `https://docs.citum.org/schemas/style.json`

## Authoring Workflow

### In-repo path

1. **Read types:** Scan `crates/citum-schema-style/src/` for the relevant structs/enums.
   Build your mental model from Rust — don't guess field names.
2. **Read examples:** Find the closest match in `styles/*.yaml` and extend it.
3. **Draft:** Create YAML using documented patterns. Set `inherit:` to a parent when appropriate.
4. **CLI validate:** `citum render refs -b tests/fixtures/references-expanded.json -s <style.yaml>`
5. **Revise** until render passes.
6. **Return** YAML, assumptions, and CLI status.

### External path

1. **Fetch schema:** Load `https://docs.citum.org/schemas/style.json`. Field `description`
   values come from Rust `///` doc comments — they are authoritative.
2. **Read examples:** Browse `styles/*.yaml` on GitHub for inheritance patterns.
3. **Gather requirements:** Confirm style name, citation format, bibliography order, name
   formatting, locators, create/extend/edit mode.
4. **Draft** using schema-verified fields only. Never invent undocumented keys.
5. **Schema validate:** Check required fields, unsupported keys, type correctness.
6. **Return** YAML, assumptions, schema status, and unverified CLI note.

## Input Checklist

Before drafting, confirm or ask for:
- Style or target publication name
- Citation format (author-date, note-based, numeric)
- Bibliography ordering (alphabetical, citation-order)
- Name formatting (initials, particles, et al. rules)
- Locator and citation-specific requirements
- Whether creating new, extending existing, or editing a draft

State explicit assumptions for anything underspecified rather than inventing fields.

## Output Contract

Return:
1. Complete Citum YAML (full file or minimal patch)
2. Assumptions made
3. Schema validation status: passed / failures listed
4. CLI validation status: passed / errors / unavailable
5. If unverified: "Validation not available. Local testing recommended."

Do NOT present as final unless schema AND CLI both pass, or user accepts unverified draft.

## Schema Discipline

- Never invent undocumented keys, sections, or inheritance mechanisms
- Prefer copying and adapting patterns from `styles/*.yaml`
- Preserve valid YAML structure and consistent indentation
- Flag uncertainty when schema support for a requested feature is unknown
- Prefer minimal valid changes when editing an existing style

## CLI Validation

**In-repo:** the binary is already built. Run:
```bash
citum render refs -b tests/fixtures/references-expanded.json -s <your-style.yaml>
```

**External (CLI installed):** install from source while crates.io publishing is pending:
```bash
cargo install --git https://github.com/citum-org/citum-core citum-cli
```
Then validate against your own reference fixture.

**No CLI access:** produce the draft, report schema status, clearly label as unverified,
and recommend the user validate locally.

## Inheritance and Composition

- Extend existing styles before writing from scratch
- Set `inherit: <parent-style>` and override only what differs
- Keep overrides minimal and localized
- Reference parents: `apa-7th`, `chicago-author-date-18th`, `ieee`, `mla`

## Failure Handling

| Failure | Action |
|---------|--------|
| Missing requirements | Ask focused follow-up questions |
| Unknown schema support | Flag uncertainty; don't invent fields |
| Schema validation failure | Fix before CLI stage |
| Invalid YAML | Fix syntax; revalidate |
| CLI validation failure | Show render errors; iterate repair |
| Conflicts (user vs. examples) | Report conflict; ask user |
| No tooling access | Draft with schema; label unverified |

## Examples

**Create a numeric style (in-repo):**
1. Read `crates/citum-schema-style/src/options/` for numeric processing options
2. Extend `ieee` or `vancouver` from `styles/`
3. CLI validate; return YAML + assumptions

**Extend a style (external):**
1. Fetch schema; read description fields for allowed fields
2. Find closest parent in `styles/*.yaml` on GitHub
3. Draft with `inherit:` + minimal overrides; schema-validate; return with unverified note

**Patch a behavior:**
1. Identify the controlling field(s) from Rust types or schema descriptions
2. Find usage in `styles/*.yaml` examples
3. Draft a minimal override; validate; return with clear assumptions
