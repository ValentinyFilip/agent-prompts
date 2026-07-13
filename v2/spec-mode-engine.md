# Spec Mode Engine (SDD)

This rule governs **Spec Mode**—used for complex features, multi-file updates, architecture additions, or database migrations. 

When executing Spec Mode, I must build and maintain specifications in pairs: **Human-Readable Original (`*.original.md`)** first, followed immediately by its **Caveman-Compressed** counterpart (`*.md`).

---

## Phase 1: Requirements Definition (SDD) [SPEC MODE ONLY]
Before designing or writing code, I must align with the user on *what* is being built.
1. **Create `requirements.original.md`**: Inside `.specs/<feature-name>/`, write a highly detailed, professional requirements document containing an Introduction, Glossary, and EARS Requirements Table:
   `WHEN [trigger/condition] THE [system/component] SHALL [expected response]`
2. **Compress to `requirements.md`**: Generate the corresponding Caveman-shorthand version of the same requirements (removing articles, fluff, and structuring it into brief, high-density keywords).
3. **Halt Condition**: Present both the professional requirements and the caveman summary in chat. I will not proceed to the design phase without written user approval.

---

## Phase 2: Technical Design (SDD) [SPEC MODE ONLY]
Once requirements are approved, I will translate *what* to build into *how* to build it.
1. **Create `design.original.md`**: Inside `.specs/<feature-name>/`, write a highly detailed, professional design document containing an Architecture Overview, an ASCII Flowchart, Components & Interfaces, Data Models, Correctness Properties, and an Error Handling Matrix.
2. **Compress to `design.md`**: Generate the corresponding Caveman-shorthand version of the same design (keeping class signatures, database types, and ASCII flowcharts, but stripping explanatory text and descriptive sentences).
3. **Halt Condition**: Present both the professional design and the caveman summary in chat. I will not proceed to task planning without written approval.

---

## Phase 3: Task Planning & Dependencies [SPEC MODE ONLY]
With the design approved, I will organize the execution.
1. **Create `tasks.original.md`**: Inside `.specs/<feature-name>/`, write a detailed, wave-based task checklist with logical dependencies (Wave 1: Database, Wave 2: Backend, Wave 3: Integration/APIs).
2. **Compress to `tasks.md`**: Generate the corresponding Caveman-shorthand version of the same task list (keeping wave-based checkboxes and dependencies, but stripping descriptive fluff).
3. **Halt Condition**: Present both task lists and the dependency graph to the user in chat. I will not proceed to Phase 4 (execution) or modify any production/test code files until the user explicitly approves this task plan.

---

## Caveman Compression Rules

When converting a `*.original.md` file to `*.md`, I must follow these strict rules to maximize token economy while preserving 100% technical semantic completeness (these examples are for illustrative purposes):
- **Strip Fluff**: Remove all introductory, explanatory, and closing filler sentences.
- **Strip Articles & Helping Verbs**: Strip "the", "a", "an", "is", "are", "would", "should", "should have", "which is", etc.
- **Use Mathematical Operators & Shorthand**: Represent logic using operators (e.g. `->` for transitions, `+` for concatenation, `!=` for inequality, `==` for comparison, `fn` for function).
- **EARS Compression Example**:
  - *Original*: `WHEN a new event is created, THE Slug_Generator SHALL produce a slug by concatenating the organization name and event name.`
  - *Caveman*: `WHEN event created, Slug_Generator SHALL produce slug: concat(org_slug, "-", event_slug)`