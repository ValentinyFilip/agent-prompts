---
name: steering-manager
description: Create, update, audit, and Caveman-compress paired project steering documents under .steering/. Use this skill whenever a user mentions steering files, project constitution or memory documents, human-readable *.original.md files, Caveman counterparts, steering metadata, or asks to run /steering-manager.
---

# Steering Manager

Manage project steering as paired Markdown documents:

- `*.original.md`: professional, complete, human-readable source of truth.
- `*.md`: token-efficient Caveman counterpart with identical metadata and technical facts.

Treat `.steering/` as a project constitution. Never infer permission to overwrite content, and never expose names or details copied from unrelated reference projects.

## Command Dispatch

Parse the first argument as one of these commands:

```text
/steering-manager create <type> <name>
/steering-manager update <file-path>
/steering-manager audit
/steering-manager compress <original-file-path>
```

If arguments are missing or invalid, show the valid syntax and request only the missing value. Do not begin a write with unresolved ambiguity.

## Non-Negotiable Metadata Contract

Every created or managed steering document, including both files in a pair, must begin with exactly these six fields in exactly this order:

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

Rules:

1. Do not add, remove, rename, or reorder frontmatter fields.
2. Use a unique lowercase kebab-case `id`. Derive it from scope plus document name, then check all recursively discovered steering IDs for collisions.
3. Keep `title`, `description`, and `applies_to` as quoted YAML strings.
4. Keep `tags` as an inline YAML string array. Require at least one non-empty, lowercase kebab-case tag; remove duplicates while preserving order.
5. Use the actual current local date for `last_updated` in `YYYY-MM-DD` form. Never copy the example placeholder into a generated file.
6. Preserve the six metadata values byte-for-byte between each original and compressed counterpart.
7. Place one blank line after the closing `---` before document content.

## Global Safety Rules

1. Resolve the project root before acting. Operate only inside its `.steering/` directory.
2. Reject paths that escape `.steering/`, including traversal and resolved symlink/junction escapes.
3. Read all existing `.steering/**/*.original.md` and `.steering/**/*.md` needed for collision, pairing, style, and scope checks before writing.
4. Never modify unrelated steering files.
5. Preserve code, commands, paths, glob patterns, type names, schema names, versions, enum values, numeric constants, URLs, state transitions, warnings, and normative force.
6. Do not invent project facts. Ask for missing substantive content.
7. Never leak project-specific examples from another repository. Templates below are deliberately generic.
8. Before every write, report the exact absolute target path. After every write, re-read the file and validate it.

## Pair and Path Conventions

For a logical base path `<base>`:

```text
Human source: <base>.original.md
Compressed:   <base>.md
```

Normalize command paths as follows:

- A path ending in `.original.md` identifies its original directly.
- A path ending in `.md` identifies a pair base; remove `.md`, and then use `<base>.original.md` as the source.
- A path without a Markdown suffix identifies a pair base.
- Reject any other suffix.

For `create`, map type to destination:

| Type argument | Destination base |
|---|---|
| `core` | `.steering/core/<name>` |
| `vision` | `.steering/vision/<name>` |
| `project/<scope>` | `.steering/projects/<scope>/<name>` |
| `service/<scope>` | `.steering/services/<scope>/<name>` |

`<scope>` and `<name>` must be kebab-case path segments. Neither may be empty, `.`, `..`, absolute, or contain a slash beyond the single type separator. Refuse creation if either target exists; direct the user to `update` instead.

## Compression Standard (Delegated to `/caveman-compress`)

When creating or updating the compressed counterpart `<base>.md`, invoke the external `/caveman-compress` skill on the original file:

1. Execute `/caveman-compress <base>.original.md`.
2. Ensure the six-field frontmatter header is preserved exactly byte-for-byte.
3. Ensure the output is written to `<base>.md`.

## `/steering-manager create <type> <name>`

1. Validate type, scope, name, destination containment, nonexistence, and ID uniqueness.
2. Prompt for these metadata values if the user has not already supplied them:
   - `title`
   - `description`
   - `applies_to`
   - `tags`
3. Ask for the substantive steering facts required by the chosen template. Reuse facts already supplied in the conversation; do not ask twice.
4. Set `last_updated` to the current date.
5. Write `<base>.original.md` in polished professional English.
6. Run `/caveman-compress <base>.original.md` to generate `<base>.md`.
7. Re-read both files. Validate metadata equality, pair naming, factual equivalence, and absence of template placeholders.
8. Report both absolute paths and a concise validation result.

### Type templates

Choose the closest template; omit irrelevant sections and add project-supported sections when needed.

**`core`**

```markdown
<six-field frontmatter>

# <Title>

## Purpose
## Product and Domain Boundaries
## Repository Structure
## Technology and Tooling
## Cross-Cutting Conventions
## Testing and Verification
## Build and Common Commands
## Non-Negotiable Rules
```

**`project/<scope>`**

```markdown
<six-field frontmatter>

# <Title>

## Purpose
## Scope and Ownership
## Project Layout
## Domain Model and Lifecycles
## Interfaces and Dependencies
## Data and Persistence
## Configuration
## Testing
## Conventions and Rules
```

**`service/<scope>`**

```markdown
<six-field frontmatter>

# <Title>

## Responsibility
## Service Boundary
## API or Message Contracts
## Processing and State Transitions
## Persistence
## External Integrations
## Reliability and Security
## Operations and Observability
## Testing
## Rules and Known Constraints
```

**`vision`**

```markdown
<six-field frontmatter>

# <Title>

## Vision
## Current State
## Target State
## Architectural Principles
## Migration Stages
## Compatibility and Deprecation Policy
## Success Measures
## Risks and Guardrails
```

## `/steering-manager update <file-path>`

This command uses the Safe-Write Protocol. Approval cannot be assumed from the update request itself.

### Stage 1: propose original

1. Normalize the path and resolve the original/counterpart pair.
2. Read both files. If the original is missing, stop and suggest `create`.
3. Apply only the requested edits to `<base>.original.md` in professional English.
4. Set `last_updated` to the current date.
5. Validate and re-read the original.
6. Halt. Instruct the user to review the proposed original with Git diff or an editor diff, then provide explicit written approval.

### Stage 2: after explicit approval

1. Confirm approval refers to the proposed original.
2. Execute `/caveman-compress <base>.original.md` to refresh `<base>.md`.
3. Copy all six frontmatter values exactly from the original.
4. Re-read and validate the pair.
5. Report the counterpart path and validation result.

## `/steering-manager audit`

Audit recursively and make no changes.

1. Enumerate `.steering/**/*.original.md` and `.steering/**/*.md`.
2. For each file, parse frontmatter and verify:
   - opening and closing `---` delimiters;
   - exactly six fields, in required order;
   - non-empty valid values;
   - kebab-case unique `id`;
   - quoted scalar strings for `id`, `title`, `description`, `applies_to`, and `last_updated`;
   - inline string array with at least one unique kebab-case item for `tags`;
   - real `YYYY-MM-DD` date.
3. For every original, require the same-base compressed counterpart.
4. For every compressed file, require the same-base original.
5. Compare pair metadata for exact equality.
6. Mark a pair out of sync when any original technical fact is missing or contradicted in the compressed file. Also flag a counterpart with an older modification time than its original as a stale candidate requiring semantic comparison; modification time alone is not proof of factual drift.
7. Detect unexpanded placeholders and project-specific leakage from template examples.
8. Return a table:

```text
Status | Original | Counterpart | Findings
PASS   | ...      | ...         | —
FAIL   | ...      | ...         | missing field: tags; counterpart absent
WARN   | ...      | ...         | counterpart older; semantic comparison passes
```

Finish with counts for pairs, passes, warnings, failures, orphan originals, orphan counterparts, metadata errors, and out-of-sync pairs. Exit/report failure when any `FAIL` exists. Do not repair during audit; recommend the exact `update` or `compress` command.

## `/steering-manager compress <original-file-path>`

1. Require an existing `*.original.md` path inside `.steering/` or `.specs/`.
2. Validate frontmatter metadata.
3. Execute `/caveman-compress <original-file-path>`.
4. Report absolute counterpart path and compression status.

`compress` is an explicit refresh command and may replace the counterpart without the two-stage update halt. It must never modify the original.

## Project-Agnostic Few-Shot Examples

These examples demonstrate form and compression only. Never copy placeholder facts into a real project.

### Example 1: project original

```markdown
---
id: "project-orders-api"
title: "Orders API Steering Rules"
description: "Defines the boundaries, architecture, and implementation rules for the orders API."
applies_to: "src/MySystem.Api/Orders/**"
tags: ["orders", "api", "backend"]
last_updated: "2026-07-30"
---

# Orders API Steering Rules

## Purpose

`MySystem.Api` owns order creation and status queries. It delegates facility synchronization to `ExternalCAFM` and must not write to that system's database directly.

## Project Layout

Commands and queries are organized as vertical slices under `Features/<FeatureName>/`. Each slice keeps its endpoint, request, response, validator, and handler together.

## Rules

- New HTTP operations must use the existing endpoint framework; controllers are prohibited.
- External calls must use the registered resilient client and propagate cancellation tokens.
- The legacy `Features/Proccessing/` spelling is persisted in deployment scripts and must not be renamed.
```

### Example 1: compressed counterpart

```markdown
---
id: "project-orders-api"
title: "Orders API Steering Rules"
description: "Defines the boundaries, architecture, and implementation rules for the orders API."
applies_to: "src/MySystem.Api/Orders/**"
tags: ["orders", "api", "backend"]
last_updated: "2026-07-30"
---

# Orders API Steering Rules

## Purpose

`MySystem.Api` owns order creation + status queries; facility sync -> `ExternalCAFM`; NEVER direct-write external DB.

## Layout

Vertical slices: `Features/<FeatureName>/` -> endpoint + request + response + validator + handler.

## Rules

- New HTTP ops MUST use existing endpoint framework; NO controllers.
- External calls MUST use registered resilient client + propagate cancellation tokens.
- Preserve legacy `Features/Proccessing/` spelling (deployment dependency); NO rename.
```

## Completion Checklist

Before declaring any command complete, verify:

- operation stayed inside `.steering/` or `.specs/`;
- no unrelated file changed;
- pair names are correct;
- frontmatter has exactly six ordered fields;
- metadata matches across the pair;
- original uses professional English;
- compressed file preserves every technical fact and normative constraint;
- no foreign project names or unresolved placeholders leaked into generated output;
- written files were re-read successfully.
