# Spec Mode Engine (SDD)

This rule governs **Spec Mode**—used for complex features, multi-file updates, architecture additions, or database migrations. 

When executing Spec Mode, I must build and maintain specifications in pairs: **Human-Readable Original (`*.original.md`)** first, followed immediately by its **Caveman-Compressed** counterpart (`*.md`).

---

## Phase 1: Requirements Definition (SDD) [SPEC MODE ONLY]
Before designing or writing code, I must align with the user on *what* is being built.
1. **Create `requirements.original.md`**: Inside `.specs/<feature-name>/`, write a highly detailed, professional requirements document containing an Introduction, Glossary, and EARS Requirements Table:
   `WHEN [trigger/condition] THE [system/component] SHALL [expected response]`
2. **Halt for Review**: Once the original is written, I must halt and instruct the user to inspect `.specs/<feature-name>/requirements.original.md` (e.g., via git diff, local editor, or terminal diff). I will not generate the compressed version or proceed until the user explicitly approves the written file.
3. **Compress to `requirements.md`**: Once approved, generate the corresponding Caveman-shorthand version of the same requirements (removing articles, fluff, and structuring it into brief, high-density keywords according to the `# Caveman Compression Rules` section of this chapter).

---

## Phase 2: Technical Design (SDD) [SPEC MODE ONLY]
Once requirements are approved, I will translate *what* to build into *how* to build it.
1. **Create `design.original.md`**: Inside `.specs/<feature-name>/`, write a highly detailed, professional design document containing an Architecture Overview, an ASCII Flowchart, Components & Interfaces, Data Models, Correctness Properties, and an Error Handling Matrix.
2. **Halt for Review**: Once the original is written, I must halt and instruct the user to inspect `.specs/<feature-name>/design.original.md` (e.g., via git diff, local editor, or terminal diff). I will not generate the compressed version or proceed until the user explicitly approves the written file.
3. **Compress to `design.md`**: Once approved, generate the corresponding Caveman-shorthand version of the same design (keeping class signatures, database types, and ASCII flowcharts, but stripping explanatory text and descriptive sentences according to the `# Caveman Compression Rules` section of this chapter).

---

## Phase 3: Task Planning & Dependencies [SPEC MODE ONLY]
With the design approved, I will organize the execution.
1. **Create `tasks.original.md`**: Inside `.specs/<feature-name>/`, write a detailed, wave-based task checklist with logical dependencies (Wave 1: Database, Wave 2: Backend, Wave 3: Integration/APIs).
2. **Halt for Review**: Once the original is written, I must halt and instruct the user to inspect `.specs/<feature-name>/tasks.original.md` (e.g., checking wave boundaries, task descriptions, and dependencies). I will not proceed to the execution phase or modify any code files until the user explicitly approves the task plan.
3. **Compress to `tasks.md`**: Once approved, generate the corresponding Caveman-shorthand version of the same task list (keeping wave-based checkboxes and dependencies, but stripping descriptive fluff).
4. **Transition to Execution**: Upon final approval of the task list, I will commence execution by entering the `# Phase 4: Pragmatic Execution & Verification (Both Modes)` section of the `# Global Spec-Driven Development, TDD & IDE MCP Engine` chapter.

---

## Caveman Compression Rules

When converting a `*.original.md` file to `*.md`, I must follow these strict rules to maximize token economy while preserving 100% technical semantic completeness:
- **Strip Fluff**: Remove all introductory, explanatory, and closing filler sentences.
- **Strip Articles & Helping Verbs**: Strip "the", "a", "an", "is", "are", "would", "should", "should have", "which is", etc.
- **Use Mathematical Operators & Shorthand**: Represent logic using operators (e.g. `->` for transitions, `+` for concatenation, `!=` for inequality, `==` for comparison, `fn` for function).
- **EARS Compression Example**:
  - *Original*: `WHEN a new event is created, THE Slug_Generator SHALL produce a slug by concatenating the organization name and event name.`
  - *Caveman*: `WHEN event created, Slug_Generator SHALL produce slug: concat(org_slug, "-", event_slug)`
