# Local Memory Governance, Safety & Transition Protocol

I must adhere to strict safe-write rules to prevent workspace corruption, resource wastage, and context inflation.

---

## 1. Steering Memory Governance (Delegated to `/steering-manager`)
I am strictly forbidden from silently or autonomously modifying any file inside `.steering/`. Doing so corrupts our project "constitution" without human verification.

If a finished task or Wave reveals that our long-term memory needs updating (e.g., adding a database table, registering an external connector, or documenting a newly introduced vertical slice pattern):
1. I must invoke: `/steering-manager update <file-path>.original.md` (or `/steering-manager create <type> <name>` for new files).
2. The `/steering-manager` skill will write the proposed original with the ASD-STE100 rules in `.claude/rules/language-style.md`. It will halt and prompt the user to inspect the Git diff.
3. Once approved by the user, the `/steering-manager` skill will automatically generate the Caveman-compressed `*.md` counterpart.

---

## 2. Agent Safety & Cost Guardrails (Both Modes)
To control costs and ensure project safety, I must operate under these strict engineering boundaries:

1. **The Loop Breaker (Rule of 3)**:
   - If a compile/build command or a test suite fails, I have a maximum of **3 attempts** to autonomously fix the code and re-run the check.
   - If the error persists on the 3rd attempt, I must immediately halt, explain the specific compilation/logic block I am struggling with, and ask the human user for guidance. I must never loop indefinitely.
2. **Token & Context Economy (Surgical Edits)**:
   - I will prioritize local semantic tools to analyze scope instead of recursively opening files (as governed by `.claude/rules/local-tools.md`). This drastically reduces context window inflation and keeps API costs low.
   - When modifying files, I will write precise, surgical edits. I will avoid rewriting entire files if only a few lines need to be changed.
3. **Dependency Lockdown**:
   - I am strictly forbidden from installing new external libraries or packages (e.g., via `npm install`, `dotnet add package`, etc.) without the user's explicit, written permission. I must prioritize working with the pre-existing dependencies outlined in our technical steering files.
4. **Atomic Git Checkpoints**:
   - At the completion of each logical Wave of tasks (in Spec Mode) or upon completing the task (in Vibe Mode), I must stop, run `git status` via the terminal, and propose a clean, descriptive local Git commit message to the user.

---

## 3. Strict ASD-STE100 Protocol for Direct Chat
I must use the strict ASD-STE100 chat rules in `.claude/rules/language-style.md`.

---

## 4. Claude Code Model Transition Protocol (Both Modes)
At each Halt Condition, I must explicitly advise the human how to select the appropriate model and effort level in Claude Code. I must use Claude Code's stable model aliases rather than hard-coding dated model versions:

- **At the absolute start of Phase 1 (Initial Setup & Planning)**:
  Before digesting the initial prompt, I will display a quick greeting and remind the human of the recommended Claude Code settings:
  > 💡 *Before Phase 1, run `/model opus` and `/effort high` or `/effort xhigh` in Claude Code.*

- **At the End of Phase 2 (Technical Design Approval)**:
  Prompt the human to switch from the high-reasoning planning model to the rapid task-structuring model:
  > 💡 *Spec design approved. Before Wave-based Task Planning, run `/model sonnet` and `/effort medium` in Claude Code.*

- **At the End of Phase 3 (Task Planning Approval)**:
  Prompt the human to select an execution model before running code:
  > 💡 *Task planning approved. Before the first TDD Wave, run `/model sonnet` and choose `/effort medium` or `/effort low` in Claude Code.*

If an organization policy does not allow the recommended alias or effort level, I must ask the human to select the strongest permitted equivalent. I must not attempt to bypass managed Claude Code settings.
