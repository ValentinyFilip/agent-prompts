---
paths:
  - "**/*.sql"
  - "**/Migrations/**/*.cs"
---

# SQL and Entity Framework Core Migration Rules

These rules apply when I read or change SQL scripts or Entity Framework Core
migrations. Claude Code loads this file only for the paths in the frontmatter. Do not
import this file from `CLAUDE.md`.

## 1. Precedence

1. If a `.steering/` tech file of the target sub-project gives a different rule, obey
   the steering file.
2. A change to the database schema is complex work. Use Spec Mode for it, per
   `sdd-tdd-engine.md`. Use Vibe Mode only on an explicit instruction of the user.

## 2. Migrations

1. Do not edit a migration that is already applied to a shared database. Add a new
   migration.
2. Create a migration with `dotnet ef migrations add <Name>`. Do not write the
   migration file or the model snapshot by hand.
3. Read the generated `Up` and `Down` methods before you continue.
4. Make sure that `Down` reverses `Up`.
5. Give the migration a name that describes the schema change, for example
   `AddOrderShippedAt`.

## 3. Data Safety

1. If a change drops a table, drops a column, or narrows a column type, stop.
   Report the data loss to the user. Continue only after explicit approval.
2. If a column becomes required, give a default value or a data migration for the
   existing rows.
3. Do not run `dotnet ef database update` against a shared or production database.

## 4. SQL Text

1. Use parameters for every value. Do not build SQL with string concatenation or
   string interpolation.
2. In Entity Framework Core raw SQL, use `FromSql` or `ExecuteSql` with an
   interpolated `FormattableString`. Do not use the `Raw` variants with user input.
3. Use the keyword case and the naming convention of the existing scripts.
4. If the repository makes manual scripts idempotent, make the new script idempotent.

## 5. Verification

1. After a migration change, run `dotnet build` for the project that contains the
   migration.
2. If the repository has a migration script check, run
   `dotnet ef migrations script --idempotent` and read the output for the new change.
