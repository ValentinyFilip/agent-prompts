---
paths:
  - "**/*.md"
---

# Markdown Document Rules

These rules apply when I write or change a Markdown file. Claude Code loads this file
only for the paths in the frontmatter. Do not import this file from `CLAUDE.md`.

## 1. Scope

1. This file controls the structure of a Markdown file. `language-style.md` controls
   the prose style: ASD-STE100 or Caveman shorthand.
2. For a `.steering/` or `.specs/` file, `steering-contract.md` also applies. If the two
   files conflict, obey `steering-contract.md`.
3. For a Caveman file, keep the structure rules in sections 2 to 4. Do not expand the
   prose.

## 2. Structure

1. Use one `#` heading at the top of the file. Frontmatter can come before it.
2. Do not skip a heading level. A `###` heading comes after a `##` heading.
3. Put one blank line before and after each heading, list, table, and code block.
4. Use `-` for unordered lists. Use `1.` for ordered steps.
5. Use a numbered list for procedural steps. Use one instruction in each step.

## 3. Code and References

1. Give each fenced code block a language tag, for example `bash`, `csharp`, `sql`,
   `yaml`, or `text`.
2. Put file paths, commands, identifiers, and configuration keys in inline code.
3. Use relative links for files in the same repository.
4. Do not change the text inside a code block when you edit the prose around it.

## 4. Tables

1. Use a table only for data that has the same fields in each row.
2. Keep the column count the same in each row.
3. Do not put a long paragraph in a table cell.

## 5. Verification

1. After a change, read the changed section again.
2. Make sure that each link target exists.
3. If the repository has a Markdown lint configuration, run that lint tool.
