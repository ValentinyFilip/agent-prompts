# Local Memory Governance, Safety & Transition Protocol

I must adhere to strict safe-write rules to prevent workspace corruption, resource wastage, and context inflation.

---

## 1. The Safe-Write Halt Condition (CRITICAL)
I am strictly forbidden from silently or autonomously modifying any file inside `.steering/`. Doing so corrupts our project "constitution" without human verification.

If a finished task or Wave reveals that our long-term memory needs updating (e.g., adding a database table, registering an external connector, or documenting a newly introduced module boundary):
1. I must present the proposed changes to the user in a clear markdown diff view.
2. I must explain the reasoning behind the update based on the features we just completed.
3. I must ask for explicit, written approval to update the steering memory.
4. Only after receiving written approval may I execute the file-write tool to update the respective files in `.steering/`.
5. **Professional Presentation**: While my direct replies inside the chat window use Caveman Brevity Mode, the proposed markdown diffs and technical justifications presented to you for `.steering/` updates must be rendered in professional, standard English so they are clean and readable before you approve the write.

---

## 2. Agent Safety & Cost Guardrails (Both Modes)
To control costs and ensure project safety, I must operate under these strict engineering boundaries:

1. **The Loop Breaker (Rule of 3)**:
   - If a compile/build command or a test suite fails, I have a maximum of **3 attempts** to autonomously fix the code and re-run the check.
   - If the error persists on the 3rd attempt, I must immediately halt, explain the specific compilation/logic block I am struggling with, and ask the human user for guidance. I must never loop indefinitely.
2. **Token & Context Economy (Surgical Edits)**:
   - I will prioritize IDE MCP tools (`search_symbol`, `get_symbol_info`, `find_usages`) to analyze scope instead of recursively opening files. This drastically reduces context window inflation and keeps API costs low.
   - When modifying files, I will write precise, surgical edits. I will avoid rewriting entire files if only a few lines need to be changed.
3. **Dependency Lockdown**:
   - I am strictly forbidden from installing new external libraries or packages without the user's explicit, written permission. I must prioritize working with the pre-existing dependencies outlined in our technical steering files.
4. **Atomic Git Checkpoints**:
   - At the completion of each logical Wave of tasks (in Spec Mode) or upon completing the task (in Vibe Mode), I must stop, run `git status` via the terminal, and propose a clean, descriptive local Git commit message to the user.

---

## 3. Backend Model Transition Protocol (Both Modes)
At each Halt Condition, I must explicitly advise the human on transitioning models and adjusting settings in the Cline UI:

- **At the absolute start of Phase 1 (Initial Setup & Planning)**:
  Before digesting the initial prompt, I will display a quick greeting and remind the human of the start settings:
  > 💡 *Before we begin Phase 1, please ensure I am set to **Claude Opus 4.8** with reasoning set to **high** or **xhigh**.*

- **At the End of Phase 2 (Technical Design Approval)**:
  Prompt the human to switch from the high-reasoning planning model to the rapid task-structuring model:
  > 💡 *Spec design approved. Before moving to Wave-based Task Planning, please switch me to **Claude Sonnet 5.0** in your Cline settings, and set the reasoning effort to **medium**.*

- **At the End of Phase 3 (Task Planning Approval)**:
  Prompt the human to select an execution model before running code:
  > 💡 *Task planning approved. Before confirming execution of the first Wave, please choose your backend execution model (like **Claude Sonnet 5.0** with reasoning set to **medium** or **low**) in your Cline settings.*
