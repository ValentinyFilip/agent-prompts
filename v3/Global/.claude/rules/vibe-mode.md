# Vibe Mode Engine (Rapid Execution)

This rule governs **Vibe Mode**—used for minor bug fixes, styling tweaks, quick utility updates, or minor edits to singular files. 

When executing Vibe Mode, I bypass the formal planning phases to enable rapid development, but I must still adhere to project constraints.

---

## Vibe Mode Execution Loop

1. **Alignment Check**: I must still execute the alignment steps defined in the `# Workspace Alignment Step (ALWAYS RUN FIRST AT BOOT)` section of the `# Claude Code Memory Bank & Steering Protocol` chapter (`.claude/rules/memory-bank.md`), including the plugin detection check in the agentic plugin integration rule's section 1, if such a rule is present under `rules/`, to ensure my session memory is aligned with the active project rules, naming conventions, and tech stack boundaries.
   I must show the boot evidence block defined in `.claude/CLAUDE.md` section 3.1. Vibe Mode bypasses the planning documents. It does not bypass the alignment or the proof of read.
2. **Grill Gate**: I must run the pre-delegation contract in `.claude/rules/governance-rules.md` section 4. In Vibe Mode I keep the gate to one compact question round. If the five points are already clear, I state them in one short block and continue at once.
3. **Bypass SDD Specs**: I do not create any documents inside `.specs/`. I explicitly announce: `"Entering Vibe Mode. Bypassing planning documents. Commencing execution."` and jump directly to the `# Phase 4: Pragmatic Execution & Verification (Both Modes)` section of the `# Claude Code Spec-Driven Development, TDD & Delegation Engine` chapter (`.claude/rules/sdd-tdd-engine.md`).
4. **Execution & Verification**: I execute the requested code change. If an agentic plugin integration is present, I delegate investigation, implementation, and testing per its integration rule. If absent, I act directly. Even in Vibe Mode, I must perform pragmatic testing:
   - For backend/core logic, I run the test suite via the terminal, the active semantic tool runner (as governed by the `# Semantic Code Intelligence Protocol` chapter in `.agent-local/local-tools.md`), or a delegated plugin test agent, to verify that my changes are correct before marking the task complete.
   - For simple files, config edits, or UI tweaks, I run build/lint checks in the terminal to verify.
5. **Git Checkpoint**: Upon completing the task, I run `git status` via the terminal and propose a clean, descriptive Git commit message to the user as defined in the `# 2. Agent Safety & Cost Guardrails` section of the `# Local Memory Governance, Safety & Transition Protocol` chapter (`.claude/rules/governance-rules.md`).
