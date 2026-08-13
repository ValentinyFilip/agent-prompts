# Project Operating Contract (Mandatory)

This file reinforces the user-level operating contract at `~/.claude/CLAUDE.md` for this
repository. The user-level rule files stay in force. This file adds the repository-local
requirements.

If the user-level contract is absent, treat the rules in `~/.claude/rules/` as mandatory
anyway, and report the missing contract one time.

---

## 1. Repository-Local Rules

@.agent-local/local-tools.md

---

## 2. Boot Sequence for This Repository

Run these steps before any other work:

1. List the files in `.steering/`. Exclude each `*.original.md` file.
2. Read only the Caveman-compressed `.md` files in `.steering/`.
3. Identify the target sub-project or sub-projects of the request.
4. If `.agent-local/activeContext.md` exists, read it.
5. If `.agent-local/sessionProgress.md` exists, read it.
6. Announce Spec Mode or Vibe Mode.

Do not edit a file before step 6 is complete.

---

## 3. Repository Constraints

1. Use the semantic tools in `.agent-local/local-tools.md` before you open files
   recursively.
2. Never edit a file in `.steering/` directly. Use the `/steering-manager` skill.
3. Never install a package without written permission.
4. Update `.agent-local/activeContext.md` and `.agent-local/sessionProgress.md` at the
   end of each turn that changes files.
5. At the end of each Wave or Vibe Mode task, run `git status` and propose a commit
   message.

---

## 4. Compliance Self-Check

Before the first tool call, confirm that the boot sequence is complete, that the mode is
announced, and that the target sub-project is identified. If a check fails, correct the
work before you continue.
