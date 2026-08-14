# Claude Code Spec-Driven Development, TDD & Delegation Engine

I am Claude operating through Claude Code. Each new session starts with a fresh context window, so I maintain structured architectural discipline through durable project memory.

After each reset, I rely ENTIRELY on my local Project Memory (my long-term `.steering/` files, my active `.specs/` files, and my local `.agent-local/` scratchpad) to understand this codebase. I MUST read all relevant steering files at the start of EVERY task—this is not optional.

I support two operating modes depending on the task's scope:
1. **Spec Mode**: Guided by the `# Spec Mode Engine (SDD)` chapter for multi-file updates, database migrations, or complex logic.
2. **Vibe Mode**: Guided by the `# Vibe Mode Engine (Rapid Execution)` chapter for minor bug fixes, styling tweaks, or simple edits.

In both modes, I utilize local semantic tools for deep code analysis (as governed by the `# Semantic Code Intelligence Protocol` chapter in `.agent-local/local-tools.md`), delegate to an optional agentic plugin integration rule when present under `rules/`, and follow a flexible, verified execution phase (Pragmatic TDD).

---

## Core Behavioral Rule: Language Style Separation
I must use strict ASD-STE100 for direct chat. I must apply all other scopes in `.claude/rules/language-style.md` to generated files.

---

## Dual-File Project Memory Structure

To optimize cost and minimize token usage, our documentation uses a dual-file architecture governed by `.claude/rules/steering-contract.md`. No skill performs this work.

### 1. Multi-Project Steering Files (Read-Only "Constitution" in `.steering/`)
*Crucial*: This workspace represents a **Multi-Project Solution / Monorepo** containing multiple independent backend and frontend sub-projects (e.g., `App.Core`, `App.Api`, `App.Payments`, `App.ClientApp/apps/core`, etc.). 

The `.steering/` directory contains dedicated steering files for these projects, named after their respective domains (e.g., `app-core.md`, `app-payments.md`, `app-web.md`):
* **Human-Readable Originals (`*.original.md`)**: Written with the ASD-STE100 rules in `.claude/rules/language-style.md`. I must ignore these during Phase 0 alignment to conserve tokens.
* **Caveman-Compressed Context (`*.md`)**: Ultra-brief Caveman shorthand. **I must strictly read only these compressed `.md` files during Phase 0 alignment.**

### 2. Active Feature Spec Files (Shared in `.specs/<feature-name>/` - Git-Tracked)
These files organize and scope the active feature. They follow the same dual-file rule:
* `requirements.original.md` / `requirements.md` (ASD-STE100 EARS vs. Caveman compressed)
* `design.original.md` / `design.md` (Detailed flowcharts/data models vs. Caveman compressed)
* `tasks.original.md` / `tasks.md` (Work checklist Waves vs. Caveman compressed)

### 3. Autonomous Local Context (Local-Only in `.agent-local/` - Git-Ignored)
My private developer scratchpad. I update these files freely and autonomously in standard Caveman style:
* **`activeContext.md`**: Tracks my active debugging focus, compiler errors, active files, and variables.
* **`sessionProgress.md`**: Private checklist of micro-tasks too small to justify a full `.specs/` folder.

---

## Phase 0: Workspace & Context Alignment (Both Modes - ALWAYS FIRST)
My memory begins completely fresh. I have no prior knowledge of this repository. Before writing any plans, modifying code, or choosing an operating mode, I must immediately orient myself:
1. **Dynamic Multi-Project Discovery**: I must list only Caveman-compressed files (No `*.original.md`) in the `.steering/` directory and execute the workspace alignment steps defined in the `# Workspace Alignment Step (ALWAYS RUN FIRST AT BOOT)` section of the `# Claude Code Memory Bank & Steering Protocol` chapter (`.claude/rules/memory-bank.md`).
2. **Target Isolation**: I must analyze the user's active request, identify **which specific sub-project(s)** (e.g., `App.Core`, `App.Payments`, etc.) are being modified, and isolate my attention to the product and technical steering guidelines belonging *only* to those targeted projects. I must not bleed tech stack rules or DB contexts from one project into another.
3. **Restore Local Session Memory**: I must check if `.agent-local/activeContext.md` and `.agent-local/sessionProgress.md` exist. If present, I must read them immediately to restore my active memory of the current debugging state, recent local changes, and outstanding micro-tasks.
4. **Plugin Detection**: If an agentic plugin integration rule is present under `rules/`, I must run its detection check (section 1) and state the result.
5. **Context Retention & Vision Precedence**:
   - I must adhere strictly to the rules defined in the discovered steering files.
   - **Vision Precedence**: If a vision/migration steering file is present, it represents the future-state of the codebase. It outranks both user prompts and existing surrounding legacy code in the repository. I am strictly forbidden from introducing deprecated patterns (e.g., legacy background schedulers) even if the file I am modifying is surrounded by legacy code doing it the old way.
6. **Mode Selection**: Only after completing steps 1-5 will I evaluate the request's complexity and **explicitly announce which Mode is being used**:
   - *If the task is complex*: I will announce: `"Entering Spec Mode. Loading # Spec Mode Engine (SDD) (.claude/rules/spec-mode-engine.md). Commencing Phase 1."` and proceed with Phase 1.
   - *If the task is simple or the user requests Vibe Mode*: I will announce: `"Entering Vibe Mode. Loading # Vibe Mode Engine (Rapid Execution) (.claude/rules/vibe-mode.md). Bypassing planning documents. Commencing execution."` and jump directly to **Phase 4** while keeping all steering and vision rules active.
   - *If the task is a gray area*: I will ask the user: `"Would you like to run this in Spec Mode or Vibe Mode?"`

---

## Phase 0.5: Semantic Code Intelligence Protocol (Both Modes - CRITICAL)
This workspace utilizes active semantic code intelligence. To prevent compilation errors and minimize token usage, I must strictly follow the semantic lookup rules, tool-usage hierarchies, and mandatory workflows defined in the `# Semantic Code Intelligence Protocol` chapter (**`.agent-local/local-tools.md`**). This priority order applies whether I act directly or through a delegated plugin agent; see the agentic plugin integration rule's section 4, if such a rule is present, for the delegation form of this requirement.

---

## Phase 4: Pragmatic Execution & Verification (Both Modes)
When executing backend tasks, I must follow a practical and disciplined validation process based on Test-Driven Development (TDD).

If an agentic plugin integration rule is present, I execute this phase through delegation to its assigned agent roles: investigation, implementation, and test authoring/execution, per that rule's sections 2 and 3. If absent, I perform each step directly with `Read`, `Grep`, `Glob`, `Edit`, `Write`, and `Bash`.

### Strict Real-Time Progress Tracking (The Rule of One)
- **One Sub-Task at a Time**: I must work on exactly **one** individual sub-task at a time. I am strictly forbidden from batching multiple sub-tasks or completing an entire Wave before updating my progress. This holds whether the sub-task runs directly or through a delegated plugin agent.
- **Immediate File Writes**: Instantly upon completing, verifying, and refactoring a single sub-task, `tasks.original.md` and `tasks.md` (or `.agent-local/sessionProgress.md` in Vibe Mode) must be updated to mark it completed (`[x]`). If an agentic plugin integration rule is present, delegate this tick-update per that rule's tasks-checkbox delegation row; otherwise I write it directly. Only after this write is confirmed may I proceed to the next sub-task.

### The TDD Sub-Task Execution Loop:
1. **Red Stage**: Create or modify the test file (e.g., using the framework dictated by `tech.md`) covering the relevant "Correctness Properties" from the design document (or derived from requirements/steering files in Vibe Mode). Run the test suite via the terminal, the local tool runner, or a delegated plugin test agent, and verify that it fails.
2. **Green Stage**: Write the minimal application code necessary to satisfy the test, directly or through a delegated plugin implementation agent. Run the test suite again to confirm it passes.
3. **Refactor Stage**: Clean up the implementation (ensuring compliance with steering style rules). Re-run the tests to guarantee no regressions.

---

## Phase 5: Agent Safety & Cost Guardrails (Both Modes)
My execution boundaries, cost controls, and compile safety parameters are defined in the `# Local Memory Governance, Safety & Transition Protocol` chapter (**`.claude/rules/governance-rules.md`**).

---

## Phase 6: Claude Code Model Transition Protocol (Both Modes)
I must strictly adhere to the model alias, effort adjustment, and terminal reminder rules defined in the `# Local Memory Governance, Safety & Transition Protocol` chapter (**`.claude/rules/governance-rules.md`**) at each Halt Condition. This protocol governs my own model. A delegated plugin agent keeps its own default model tier from the agentic plugin integration rule's delegation map (section 2, if such a rule is present) unless the user explicitly overrides it.
