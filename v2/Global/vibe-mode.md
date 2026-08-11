# Vibe Mode Engine (Rapid Execution)

This rule governs **Vibe Mode**—used for minor bug fixes, styling tweaks, quick utility updates, or minor edits to singular files. 

When executing Vibe Mode, I bypass the formal planning phases to enable rapid development, but I must still adhere to project constraints.

---

## Vibe Mode Execution Loop

1. **Alignment Check**: I must still execute the alignment steps defined in the `# Workspace Alignment Step (ALWAYS RUN FIRST AT BOOT)` section of the `# Cline's Memory Bank & Steering Protocol` chapter (`.clinerules/memory-bank.md`) to ensure my session memory is aligned with the active project rules, naming conventions, and tech stack boundaries.
2. **Bypass SDD Specs**: I do not create any documents inside `.specs/`. I explicitly announce: `"Entering Vibe Mode. Bypassing planning documents. Commencing execution."` and jump directly to the `# Phase 4: Pragmatic Execution & Verification (Both Modes)` section of the `# Global Spec-Driven Development, TDD & IDE MCP Engine` chapter (`.clinerules/sdd-tdd-engine.md`).
3. **Execution & Verification**: I execute the requested code change. Even in Vibe Mode, I must perform pragmatic testing:
   - For backend/core logic, I run the test suite via the terminal or the active semantic tool runner (as governed by the `# Semantic Code Intelligence Protocol` chapter in `.agent-local/local-tools.md`) to verify that my changes are correct before marking the task complete.
   - For simple files, config edits, or UI tweaks, I run build/lint checks in the terminal to verify.
4. **Git Checkpoint**: Upon completing the task, I run `git status` via the terminal and propose a clean, descriptive Git commit message to the user as defined in the `# 2. Agent Safety & Cost Guardrails` section of the `# Local Memory Governance, Safety & Transition Protocol` chapter (`.clinerules/governance-rules.md`).
