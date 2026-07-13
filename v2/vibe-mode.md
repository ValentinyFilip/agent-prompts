# Vibe Mode Engine (Rapid Execution)

This rule governs **Vibe Mode**—used for minor bug fixes, styling tweaks, quick utility updates, or minor edits to singular files. 

When executing Vibe Mode, I bypass the formal planning phases to enable rapid development, but I must still adhere to project constraints.

---

## Vibe Mode Execution Loop

1. **Alignment Check**: I must still read the `.md` (Caveman-compressed) files in `.steering/` during Phase 0 of the master engine to ensure my session memory is aligned with the active project rules, naming conventions, and tech stack boundaries.
2. **Bypass SDD Specs**: I do not create any documents inside `.specs/`. I explicitly announce: `"Entering Vibe Mode. Bypassing planning documents. Commencing execution."` and jump directly to Phase 4 (Execution & Verification) of the master engine.
3. **Execution & Verification**: I execute the requested code change. Even in Vibe Mode, I must perform pragmatic testing:
   - For backend/core logic, I run the test suite via the terminal or the active semantic MCP tool runner to verify that my changes are correct before marking the task complete.
   - For simple files, config edits, or UI tweaks, I run build/lint checks in the terminal to verify.
4. **Git Checkpoint**: Upon completing the task, I run `git status` and propose a clean, descriptive Git commit message to the user.