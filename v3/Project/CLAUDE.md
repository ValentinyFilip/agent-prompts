# CLAUDE.md

Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

This file supplements the user-level rule set. The operating contract at `~/.claude/CLAUDE.md` and the rule files in `~/.claude/rules/` are mandatory and are not repeated here. They define the boot sequence, the boot evidence block, the grill gate, the mode announcement, the halt conditions, the automatic delegation rule, the non-negotiable behaviors, the precedence order, the compliance self-check, and the skill requirements. Three of these run for every task, at every size: the boot reads with their evidence block, the grill gate, and delegation to the plugin agents when a plugin integration rule is present. If `~/.claude/CLAUDE.md` is absent, treat the rule files in `~/.claude/rules/` as mandatory anyway and report the missing contract one time.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.

## 5. Documenting sessions

When producing handover docs, Architecture Decision Records (ADRs), or UX designs:
- **Location**: Store session documents inside the active feature folder `.specs/<feature-name>/` (or `.specs/sessions/<kebab-case-topic>/` for general non-feature tasks).
- **Handover Docs**: Write `[topic].handover.original.md` with the ASD-STE100 rules in `~/.claude/rules/language-style.md`.
- **Architecture Decision Records (ADR)**: Write `[topic].adr.original.md` with the ASD-STE100 rules in `~/.claude/rules/language-style.md`.
- **Interactive UX/UI Prototypes**: Save interactive HTML mockups as `[topic].handover.html` (standalone interactive HTML).
- **Dual-File Compression Protocol**: Do NOT delete `*.original.md` files—they are required for human developers and Git diff reviews. Immediately generate the Caveman-compressed counterpart (`*.md`) by applying `~/.claude/rules/steering-contract.md` section 4 directly.

## 6. Repository-Local Tooling

Load the repository-local semantic tool rule:

@.agent-local/local-tools.md

Use the semantic tools in that rule before you open files recursively. If a task is delegated to an agentic plugin agent, pass this priority on to that agent per the agentic plugin integration rule in `~/.claude/rules/`: the agent reads `.agent-local/local-tools.md` first and prefers a listed semantic tool over `Grep`, `Glob`, or `Bash`.
