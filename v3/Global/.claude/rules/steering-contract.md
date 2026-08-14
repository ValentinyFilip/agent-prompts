# Steering and Spec File Contract

This rule replaces the `/steering-manager` skill. No skill performs `.steering/` or
`.specs/` writes in this rule set. I apply this contract directly, whether I act
myself or a delegated plugin agent (per the agentic plugin integration rule under
`rules/`, if present, section 5) acts on my behalf.

`.steering/` documents use the full contract in this file: frontmatter, path mapping,
and compression. `.specs/<feature-name>/` documents (`requirements`, `design`,
`tasks`) use their own fixed structure from `spec-mode-engine.md` and only the
compression rules in section 4 of this file; they do not carry the six-field
frontmatter block.

---

## 1. Dual-File Pair

For a logical base path `<base>`:

- `<base>.original.md`: human-readable source, written with the ASD-STE100 rules in
  `.claude/rules/language-style.md`.
- `<base>.md`: Caveman-compressed counterpart, identical technical facts, identical
  frontmatter.

Never overwrite, rename, move, or delete the original when writing the counterpart.
Never create `<base>.original.original.md` or another backup file.

---

## 2. Six-Field Frontmatter Contract (`.steering/` only)

Every `.steering/` document, both files of the pair, starts with exactly these six
fields, in exactly this order:

```yaml
---
id: "<kebab-case-id>"
title: "<human-readable-title>"
description: "<short-1-2-sentence-summary>"
applies_to: "<glob-pattern-or-path>"
tags: ["<tag1>", "<tag2>"]
last_updated: "YYYY-MM-DD"
---
```

1. Do not add, remove, rename, or reorder these fields.
2. Use a unique lowercase kebab-case `id`. Check existing `.steering/**/*.md` ids for
   collisions before assigning one.
3. Keep `title`, `description`, and `applies_to` as quoted YAML strings.
4. Keep `tags` as an inline YAML string array, at least one lowercase kebab-case tag,
   no duplicates.
5. Use the real current local date for `last_updated`, in `YYYY-MM-DD` form.
6. Preserve all six values byte-for-byte between the original and the counterpart.
7. Place one blank line after the closing `---` before the document content.

---

## 3. Path Mapping (`.steering/` only)

| Document covers | Destination base |
|---|---|
| Global product scope, business domains, feature catalog | `.steering/product` |
| Global tech stack, frameworks, runtimes, build commands | `.steering/tech` |
| Monorepo layout, solution projects, directory conventions | `.steering/structure` |
| Global system architecture, diagram, data strategy | `.steering/architecture` |
| Target future state, migration stages, deprecation rules | `.steering/vision` |
| Sub-project domain rules, lifecycle, jobs, feature slices | `.steering/projects/<scope>/<scope>` |
| Sub-project architecture deep-dive | `.steering/projects/<scope>/<scope>-architecture` |
| Sub-project tech stack and configuration | `.steering/projects/<scope>/<scope>-tech` |
| External service integration, raw schema, API contract | `.steering/externals/<scope>/<scope>-glossary` |

`<scope>` is one lowercase kebab-case path segment. `project/<scope>` always uses
`<scope>` as both directory name and filename. Never write
`.steering/projects/<scope>.original.md`,`.steering/projects/<scope>.md`,
`.steering/projects/<scope>/project.original.md`, or
`.steering/projects/<scope>/project.md`.

Before writing, state the resolved absolute original and compressed paths.

---

## 4. Compression (`.steering/` and `.specs/`)

Compress only natural-language prose. Apply the Caveman rules in
`.claude/rules/language-style.md` section 2, plus these steering/spec-specific
preservation rules:

Copy exactly, never compress or alter:
- the complete six-field frontmatter block (`.steering/` only), including order,
  quoting, values, and delimiters;
- all Markdown headings and their hierarchy;
- fenced and indented code blocks, including spacing and comments;
- inline code and its backticks;
- URLs, Markdown link destinations, file paths, commands, identifiers,
  configuration keys, API endpoints, and environment variables;
- product names, proper nouns, dates, versions, numeric values, units, quoted
  errors, and log messages;
- EARS keywords (`WHEN`, `IF`, `THEN`, `SHALL`, `SHALL NOT`, `MAY`);
- list nesting, table structure, diagrams, and meaningful document order.

Preserve every requirement, prohibition, permission, condition, exception,
dependency, rationale, example, and scope boundary. Preserve normative force:
keep `MUST`, `SHALL`, `MUST NOT`, `SHALL NOT`, `SHOULD`, and `MAY` semantically
distinct. Do not add facts, advice, interpretation, or assumptions.

Before writing the compressed file, compare it against the original:
1. The original stays byte-for-byte unchanged.
2. Frontmatter (`.steering/` only) is byte-for-byte identical.
3. Every technical fact and normative constraint is present and uncontradicted.
4. The destination is the canonical sibling `<base>.md`.
5. Re-read both files after writing and repeat this check.

---

## 5. Write Protocol (Create, Update, and Delegated Writes)

One protocol covers `.steering/` create, `.steering/` update, and `.specs/` writes.
Always halt before compressing, even on a first-time create; do not treat create as
exempt from approval.

**Stage 1 — propose the original:**
1. Resolve the path (section 3, for `.steering/`) or use the fixed `.specs/` name
   from `spec-mode-engine.md`.
2. For an update, read the existing original and counterpart first.
3. Write or edit `<base>.original.md` only, with the frontmatter contract (section 2,
   `.steering/` only) and the ASD-STE100 rules in `language-style.md`.
4. Halt. Report the absolute path and the content or diff. Wait for the user's
   explicit approval. Do not proceed on silence.

**Stage 2 — after explicit approval:**
1. Apply section 4 to generate `<base>.md`.
2. Re-read and validate both files against section 4's checklist.
3. Report the counterpart path and validation result.

When this write is delegated to a plugin agent, Stage 1 and Stage 2 are two separate
delegations to the same agent, per the agentic plugin integration rule's delegation
process (its section 5). The subagent never writes `<base>.md`
before the orchestrator confirms the user approved `<base>.original.md`.

---

## 6. Global Safety Rules

1. Act only inside `.steering/` or `.specs/` for this contract's writes.
2. Reject any resolved path that escapes those directories, including traversal.
3. Read all existing `.steering/**/*.original.md` and `.steering/**/*.md` needed for
   collision, pairing, and scope checks before writing.
4. Never modify a file unrelated to the requested change.
5. Never invent a project fact. Ask the user for missing substantive content.
6. Never copy an example or placeholder from an unrelated reference project into a
   generated file.

---

## 7. Audit (Report-Only)

On request, audit `.steering/` without changing anything:
1. Enumerate `.steering/**/*.original.md` and `.steering/**/*.md`.
2. Verify frontmatter: six fields, required order, non-empty values, unique
   kebab-case `id`, quoted scalars, valid `tags` array, real `YYYY-MM-DD` date.
3. Require a same-base counterpart for every original and vice versa.
4. Compare pair metadata for exact equality.
5. Flag a pair where a technical fact is missing or contradicted between the two
   files.
6. Flag any noncanonical layout from section 3 (for example
   `.steering/projects/<scope>.original.md` or `.../project.original.md`).
7. Report a table: PASS, FAIL, or WARN per file or pair. Do not fix anything during
   an audit; migration or correction needs a separate, approved write.
