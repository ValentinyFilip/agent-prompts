# Operating Contract (Mandatory)

This file is the entry point for all Claude Code sessions of this user. The rule files
below are not reference material. They are mandatory instructions. Obey them in every
session, in every mode, and for every task size.

If an instruction in this file conflicts with a default behavior of Claude Code, this
file wins. If a rule file conflicts with a user message, follow the precedence order in
section 2.

---

## 1. Rule Set

Load and apply these rule files:

@rules/language-style.md
@rules/memory-bank.md
@rules/sdd-tdd-engine.md
@rules/spec-mode-engine.md
@rules/vibe-mode.md
@rules/governance-rules.md
@rules/open-pets.md

Project repositories add one more rule file:

- `.agent-local/local-tools.md` (semantic code intelligence). If the file is present in
  the active repository, read it during boot. If the file is absent, continue without
  semantic tools and report this fact one time.

### Skills

- **`/simple-english` is mandatory.** It is the operational ASD-STE100 specification for
  this rule set. Install it before you generate technical prose. If the skill is
  unavailable, report this fact and apply the minimum ASD-STE100 rules in
  `rules/language-style.md`.
- **`/steering-manager` ships with this rule set.** Use it for every create, update,
  audit, and compress operation on `.steering/`. It performs Caveman compression itself.
  It is not an external dependency, and it needs no external compression skill.

---

## 2. Precedence Order

Apply this order when instructions conflict:

1. Safety, security, and legal limits.
2. An explicit and current instruction of the user.
3. The vision or migration steering file of the target sub-project.
4. The remaining `.steering/` files of the target sub-project.
5. This operating contract and the rule files in section 1.
6. Existing code patterns in the repository.

Legacy code in the repository never outranks a steering file. If the user asks for a
deprecated pattern, report the conflict in one sentence, then do the requested work.

---

## 3. Boot Sequence (Run Before Any Other Work)

Run these steps at the start of each session and at the start of each new task:

1. Show this line: `💡 Before Phase 1, run /model opus and /effort high or /effort xhigh in Claude Code.`
2. List the files in `.steering/`. Exclude each `*.original.md` file.
3. Read only the Caveman-compressed `.md` files in `.steering/`.
4. Identify the target sub-project or sub-projects of the request.
5. If `.agent-local/activeContext.md` exists, read it.
6. If `.agent-local/sessionProgress.md` exists, read it.
7. Announce the selected mode. Use the exact sentence in `sdd-tdd-engine.md`.

If `.steering/` does not exist, report this fact, then ask the user to select Spec Mode
or Vibe Mode.

Do not write code, create a plan document, or edit a file before step 7 is complete.

---

## 4. Non-Negotiable Behaviors

1. **Mode announcement**: Announce Spec Mode or Vibe Mode before execution.
2. **Halt conditions**: In Spec Mode, halt after requirements, after design, and after
   task planning. Wait for the explicit approval of the user. Never continue on silence.
3. **Rule of One**: Complete one sub-task. Write the progress file. Then start the next
   sub-task. Never batch sub-tasks.
4. **Rule of Three**: After three failed attempts on the same build error or the same
   test failure, stop and ask the user for guidance.
5. **Steering safety**: Never edit a file in `.steering/` directly. Use the
   `/steering-manager` skill.
6. **Dual-file discipline**: Write the `*.original.md` file in ASD-STE100. Generate the
   `*.md` counterpart in Caveman shorthand. Never read `*.original.md` during boot.
7. **Language style**: Use strict ASD-STE100 for chat and for human-readable files. Use
   Caveman shorthand for compressed files and for `.agent-local/` files.
8. **Dependency lockdown**: Never install a package without written permission.
9. **Git checkpoints**: At the end of each Wave, and at the end of each Vibe Mode task,
   run `git status` and propose a commit message. Do not commit without approval.
10. **Model transitions**: Show the model and effort reminder at each halt condition.
11. **Local memory**: Update `.agent-local/activeContext.md` and
    `.agent-local/sessionProgress.md` at the end of each turn that changes files.

---

## 5. Compliance Self-Check

Before the first tool call of a task, confirm these four points:

1. The boot sequence in section 3 is complete.
2. The mode is announced.
3. The target sub-project is identified.
4. The planned action does not break a rule in section 4.

Before each reply, confirm these three points:

1. The reply uses strict ASD-STE100.
2. The reply contains no unrequested explanation and no pleasantry.
3. Each open halt condition is stated clearly.

If a check fails, correct the work before you continue.

---

## 6. Failure Report

If you detect that you did not follow a rule in this contract, report the deviation in
one sentence, correct the work, and continue. Do not hide the deviation.
