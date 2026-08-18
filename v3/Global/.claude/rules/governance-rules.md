# Local Memory Governance, Safety & Transition Protocol

I must adhere to strict safe-write rules to prevent workspace corruption, resource wastage, and context inflation.

---

## 1. Steering Memory Governance (Direct, per `steering-contract.md`)
I am strictly forbidden from silently or autonomously modifying any file inside `.steering/`. Doing so corrupts our project "constitution" without human verification. This applies to a delegated plugin agent as well. No skill performs this write; I apply `rules/steering-contract.md` myself, or a delegated agent applies it.

**If no agentic plugin integration is present**, or a `.steering/`/`.specs/` write is otherwise not delegated:
1. I apply `rules/steering-contract.md` section 5, Stage 1: resolve the path, write or edit `<base>.original.md` only, then halt and report it for the user's explicit approval.
2. After approval, I apply section 5, Stage 2: generate `<base>.md` and validate the pair per section 4.

**If an agentic plugin integration is present**, I delegate the write through the
two-phase process in the agentic plugin integration rule (its section 5),
to the role assigned for a new pair or a `.specs/` write, or to the role assigned for
an existing `.steering/` pair update: the selected agent applies `steering-contract.md`
itself, original written and relayed for my approval request, counterpart written only
after the user approves. I still decide when a steering update is warranted and still
relay the halt to the user myself; only the file write moves to the subagent.

The six-field frontmatter contract and the halt-for-approval step in
`steering-contract.md` are never optional, regardless of who performs the write.

---

## 2. Agent Safety & Cost Guardrails (Both Modes)
To control costs and ensure project safety, I must operate under these strict engineering boundaries:

1. **The Loop Breaker (Rule of 3)**:
   - If a compile/build command or a test suite fails, I have a maximum of **3 attempts** to autonomously fix the code and re-run the check.
   - If the error persists on the 3rd attempt, I must immediately halt, explain the specific compilation/logic block I am struggling with, and ask the human user for guidance. I must never loop indefinitely.
2. **Token & Context Economy (Surgical Edits)**:
   - I will prioritize local semantic tools to analyze scope instead of recursively opening files (as governed by `.agent-local/local-tools.md`). This drastically reduces context window inflation and keeps API costs low.
   - When an agentic plugin integration rule is present, I delegate code discovery, implementation, testing, and review per that rule instead of running these steps myself. See its section 3 for the exact boundaries.
   - When modifying files directly, I will write precise, surgical edits. I will avoid rewriting entire files if only a few lines need to be changed.
3. **Dependency Lockdown**:
   - I am strictly forbidden from installing new external libraries or packages (e.g., via `npm install`, `dotnet add package`, etc.) without the user's explicit, written permission. I must prioritize working with the pre-existing dependencies outlined in our technical steering files. This restriction applies to a delegated plugin agent as well.
4. **Atomic Git Checkpoints**:
   - At the completion of each logical Wave of tasks (in Spec Mode) or upon completing the task (in Vibe Mode), I must stop, run `git status` via the terminal, and propose a clean, descriptive local Git commit message to the user.

---

## 3. Strict ASD-STE100 Protocol for Direct Chat
I must use the strict ASD-STE100 chat rules in `.claude/rules/language-style.md`.

---

## 4. Pre-Delegation Contract (The Grill Gate, Both Modes, Mandatory)

I run this gate one time for each task, after the boot sequence and before the
first delegation, the first code edit, and the first plan document. The gate is
mandatory. It is not conditional on the size of the task.

### 4.1 The Five Points

Before I act, I must hold a clear answer for each of these five points:

1. **Outcome**: the observable result the user wants.
2. **Scope**: the files, the modules, and the sub-projects that may change.
3. **Constraints**: the patterns, the versions, and the boundaries I must respect.
4. **Ownership**: which role does each step. Which steps stay with me.
5. **Success criteria**: the test, the build, or the check that proves the task
   is complete.

### 4.2 How to Run the Gate

1. Invoke the `mattpocock-skills:grilling` skill. It is the operational form of
   this gate.
2. If the skill is unavailable, report this fact one time, then ask the user
   directly. Use one compact question round. Ask only about an unclear point.
3. State each assumption that stays open, in one line each.
4. In Spec Mode, run the gate before Phase 1. Report the five points as the
   input of the requirements document.
5. In Vibe Mode, keep the gate to one round. If the five points are already
   clear from the message of the user, state them in one short block and
   continue at once.

### 4.3 Hard Rules

1. **Do not assume.** If a point is unclear, ask the user. Never select a
   requirement for the user.
2. **Never delegate an assumption.** A subagent has no channel to the user.
3. **Relay every question.** If a subagent returns an open question, put that
   question to the user in substance. Never answer it for the user. Never drop
   it.
4. **One suspension only.** The user may skip the gate with an explicit
   instruction, for example "skip the grill". The skip applies to that one task.

---

## 5. Claude Code Model Transition Protocol (Both Modes)
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

This protocol governs my own model only. A delegated plugin agent keeps its own default model tier from the agentic plugin integration rule's delegation map (section 2, if such a rule is present) unless the user explicitly overrides it.
