# CLAUDE.md

Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

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
- **Handover Docs**: Write `[topic].handover.original.md` in standard professional English.
- **Architecture Decision Records (ADR)**: Write `[topic].adr.original.md` in standard professional English.
- **Interactive UX/UI Prototypes**: Save interactive HTML mockups as `[topic].handover.html` (standalone interactive HTML).
- **Dual-File Compression Protocol**: Do NOT delete `*.original.md` files—they are required for human developers and Git diff reviews. Immediately generate the Caveman-compressed counterpart (`*.md`) using:
  1. **Primary Tool**: Run `/steering-manager compress <path>`.
  2. **Fallback**: If `/steering-manager` is not available, run `/compress <path>` or `/caveman-compress <path>`.

## 6. Workspace Alignment & Context Loading

At the start of every session:
- **Read Steering Memory**: Read only the Caveman-compressed `*.md` files inside `.steering/` (ignoring `*.original.md` files) to align with product rules, tech stack boundaries, and structure conventions.
- **Restore Local Memory**: Read `.agent-local/activeContext.md` and `sessionProgress.md` if present to restore active local debugging memory.
