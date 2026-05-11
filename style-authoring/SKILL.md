---
name: style-authoring
description: "Author and validate Citum citation styles using schema-first workflow. Activate on style authoring requests: 'create a style', 'author a style', 'write a citation style'."
---

# Style Authoring

Use this skill to author, revise, or validate Citum citation styles with machine-backed validation.

## Purpose

Help draft Citum YAML styles that are valid against the published JSON Schema and pass
Citum CLI rendering checks. The workflow is schema-first: validate structure early,
then test behavior. Never present a draft as final unless both schema and CLI validation
pass, or the user explicitly accepts an unverified draft.

## Source of Truth

- **Schema:** `https://raw.githubusercontent.com/citum-org/citum-core/main/docs/schemas/style.json`
- **Examples:** `styles/*.yaml` from [citum-core](https://github.com/citum-org/citum-core)
- **CLI:** `citum render refs -b <refs.json> -s <style.yaml>` (requires local Citum checkout)
- **Docs:** In citum-core: `docs/guides/AUTHORING_LOCALES.md`, `docs/specs/AUTHORING_AGENT_SKILL.md`

## Authoring Workflow

1. **Gather requirements:** Ask for style name, citation format (author-date / note / numeric), bibliography order, name formatting, locator handling, and whether this is a new style, extension, or edit.
2. **Draft:** Create a Citum YAML using real patterns from `styles/*.yaml` in citum-core. Extend existing styles when semantically appropriate.
3. **Validate schema:** Check the draft against the published JSON Schema URL or a local `docs/schemas/style.json`. Report any missing required fields or unsupported keys.
4. **Validate CLI:** Run `citum render refs -b tests/fixtures/references-expanded.json -s <style.yaml>` in a citum-core checkout if available. Report render success and any errors.
5. **Revise:** Fix validation failures. Repeat until both schema and CLI pass.
6. **Return:** Present the final YAML, key assumptions, and both validation statuses.

## Input Checklist

Before drafting, confirm or ask for:
- Style or target publication name
- Citation format (author-date, note-based, numeric, etc.)
- Bibliography ordering (alphabetical, citation-order, etc.)
- Name formatting (initials, particles, et al. rules)
- Locator and citation-specific requirements
- Whether creating new, extending existing, or editing a draft

If underspecified, state explicit assumptions rather than inventing fields.

## Output Contract

Return:
1. Complete Citum YAML (full file or minimal patch)
2. Assumptions made (e.g., "assuming numeric format, alphabetical order")
3. Schema validation status: "passed" or list of failures
4. CLI validation status: "passed", "failed with errors", or "unavailable (no local citum checkout)"
5. If unverified: "Validation not available. Local testing recommended."

Do NOT present as final unless schema AND CLI both pass, or user accepts unverified draft.

## Schema Validation

Use the published JSON Schema to catch structural errors early:
- Check required fields against the schema
- Detect unsupported keys
- Verify type correctness
- If your environment supports it, validate the YAML directly against the schema URL
- Treat schema validity as necessary but not sufficient (CLI must pass too)

## CLI Validation

When a local citum-core checkout is available:
```bash
citum render refs -b tests/fixtures/references-expanded.json -s <your-style.yaml>
```

This tests actual rendering behavior. If it fails, report the error message and ask the user
to share the error log so you can repair the draft.

If no CLI access: produce the draft, report schema status if available, clearly label as
unverified, and recommend the user validate locally.

## Inheritance and Composition

Reuse and extend before writing from scratch:
- Find a similar published style in `styles/*.yaml`
- Extend it by setting `inherit: <parent-style>` and only override what differs
- Keep overrides minimal and localized
- Explain the relationship: "This extends [parent] by overriding [fields]"

Example parents: `apa`, `chicago-author-date`, `mla`, `harvard-numeric`

## Failure Handling

| Failure | Action |
|---------|--------|
| Missing requirements | Ask focused follow-up questions |
| Unknown schema support | Flag uncertainty; don't invent fields |
| Schema validation failure | Show errors; revise before CLI stage |
| Invalid YAML | Fix syntax; revalidate |
| CLI validation failure | Show render errors; iterate repair |
| Conflicts (user vs. examples) | Report conflict; ask user |
| No tooling access | Draft with schema URL; label unverified |

## Examples

**Create a simple numeric style:**
1. Ask: citation format, order, any special requirements
2. Draft extending a numeric parent (e.g., `ieee`)
3. Validate schema: check `info`, `options.processing`, `templates` sections
4. Run CLI: `citum render refs -b tests/fixtures/references-expanded.json -s new-style.yaml`
5. Return validated YAML and assumptions

**Extend an existing style:**
1. Confirm: which parent style, what to override
2. Create minimal patch: `inherit: parent-name`, then only changed fields
3. Schema check: verify overrides match parent's type signatures
4. CLI test: confirm render behavior matches intent
5. Return patch YAML with documented changes

**Patch a specific behavior:**
1. Identify the field(s) that control the behavior
2. Find examples in `styles/*.yaml` of that field in use
3. Draft a minimal override or new rule
4. Validate, test, return with clear assumptions
