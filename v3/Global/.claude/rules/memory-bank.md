# Claude Code Memory Bank & Steering Protocol

I am Claude operating through Claude Code. Each new session starts with a fresh context window, so I use disciplined project documentation to restore the repository's durable context.

After each reset, I begin completely fresh with zero context. I rely ENTIRELY on our Project Memory to understand the repository and continue work. I MUST read all relevant memory files at the start of EVERY task—this is not optional.

---

## The Dual-File Token-Preservation Protocol (CRITICAL)

To prevent context inflation, save context window tokens, and control costs, our project uses a strict Dual-File Memory Architecture governed by `.claude/rules/steering-contract.md`. I must handle these files with absolute discipline:

### 1. File Formats: Human vs. Agent
* **Human-Readable Originals (`*.original.md`)**:
  - *Purpose*: Written with the ASD-STE100 rules in `.claude/rules/language-style.md`.
  - *Target*: Strictly for human developers to read, edit, and review.
  - *Constraint*: **I am strictly forbidden from reading these files during Phase 0 workspace alignment.** Reading them wastes valuable context tokens.
* **Caveman-Compressed Context (`*.md`)**:
  - *Purpose*: The exact same technical specifications as the original file, but heavily compressed into ultra-brief "Caveman shorthand" (stripping out articles like "the", "a", "an", auxiliary verbs, and filler).
  - *Target*: Strictly for my AI context window.
  - *Constraint*: **I must strictly read only these compressed files during Phase 0 workspace alignment.**

### 2. Steering Management
For creating, updating, auditing, or compressing any file inside `.steering/`, I apply
`.claude/rules/steering-contract.md` directly. No skill performs this write.

If an agentic plugin integration rule is present under `rules/`, the write
itself moves to a delegated plugin agent through that rule's two-phase process: one
agent role for a new pair or a `.specs/` write, another role for an existing
`.steering/` pair update. The selected agent applies the same `steering-contract.md`
rules on my behalf.

---

## Core Memory Directories

### A. Read-Only Team Steering Files (Shared in `.steering/`)
These files are committed to Git and serve as our project constitution. Managed directly, per `steering-contract.md`:
* `[project-name].original.md` (Human) $\rightarrow$ `[project-name].md` (Caveman Shorthand)
* `[project-name]-tech.original.md` (Human) $\rightarrow$ `[project-name]-tech.md` (Caveman Shorthand)
* `structure.original.md` (Human) $\rightarrow$ `structure.md` (Caveman Shorthand)
* `vision.original.md` (Human) $\rightarrow$ `vision.md` (Caveman Shorthand)

### B. Active Feature Spec Files (Shared in `.specs/<feature-name>/`)
These files organize the active feature:
* `requirements.original.md` (Human) $\rightarrow$ `requirements.md` (Caveman Shorthand)
* `design.original.md` (Human) $\rightarrow$ `design.md` (Caveman Shorthand)
* `tasks.original.md` (Human) $\rightarrow$ `tasks.md` (Caveman Shorthand)

### C. Autonomous Local Context (Local-Only in `.agent-local/` - Git-Ignored)
My private developer scratchpad. **These files must be written and maintained exclusively in Caveman Shorthand**:
* **`activeContext.md`**: Tracks my current debugging focus, compiler errors, active files, and variables.
* **`sessionProgress.md`**: Checklist of micro-tasks too small to justify a full `.specs/` folder.

---

## Workspace Alignment Step (ALWAYS RUN FIRST AT BOOT)
When my memory resets and a task begins, I must execute these steps in order before choosing an operating mode or writing code:
1. List only Caveman-compressed files (No `*.original.md`) in the `.steering/` directory.
2. **Read ONLY the `.md` (Caveman-compressed) files** inside `.steering/` to semantically map our Product, Technical, Structure, and Migration boundaries. Do not read the `*.original.md` files.
3. Identify **which specific sub-project(s)** the active task targets, and isolate my attention to the guidelines of those target projects.
4. Check if `.agent-local/activeContext.md` and `.agent-local/sessionProgress.md` exist. If present, **read them immediately** (both are in Caveman shorthand) to restore my active memory of the current debugging state, recent local changes, and outstanding micro-tasks.
5. **Show the proof of read**: I must show the boot evidence block defined in `.claude/CLAUDE.md` section 3.1. One line for each file that I read, plus `absent` for each file that does not exist.

These steps are not optional and they are not conditional on the size of the task. A small edit uses the same alignment as a large feature. I am strictly forbidden from stating a steering fact, a tech stack fact, or a naming convention that I did not read in this session. If I skip the alignment, I must report the deviation in one sentence, run the alignment, and continue.

---

## Memory Bank Updates
My local memory bank files (`.agent-local/activeContext.md` and `sessionProgress.md`) must be updated in brief Caveman style:
1. After discovering new workspace patterns or compiler quirks.
2. At the end of **every single turn** where I make changes or discover crucial local insights.
3. When the user requests to **update memory bank** (I must review all files).

For updating the team-wide `.steering/` files with newly discovered patterns, I must apply `.claude/rules/steering-contract.md` section 5 (update path).

---

## Safe Generation Constraint
- **Language Style**: I must use Caveman style and ASD-STE100 only in the scopes defined in `.claude/rules/language-style.md`.

---

## Governance Pointer
For updating the team-wide `.steering/` files with newly discovered patterns, I must follow the strict approval rules defined in the `# 1. Steering Memory Governance (Direct, per steering-contract.md)` section of the `# Local Memory Governance, Safety & Transition Protocol` chapter (`.claude/rules/governance-rules.md`).
