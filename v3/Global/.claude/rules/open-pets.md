# OpenPets Desktop Companion Integration

The active desktop companion platform on this machine is **OpenPets**, exposed to Claude Code through an MCP server. Its logical tools are `openpets_status`, `openpets_react`, and `openpets_say`; Claude Code may display them with server-qualified names such as `mcp__<server-name>__<tool-name>`. I must inspect the current tool list to resolve the exact names instead of assuming a fixed prefix. If the server configuration is unclear, I must ask the user to run `/mcp`.

---

## 1. Tool Usage Guidelines
- **Use `openpets_status`**: Run the matching MCP tool before using other OpenPets tools or when checking local connection availability.
- **Use `openpets_say`**: Use the matching MCP tool only for short, non-spammy, meaningful notifications in speech bubbles (e.g., "Halt! Review design." or "TDD Green!").
- **Unavailable Server**: If OpenPets is not listed by `/mcp` or its tools are unavailable, continue the engineering task without OpenPets. Do not treat the optional companion as a blocker.
- **Do Not Spam**: Do not trigger speech bubbles or visual reactions for every minor internal step. Keep updates meaningful.

---

## 2. Safety & Local Privacy Guardrail
- **No Leaks**: I am strictly forbidden from sending raw code, compiler logs, terminal command outputs, file paths, URLs, secrets, API tokens, user prompts, or private variables to `openpets_say`.

---

## 3. Dynamic State Selection (Autonomous)
Instead of following a rigid script, I must use my own contextual reasoning to dynamically select the most appropriate state using the matching `openpets_react` MCP tool.

I must evaluate my current action in the workspace and map it strictly to one of the following supported states shown in my local dropdown configuration:

*Supported States for `openpets_react`:*
* **`Idle`**: Best used when resting, doing nothing, or between tasks.
* **`Review`**: Best used during conceptual phases, parsing files, reading code, requirements planning, or technical design review.
* **`Running`**: Best used when actively coding, writing C# files, refactoring logic, running compilers, or executing test suites.
* **`Waiting`**: Best used when hitting a Halt Condition and waiting for the user's input/approval.
* **`Waving`**: Best used when greeting the user at start, checking off a completed sub-task in `tasks.md`, or saying goodbye.
* **`Jumping`**: Best used to celebrate a fully green build, passing tests, or a completed Git commit.
* **`Failed`**: Best used when compiling fails, tests break, or the Loop Breaker (Rule of 3) is triggered.
