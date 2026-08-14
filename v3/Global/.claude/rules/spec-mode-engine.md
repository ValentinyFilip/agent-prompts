# Spec Mode Engine (SDD)

This rule governs **Spec Mode**—used for complex features, multi-file updates, architecture additions, or database migrations. 

When executing Spec Mode, I must build and maintain specifications in pairs: **Human-Readable Original (`*.original.md`)** first, followed immediately by its **Caveman-Compressed** counterpart (`*.md`).

If an agentic plugin integration is present, I may delegate repository research to its investigation-role agent before writing requirements or design. The decision of what each phase's file must say, and the halt-for-approval judgment, stay with me. The file write itself moves to the assigned authoring-role agent, through the two-phase process in the agentic plugin integration rule's section 5: it writes the original, I relay it for approval, and only after the user approves do I delegate the compression step. I do not delegate authoring or approval to any planning-role agent.

---

## Phase 1: Requirements Definition (SDD) [SPEC MODE ONLY]
Before designing or writing code, I must align with the user on *what* is being built.
1. **Create `requirements.original.md`**: Inside `.specs/<feature-name>/`, write the requirements document with the ASD-STE100 rules in `.claude/rules/language-style.md`. Include an Introduction, Glossary, and EARS Requirements Table:
   `WHEN [trigger/condition] THE [system/component] SHALL [expected response]`
2. **Halt for Review**: Once the original is written, I must halt and instruct the user to inspect `.specs/<feature-name>/requirements.original.md` (e.g., via git diff, local editor, or terminal diff). I will not proceed until the user explicitly approves the written file.
3. **Compress to `requirements.md`**: Once approved, apply `.claude/rules/steering-contract.md` section 4 directly to generate the corresponding Caveman-shorthand `requirements.md` file.

---

## Phase 2: Technical Design (SDD) [SPEC MODE ONLY]
Once requirements are approved, I will translate *what* to build into *how* to build it.
1. **Create `design.original.md`**: Inside `.specs/<feature-name>/`, write the design document with the ASD-STE100 rules in `.claude/rules/language-style.md`. Include an Architecture Overview, an ASCII Flowchart, Components & Interfaces, Data Models, Correctness Properties, and an Error Handling Matrix.
2. **Halt for Review**: Once the original is written, I must halt and instruct the user to inspect `.specs/<feature-name>/design.original.md` (e.g., via git diff, local editor, or terminal diff). I will not proceed until the user explicitly approves the written file.
3. **Compress to `design.md`**: Once approved, apply `.claude/rules/steering-contract.md` section 4 directly to generate the corresponding Caveman-shorthand `design.md` file.

---

## Phase 3: Task Planning & Dependencies [SPEC MODE ONLY]
With the design approved, I will organize the execution.
1. **Create `tasks.original.md`**: Inside `.specs/<feature-name>/`, write a detailed, wave-based task checklist with logical dependencies (Wave 1: Database, Wave 2: Backend, Wave 3: Integration/APIs). Mark each sub-task with its intended executor: direct action, or delegation to a plugin agent per the agentic plugin integration rule's delegation map, if such a rule is present under `rules/`.
2. **Halt for Review**: Once the original is written, I must halt and instruct the user to inspect `.specs/<feature-name>/tasks.original.md` (e.g., checking wave boundaries, task descriptions, and dependencies). I will not proceed to Phase 4 (execution) or modify any code files until the user explicitly approves the task plan.
3. **Compress to `tasks.md`**: Once approved, apply `.claude/rules/steering-contract.md` section 4 directly to generate the corresponding Caveman-shorthand `tasks.md` file.
4. **Transition to Execution**: Upon final approval, commence Phase 4 (Pragmatic Execution & Verification).
